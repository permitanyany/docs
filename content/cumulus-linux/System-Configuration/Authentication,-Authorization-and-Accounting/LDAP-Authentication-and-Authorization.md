---
title: LDAP Authentication and Authorization
author: Cumulus Networks
weight: 279
aliases:
 - /display/DOCS/LDAP+Authentication+and+Authorization
 - /pages/viewpage.action?pageId=8362556
pageID: 8362556
product: Cumulus Linux
version: 3.7
imgData: cumulus-linux
siteSlug: cumulus-linux
---
Cumulus Linux uses Pluggable Authentication Modules (PAM) and Name
Service Switch (NSS) for user authentication.

NSS specifies the order of information sources used to resolve names for
each service. Using this with authentication and authorization, it
provides the order and location used for user lookup and group mapping
on the system. PAM handles the interaction between the user and the
system, providing login handling, session setup, authentication of
users, and authorization of user actions.

NSS enables PAM to use LDAP to provide user authentication, group
mapping, and information for other services on the system.

## Configure LDAP Authentication

There are 3 common ways to configure LDAP authentication on Linux:

  - libnss-ldap
  - libnss-ldapd
  - libnss-sss

This chapter describes using `libnss-ldapd` only. From internal testing,
this library worked best with Cumulus Linux and is the easiest to
configure, automate, and troubleshoot.

## Install libnss-ldapd

The `libpam-ldapd` package depends on `nslcd`. To install
`libnss-ldapd`, `libpam-ldapd`, and `ldap-utils`, run the following command:

    cumulus@switch:~$ sudo apt-get install libnss-ldapd libpam-ldapd ldap-utils nslcd

Follow the interactive prompts to answer questions about the LDAP URI,
search base distinguished name (DN), and services that must have LDAP
lookups enabled. This creates a very basic LDAP configuration using
anonymous bind and initiates user search under the base DN specified.

{{%notice note%}}

Alternatively, you can pre-seed these parameters using the
`debconf-utils`. To use this method, run `apt-get install debconf-utils`
and create the pre-seeded parameters using `debconf-set-selections` with
the appropriate answers. Run `debconf-show <pkg>` to check the settings.
Here is an example of how to pre-seed answers to the installer questions
using `debconf-set-selections`:

    root# debconf-set-selections <<'zzzEndOfFilezzz'
     
    # LDAP database user. Leave blank will be populated later!
    # This way of setting binddn and bindpw doesn't seem to work.
    # So have to manually do it. But interactive apt-get mode works.
    nslcd nslcd/ldap-binddn  string
     
    # LDAP user password. Leave blank!
    nslcd nslcd/ldap-bindpw   password
     
    # LDAP server search base:
    nslcd nslcd/ldap-base string  ou=support,dc=rtp,dc=example,dc=test
     
    # LDAP server URI. Using ldap over ssl.
    nslcd nslcd/ldap-uris string  ldaps://myadserver.rtp.example.test
     
    # New to 0.9. restart cron, exim and others libraries without asking
    nslcd libraries/restart-without-asking: boolean true
     
    # LDAP authentication to use:
    # Choices: none, simple, SASL
    # Using simple because its easy to configure. Security comes by using LDAP over SSL
    # keep /etc/nslcd.conf 'rw' to root for basic security of bindDN password
    nslcd nslcd/ldap-auth-type    select  simple
     
    # Don't set starttls to true
    nslcd nslcd/ldap-starttls     boolean false
     
    # Check server's SSL certificate:
    # Choices: never, allow, try, demand
    nslcd nslcd/ldap-reqcert      select  never
     
    # Choices: Ccreds credential caching - password saving, Unix authentication, LDAP Authentication , Create home directory on first time login, Ccreds credential caching - password checking
    # This is where "mkhomedir" pam config is activated that allows automatic creation of home directory
    libpam-runtime        libpam-runtime/profiles multiselect     ccreds-save, unix, ldap, mkhomedir , ccreds-check
     
    # for internal use; can be preseeded
    man-db        man-db/auto-update      boolean true
     
    # Name services to configure:
    # Choices: aliases, ethers, group, hosts, netgroup, networks, passwd, protocols, rpc, services,  shadow
    libnss-ldapd  libnss-ldapd/nsswitch   multiselect     group, passwd, shadow
    libnss-ldapd  libnss-ldapd/clean_nsswitch     boolean false
     
    ## define platform specific libnss-ldapd debconf questions/answers.
    ## For demo used amd64.
    libnss-ldapd:amd64    libnss-ldapd/nsswitch   multiselect     group, passwd, shadow
    libnss-ldapd:amd64    libnss-ldapd/clean_nsswitch     boolean false
    # libnss-ldapd:powerpc   libnss-ldapd/nsswitch   multiselect     group, passwd, shadow
    # libnss-ldapd:powerpc    libnss-ldapd/clean_nsswitch     boolean false
     
    zzzEndOfFilezzz

{{%/notice%}}

After the install is complete, the *name service LDAP caching daemon*
(`nslcd`) runs. This service handles all of the LDAP protocol
interactions and caches information returned from the LDAP server. In
the `/etc/nsswitch.conf` file, `ldap` **** is appended and is the
secondary information source for *passwd*, *group*, and *shadow*. The
local files (`/etc/passwd`, `/etc/groups` and `/etc/shadow`) are used
first, as specified by the `compat` source.

    passwd: compat ldap
    group: compat ldap
    shadow: compat ldap

{{%notice warning%}}

Cumulus Networks recommends that you keep `compat` as the first source
in NSS for *passwd*, *group*, and *shadow*. This prevents you from
getting locked out of the system.

{{%/notice%}}

## Configure nslcd.conf

After installation, you need to update the main configuration file
(`/etc/nslcd.conf`) to accommodate the expected LDAP server settings.
The [nslcd.conf man page](http://linux.die.net/man/5/nslcd.conf) details
all the available configuration options. Some of the more important
options relate to security and how the queries are handled.

{{%notice note%}}

After first editing the `/etc/nslcd.conf` file, and/or enabling LDAP in
the `/etc/nsswitch.conf` file, you need to restart `netd` with the `sudo
systemctl restart netd` command. Also, restart `netd` if you disable LDAP.

{{%/notice%}}

### Connection

The LDAP client starts a session by connecting to the LDAP server on TCP
and UDP port 389, or on port 636 for LDAPS. Depending on the
configuration, this connection might be unauthenticated (anonymous
bind); otherwise, the client must provide a bind user and password. The
variables used to define the connection to the LDAP server are the URI
and bind credentials.

The URI is mandatory and specifies the LDAP server location using the
FQDN or IP address. The URI also designates whether to use `ldap://` for
clear text transport, or `ldaps://` for SSL/TLS encrypted transport. You
can also specify an alternate port in the URI. Typically, in production
environments, it is best to utilize the LDAPS protocol; otherwise, all
communications are clear text and not secure.

After the connection to the server is complete, the BIND operation
authenticates the session. The BIND credentials are optional, and if not
specified, an anonymous bind is assumed. This is typically not allowed
in most production environments. Configure authenticated (Simple) BIND
by specifying the user (*binddn*) and password (*bindpw*) in the
configuration. Another option is to use SASL (Simple Authentication and
Security Layer) BIND, which provides authentication services using other
mechanisms, like Kerberos. Contact your LDAP server administrator for
this information as it depends on the configuration of the LDAP server
and the credentials that are created for the client device.

    # The location at which the LDAP server(s) should be reachable.
    uri ldaps://ldap.example.com
    # The DN to bind with for normal lookups.
    binddn cn=CLswitch,ou=infra,dc=example,dc=com
    bindpw CuMuLuS

### Search Function

When an LDAP client requests information about a resource, it must
connect and bind to the server. Then, it performs one or more resource
queries depending on the lookup. All search queries sent to the LDAP
server are created using the configured search *base*, *filter*, and the
desired entry (*uid=myuser*) being searched. If the LDAP directory is
large, this search might take a significant amount of time. It is a good
idea to define a more specific search base for the common *maps*
(*passwd* and *group*).

    # The search base that will be used for all queries.
    base dc=example,dc=com
    # Mapped search bases to speed up common queries.
    base passwd ou=people,dc=example,dc=com
    base group ou=groups,dc=example,dc=com

### Search Filters

It is also common to use search filters to specify criteria used when
searching for objects within the directory. This is used to limit the
search scope when authenticating users. The default filters applied are:

    filter passwd (objectClass=posixAccount)
    filter group (objectClass=posixGroup)

### Attribute Mapping

The *map* configuration allows you to override the attributes pushed
from LDAP. To override an attribute for a given *map*, specify the
attribute name and the new value. This is useful to ensure that the
shell is *bash* and the home directory is `/home/cumulus`:

    map    passwd homeDirectory "/home/cumulus"
    map    passwd shell "/bin/bash"

{{%notice note%}}

In LDAP, the ***map*** refers to one of the supported maps specified in
the manpage for `nslcd.conf` (such as *passwd* or *group*).

{{%/notice%}}

### Example Configuration

<details>
<summary>Here is an example configuration using Cumulus Linux ...</summary>

    # /etc/nslcd.conf
    # nslcd configuration file. See nslcd.conf(5)
    # for details.
     
    # The user and group nslcd should run as.
    uid nslcd
    gid nslcd
     
    # The location at which the LDAP server(s) should be reachable.
    uri ldaps://myadserver.rtp.example.test
     
    # The search base that will be used for all queries.
    base ou=support,dc=rtp,dc=example,dc=test
     
    # The LDAP protocol version to use.
    #ldap_version 3
     
    # The DN to bind with for normal lookups.
    # defconf-set-selections doesn't seem to set this. so have to manually set this.
    binddn CN=cumulus admin,CN=Users,DC=rtp,DC=example,DC=test
    bindpw 1Q2w3e4r!
     
    # The DN used for password modifications by root.
    #rootpwmoddn cn=admin,dc=example,dc=com
     
    # SSL options
    #ssl off (default)
    # Not good does not prevent man in the middle attacks
    #tls_reqcert demand(default)
    tls_cacertfile /etc/ssl/certs/rtp-example-ca.crt
     
    # The search scope.
    #scope sub
     
    # Add nested group support
    # Supported in nslcd 0.9 and higher.
    # default wheezy install of nslcd supports on 0.8. wheezy-backports has 0.9
    nss_nested_groups yes
     
    # Mappings for Active Directory
    # (replace the SIDs in the objectSid mappings with the value for your domain)
    # "dsquery * -filter (samaccountname=testuser1) -attr ObjectSID" where cn == 'testuser1'
    pagesize 1000
    referrals off
    idle_timelimit 1000
     
    # Do not allow uids lower than 100 to login (aka Administrator)
    # not needed as pam already has this support
    # nss_min_uid 1000
     
    # This filter says to get all users who are part of the cumuluslnxadm group. Supports nested groups.
    # Example, mary is part of the snrnetworkadm group which is part of cumuluslnxadm group
    # Ref: http://msdn.microsoft.com/en-us/library/aa746475%28VS.85%29.aspx (LDAP_MATCHING_RULE_IN_CHAIN)
    filter passwd (&(Objectclass=user)(!(objectClass=computer))(memberOf:1.2.840.113556.1.4.1941:=cn=cumuluslnxadm,ou=groups,ou=support,dc=rtp,dc=example,dc=test))
    map    passwd uid           sAMAccountName
    map    passwd uidNumber     objectSid:S-1-5-21-1391733952-3059161487-1245441232
    map    passwd gidNumber     objectSid:S-1-5-21-1391733952-3059161487-1245441232
    map    passwd homeDirectory "/home/$sAMAccountName"
    map    passwd gecos         displayName
    map    passwd loginShell    "/bin/bash"
     
    # Filter for any AD group or user in the baseDN. the reason for filtering for the
    # user to make sure group listing for user files don't say '<user> <gid>'. instead will say '<user> <user>'
    # So for cosmetic reasons..nothing more.
    filter group (&(|(objectClass=group)(Objectclass=user))(!(objectClass=computer)))
    map    group gidNumber     objectSid:S-1-5-21-1391733952-3059161487-1245441232
    map    group cn            sAMAccountName
</details>

## Troubleshooting

This section provides troubleshooting tips.

### nslcd Debug Mode

When setting up LDAP authentication for the first time, Cumulus Networks
recommends you turn off the `nslcd` service using the `systemctl stop
nslcd.service` command and run it in debug mode. Debug mode works
whether you are using LDAP over SSL (port 636) or an unencrypted LDAP
connection (port 389).

    cumulus@switch:~$ sudo systemctl stop nslcd.service
    cumulus@switch:~$ sudo nslcd -d

After you enable debug mode, run the following command to test LDAP
queries:

    cumulus@switch:~$ sudo getent myuser

If LDAP is configured correctly, the following messages appear after you
run the `getent` command:

    nslcd: DEBUG: accept() failed (ignored): Resource temporarily unavailable
    nslcd: [8e1f29] DEBUG: connection from pid=11766 uid=0 gid=0
    nslcd: [8e1f29] <passwd(all)> DEBUG: myldap_search(base="dc=example,dc=com", filter="(objectClass=posixAccount)")
    nslcd: [8e1f29] <passwd(all)> DEBUG: ldap_result(): uid=myuser,ou=people,dc=example,dc=com
    nslcd: [8e1f29] <passwd(all)> DEBUG: ldap_result(): ... 152 more results
    nslcd: [8e1f29] <passwd(all)> DEBUG: ldap_result(): end of results (162 total)

In the output above, *\<passwd(all)\>* indicates that the entire
directory structure is queried.

You can query a specific user with the following command:

    cumulus@switch:~$ sudo getent passwd myuser

You can replace *myuser* with any username on the switch. The following
debug output indicates that user *myuser* exists:

    nslcd: DEBUG: add_uri(ldap://10.50.21.101)
    nslcd: version 0.8.10 starting
    nslcd: DEBUG: unlink() of /var/run/nslcd/socket failed (ignored): No such file or directory
    nslcd: DEBUG: setgroups(0,NULL) done
    nslcd: DEBUG: setgid(110) done
    nslcd: DEBUG: setuid(107) done
    nslcd: accepting connections
    nslcd: DEBUG: accept() failed (ignored): Resource temporarily unavailable
    nslcd: [8b4567] DEBUG: connection from pid=11369 uid=0 gid=0
    nslcd: [8b4567] <passwd="myuser"> DEBUG: myldap_search(base="dc=cumulusnetworks,dc=com", filter="(&(objectClass=posixAccount)(uid=myuser))")
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_initialize(ldap://<ip_address>)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_rebind_proc()
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_PROTOCOL_VERSION,3)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_DEREF,0)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_TIMELIMIT,0)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_TIMEOUT,0)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_NETWORK_TIMEOUT,0)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_REFERRALS,LDAP_OPT_ON)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_set_option(LDAP_OPT_RESTART,LDAP_OPT_ON)
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_simple_bind_s(NULL,NULL) (uri="ldap://<ip_address>")
    nslcd: [8b4567] <passwd="myuser"> DEBUG: ldap_result(): end of results (0 total)

Notice how the `<passwd="myuser">` shows that the specific *myuser* user
was queried.

### Common Problems

This section discusses common problems.

#### SSL/TLS

  - The FQDN of the LDAP server URI does not match the FQDN in the
    CA-signed server certificate exactly.
  - `nslcd` cannot read the SSL certificate and reports a *Permission
    denied* error in the debug during server connection negotiation.
    Check the permission on each directory in the path of the root SSL
    certificate. Ensure that it is readable by the `nslcd` user.

#### NSCD

  - If the `nscd cache` daemon is also enabled and you make some changes
    to the user from LDAP, you can clear the cache using the following
    commands:

        nscd --invalidate = passwd
        nscd --invalidate = group
  - The `nscd` package works with `nslcd` to cache name entries returned
    from the LDAP server. This might cause authentication failures. To
    work around these issues:

    1.  Disable `nscd` by running:

            cumulus@switch:~$ sudo nscd -K

    2.  Restart the `nslcd` service:

            cumulus@switch:~$ sudo systemctl restart nslcd.service

    3.  Try the authentication again.

#### LDAP

  - The search filter returns wrong results. Check for typos in the
    search filter. Use `ldapsearch` to test your filter.

  - Optionally, configure the basic LDAP connection and search
    parameters in `/etc/ldap/ldap.conf`.

        # ldapsearch -D 'cn=CLadmin' -w 'CuMuLuS' "(&(ObjectClass=inetOrgUser)(uid=myuser))"

  - When a local username also exists in the LDAP database, the order of
    the information sources in `/etc/nsswitch` can be updated to query
    LDAP before the local user database. This is generally not
    recommended. For example, the configuration below ensures that LDAP
    is queried before the local database.

        # /etc/nsswitch.conf
        passwd:         ldap compat

## Configure LDAP Authorization

Linux uses the *sudo* command to allow non-administrator users (such as
the default *cumulus* user account) to perform privileged operations. To
control the users authorized to use sudo, the `/etc/sudoers` file and
files located in the `/etc/sudoers.d/` directory have a series of rules
defined. Typically, the rules are based on groups, but can also be
defined for specific users. Therefore, sudo rules can be added using the
group names from LDAP. For example, if a group of users are associated
with the group *netadmin*, you can add a rule to give those users sudo
privileges. Refer to the sudoers manual (`man sudoers`) for a complete
usage description. Here's an illustration of this in `/etc/sudoers`:

    # The basic structure of a user specification is “who where = (as_whom) what”.
    %sudo ALL=(ALL:ALL) ALL
    %netadmin ALL=(ALL:ALL) ALL

## Active Directory Configuration

Active Directory (AD) is a fully featured LDAP-based NIS server created
by Microsoft. It offers unique features that classic OpenLDAP servers
lack. Therefore, it can be more complicated to configure on the client
and each version of AD is a little different in how it works with
Linux-based LDAP clients. Some more advanced configuration examples,
from testing LDAP clients on Cumulus Linux with Active Directory
(AD/LDAP), are available in our 
[knowledge base](https://support.cumulusnetworks.com/hc/en-us/articles/204383797).

## LDAP Verification Tools

Typically, password and group information is retrieved from LDAP and
cached by the LDAP client daemon. To test the LDAP interaction, you can
use these command-line tools to trigger an LDAP query from the device.
This helps to create the best filters and verify the information sent
back from the LDAP server.

### Identify a User with the id Command

The `id` command performs a username lookup by following the lookup
information sources in NSS for the *passwd* service. This simply returns
the user ID, group ID and the group list retrieved from the information
source. In the following example, the user *cumulus* is locally defined
in `/etc/passwd`, and *myuser* is on LDAP. The NSS configuration has the
passwd map configured with the sources `compat ldap`:

    cumulus@switch:~$ id cumulus
    uid=1000(cumulus) gid=1000(cumulus) groups=1000(cumulus),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev)
    cumulus@switch:~$ id myuser
    uid=1230(myuser) gid=3000(Development) groups=3000(Development),500(Employees),27(sudo)

### getent

The `getent` command retrieves all records found with NSS for a given
map. It can also get a specific entry under that map. You can perform
tests with the passwd, group, shadow, or any other map configured in
`/etc/nsswitch.conf`. The output from this command is formatted
according to the map requested. Therefore, for the passwd service, the
structure of the output is the same as the entries in `/etc/passwd`. The
group map outputs the same structure as `/etc/group`. In this example,
looking up a specific user in the passwd map, the user *cumulus* is
locally defined in `/etc/passwd`, and *myuser* is only in LDAP.

    cumulus@switch:~$ getent passwd cumulus
    cumulus:x:1000:1000::/home/cumulus:/bin/bash
    cumulus@switch:~$ getent passwd myuser
    myuser:x:1230:3000:My Test User:/home/myuser:/bin/bash

In the next example, looking up a specific group in the group service,
the group *cumulus* is locally defined in `/etc/groups`, and *netadmin*
is on LDAP.

    cumulus@switch:~$ getent group cumulus
    cumulus:x:1000:
    cumulus@switch:~$ getent group netadmin
    netadmin:*:502:larry,moe,curly,shemp

Running the command `getent passwd` or `getent group` without a specific
request returns **all** local and LDAP entries for the *passwd* and *group* maps.

### LDAP search

The `ldapsearch` command performs LDAP operations directly on the LDAP
server. This does not interact with NSS. This command helps display what
the LDAP daemon process is receiving back from the server. The command
has many options. The simplest uses anonymous bind to the host and
specifies the search DN and the attribute to look up.

    cumulus@switch:~$ ldapsearch -H ldap://ldap.example.com -b dc=example,dc=com -x uid=myuser

<details>
<summary>Click to expand the command output ... </summary>

    # extended LDIF
    #
    # LDAPv3
    # base <dc=example,dc=com> with scope subtree
    # filter: uid=myuser
    # requesting: ALL
    #
    # myuser, people, example.com
    dn: uid=myuser,ou=people,dc=example,dc=com
    cn: My User
    displayName: My User
    gecos: myuser
    gidNumber: 3000
    givenName: My
    homeDirectory: /home/myuser
    initials: MU
    loginShell: /bin/bash
    mail: myuser@example.com
    objectClass: inetOrgPerson
    objectClass: posixAccount
    objectClass: shadowAccount
    objectClass: top
    shadowExpire: -1
    shadowFlag: 0
    shadowMax: 999999
    shadowMin: 8
    shadowWarning: 7
    sn: User
    uid: myuser
    uidNumber: 1234

    # search result
    search: 2
    result: 0 Success

    # numResponses: 2
    # numEntries: 1
</details>

### LDAP Browsers

There are several GUI LDAP clients available that help to work with LDAP
servers. These are free tools to help show the structure of the LDAP
database graphically.

  - [Apache Directory Studio](http://directory.apache.org/studio/)
  - [LDAPManager](http://ldapmanager.sourceforge.net/)

## Related Information

  - [Debian - configuring LDAP authentication](https://wiki.debian.org/LDAP/NSS)
  - [Debian - configuring PAM to use LDAP](https://wiki.debian.org/LDAP/PAM)
  - [GitHub - Arthur de Jong nslcd.conf file](https://raw.githubusercontent.com/arthurdejong/nss-pam-ldapd/master/nslcd.conf)
  - [Debian backports](http://backports.debian.org/Instructions/)
