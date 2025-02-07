---
title: Border Gateway Protocol - BGP
author: Cumulus Networks
weight: 181
aliases:
 - /display/CL36/Border+Gateway+Protocol+++BGP
 - /display/CL36/Border+Gateway+Protocol+-+BGP
 - /display/CL36/Border+Gateway+Protocol+BGP
 - /pages/viewpage.action?pageId=8362396
pageID: 8362396
product: Cumulus Linux
version: '3.6'
imgData: cumulus-linux-36
siteSlug: cumulus-linux-36
---
BGP is the routing protocol that runs the Internet. It is an
increasingly popular protocol for use in the data center as it lends
itself well to the rich interconnections in a Clos topology.
Specifically, BGP:

  - Does not require routing state to be periodically refreshed, unlike
    OSPF.

  - Is less chatty than its link-state siblings. For example, a link or
    node transition can result in a bestpath change, causing BGP to send
    updates.

  - Is multi-protocol and extensible.

  - Has many robust vendor implementations.

  - Is very mature as a protocol and comes with many years of
    operational experience.

[RFC 7938](https://tools.ietf.org/html/rfc7938) provides further details
of the use of BGP within the data center.

## Autonomous System Number (ASN)

One of the key concepts in BGP is an *autonomous* *system number* or
ASN. An [autonomous
system](https://en.wikipedia.org/wiki/Autonomous_System_%28Internet%29)
is defined as a set of routers under a common administration. Because
BGP was originally designed to peer between independently managed
enterprises and/or service providers, each such enterprise is treated as
an autonomous system, responsible for a set of network addresses. Each
such autonomous system is given a unique number called its ASN. ASNs are
handed out by a central authority (ICANN). However, ASNs between 64512
and 65535 are reserved for private use. Using BGP within the data center
relies on either using this number space or using the single ASN you
own.

The ASN is central to how BGP builds a forwarding topology. A BGP route
advertisement carries with it not only the originator’s ASN, but also
the list of ASNs that this route advertisement has passed through. When
forwarding a route advertisement, a BGP speaker adds itself to this
list. This list of ASNs is called the *AS path*. BGP uses the AS path to
detect and avoid loops.

ASNs were originally 16-bit numbers, but were later modified to be
32-bit. FRRouting supports both 16-bit and 32-bit ASNs, but most
implementations still run with 16-bit ASNs.

## eBGP and iBGP

When BGP is used to peer between autonomous systems, the peering is
referred to as *external BGP* or eBGP. When BGP is used within an
autonomous system, the peering used is referred to as *internal BGP* or
iBGP. eBGP peers have different ASNs while iBGP peers have the same ASN.

The heart of the protocol is the same when used as eBGP or iBGP,
however, there is a key difference in the protocol behavior between use
as eBGP and iBGP: an iBGP speaker does not forward routing information
learned from one iBGP peer to another iBGP peer to prevent loops. eBGP
prevents loops using the AS\_Path attribute.

All iBGP speakers need to be peered with each other in a full mesh. In a
large network, this requirement can quickly become unscalable. The most
popular method to scale iBGP networks is to introduce a *route
reflector*.

## Route Reflectors

Route reflectors are quite easy to understand in a Clos topology. In a
two-tier Clos network, the leaf (or tier 1) switches are the only ones
connected to end stations. Subsequently, this means that the spines
themselves do not have any routes to announce; they are merely
**reflecting** the routes announced by one leaf to the other leaves.
Therefore, the spine switches function as route reflectors while the
leaf switches serve as route reflector clients.

In a three-tier network, the tier 2 nodes (or mid-tier spines) act as
both route reflector servers and route reflector clients. They act as
route reflectors because they announce the routes learned from the tier
1 nodes to other tier 1 nodes and to tier 3 nodes. They also act as
route reflector clients to the tier 3 nodes, receiving routes learned
from other tier 2 nodes. Tier 3 nodes act only as route reflectors.

In the following illustration, tier 2 node 2.1 is acting as a route
reflector server, announcing the routes between tier 1 nodes 1.1 and 1.2
to tier 1 node 1.3. It is also a route reflector client, learning the
routes between tier 2 nodes 2.2 and 2.3 from the tier 3 node, 3.1.

{{% imgOld 0 %}}

{{%notice note%}}

**Configuring route-reflector-client Requires Specific Order**

When configuring a router to be a route reflector client, you must
specify the FRRouting configuration in a specific order; otherwise, the
router will not be a route reflector client.

You must run the `net add bgp neighbor <IPv4/IPV6>
route-reflector-client` command after the `net add bgp neighbor
<IPV4/IPV6> activate` command; otherwise, the `route-reflector-client`
command is ignored. For example:

    cumulus@switch:~$ net add bgp ipv4 unicast neighbor 14.0.0.9 activate 
    cumulus@switch:~$ net add bgp neighbor 14.0.0.9 next-hop-self
    cumulus@switch:~$ net add bgp neighbor 14.0.0.9 route-reflector-client >>> Must be after activate 
    cumulus@switch:~$ net add bgp neighbor 2001:ded:beef:2::1 remote-as 65000
    cumulus@switch:~$ net add bgp ipv6 unicast redistribute connected
    cumulus@switch:~$ net add bgp maximum-paths ibgp 4 
    cumulus@switch:~$ net add bgp neighbor 2001:ded:beef:2::1 activate 
    cumulus@switch:~$ net add bgp neighbor 2001:ded:beef:2::1 next-hop-self 
    cumulus@switch:~$ net add bgp neighbor 2001:ded:beef:2::1 route-reflector-client >>> Must be after activate 

{{%/notice%}}

### Configuring Clusters

A cluster consists of route reflectors (RRs) and their clients and is
used in iBGP environments where multiple sets of route reflectors and
their clients are configured. Configuring a unique ID per cluster (on
the route reflector server and clients) prevents looping as a route
reflector does not accept routes from another that has the same cluster
ID. Additionally, because all route reflectors in the cluster recognize
updates from peers in the same cluster, they do not install routes from
a route reflector in the same cluster; this reduces the number of
updates that need to be stored in BGP routing tables.

To configure a cluster ID on a route reflector, run the `net add bgp
cluster-id (<ipv4>|<1-4294967295>)` command. You can enter the cluster
ID as an IP address or as a 32-bit quantity.

The following example configures a cluster ID on a route reflector in IP
address format:

    cumulus@switch:~$ net add bgp cluster-id 14.0.0.9
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

The following example configures a cluster ID on a route reflector as a
32-bit quantity:

    cumulus@switch:~$ net add bgp cluster-id 321
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

## ECMP with BGP

If a BGP node hears a prefix **p** from multiple peers, it has all the
information necessary to program the routing table to forward traffic
for that prefix **p** through all of these peers; BGP supports
equal-cost multipathing (ECMP).

To perform ECMP in BGP, you may need to configure `net add bgp bestpath
as-path multipath-relax` (if you are using eBGP).

### Maximum Paths

In Cumulus Linux, the BGP `maximum-paths` setting is enabled by default,
so multiple routes are already installed. The default setting is 64
paths.

### BGP for Both IPv4 and IPv6

Unlike OSPF, which has separate versions of the protocol to announce
IPv4 and IPv6 routes, BGP is a multi-protocol routing engine, capable of
announcing both IPv4 and IPv6 prefixes. It supports announcing IPv4
prefixes over an IPv4 session and IPv6 prefixes over an IPv6 session. It
also supports announcing prefixes of both these address families over a
single IPv4 session or over a single IPv6 session.

## Configuring BGP

A basic BGP configuration looks like the following. However, the rest of
this chapter discusses how to configure various other features, from
unnumbered interfaces to route maps.

1.  Enable the BGP and Zebra daemons, `zebra` and `bgpd`, then enable
    the FRRouting service and start it, as described in [Configuring
    FRRouting](/version/cumulus-linux-36/Layer-3/Configuring-FRRouting/).

2.  Identify the BGP node by assigning an ASN and `router-id`:
    
        cumulus@switch:~$ net add bgp autonomous-system 65000
        cumulus@switch:~$ net add bgp router-id 10.0.0.1

3.  Specify where to disseminate routing information:
    
        cumulus@switch:~$ net add bgp neighbor 10.0.0.2 remote-as external
    
    If it is an iBGP session, the `remote-as` is the same as the local
    AS:
    
        cumulus@switch:~$ net add bgp neighbor 10.0.0.2 remote-as internal
    
    Specifying the IP address of the peer, allows BGP to set up a TCP
    socket with this peer, but it doesn’t distribute any prefixes to it,
    unless it is explicitly told that it must with the `activate`
    command:
    
        cumulus@switch:~$ net add bgp ipv4 unicast neighbor 10.0.0.2 activate
        cumulus@switch:~$ net add bgp ipv6 unicast neighbor 2001:db8:0002::0a00:0002 activate
    
    As you can see, `activate` has to be specified for each address
    family that is being announced by the BGP session.

4.  Specify some properties of the BGP session:
    
        cumulus@switch:~$ net add bgp neighbor 10.0.0.2 next-hop-self
    
    If this is a route-reflector client, it can be specified as follows:
    
        cumulus@switchRR:~$ net add bgp neighbor 10.0.0.1 route-reflector-client
    
    {{%notice note%}}
    
    It is node *switchRR*, the route reflector, on which the peer is
    specified as a client.
    
    {{%/notice%}}

5.  Specify what prefixes to originate:
    
        cumulus@switch:~$ net add bgp ipv4 unicast network 192.0.2.0/24
        cumulus@switch:~$ net add bgp ipv4 unicast network 203.0.113.1/24
        cumulus@switch:~$ net pending
        cumulus@switch:~$ net commit

## Using BGP Unnumbered Interfaces

Unnumbered interfaces are interfaces without unique IP addresses. In
BGP, you configure unnumbered interfaces using 
*extended next-hop encoding* (ENHE), which is defined by
[RFC 5549](https://tools.ietf.org/html/rfc5549). BGP unnumbered
interfaces provide a means of advertising an IPv4 route with an IPv6
next-hop. Prior to RFC 5549, an IPv4 route could be advertised only with
an IPv4 next-hop.

BGP unnumbered interfaces are particularly useful in deployments where
IPv4 prefixes are advertised through BGP over a section without any IPv4
address configuration on links. As a result, the routing entries are
also IPv4 for destination lookup and have IPv6 next-hops for forwarding
purposes.

### BGP and Extended Next-hop Encoding

When enabled and active, BGP makes use of the available IPv6 next-hops
for advertising any IPv4 prefixes. BGP learns the prefixes, calculates
the routes and installs them in IPv4 AFI to IPv6 AFI format. However,
ENHE in Cumulus Linux does not install routes into the kernel in IPv4
prefix to IPv6 next-hop format. For link-local peerings enabled by
dynamically learning the other end's link-local address using IPv6
neighbor discovery router advertisements, an IPv6 next-hop is converted
into an IPv4 link-local address and a static neighbor entry is installed
for this IPv4 link-local address with the MAC address derived from the
link-local address of the other end.

{{%notice note%}}

It is assumed that the IPv6 implementation on the peering device will
use the MAC address as the interface ID when assigning the IPv6
link-local address, as suggested by RFC 4291.

{{%/notice%}}

### Configuring BGP Unnumbered Interfaces

Configuring a BGP unnumbered interface requires enabling IPv6 neighbor
discovery router advertisements. The `interval` you specify is measured
in seconds and defaults to 10 seconds. Extended next-hop encoding is
sent only for the link-local address peerings:

    cumulus@switch:~$ net add bgp autonomous-system 65020
    cumulus@switch:~$ net add bgp router-id 10.0.0.21
    cumulus@switch:~$ net add bgp bestpath as-path multipath-relax
    cumulus@switch:~$ net add bgp bestpath compare-routerid
    cumulus@switch:~$ net add bgp neighbor fabric peer-group
    cumulus@switch:~$ net add bgp neighbor fabric remote-as external
    cumulus@switch:~$ net add bgp neighbor fabric description Internal Fabric Network
    cumulus@switch:~$ net add bgp neighbor fabric capability extended-nexthop
    cumulus@switch:~$ net add bgp neighbor swp1 interface peer-group fabric
    cumulus@switch:~$ net add bgp neighbor swp2 interface peer-group fabric
    cumulus@switch:~$ net add bgp neighbor swp3 interface peer-group fabric
    cumulus@switch:~$ net add bgp neighbor swp4 interface peer-group fabric
    cumulus@switch:~$ net add bgp neighbor swp29 interface peer-group fabric
    cumulus@switch:~$ net add bgp neighbor swp30 interface peer-group fabric

These commands create the following configuration in the
`/etc/frr/frr.conf` file:

    router bgp 65020
     bgp router-id 10.0.0.21
     bgp bestpath as-path multipath-relax
     bgp bestpath compare-routerid
     neighbor fabric peer-group
     neighbor fabric remote-as external
     neighbor fabric description Internal Fabric Network
     neighbor fabric capability extended-nexthop
     neighbor swp1 interface peer-group fabric
     neighbor swp2 interface peer-group fabric
     neighbor swp3 interface peer-group fabric
     neighbor swp4 interface peer-group fabric
     neighbor swp29 interface peer-group fabric
     neighbor swp30 interface peer-group fabric
    !

Notice above, for an unnumbered configuration, you can use a single
command to configure a neighbor and attach it to a
[peer group](#using-peer-groups-to-simplify-configuration) (making sure
to substitute for the interface and peer group below):

    cumulus@switch:~$ net add bgp neighbor <swpX> interface peer-group <group name>

### Managing Unnumbered Interfaces

All the relevant BGP commands are now capable of showing IPv6 next-hops
and/or the interface name for any IPv4 prefix:

    cumulus@switch:~$ net show bgp
     
    show bgp ipv4 unicast
    =====================
    BGP table version is 6, local router ID is 10.0.0.11
    Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
                  i internal, r RIB-failure, S Stale, R Removed
    Origin codes: i - IGP, e - EGP, ? - incomplete
       Network          Next Hop            Metric LocPrf Weight Path
    *> 10.0.0.11/32     0.0.0.0                  0         32768 ?
    *> 10.0.0.12/32     swp51                         0 65020 65012 ?
    *=                  swp52                         0 65020 65012 ?
    *> 10.0.0.21/32     swp51           0             0 65020 ?
    *> 10.0.0.22/32     swp52           0             0 65020 ?
    *> 172.16.1.0/24    0.0.0.0                  0         32768 i
    *> 172.16.2.0/24    swp51                         0 65020 65012 i
    *=                  swp52                         0 65020 65012 i
    Total number of prefixes 6
     
    show bgp ipv6 unicast
    =====================
    No BGP network exists

FRRouting RIB commands are also modified:

    cumulus@switch:~$ net show route
    RIB entry for route
    ===================
    Codes: K - kernel route, C - connected, S - static, R - RIP,
           O - OSPF, I - IS-IS, B - BGP, P - PIM, T - Table,
           > - selected route, * - FIB route
    K>* 0.0.0.0/0 via 192.168.0.254, eth0
    C>* 10.0.0.11/32 is directly connected, lo
    B>* 10.0.0.12/32 [20/0] via fe80::4638:39ff:fe00:5c, swp51, 1d01h04m
      *                     via fe80::4638:39ff:fe00:2b, swp52, 1d01h04m
    B>* 10.0.0.21/32 [20/0] via fe80::4638:39ff:fe00:5c, swp51, 1d01h04m
    B>* 10.0.0.22/32 [20/0] via fe80::4638:39ff:fe00:2b, swp52, 1d01h04m
    C>* 172.16.1.0/24 is directly connected, br0
    B>* 172.16.2.0/24 [20/0] via fe80::4638:39ff:fe00:5c, swp51, 1d01h04m
      *                      via fe80::4638:39ff:fe00:2b, swp52, 1d01h04m
    C>* 192.168.0.0/24 is directly connected, eth0

The following commands show how the IPv4 link-local address
*169.254.0.1* is used to install the route and static neighbor entry to
facilitate proper forwarding without having to install an IPv4 prefix
with IPv6 next-hop in the kernel:

    cumulus@switch:~$ net show route 10.0.0.12
    RIB entry for 10.0.0.12
    =======================
    Routing entry for 10.0.0.12/32
      Known via "bgp", distance 20, metric 0, best
      Last update 1d01h06m ago
      * fe80::4638:39ff:fe00:5c, via swp51
      * fe80::4638:39ff:fe00:2b, via swp52
     
    FIB entry for 10.0.0.12
    =======================
    10.0.0.12  proto zebra  metric 20 
        nexthop via 169.254.0.1  dev swp51 weight 1 onlink
        nexthop via 169.254.0.1  dev swp52 weight 1 onlink

You can use this `iproute2` command to display more neighbor
information:

    cumulus@switch:~$ ip neighbor
    192.168.0.254 dev eth0 lladdr 44:38:39:00:00:5f REACHABLE
    169.254.0.1 dev swp52 lladdr 44:38:39:00:00:2b PERMANENT
    169.254.0.1 dev swp51 lladdr 44:38:39:00:00:5c PERMANENT
    fe80::4638:39ff:fe00:2b dev swp52 lladdr 44:38:39:00:00:2b router REACHABLE
    fe80::4638:39ff:fe00:5c dev swp51 lladdr 44:38:39:00:00:5c router REACHABLE

### How traceroute Interacts with BGP Unnumbered Interfaces

Every router or end host must have an IPv4 address to complete a
`traceroute` of IPv4 addresses. In this case, the IPv4 address used is
that of the loopback device.

Even if ENHE is not used in the data center, link addresses are not
typically advertised. This is because:

  - Link addresses take up valuable FIB resources. In a large Clos
    environment, the number of such addresses can be quite large.
  - Link addresses expose an additional attack vector for intruders to
    use to either break in or engage in DDOS attacks.

Assigning an IP address to the loopback device is essential.

### Advanced: Understanding How Next-hop Fields Are Set

This section describes how the IPv6 next-hops are set in the
MP\_REACH\_NLRI 
([multiprotocol reachable NLRI](https://www.ietf.org/rfc/rfc2858.txt)) 
initiated by the system, which applies whether IPv6 prefixes or IPv4 
prefixes are exchanged with ENHE. There are two main aspects to 
determine — how many IPv6 next-hops are included in the MP\_REACH\_NLRI 
(since the RFC allows either one or two next-hops) and the values of the 
next-hop(s). This section also describes how a received MP\_REACH\_NLRI is 
handled as far as processing IPv6 next-hops.

  - Whether peering to a global IPv6 address or link-local IPv6 address,
    the determination whether to send one or two next-hops is as
    follows:
    
    1.  If reflecting the route, two next-hops are sent only if the peer
        has `nexthop-local unchanged` configured and the attribute of
        the received route has an IPv6 link-local next-hop; otherwise,
        only one next-hop is sent.
    
    2.  Otherwise (if it is not reflecting the route), two next-hops are
        sent if explicitly configured (`nexthop-local unchanged`) or the
        peer is directly connected (that is, either peering is on
        link-local address or the global IPv4 or IPv6 address is
        *directly connected*) and the route is either a
        local/self-originated route or the peer is an eBGP peer.
    
    3.  In all other cases, only one next-hop gets sent, unless an
        outbound route map adds another next-hop.

  - `route-map` can impose two next-hops in scenarios where Cumulus
    Linux only sends one next-hop — by specifying `set ipv6 nexthop
    link-local`.
  - For all routes to eBGP peers and self-originated routes to iBGP
    peers, the global next-hop (first value) is the peering address of
    the local system. If the peering is on the link-local address, this
    is the global IPv6 address on the peering interface, if present;
    otherwise, it is the link-local IPv6 address on the peering
    interface.
  - For other routes to iBGP peers (eBGP to iBGP or reflected), the
    global next-hop will be the global next-hop in the received
    attribute.
    
    {{%notice note%}}
    
    If this address were a link-local IPv6 address, it would get reset
    so that the link-local IPv6 address of the eBGP peer is not passed
    along to an iBGP peer, which most likely may be on a different link.
    
    {{%/notice%}}

  - `route-map` and/or the peer configuration can change the above
    behavior. For example, `route-map` can set the global IPv6 next-hop
    or the peer configuration can set it to *self* — which is relevant
    for *iBGP* peers. The route map or peer configuration can also set
    the next-hop to unchanged, which ensures the source IPv6 global
    next-hop is passed around — which is relevant for *eBGP* peers.
  - Whenever two next-hops are being sent, the link-local next-hop (the
    second value of the two) is the link-local IPv6 address on the
    peering interface unless it is due to `nh-local-unchanged` or
    `route-map` has set the link-local next-hop.
  - Network administrators cannot set [martian values](http://en.wikipedia.org/wiki/Martian_packet) for IPv6
    next-hops in `route-map`. Also, global and link-local next-hops are
    validated to ensure they match the respective address types.
  - In a received update, a martian check is imposed for the IPv6 global
    next-hop. If the check fails, it gets treated as an implicit withdraw.
  - If two next-hops are received in an update and the second next-hop
    is not a link-local address, it gets ignored and the update is
    treated as if only one next-hop was received.
  - Whenever two next-hops are received in an update, the second
    next-hop is used to install the route into `zebra`. As per the
    previous point, it is already assured that this is a link-local IPv6
    address. Currently, this is assumed to be reachable and is not
    registered with NHT.
  - When `route-map` specifies the next-hop as `peer-address`, the
    global IPv6 next-hop as well as the link-local IPv6 next-hop (if
    it's being sent) is set to the *peering address*. If the peering is
    on a link-local address, the former could be the link-local address
    on the peering interface, unless there is a global IPv6 address
    present on this interface.
  - When using iBGP unnumbered with IPv6 Link Local Addresses (the
    default), FRR rewrites the BGP next hop to be the adjacent link.
    This is similar behavior to eBGP next hops. However, iBGP route
    advertisement rules do not change and a full mesh or route
    reflectors is still required.

The above rules imply that there are scenarios where a generated update
has two IPv6 next-hops, and both of them are the IPv6 link-local address
of the peering interface on the local system. If you are peering with a
switch or router that is not running Cumulus Linux and expects the first
next-hop to be a global IPv6 address, a route map can be used on the
sender to specify a global IPv6 address. This conforms with the
recommendations in the Internet draft
[draft-kato-bgp-ipv6-link-local-00.txt](https://tools.ietf.org/html/draft-kato-bgp-ipv6-link-local-00),
"BGP4+ Peering Using IPv6 Link-local Address".

### Limitations

  - Interface-based peering with separate IPv4 and IPv6 sessions is not
    supported.
  - ENHE is sent for IPv6 link-local peerings only.
  - If an IPv4 /30 or /31 IP address is assigned to the interface, IPv4
    peering is used over IPv6 link-local peering.
  - If the default router lifetime in the generated IPv6 route
    advertisements (RA) is set to *0*, the receiving FRRouting instance
    drops the RA if it is on a Cumulus Linux **2.5.z** switch. To work
    around this issue, either:
    
      - Explicitly configure the switch to advertise a router lifetime
        of 0, unless a value is specifically set by the operator — with
        the assumption that the host is running Cumulus Linux 3.y.z
        version of FRRouting. When hosts see an IPv6 RA with a router
        lifetime of 0, they do not make that router a default router.
      - Use the `sysctl` on the host —
        `net.ipv6.conf.all.accept_ra_defrtr`. However, this requires
        applying this setting on all hosts, which might mean many hosts,
        especially if FRRouting is run on the hosts.

## BGP add-path

### BGP add-path RX

*BGP add-path RX* allows BGP to receive multiple paths for the same
prefix. A path identifier is used so that additional paths do not
override previously advertised paths. No additional configuration is
required for BGP add-path RX.

{{%notice note%}}

BGP advertises the add-path RX capability by default. Add-Path TX
requires an administrator to enable it. Enabling TX resets the session.

{{%/notice%}}

To view the existing capabilities, run `net show bgp neighbor`. The
existing capabilities are listed in the subsection *Add Path*, below
*Neighbor capabilities:*

    cumulus@leaf01:~$ net show bgp neighbor 
    BGP neighbor on swp51: fe80::4638:39ff:fe00:5c, remote AS 65020, local AS 65011, external link
    Hostname: spine01
     Member of peer-group fabric for session parameters
      BGP version 4, remote router ID 10.0.0.21
      BGP state = Established, up for 1d01h15m
      Last read 00:00:00, Last write 1d01h15m
      Hold time is 3, keepalive interval is 1 seconds
      Configured hold time is 3, keepalive interval is 1 seconds
      Neighbor capabilities:
        4 Byte AS: advertised and received
        AddPath:
          IPv4 Unicast: RX advertised IPv4 Unicast and received
        Extended nexthop: advertised and received
          Address families by peer:
                       IPv4 Unicast
        Route refresh: advertised and received(old & new)
        Address family IPv4 Unicast: advertised and received
        Hostname Capability: advertised and received
        Graceful Restart Capabilty: advertised and received
          Remote Restart timer is 120 seconds
          Address families by peer:
            none
     
    ...

The example output above shows that additional BGP paths can be sent and
received (TX and RX are advertised). It also shows that the BGP
neighbor, fe80::4638:39ff:fe00:5c, supports both.

To view the current additional paths, run `net show bgp <network>`. The
example output shows an additional path that has been added by the TX
node for receiving. Each path has a unique AddPath ID.

    cumulus@leaf01:~$ net show bgp 10.0.0.12
    BGP routing table entry for 10.0.0.12/32
    Paths: (2 available, best #1, table Default-IP-Routing-Table)
      Advertised to non peer-group peers:
      spine01(swp51) spine02(swp52)
      65020 65012
        fe80::4638:39ff:fe00:5c from spine01(swp51) (10.0.0.21)
        (fe80::4638:39ff:fe00:5c) (used)
          Origin incomplete, localpref 100, valid, external, multipath, bestpath-from-AS 65020, best
          AddPath ID: RX 0, TX 6
          Last update: Wed Nov 16 22:47:00 2016
      65020 65012
        fe80::4638:39ff:fe00:2b from spine02(swp52) (10.0.0.22)
        (fe80::4638:39ff:fe00:2b) (used)
          Origin incomplete, localpref 100, valid, external, multipath
          AddPath ID: RX 0, TX 3
          Last update: Wed Nov 16 22:47:00 2016

### BGP add-path TX

AddPath TX allows BGP to advertise more than just the bestpath for a
prefix. Consider the following topology:

``` 
          r8
          |
          |
  r1 ----    ---- r6
  r2 ---- r7 ---- r5
          ||
          ||
        r3 r4
```

In this topology:

  - r1 and r2 are in AS 100

  - r3 and r4 are in AS 300

  - r5 and r6 are in AS 500

  - r7 is in AS 700

  - r8 is in AS 800

  - r7 learns 1.1.1.1/32 from r1, r2, r3, r4, r5, and r6. Among these r7
    picks the path from r1 as the bestpath for 1.1.1.1/32

The example below configures the r7 session to advertise the bestpath
learned from each AS. In this case, this means a path from AS 100, a
path from AS 300, and a path from AS 500. The `net show bgp 1.1.1.1/32`
from r7 has "bestpath-from-AS 100" so the user can see what the bestpath
is from each AS:

    cumulus@r7:~$ net add bgp autonomous-system 700
    cumulus@r7:~$ net add bgp neighbor 192.0.2.2 addpath-tx-bestpath-per-AS

The output below shows the result on r8:

    cumulus@r8:~$ net show bgp 1.1.1.1/32
    BGP routing table entry for 1.1.1.1/32
    Paths: (3 available, best #3, table Default-IP-Routing-Table)
      Advertised to non peer-group peers:
      r7(10.7.8.1)
      700 100
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external
          Community: 1:1
          AddPath ID: RX 2, TX 4
          Last update: Thu Jun  2 00:57:14 2016
     
      700 300
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external
          Community: 3:3
          AddPath ID: RX 4, TX 3
          Last update: Thu Jun  2 00:57:14 2016
     
      700 500
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external, bestpath-from-AS 700, best
          Community: 5:5
          AddPath ID: RX 6, TX 2
          Last update: Thu Jun  2 00:57:14 2016

The example below shows the results if r7 is configured to advertise all
paths to r8:

    cumulus@r7:~$ net add bgp autonomous-system 700
    cumulus@r7:~$ net add bgp neighbor 192.0.2.2 addpath-tx-all-paths

The output below shows the result on r8:

    cumulus@r8:~$ net show bgp 1.1.1.1/32
    BGP routing table entry for 1.1.1.1/32
    Paths: (3 available, best #3, table Default-IP-Routing-Table)
      Advertised to non peer-group peers:
      r7(10.7.8.1)
      700 100
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external
          Community: 1:1
          AddPath ID: RX 2, TX 4
          Last update: Thu Jun  2 00:57:14 2016
     
      700 300
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external
          Community: 3:3
          AddPath ID: RX 4, TX 3
          Last update: Thu Jun  2 00:57:14 2016
     
      700 500
        10.7.8.1 from r7(10.7.8.1) (10.0.0.7)
          Origin IGP, localpref 100, valid, external, bestpath-from-AS 700, best
          Community: 5:5
          AddPath ID: RX 6, TX 2
          Last update: Thu Jun  2 00:57:14 2016

## Fast Convergence Design Considerations

Without getting into the why (see the IETF draft cited in Useful Links
below that talks about BGP use within the data center), we strongly
recommend the following use of addresses in the design of a BGP-based
data center network:

  - Use of interface addresses: Set up BGP sessions only using
    interface-scoped addresses. This allows BGP to react quickly to link
    failures.
  - Use of next-hop-self: Every BGP node says that it knows how to
    forward traffic to the prefixes it is announcing. This reduces the
    requirement to announce interface-specific addresses and thereby
    reduces the size of the forwarding table.

### Specifying the Interface Name in the neighbor Command

When you are configuring BGP for the neighbors of a given interface, you
can specify the interface name instead of its IP address. All the other
`neighbor` command options remain the same.

This is equivalent to BGP peering to the link-local IPv6 address of the
neighbor on the given interface. The link-local address is learned via
IPv6 neighbor discovery router advertisements.

Consider the following example configuration in the `/etc/frr/frr.conf`
file:

    router bgp 65000
      bgp router-id 10.0.0.1
      neighbor swp1 interface
      neighbor swp1 remote-as internal
      neighbor swp1 next-hop-self
    !
      address-family ipv6
      neighbor swp1 activate
      exit-address-family

You create the above configuration with the following NCLU commands:

    cumulus@switch:~$ net add bgp autonomous-system 65000
    cumulus@switch:~$ net add bgp router-id 10.0.0.1
    cumulus@switch:~$ net add bgp neighbor swp1 interface
    cumulus@switch:~$ net add bgp neighbor swp1 remote-as internal
    cumulus@switch:~$ net add bgp neighbor swp1 next-hop-self
    cumulus@switch:~$ net add bgp ipv6 unicast neighbor swp1 activate

{{%notice note%}}

By default, Cumulus Linux sends IPv6 neighbor discovery router
advertisements. Cumulus Networks recommends you adjust the interval of
the router advertisement to a shorter value 
(`net add interface <interface> ipv6 nd ra-interval <interval>`) to address 
scenarios when nodes come up and miss router advertisement processing to  
relay the neighbor's link-local address to BGP. The `interval` is measured in
seconds and defaults to 10 seconds.

{{%/notice%}}

## Using Peer Groups to Simplify Configuration

When there are many peers to connect to, the amount of redundant
configuration becomes overwhelming. For example, repeating the
`activate` and `next-hop-self` commands for even 60 neighbors makes for
a very long configuration file. Using `peer-group` addresses this
problem.

Instead of specifying properties of each individual peer, FRRouting
allows for defining one or more peer groups and associating all the
attributes common to that peer session to a peer group. A peer needs to
be attached to a peer group only once, when it then inherits all address
families activated for that peer group.

After doing this, the only task is to associate an IP address with a
peer group. Here is an example of defining and using peer groups:

    cumulus@switch:~$ net add bgp neighbor tier-2 peer-group
    cumulus@switch:~$ net add bgp ipv4 unicast
    cumulus@switch:~$ net add bgp neighbor tier-2 activate
    cumulus@switch:~$ net add bgp neighbor tier-2 next-hop-self
    cumulus@switch:~$ net add bgp neighbor 10.0.0.2 peer-group tier-2
    cumulus@switch:~$ net add bgp neighbor 192.0.2.2 peer-group tier-2

{{%notice note%}}

BGP peer-group restrictions have been replaced with update-groups, which
dynamically examine all peers and group them if they have the same
outbound policy.

{{%/notice%}}

## Configuring BGP Dynamic Neighbors

The *BGP dynamic neighbor* feature provides BGP peering to a group of
remote neighbors within a specified range of IPv4 or IPv6 addresses for
a BGP peer group. You can configure each range as a subnet IP address.

You configure dynamic neighbors using the `bgp listen range <IP address>
peer-group <GROUP>` command. After you configure the dynamic neighbors,
a BGP speaker can listen for and form peer relationships with any
neighbor in the IP address range and mapped to a peer group.

    cumulus@switch:~$ net add bgp autonomous-system 65001
    cumulus@switch:~$ net add bgp listen range 10.1.1.0/24 peer-group SPINE

You can limit the number of dynamic peers by specifying that limit in
the `bgp listen limit` command (the default value is *100*):

    cumulus@switch:~$ net add bgp listen limit 5

Collectively, a sample configuration for IPv4 looks like this:

    cumulus@switch:~$ net add bgp autonomous-system 65001
    cumulus@switch:~$ net add bgp neighbor SPINE peer-group
    cumulus@switch:~$ net add bgp neighbor SPINE remote-as 65000
    cumulus@switch:~$ net add bgp listen limit 5
    cumulus@switch:~$ net add bgp listen range 10.1.1.0/24 peer-group SPINE

These commands produce an IPv4 configuration that looks like this:

    router bgp 65001 
     neighbor SPINE peer-group
     neighbor SPINE remote-as 65000
      bgp listen limit 5
      bgp listen range 10.1.1.0/24 peer-group SPINE

## Configuring BGP Peering Relationships across Switches

A BGP peering relationship is typically initiated with the `neighbor
x.x.x.x remote-as [internal|external]` command.

Specifying *internal* signifies an iBGP peering; that is, the neighbor
only creates or accepts a connection with the specified neighbor if the
remote peer AS number matches this BGP AS number.

Specifying *external* signifies an eBGP peering; that is, the neighbor
will only create a connection with the neighbor if the remote peer AS
number does **not** match this BGP AS number.

You can make this distinction using the `neighbor` command or the
`peer-group` command.

In general, use the following syntax with the `neighbor` command:

    cumulus@switch:~$ net add bgp neighbor [<IP address>|<BGP peer>|<interface>] remote-as [<value>|internal|external]

Some example configurations follow.

{{%notice info has%}}

To connect to **the same AS** using the `neighbor` command, modify your
configuration similar to the following:

    cumulus@switch:~$ net add bgp autonomous-system 500
    cumulus@switch:~$ net add bgp neighbor 192.168.1.2 remote-as internal

These commands create the following configuration snippet:

    router bgp 500
    neighbor 192.168.1.2 remote-as internal

{{%/notice%}}

{{%notice info has%}}

To connect to a **different AS** using the `neighbor` command, modify
your configuration similar to the following:

    cumulus@switch:~$ net add bgp autonomous-system 500
    cumulus@switch:~$ net add bgp neighbor 192.168.1.2 remote-as external

These commands create the following configuration snippet:

    router bgp 500
    neighbor 192.168.1.2 remote-as external

{{%/notice%}}

{{%notice info has%}}

To connect to **the same AS** using the `peer-group` command, modify
your configuration similar to the following:

    cumulus@switch:~$ net add bgp autonomous-system 500
    cumulus@switch:~$ net add bgp neighbor swp1 interface
    cumulus@switch:~$ net add bgp neighbor IBGP peer-group
    cumulus@switch:~$ net add bgp neighbor IBGP remote-as internal
    cumulus@switch:~$ net add bgp neighbor swp1 interface peer-group IBGP
    cumulus@switch:~$ net add bgp neighbor 192.0.2.3 peer-group IBGP
    cumulus@switch:~$ net add bgp neighbor 192.0.2.4 peer-group IBGP

These commands create the following configuration snippet:

    router bgp 500
    neighbor swp1 interface
    neighbor IBGP peer-group
    neighbor IBGP remote-as internal
    neighbor swp1 peer-group IBGP
    neighbor 192.0.2.3 peer-group IBGP
    neighbor 192.0.2.4 peer-group IBGP

{{%/notice%}}

{{%notice info has%}}

To connect to a **different AS** using the `peer-group` command, modify
your configuration similar to the following:

    cumulus@switch:~$ net add bgp autonomous-system 500
    cumulus@switch:~$ net add bgp neighbor swp2 interface
    cumulus@switch:~$ net add bgp neighbor EBGP peer-group
    cumulus@switch:~$ net add bgp neighbor EBGP remote-as external
    cumulus@switch:~$ net add bgp neighbor 192.0.2.2 peer-group EBGP
    cumulus@switch:~$ net add bgp neighbor swp2 interface peer-group EBGP
    cumulus@switch:~$ net add bgp neighbor 192.0.2.4 peer-group EBGP

These commands create the following configuration snippet:

    router bgp 500
    neighbor swp2 interface
    neighbor EBGP peer-group
    neighbor EBGP remote-as external
    neighbor 192.0.2.2 peer-group EBGP
    neighbor swp2 peer-group EBGP
    neighbor 192.0.2.4 peer-group EBGP

{{%/notice%}}

## Configuring MD5-enabled BGP Neighbors

The following sections outline how to configure an MD5-enabled BGP
neighbor. Each process assumes that FRRouting is used as the routing
platform, and consists of two switches (`AS 65011` and `AS 65020`),
connected by the link 10.0.0.100/30, with the following configurations:

    cumulus@leaf01:~$ net show bgp summary 
    show bgp ipv4 unicast summary
    =============================
    BGP router identifier 10.0.0.11, local AS number 65011 vrf-id 0
    BGP table version 6
    RIB entries 11, using 1320 bytes of memory
    Peers 2, using 36 KiB of memory
    Peer groups 1, using 56 bytes of memory
    Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    spine01(swp51)  4 65020   93587   93587        0    0    0 1d02h00m        3
    spine02(swp52)  4 65020   93587   93587        0    0    0 1d02h00m        3
    Total number of neighbors 2
     
    show bgp ipv6 unicast summary
    =============================
    No IPv6 neighbor is configured

    cumulus@spine01:~$ net show bgp summary 
    show bgp ipv4 unicast summary
    =============================
    BGP router identifier 10.0.0.21, local AS number 65020 vrf-id 0
    BGP table version 5
    RIB entries 9, using 1080 bytes of memory
    Peers 4, using 73 KiB of memory
    Peer groups 1, using 56 bytes of memory
    Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    leaf01(swp1)    4 65011     782     782        0    0    0 00:12:54        2
    leaf02(swp2)    4 65012     781     781        0    0    0 00:12:53        2
    swp3            4     0       0       0        0    0    0 never    Idle       
    swp4            4     0       0       0        0    0    0 never    Idle       
    Total number of neighbors 4
     
    show bgp ipv6 unicast summary
    =============================
    No IPv6 neighbor is configured
     

### Manually Configuring an MD5-enabled BGP Neighbor

1.  SSH into leaf01.

2.  Configure the password for the neighbor:
    
        cumulus@leaf01:~$ net add bgp neighbor 10.0.0.102 password mypassword

3.  Confirm the configuration has been implemented with the `net show
    bgp summary` command:
    
        cumulus@leaf01:~$ net show bgp summary 
        show bgp ipv4 unicast summary
        =============================
        BGP router identifier 10.0.0.11, local AS number 65011 vrf-id 0
        BGP table version 18
        RIB entries 11, using 1320 bytes of memory
        Peers 2, using 36 KiB of memory
        Peer groups 1, using 56 bytes of memory
        Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
        spine01(swp51)  4 65020   96144   96146        0    0    0 00:30:29        3
        spine02(swp52)  4 65020   96209   96217        0    0    0 1d02h44m        3
        Total number of neighbors 2
         
        show bgp ipv6 unicast summary
        =============================
        No IPv6 neighbor is configured

4.  SSH into spine01.

5.  Configure the password for the neighbor:
    
        cumulus@spine01:~$ net add bgp neighbor 10.0.0.101 password mypassword

6.  Confirm the configuration has been implemented with the `net show
    bgp summary` command:
    
        cumulus@spine01:~$ net show bgp summary 
        show bgp ipv4 unicast summary
        =============================
        BGP router identifier 10.0.0.21, local AS number 65020 vrf-id 0
        BGP table version 5
        RIB entries 9, using 1080 bytes of memory
        Peers 4, using 73 KiB of memory
        Peer groups 1, using 56 bytes of memory
        Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
        leaf01(swp1)    4 65011     782     782        0    0    0 00:12:54        2
        leaf02(swp2)    4 65012     781     781        0    0    0 00:12:53        2
        swp3            4     0       0       0        0    0    0 never    Idle       
        swp4            4     0       0       0        0    0    0 never    Idle       
        Total number of neighbors 4
         
        show bgp ipv6 unicast summary
        =============================
        No IPv6 neighbor is configured

{{%notice note%}}

The MD5 password configured against a BGP listen-range peer-group (used
to accept and create dynamic BGP neighbors) is not enforced. This means
that connections are accepted from peers that do not specify a password.

{{%/notice%}}

## Configuring eBGP Multihop

The eBGP Multihop option lets you use BGP to exchange routes with an
external peer that is more than one hop away.

1.  To establish a connection between two eBGP peers that are not
    directly connected:
    
        cumulus@leaf02:mgmt-vrf:~$ net add bgp neighbor <ip> remote-as external
        cumulus@leaf02:mgmt-vrf:~$ net add bgp neighbor <ip> ebgp-multihop

2.  Confirm the configuration with the `net show bgp neighbor <ip>`
    command:
    
        cumulus@leaf02:mgmt-vrf:~$ net show bgp neighbor 10.0.0.11
        BGP neighbor is 10.0.0.11, remote AS 65011, local AS 65012, external link
        Hostname: leaf01
          BGP version 4, remote router ID 10.0.0.11
          BGP state = Established, up for 00:02:54
          Last read 00:00:00, Last write 00:00:00
          Hold time is 9, keepalive interval is 3 seconds
          Neighbor capabilities:
            4 Byte AS: advertised and received
            AddPath:
              IPv4 Unicast: RX advertised IPv4 Unicast and received
            Route refresh: advertised and received(old & new)
            Address Family IPv4 Unicast: advertised and received
            Hostname Capability: advertised (name: leaf02,domain name: n/a) received (name: leaf01,domain name: n/a)
            Graceful Restart Capability: advertised and received
              Remote Restart timer is 120 seconds
              Address families by peer:
                none
          Graceful restart informations:
            End-of-RIB send: IPv4 Unicast
            End-of-RIB received: IPv4 Unicast
          Message statistics:
            Inq depth is 0
            Outq depth is 0
                                 Sent       Rcvd
            Opens:                  1          1
            Notifications:          0          0
            Updates:             2868       2872
            Keepalives:            60         60
            Route Refresh:          0          0
            Capability:             0          0
            Total:               2929       2933
          Minimum time between advertisement runs is 0 seconds
         For address family: IPv4 Unicast
          Update group 2, subgroup 4
          Packet Queue length 0
          Community attribute sent to this neighbor(all)
          9 accepted prefixes
          Connections established 1; dropped 0
          Last reset never    
        External BGP neighbor may be up to 255 hops away.    
        Local host: 10.0.0.12, Local port: 40135
        Foreign host: 10.0.0.11, Foreign port: 179
        Nexthop: 10.0.0.12
        Nexthop global: ::
        Nexthop local: ::
        BGP connection: non shared network
        BGP Connect Retry Timer in Seconds: 10
        Estimated round trip time: 1 ms
        Read thread: on  Write thread: on

## Configuring BGP TTL Security

The steps below cover how to configure BGP ttl security on Cumulus
Linux, using a leaf (`leaf01`), and spine (`spine01`) for the example
output:

1.  SSH into leaf01 and configure it for TTL security:
    
        cumulus@leaf01:~$ net add bgp autonomous-system 65000
        cumulus@leaf01:~$ net add bgp neighbor [spine01-IP] ttl-security hops [value]

2.  SSH into spine01 and configure it for TTL security:
    
        cumulus@spine01:~$ net add bgp autonomous-system 65001
        cumulus@spine01:~$ net add bgp neighbor [leaf01-IP] ttl-security hops [value]

3.  Confirm the configuration with the `show ip bgp neighbor` command:
    
        cumulus@spine01:mgmt-vrf:~$ net show bgp neighbor swp1
        BGP neighbor on swp1: fe80::4638:39ff:fe00:5b, remote AS 65011, local AS 65020, external link
        Hostname: leaf01
          BGP version 4, remote router ID 10.0.0.11
          BGP state = Established, up for 00:10:45
          Last read 00:00:03, Last write 00:00:03
          Hold time is 9, keepalive interval is 3 seconds
          Neighbor capabilities:
            4 Byte AS: advertised and received
            AddPath:
              IPv4 Unicast: RX advertised IPv4 Unicast and received
            Extended nexthop: advertised and received
              Address families by peer:
                           IPv4 Unicast
            Route refresh: advertised and received(old & new)
            Address Family IPv4 Unicast: advertised and received
            Hostname Capability: advertised (name: spine01,domain name: n/a) received (name: leaf01,domain name: n/a)
            Graceful Restart Capabilty: advertised and received
              Remote Restart timer is 120 seconds
              Address families by peer:
                none
          Graceful restart informations:
            End-of-RIB send: IPv4 Unicast
            End-of-RIB received: IPv4 Unicast
          Message statistics:
            Inq depth is 0
            Outq depth is 0
                                 Sent       Rcvd
            Opens:                 46          2
            Notifications:         41          0
            Updates:               38         34
            Keepalives:         49334      49331
            Route Refresh:          0          0
            Capability:             0          0
            Total:              49459      49367
          Minimum time between advertisement runs is 0 seconds
        
         For address family: IPv4 Unicast
          Update group 1, subgroup 1
          Packet Queue length 0
          Community attribute sent to this neighbor(all)
          3 accepted prefixes
        
          Connections established 2; dropped 1
          Last reset 00:17:37, due to NOTIFICATION sent (Hold Timer Expired)    
        External BGP neighbor may be up to 1 hops away.    
        Local host: fe80::4638:39ff:fe00:5c, Local port: 35564
        Foreign host: fe80::4638:39ff:fe00:5b, Foreign port: 179
        Nexthop: 10.0.0.21
        Nexthop global: fe80::4638:39ff:fe00:5c
        Nexthop local: fe80::4638:39ff:fe00:5c
        BGP connection: shared network
        BGP Connect Retry Timer in Seconds: 10
        Read thread: on  Write thread: on

## Configuration Tips

### BGP Advertisement Best Practices

Limiting the exchange of routing information at various parts in the
network is a best practice you should follow. The following image
illustrates one way you can do so in a typical Clos architecture:

{{% imgOld 1 %}}

### Utilizing Multiple Routing Tables and Forwarding

You can run multiple routing tables (one for in-band/data plane traffic
and one for out-of-band/management plane traffic) on the same switch
using [management VRF](/version/cumulus-linux-36/Layer-3/Management-VRF)
(multiple routing tables and forwarding).

{{%notice note%}}

In Cumulus Linux 3.0 and later, BGP and static routing (IPv4 and IPv6)
are supported within a VRF context. For more information, refer to
[Virtual Routing and Forwarding - VRF](/version/cumulus-linux-36/Layer-3/Virtual-Routing-and-Forwarding-VRF).

{{%/notice%}}

### Using BGP Community Lists

You can use [*community lists*](http://www.nongnu.org/quagga/docs/docs-multi/BGP-Community-Lists.html#BGP-Community-Lists)
to define a BGP community to tag one or more routes. You can then use
the communities to apply route policy on either egress or ingress.

The BGP community list can be either *standard* or *expanded.* The
standard BGP community list is a pair of values (such as *100:100*) that
can be tagged on a specific prefix and advertised to other neighbors or
applied on route ingress. Alternately, it can be one of four BGP default
communities:

  - *internet*: a BGP community that matches all routes
  - *local-AS*: a BGP community that restrict routes to your
    confederation's sub-AS
  - *no-advertise*: a BGP community that isn't advertised to anyone
  - *no-export*: a BGP community that isn't advertised to the eBGP peer

An expanded BGP community list takes a regular expression of communities
matches the listed communities.

When the neighbor receives the prefix, it examines the community value
and takes action accordingly, such as permitting or denying the
community member in the routing policy.

Here's an example of standard community list filter:

    cumulus@switch:~$ net add routing community-list standard COMMUNITY1 permit 100:100

You can apply the community list to a route map to define the routing
policy:

    cumulus@switch:~$ net add bgp table-map ROUTE-MAP1

### Additional Default Settings

Other default settings not discussed in detail in this chapter include
the following; they're all enabled by default:

  - `bgp deterministic-med`, which ensures path ordering no longer
    impacts bestpath selection.
  - `bgp show-hostname`, which displays the hostname in show command
    output.
  - `bgp network import-check`, which enables the advertising of the BGP
    network in IGP.

### Configuring BGP Neighbor maximum-prefixes

The maximum number of route announcements, or prefixes, allowed by a BGP
neighbor can be configured using the `maximum-prefixes` command in the
CLI. Replace the `PEER` input with the relevant peer, and replace
`NUMBER` with the maximum number of prefixes desired:

    frr(config)# neighbor PEER maximum-prefix NUMBER

## Troubleshooting BGP

The most common starting point for troubleshooting BGP is to view the
summary of neighbors connected to and some information about these
connections. A sample output of this command is as follows:

    cumulus@switch:~$ net show bgp summary 
    show bgp ipv4 unicast summary
    =============================
    BGP router identifier 10.0.0.11, local AS number 65011 vrf-id 0
    BGP table version 8
    RIB entries 11, using 1320 bytes of memory
    Peers 2, using 36 KiB of memory
    Peer groups 1, using 56 bytes of memory
    Neighbor        V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    spine01(swp51)  4 65020     549     551        0    0    0 00:09:03        3
    spine02(swp52)  4 65020     548     550        0    0    0 00:09:02        3
    Total number of neighbors 2
     
    show bgp ipv6 unicast summary
    =============================
    No IPv6 neighbor is configured

{{%notice tip%}}

You can determine whether the sessions above are iBGP or eBGP sessions
by looking at the ASNs.

{{%/notice%}}

It is also useful to view the routing table as defined by BGP:

    cumulus@switch:~$ net show bgp ipv4
    ERROR: Command not found
    Use 'net help KEYWORD(s)' to list all options that use KEYWORD(s)
    cumulus@leaf01:~$ net show bgp ipv4 
        unicast  :  add help text
    cumulus@leaf01:~$ net show bgp ipv4 unicast 
    BGP table version is 8, local router ID is 10.0.0.11
    Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
                  i internal, r RIB-failure, S Stale, R Removed
    Origin codes: i - IGP, e - EGP, ? - incomplete
       Network          Next Hop            Metric LocPrf Weight Path
    *> 10.0.0.11/32     0.0.0.0                  0         32768 ?
    *= 10.0.0.12/32     swp52                         0 65020 65012 ?
    *>                  swp51                         0 65020 65012 ?
    *> 10.0.0.21/32     swp51           0             0 65020 ?
    *> 10.0.0.22/32     swp52           0             0 65020 ?
    *> 172.16.1.0/24    0.0.0.0                  0         32768 i
    *= 172.16.2.0/24    swp52                         0 65020 65012 i
    *>                  swp51                         0 65020 65012 i
    Total number of prefixes 6

A more detailed breakdown of a specific neighbor can be obtained using
`net show bgp neighbor <neighbor>`:

    cumulus@switch:~$ net show bgp neighbor swp51
    BGP neighbor on swp51: fe80::4638:39ff:fe00:5c, remote AS 65020, local AS 65011, external link
    Hostname: spine01
     Member of peer-group fabric for session parameters
      BGP version 4, remote router ID 10.0.0.21
      BGP state = Established, up for 00:11:30
      Last read 00:00:00, Last write 00:11:26
      Hold time is 3, keepalive interval is 1 seconds
      Configured hold time is 3, keepalive interval is 1 seconds
      Neighbor capabilities:
        4 Byte AS: advertised and received
        AddPath:
          IPv4 Unicast: RX advertised IPv4 Unicast and received
        Extended nexthop: advertised and received
          Address families by peer:
                       IPv4 Unicast
        Route refresh: advertised and received(old & new)
        Address family IPv4 Unicast: advertised and received
        Hostname Capability: advertised and received
        Graceful Restart Capabilty: advertised and received
          Remote Restart timer is 120 seconds
          Address families by peer:
            none
      Graceful restart informations:
        End-of-RIB send: IPv4 Unicast
        End-of-RIB received: IPv4 Unicast
      Message statistics:
        Inq depth is 0
        Outq depth is 0
                             Sent       Rcvd
        Opens:                  1          1
        Notifications:          0          0
        Updates:                7          6
        Keepalives:           690        689
        Route Refresh:          0          0
        Capability:             0          0
        Total:                698        696
      Minimum time between advertisement runs is 0 seconds
     For address family: IPv4 Unicast
      fabric peer-group member
      Update group 1, subgroup 1
      Packet Queue length 0
      Community attribute sent to this neighbor(both)
      Inbound path policy configured
      Outbound path policy configured
      Incoming update prefix filter list is *dc-leaf-in
      Outgoing update prefix filter list is *dc-leaf-out
      3 accepted prefixes
      Connections established 1; dropped 0
      Last reset never
    Local host: fe80::4638:39ff:fe00:5b, Local port: 48424
    Foreign host: fe80::4638:39ff:fe00:5c, Foreign port: 179
    Nexthop: 10.0.0.11
    Nexthop global: fe80::4638:39ff:fe00:5b
    Nexthop local: fe80::4638:39ff:fe00:5b
    BGP connection: shared network
    BGP Connect Retry Timer in Seconds: 3
    Estimated round trip time: 3 ms
    Read thread: on  Write thread: off

To see the details of a specific route such as from whom it was
received, to whom it was sent, and so forth, use the 
`net show bgp <ip address/prefix>` command:

    cumulus@leaf01:~$ net show bgp 10.0.0.11/32
    BGP routing table entry for 10.0.0.11/32
    Paths: (1 available, best #1, table Default-IP-Routing-Table)
      Advertised to non peer-group peers:
      spine01(swp51) spine02(swp52)
      Local
        0.0.0.0 from 0.0.0.0 (10.0.0.11)
          Origin incomplete, metric 0, localpref 100, weight 32768, valid, sourced, bestpath-from-AS Local, best
          AddPath ID: RX 0, TX 9
          Last update: Fri Nov 18 01:48:17 2016

This shows that the routing table prefix seen by BGP is 10.0.0.11/32,
that this route was advertised to two neighbors, and that it was not
heard by any neighbors.

### Debugging Tip: Logging Neighbor State Changes

It is very useful to log the changes that a neighbor goes through to
troubleshoot any issues associated with that neighbor. This is done
using the `log-neighbor-changes` command, which is enabled by default.

The output is sent to the specified log file, usually
`/var/log/frr/bgpd.log`, and looks like this:

    2016/07/08 10:12:06.572827 BGP: %NOTIFICATION: sent to neighbor 10.0.0.2 6/3 (Cease/Peer Unconfigured) 0 bytes
    2016/07/08 10:12:06.572954 BGP: Notification sent to neighbor 10.0.0.2: type 6/3
    2016/07/08 10:12:16.682071 BGP: %ADJCHANGE: neighbor 192.0.2.2 Up
    2016/07/08 10:12:16.682660 BGP: %ADJCHANGE: neighbor 10.0.0.2 Up

### Troubleshooting Link-local Addresses

To verify that `frr` learned the neighboring link-local IPv6 address via
the IPv6 neighbor discovery router advertisements on a given interface,
use the `show interface <if-name>` command. If `ipv6 nd suppress-ra`
isn't enabled on both ends of the interface, then `Neighbor address(s):`
should have the other end's link-local address. That is the address that
BGP would use when BGP is enabled on that interface.

{{%notice note%}}

IPv6 route advertisements (RAs) are automatically enabled on an
interface with IPv6 addresses, so the step `no ipv6 nd suppress-ra` is
no longer needed for BGP unnumbered.

{{%/notice%}}

Use `vtysh` to verify the configuration:

    cumulus@switch:~$ sudo vtysh
     
    Hello, this is Quagga (version 0.99.23.1+cl3u2).
    Copyright 1996-2005 Kunihiro Ishiguro, et al.
     
    R7# show interface swp1
    Interface swp1 is up, line protocol is up
      Link ups:       0    last: (never)
      Link downs:     0    last: (never)
      PTM status: disabled
      vrf: Default-IP-Routing-Table
      index 4 metric 0 mtu 1500 
      flags: <UP,BROADCAST,RUNNING,MULTICAST>
      HWaddr: 44:38:39:00:00:5c
      inet6 fe80::4638:39ff:fe00:5c/64
      ND advertised reachable time is 0 milliseconds
      ND advertised retransmit interval is 0 milliseconds
      ND router advertisements are sent every 10 seconds
      ND router advertisements lifetime tracks ra-interval
      ND router advertisement default router preference is medium
      Hosts use stateless autoconfig for addresses.
      Neighbor address(s):
      inet6 fe80::4638:39ff:fe00:5b/128

Instead of the IPv6 address, the peering interface name is displayed in
the `show ip bgp summary` command and wherever else applicable:

    cumulus@switch:~$ net show bgp summary
    BGP router identifier 10.0.0.21, local AS number 65020 vrf-id 0
    BGP table version 15
    RIB entries 17, using 2040 bytes of memory
    Peers 6, using 97 KiB of memory
    Peer groups 1, using 56 bytes of memory
     
    Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
    leaf01(swp1)    4 65011    2834    2843        0    0    0 02:21:35        2
    leaf02(swp2)    4 65012    2834    2844        0    0    0 02:21:36        2
    leaf03(swp3)    4 65013    2834    2843        0    0    0 02:21:35        2
    leaf04(swp4)    4 65014    2834    2844        0    0    0 02:21:36        2
    edge01(swp29)   4 65051    8509    8505        0    0    0 02:21:37        3
    edge01(swp30)   4 65051    8506    8503        0    0    0 02:21:35        3
     
    Total number of neighbors 6

Most of the show commands can take the interface name instead of the IP
address, if that level of specificity is needed:

    cumulus@leaf01:~$ net show bgp neighbor 
        fabric  :  BGP neighbor or peer-group
        swp51   :  BGP neighbor or peer-group
        swp52   :  BGP neighbor or peer-group
        <ENTER>

    cumulus@leaf01:~$ net show bgp neighbor swp51
    BGP neighbor on swp51: fe80::4638:39ff:fe00:5c, remote AS 65020, local AS 65011, external link
    Hostname: spine01
     Member of peer-group fabric for session parameters
      BGP version 4, remote router ID 0.0.0.0
      BGP state = Connect
      Last read 20:16:21, Last write 20:55:51
      Hold time is 30, keepalive interval is 10 seconds
      Configured hold time is 30, keepalive interval is 10 seconds
      Message statistics:
        Inq depth is 0
        Outq depth is 0
                             Sent       Rcvd
        Opens:                  1          1
        Notifications:          1          0
        Updates:                7          6
        Keepalives:          2374       2373
        Route Refresh:          0          0
        Capability:             0          0
        Total:               2383       2380
      Minimum time between advertisement runs is 5 seconds
     For address family: IPv4 Unicast
      fabric peer-group member
      Not part of any update group
      Community attribute sent to this neighbor(both)
      Inbound path policy configured
      Outbound path policy configured
      Incoming update prefix filter list is *dc-leaf-in
      Outgoing update prefix filter list is *dc-leaf-out
      0 accepted prefixes
      Connections established 1; dropped 1
      Last reset 20:16:20, due to NOTIFICATION sent (Cease/Other Configuration Change)
    BGP Connect Retry Timer in Seconds: 3
    Next connect timer due in 1 seconds
    Read thread: on  Write thread: on

## Enabling Read-only Mode

You can enable read-only mode for when the BGP process restarts or when
the BGP process is cleared using `clear ip bgp *`. When enabled,
read-only mode begins as soon as the first peer reaches its
*established* state and a timer for `<max-delay>` seconds is started.

While in read-only mode, BGP doesn't run best-path or generate any
updates to its peers. This mode continues until:

  - All the configured peers, except the shutdown peers, have sent an
    explicit EOR (End-Of-RIB) or an implicit EOR. The first keep-alive
    after BGP has reached the established state is considered an
    implicit EOR. If the `<establish-wait>` option is specified, then
    BGP will wait for peers to reach the established state from the
    start of the `update-delay` until the `<establish-wait>` period is
    over; that is, the minimum set of established peers for which EOR is
    expected would be peers established during the `establish-wait`
    window, not necessarily all the configured neighbors.
  - The `max-delay` period is over.

Upon reaching either of these two conditions, BGP resumes the decision
process and generates updates to its peers.

To enable read-only mode:

    cumulus@switch:$ net add bgp update-delay <max-delay in 0-3600 seconds> [<establish-wait in 1-3600 seconds>]

The default `<max-delay>` is 0 — the feature is off by default.

Use output from `show ip bgp summary` for information about the state of
the update delay.

This feature can be useful in reducing CPU/network usage as BGP
restarts/clears. It's particularly useful in topologies where BGP learns
a prefix from many peers. Intermediate best paths are possible for the
same prefix as peers get established and start receiving updates at
different times. This feature is also valuable if the network has a high
number of such prefixes.

## Applying a Route Map for Route Updates

There are two ways you can apply 
[route maps](http://www.nongnu.org/quagga/docs/docs-multi/Route-Map.html#Route-Map)
for BGP:

  - By filtering routes from BGP into Zebra
  - By filtering routes from Zebra into the Linux kernel

### Filtering Routes from BGP into Zebra

For the first way, you can apply a route map on route updates from BGP
to Zebra. All the applicable match operations are allowed, such as match
on prefix, next-hop, communities, and so forth. Set operations for this
attach-point are limited to metric and next-hop only. Any operation of
this feature does not affect BGPs internal RIB.

Both IPv4 and IPv6 address families are supported. Route maps work on
multi-paths as well. However, the metric setting is based on the best
path only.

To apply a route map to filter route updates from BGP into Zebra:

    cumulus@switch:$ net add bgp table-map <route-map-name>

### Filtering Routes from Zebra into the Linux Kernel

To apply a route map to filter route updates from Zebra into the Linux
kernel:

    cumulus@switch:$ net add routing protocol bgp route-map <route-map-name>

## Protocol Tuning

### Converging Quickly On Link Failures

In the Clos topology, we recommend that you only use interface addresses
to set up peering sessions. This means that when the link fails, the BGP
session is torn down immediately, triggering route updates to propagate
through the network quickly. This requires the following commands be
enabled for all links: `link-detect` and `ttl-security hops <hops>`.
`ttl-security hops` specifies how many hops away the neighbor is. For
example, in a Clos topology, every peer is at most 1 hop away.

{{%notice note%}}

See Caveats and Errata below for information regarding 
`ttl-security hops`.

{{%/notice%}}

Here is an example:

    cumulus@switch:~$ net add bgp neighbor 10.0.0.2 ttl-security hops 1

### Converging Quickly On Soft Failures

It is possible that the link is up, but the neighboring BGP process is
hung or has crashed. If a BGP process crashes, FRRouting’s `watchquagga`
daemon, which monitors the various FRRouting daemons, will attempt to
restart it. If the process is also hung, `watchquagga` will attempt to
restart the process. BGP itself has a keepalive timer that is exchanged
between neighbors. By default, this keepalive timer is set to 3 seconds.
This time can be increased to a higher number, which decreases CPU load,
especially in the presence of a lot of neighbors. `keepalive-time` is
the periodicity with which the keepalive message is sent. `hold-time`
specifies how many keepalive messages can be lost before the connection
is considered invalid. It is usually set to 3 times the keepalive time,
so it defaults to 9 seconds. Here is an example of changing these
timers:

    cumulus@switch:~$ net add bgp neighbor swp51 timers 10 30

The following display snippet shows that the default values have been
modified for this neighbor:

    cumulus@switch:~$ net show bgp neighbor swp51 
    BGP neighbor on swp51: fe80::4638:39ff:fe00:5c, remote AS 65020, local AS 65011, external link
    Hostname: spine01
     Member of peer-group fabric for session parameters
      BGP version 4, remote router ID 0.0.0.0
      BGP state = Connect
      Last read 00:00:13, Last write 00:39:43
      Hold time is 30, keepalive interval is 10 seconds
      Configured hold time is 30, keepalive interval is 10 seconds
    ...

### Reconnecting Quickly

A BGP process attempts to connect to a peer after a failure (or on
startup) every `connect-time` seconds. By default, this is 10 seconds.
To modify this value, use:

    cumulus@switch:~$ net add bgp neighbor swp51 timers connect 30

This command has to be specified per each neighbor, peer-group doesn’t
support this option in `frr`.

### Advertisement Interval

BGP by default chooses stability over fast convergence. This is very
useful when routing for the Internet. For example, unlike link-state
protocols, BGP typically waits for a duration of
`advertisement-interval` seconds between sending consecutive updates to
a neighbor. This ensures that an unstable neighbor flapping routes won’t
be propagated throughout the network. By default, this is set to 0
seconds for both eBGP and iBGP sessions, which allows for very fast
convergence. You can modify this as follows:

    cumulus@switch:~$ net add bgp neighbor swp51 advertisement-interval 5

The following output shows the modified value:

    cumulus@switch:~$ net show bgp neighbor swp51 
    BGP neighbor on swp51: fe80::4638:39ff:fe00:5c, remote AS 65020, local AS 65011, external link
    Hostname: spine01
     Member of peer-group fabric for session parameters
      BGP version 4, remote router ID 0.0.0.0
      BGP state = Connect
      Last read 00:04:37, Last write 00:44:07
      Hold time is 30, keepalive interval is 10 seconds
      Configured hold time is 30, keepalive interval is 10 seconds
      Message statistics:
        Inq depth is 0
        Outq depth is 0
                             Sent       Rcvd
        Opens:                  1          1
        Notifications:          1          0
        Updates:                7          6
        Keepalives:          2374       2373
        Route Refresh:          0          0
        Capability:             0          0
        Total:               2383       2380
      Minimum time between advertisement runs is 5 seconds
    ...

{{%notice note%}}

This command is not supported with peer-groups.

{{%/notice%}}

See this [IETF draft](http://tools.ietf.org/html/draft-jakma-mrai-02)
for more details on the use of this value.

## Caveats and Errata

### ttl-security Issue

Enabling `ttl-security` does not cause the hardware to be programmed
with the relevant information. This means that frames will come up to
the CPU and be dropped there. It is recommended that you use the `net
add acl` command to explicitly add the relevant entry to hardware.

For example, you can configure a file, like
`/etc/cumulus/acl/policy.d/01control_plane_bgp.rules`, with a rule like
this for TTL:

    INGRESS_INTF = swp1 
        INGRESS_CHAIN = INPUT, FORWARD 
     
        [iptables]
        -A $INGRESS_CHAIN --in-interface $INGRESS_INTF -p tcp --dport bgp -m ttl --ttl 255 POLICE --set-mode pkt --set-rate 2000 --set-burst 1000 
    -A $INGRESS_CHAIN --in-interface $INGRESS_INTF -p tcp --dport bgp DROP

{{%notice note%}}

For more information about ACLs, see [Netfilter (ACLs)](/version/cumulus-linux-36/System-Configuration/Netfilter-ACLs/).

{{%/notice%}}

### BGP Dynamic Capabilities not Supported

Dynamic capabilities, which enable BGP to renegotiate a new feature for
an already established peer, are not supported in Cumulus Linux.

## Related Information

  - [Bidirectional forwarding detection](/version/cumulus-linux-36/Layer-3/Bidirectional-Forwarding-Detection-BFD)
    (BFD) and BGP
  - [Wikipedia entry for BGP](http://en.wikipedia.org/wiki/Border_Gateway_Protocol) (includes
    list of useful RFCs)
  - [FRR BGP documentation](https://frrouting.org/user-guide/bgp.html)
  - [IETF draft discussing BGP use within data centers](http://tools.ietf.org/html/draft-lapukhov-bgp-routing-large-dc-04)
  - [RFC 1657, Definitions of Managed Objects for the Fourth Version of the Border Gateway Protocol (BGP-4) using SMIv2](https://tools.ietf.org/html/rfc1657)
  - [RFC 1997, BGP Communities Attribute](https://tools.ietf.org/html/rfc1997)
  - [RFC 2385, Protection of BGP Sessions via the TCP MD5 Signature Option](https://tools.ietf.org/html/rfc2385)
  - [RFC 2439, BGP Route Flap Damping](https://tools.ietf.org/html/rfc2439)
  - [RFC 2545, Use of BGP-4 Multiprotocol Extensions for IPv6 Inter-Domain Routing](https://tools.ietf.org/html/rfc2545)
  - [RFC 2918, Route Refresh Capability for BGP-4](https://tools.ietf.org/html/rfc2918)
  - [RFC 4271, A Border Gateway Protocol 4 (BGP-4)](https://tools.ietf.org/html/rfc4271)
  - [RFC 4360, BGP Extended Communities Attribute](https://tools.ietf.org/html/rfc4360)
  - [RFC 4456, BGP Route Reflection – An Alternative to Full Mesh Internal BGP (iBGP)](https://tools.ietf.org/html/rfc4456)
  - [RFC 4760, Multiprotocol Extensions for BGP-4](https://tools.ietf.org/html/rfc4760)
  - [RFC 5004, Avoid BGP Best Path Transitions from One External to Another](https://tools.ietf.org/html/rfc5004)
  - [RFC 5065, Autonomous System Confederations for BGP](https://tools.ietf.org/html/rfc5065)
  - [RFC 5291, Outbound Route Filtering Capability for BGP-4](https://tools.ietf.org/html/rfc5291)
  - [RFC 5492, Capabilities Advertisement with BGP-4](https://tools.ietf.org/html/rfc5492)
  - [RFC 5549, Advertising IPv4 Network Layer Reachability Information with an IPv6 Next Hop](https://tools.ietf.org/html/rfc5549)
  - [RFC 6793, BGP Support for Four-Octet Autonomous System (AS) Number Space](https://tools.ietf.org/html/rfc6793)
  - [RFC 7911, Advertisement of Multiple Paths in BGP](https://tools.ietf.org/html/rfc7911)
  - [draft-walton-bgp-hostname-capability-02, Hostname Capability for BGP](https://tools.ietf.org/html/draft-walton-bgp-hostname-capability-00)
