---
title: Monitor the OSPF Service
author: Cumulus Networks
weight: 169
aliases:
 - /display/NETQ21/Monitor+the+OSPF+Service
 - /pages/viewpage.action?pageId=12321517
pageID: 12321517
product: Cumulus NetQ
version: '2.1'
imgData: cumulus-netq-21
siteSlug: cumulus-netq-21
---
The Cumulus NetQ UI enables operators to view the health of the OSPF
service on a network-wide and a per session basis, giving greater
insight into all aspects of the service. This is accomplished through
two card workflows, one for the service and one for the session. They
are described separately here.

## Monitor the OSPF Service (All Sessions)</span>

With NetQ, you can monitor the number of nodes running the OSPF service,
view switches with the most full and unestablished OSPF sessions, and
view alarms triggered by the OSPF service. For an overview and how to
configure OSPF to run in your data center network, refer to [Open
Shortest Path First -
OSPF](/display/NETQ21/Open+Shortest+Path+First+-+OSPF) or [Open Shortest
Path First v3 -
OSPFv3](/display/NETQ21/Open+Shortest+Path+First+v3+-+OSPFv3).

### OSPF Service Card Workflow</span>

The small OSPF Service card displays:

{{% imgOld 0 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><span style="color: #333c4e;"> </span></p>
<p>{{% imgOld 1 %}}</p></td>
<td><p>Indicates data is for all sessions of a Network Service or Protocol</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p><strong>OSPF</strong>: All OSPF Sessions, or the OSPF Service</p></td>
</tr>
<tr class="odd">
<td><p>{{% imgOld 2 %}}</p></td>
<td><p>Total number of switches and hosts with the OSPF service enabled during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 3 %}}</p></td>
<td><p>Total number of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>Chart</p></td>
<td><p>Distribution of OSPF-related alarms received during the designated time period</p></td>
</tr>
</tbody>
</table>

The medium OSPF Service card displays:

{{% imgOld 4 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #333c4e;"> </span></p>
<p>{{% imgOld 5 %}}</p></td>
<td><p>Indicates data is for all sessions of a Network Service or Protocol</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Network Services | All OSPF Sessions</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 6 %}}</p></td>
<td><p>Total number of switches and hosts with the OSPF service enabled during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>{{% imgOld 7 %}}</p></td>
<td><p>Total number of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>Total Nodes Running chart</p></td>
<td><p>Total number and distribution of switches and hosts with the OSPF service enabled during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>Total Alarms chart</p></td>
<td><p>Total number and distribution of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>Total Sessions chart</p></td>
<td><p>Total number and distribution of OSPF sessions during the designated time period</p></td>
</tr>
</tbody>
</table>

The large OSPF service card contains two tabs.

The *Sessions Summary* tab displays:  

{{% imgOld 8 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #333c4e;"> </span></p>
<p>{{% imgOld 9 %}}</p></td>
<td><p>Indicates data is for all sessions of a Network Service or Protocol</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Sessions Summary (visible when you hover over card)</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 10 %}}</p></td>
<td><p>Total number of switches and hosts with the OSPF service enabled during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>{{% imgOld 11 %}}</p></td>
<td><p>Total number of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>Total Nodes Running chart</p></td>
<td><p>Total number and distribution of switches and hosts with the OSPF service enabled</p></td>
</tr>
<tr class="odd">
<td><p>Total Sessions chart</p></td>
<td><p>Total number and distribution of OSPF sessions during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>Total Nodes Not Est. chart</p></td>
<td><p>Total number and distribution of switches and hosts with unestablished OSPF sessions during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>Table/Filter options</p></td>
<td><p>When the <strong>Switches with Most Sessions</strong> filter option is selected, the table displays the switches and hosts running OSPF sessions in decreasing order of session count—devices with the largest number of sessions are listed first</p>
<p>When the <strong>Switches with Most Unestablished Sessions</strong> filter option is selected, the table switches and hosts running OSPF sessions in decreasing order of unestablished sessions—devices with the largest number of unestablished sessions are listed first</p></td>
</tr>
<tr class="even">
<td><p>Show All Sessions</p></td>
<td><p>Link to view data for all OSPF sessions in the full screen card</p></td>
</tr>
</tbody>
</table>

The *Alarms* tab displays:

{{% imgOld 12 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #333c4e;"> </span></p>
<p>{{% imgOld 13 %}}</p></td>
<td><p>Indicates data is for all sessions of a Network Service or Protocol</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Alarms (visible when you hover over card)</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 14 %}}</p></td>
<td><p>Total number of switches and hosts with the OSPF service enabled during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>{{% imgOld 15 %}}</p></td>
<td><p>Total number of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="even">
<td><p>Total Alarms chart</p></td>
<td><p>Total number and distribution of OSPF-related alarms received during the designated time period</p></td>
</tr>
<tr class="odd">
<td><p>Table/Filter options</p></td>
<td><p>When the selected filter option is <strong>Switches with Most Alarms</strong>, the table displays <strong></strong> switches and hosts running OSPF in decreasing order of the count of alarms—devices with the largest number of OSPF alarms are listed first</p></td>
</tr>
<tr class="even">
<td><p>Show All Sessions</p></td>
<td><p>Link to view data for all OSPF sessions in the full screen card</p></td>
</tr>
</tbody>
</table>

The full screen OSPF Service card provides tabs for all switches, all
sessions, and all alarms.

{{% imgOld 16 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Network Services | OSPF</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 17 %}}</p></td>
<td><p>Closes full screen card and returns to workbench</p></td>
</tr>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes; select an alternate time period by clicking <span style="color: #353744;"> </span></p>
<p>{{% imgOld 18 %}}</p></td>
</tr>
<tr class="even">
<td><p>Results</p></td>
<td><p>Number of results found for the selected tab</p></td>
</tr>
<tr class="odd">
<td><p>All Switches tab</p></td>
<td><p>Displays all switches and hosts running the OSPF service. By default, the device list is sorted by <strong>hostname</strong>. This tab provides the following additional data about each device:</p>
<ul>
<li><p><strong>Agent</strong></p>
<ul>
<li><p>State: Indicates communication state of the NetQ Agent on a given device. Values include Fresh (heard from recently) and Rotten (not heard from recently).</p></li>
<li><p>Version: Software version number of the NetQ Agent on a given device. This should match the version number of the NetQ software loaded on your server or appliance; for example, 2.1.0.</p></li>
</ul></li>
<li><p><strong>ASIC</strong></p>
<ul>
<li><p>Core BW: Maximum sustained/rated bandwidth. Example values include 2.0 T and 720 G.</p></li>
<li><p>Model: Chip family. Example values include Tomahawk, Trident, and Spectrum.</p></li>
<li><p>Model Id: Identifier of networking ASIC model. Example values include <span style="color: #222222;"> BCM56960 and BCM56854. </span></p></li>
<li><p>Ports: Indicates port configuration of the switch. Example values include <span style="color: #222222;"> 32 x 100G-QSFP28, 48 x 10G-SFP+, and 6 x 40G-QSFP+. </span></p></li>
<li><p>Vendor: Manufacturer of the chip. Example values include Broadcom and Mellanox.</p></li>
</ul></li>
<li><p><strong>CPU</strong></p>
<ul>
<li><p>Arch: Microprocessor architecture type. Values include x86_64 (Intel), ARMv7 (AMD), and PowerPC.</p></li>
<li><p>Max Freq: Highest rated frequency for CPU. Example values include <span style="color: #222222;"> 2.40 GHz and 1.74 GHz. </span></p></li>
<li><p>Model: Chip family. Example values include <span style="color: #222222;"> Intel Atom C2538 and Intel Atom C2338. </span></p></li>
<li><p>Nos: Number of cores. Example values include 2, 4, and 8.</p></li>
</ul></li>
<li><p><strong>Disk Total Size</strong>: Total amount of storage space in physical disks (not total available). Example values: 10 GB, 20 GB, 30 GB.</p></li>
<li><p><strong>License State</strong>: Indicator of validity. Values include ok and bad.</p></li>
<li><p><strong>Memory Size</strong>: Total amount of local RAM. Example values include 8192 MB and 2048 MB.</p></li>
<li><p><strong>OS</strong></p>
<ul>
<li><p>Vendor: Operating System manufacturer. Values include Cumulus Networks, RedHat, Ubuntu, and CentOS.</p></li>
<li><p>Version: Software version number of the OS. Example values include 3.7.3, 2.5.x, 16.04, 7.1.</p></li>
<li><p>Version Id: Identifier of the OS version. For Cumulus, this is the same as the <em>Version</em> (3.7.x).</p></li>
</ul></li>
<li><p><strong>Platform</strong></p>
<ul>
<li><p>Date: Date and time the platform was manufactured. Example values include 7/12/18 and 10/29/2015.</p></li>
<li><p>MAC: System MAC address. Example value: <span style="color: #545454;"> 17:01:AB:EE:C3:F5. </span></p></li>
<li><p>Model: <span style="color: #222222;"> Manufacturer's model name. Examples values include AS7712-32X and S4048-ON. </span></p></li>
<li><p>Number: Manufacturer part number. Examples values include FP3ZZ7632014A, 0J09D3.</p></li>
<li><p>Revision: Release version of the platform</p></li>
<li><p>Series: Manufacturer serial number. Example values include D2060B2F044919GD000060, CN046MRJCES0085E0004.</p></li>
<li><p>Vendor: Manufacturer of the platform. Example values include Cumulus Express, Dell, EdgeCore, Lenovo, Mellanox.</p></li>
</ul></li>
<li><p><strong>Time:</strong> Date and time the data was collected from device.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>All Sessions tab</p></td>
<td><p>Displays all OSPF sessions network-wide. By default, the session list is sorted by <strong>hostname</strong>. This tab provides the following additional data about each session:</p>
<ul>
<li><p><strong>Area</strong>: Routing domain for this host device. Example values include 0.0.0.1, 0.0.0.23.</p></li>
<li><p><strong>DB State</strong>: Session state of DB</p></li>
<li><p><strong>Ifname</strong>: Name of the interface on host device where session resides. Example values include swp5, peerlink-1.</p></li>
<li><p><strong>Is IPv6</strong>: Indicates whether the address of the host device is IPv6 (true) or IPv4 (false)</p></li>
<li><p><strong>Peer</strong></p>
<ul>
<li><p>Address: IPv4 or IPv6 address of the peer device</p></li>
<li><p>Hostname: User-defined name for peer device</p></li>
<li><p>ID: Network subnet address of router with access to the peer device</p></li>
</ul></li>
<li><p><strong>State</strong>: Current state of OSPF. Values include Full, 2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.</p></li>
<li><p><strong>Timestamp</strong>: Date and time session was started, deleted, updated or marked dead (device is down)</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>All Alarms tab</p></td>
<td><p>Displays all OSPF events network-wide. By default, the event list is sorted by <strong>time</strong>, with the most recent events listed first. The tab provides the following additional data about each event:</p>
<ul>
<li><p><strong>Message</strong>: Text description of a OSPF-related event. Example: swp4 area ID mismatch with peer leaf02</p></li>
<li><p><strong>Source</strong>: Hostname of network device that generated the event</p></li>
<li><p><strong>Severity</strong>: Importance of the event. Values include critical, warning, info, and debug.</p></li>
<li><p><strong>Type</strong>: Network protocol or service generating the event. This always has a value of <em>OSPF</em> in this card workflow.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Export</p></td>
<td><p>Enables export of all or selected items in a CSV or JSON formatted file</p></td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 19 %}}</p></td>
<td><p>Enables manipulation of table display; choose columns to display and reorder columns</p></td>
</tr>
</tbody>
</table>

### View Service Status Summary</span>

A summary of the OSPF service is available from the Network Services
card workflow, including the number of nodes running the service, the
number of OSPF-related alarms, and a distribution of those alarms.

To view the summary, open the small OSPF Service card.

{{% imgOld 20 %}}

For more detail, select a different size OSPF Service card.

### View the Distribution of Sessions</span>

It is useful to know the number of network nodes running the OSPF
protocol over a period of time, as it gives you insight into the amount
of traffic associated with and breadth of use of the protocol. It is
also useful to view the health of the sessions.

To view these distributions, open the medium OSPF Service card.

{{% imgOld 21 %}}

You can dig a little deeper with the large OSPF Service card tabs.

### View Devices with the Most OSPF Sessions</span>

You can view the load from OSPF on your switches and hosts using the
large Network Services card. This data enables you to see which switches
are handling the most OSPF traffic currently, validate that is what is
expected based on your network design, and compare that with data from
an earlier time to look for any differences.

To view switches and hosts with the most OSPF sessions:

1.  Open the large OSPF Service card.

2.  Select **SWITCHES WITH MOST SESSIONS** from the filter above the
    table.  
    The table content is sorted by this characteristic, listing nodes
    running the most OSPF sessions at the top. Scroll down to view those
    with the fewest sessions.
    
    {{% imgOld 22 %}}

To compare this data with the same data at a previous time:

1.  Open another large OSPF Service card.

2.  Move the new card next to the original card if needed.

3.  Change the time period for the data on the new card by hovering over
    the card and clicking <span style="color: #353744;"> </span>
    
    {{% imgOld 23 %}}
    
    .

4.  Select the time period that you want to compare with the original
    time. We chose *Past Week* for this example.  
    
    {{% imgOld 24 %}}
    
    {{% imgOld 25 %}}
    
      
    You can now see whether there are significant differences between
    this time and the original time. If the changes are unexpected, you
    can investigate further by looking at another time frame,
    determining if more nodes are now running OSPF than previously,
    looking for changes in the topology, and so forth.

### View Devices with the Most Unestablished OSPF Sessions</span>

You can identify switches and hosts that are experiencing difficulties
establishing OSPF sessions; both currently and in the past.

To view switches with the most unestablished OSPF sessions:

1.  Open the large OSPF Service card.

2.  Select **SWITCHES WITH MOST UNESTABLISHED SESSIONS** from the filter
    above the table.  
    The table content is sorted by this characteristic, listing nodes
    with the most unestablished OSPF sessions at the top. Scroll down to
    view those with the fewest unestablished sessions.
    
    {{% imgOld 26 %}}

Where to go next depends on what data you see, but a couple of options
include:

  - Hover over the **Total Nodes Not Est.** chart to focus on the
    switches and hosts with the most unestablished sessions during that
    smaller time slice.  
    The table content changes to match the hovered content. Click on the
    chart to persist the table changes.

  - Change the time period for the data to compare with a prior time.
    
    {{% imgOld 27 %}}
    
    If the same switches are consistently indicating the most
    unestablished sessions, you might want to look more carefully at
    those switches using the Switches card workflow to determine
    probable causes. Refer to [Monitor
    Switches](/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-Switches).

  - Click **Show All Sessions** to investigate all OSPF sessions with
    events in the full screen card.

### <span id="src-12321517_MonitortheOSPFService-DevsMostAlarmsBGP" class="confluence-anchor-link"></span>View Devices with the Most OSPF-related Alarms</span>

Switches or hosts experiencing a large number of OSPF alarms may
indicate a configuration or performance issue that needs further
investigation. You can view the devices sorted by the number of OSPF
alarms and then use the Switches card workflow or the Alarms card
workflow to gather more information about possible causes for the
alarms. compare the number of nodes running OSPF with unestablished
sessions with the alarms present at the same time to determine if there
is any correlation between the issues and the ability to establish a
OSPF session.

To view switches with the most OSPF alarms:

1.  Open the large OSPF Service card.

2.  Hover over the header and click
    
    {{% imgOld 28 %}}
    
    .

3.  Select **SWITCHES WITH MOST ALARMS** from the filter above the
    table.  
    The table content is sorted by this characteristic, listing nodes
    with the most OSPF alarms at the top. Scroll down to view those with
    the fewest alarms.
    
    {{% imgOld 29 %}}

Where to go next depends on what data you see, but a few options
include:

  - Hover over the Total Alarms chart to focus on the switches
    exhibiting alarms during that smaller time slice.  
    The table content changes to match the hovered content. Click on the
    chart to persist the table changes.

  - Change the time period for the data to compare with a prior time. If
    the same switches are consistently indicating the most alarms, you
    might want to look more carefully at those switches using the
    Switches card workflow.

  - Click **Show All Sessions** to investigate all OSPF sessions with
    events in the full screen card.

### View All OSPF Events</span>

The OSPF Network Services card workflow enables you to view all of the
OSPF events in the designated time period.

To view all OSPF events:

1.  Open the full screen OSPF Service card.

2.  Click **All Alarms** tab in the navigation panel.  
    By default, events are listed in most recent to least recent order.

Where to go next depends on what data you see, but a couple of options
include:

  - Open one of the other full screen tabs in this flow to focus on
    devices or sessions.

  - Export the data for use in another analytics tool, by clicking
    **Export** and providing a name for the data file.

### View Details for All Devices Running OSPF</span>

You can view all stored attributes of all switches and hosts running
OSPF in your network in the full screen card.

To view all device details, open the full screen OSPF Service card and
click the **All Switches** tab.

{{% imgOld 30 %}}

To return to your workbench, click

{{% imgOld 31 %}}

in the top right corner.

### View Details for All OSPF Sessions</span>

You can view all stored attributes of all OSPF sessions in your network
in the full-screen card.

To view all session details, open the full screen OSPF Service card and
click the **All Sessions** tab.

{{% imgOld 32 %}}

To return to your workbench, click

{{% imgOld 33 %}}

in the top right corner.

### Take Actions on Data Displayed in Results List</span>

In the full screen OSPF Service card, you can determine which results
are displayed in the results list, and which are exported.

To take actions on the data, click in the blank column at the very left
of a row. A checkbox appears, selecting that switch, session, or alarm,
and an edit menu is shown at the bottom of the card (shown enlarged
here).

{{% imgOld 34 %}}

{{% imgOld 35 %}}

You can perform the following actions on the results list:

| Option             | Action or Behavior on Click                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| Select All         | Selects all items in the results list                                                                            |
| Clear All          | Clears all existing selections of items in the results list. This also hides the edit menu.                      |
| Open Cards         | Open the corresponding validation or trace result card.                                                          |
| Hide Selected      | Hide selected items (switches, sessions, alarms, and so forth) from the results list.                            |
| Show Only Selected | Hide unselected items (switches, sessions, alarms, and so forth) from the results list.                          |
| Export Selected    | Exports selected data into a .csv file. If you want to export to a .json file format, use the **Export** button. |

To return to original display of results, click the associated tab.

## Monitor a Single OSPF Session</span>

With NetQ, you can monitor a single session of the OSPF service, view
session state changes, and compare with alarms occurring at the same
time, as well as monitor the running OSPF configuration and changes to
the configuration file. For an overview and how to configure OSPF to run
in your data center network, refer to [Open Shortest Path First -
OSPF](/display/NETQ21/Open+Shortest+Path+First+-+OSPF) or [Open Shortest
Path First v3 -
OSPFv3](/display/NETQ21/Open+Shortest+Path+First+v3+-+OSPFv3).

{{%notice info%}}

To access the single session cards, you must open the full screen OSPF
Service (all sessions) card, click the All Sessions tab, and
double-click on a session. The full screen card automatically closes so
you can view the medium single session card.

{{%/notice%}}

### <span id="src-12321517_safe-id-TW9uaXRvcnRoZU9TUEZTZXJ2aWNlLSNUaW1lUGVyR3Jhbg" class="confluence-anchor-link"></span>Granularity of Data Shown Based on Time Period</span>

On the medium and large single OSPF session cards, the status of the
sessions is represented in heat maps stacked vertically; one for
established sessions, and one for unestablished sessions. Depending on
the time period of data on the card, the number of smaller time blocks
used to indicate the status varies. A vertical stack of time blocks, one
from each map, includes the results from all checks during that time.
The results are shown by how saturated the color is for each block. If
all sessions during that time period were established for the entire
time block, then the top block is 100% saturated (white) and the not
established block is zero percent saturated (gray). As sessions that are
not established increase in saturation, the sessions that are
established block is proportionally reduced in saturation. An example
heat map for a time period of 24 hours is shown here with the most
common time periods in the table showing the resulting time blocks.

{{% imgOld 36 %}}

| Time Period | Number of Runs | Number Time Blocks | Amount of Time in Each Block |
| ----------- | -------------- | ------------------ | ---------------------------- |
| 6 hours     | 18             | 6                  | 1 hour                       |
| 12 hours    | 36             | 12                 | 1 hour                       |
| 24 hours    | 72             | 24                 | 1 hour                       |
| 1 week      | 504            | 7                  | 1 day                        |
| 1 month     | 2,086          | 30                 | 1 day                        |
| 1 quarter   | 7,000          | 13                 | 1 week                       |

<span style="color: #36424a;"> OSPF Session Card Workflow Summary
</span>

The small OSPF Session card displays:

{{% imgOld 37 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><span style="color: #222222;"> </span></p>
<p>{{% imgOld 38 %}}</p></td>
<td><p>Indicates data is for a single session of a Network Service or Protocol</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p><strong>OSPF Session</strong></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Hostnames of the two devices in a session. Arrow points from the host to the peer.</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 39 %}}</p>
, <span style="color: #353744;"> </span>
<p>{{% imgOld 40 %}}</p>
</span></td>
<td><p>Current state of OSPF.</p>
<p>{{% imgOld 41 %}}</p>
Full or
<p>{{% imgOld 42 %}}</p>
2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.</td>
</tr>
</tbody>
</table>

The medium OSPF Session card displays:

{{% imgOld 43 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 44 %}}</p></td>
<td><p>Indicates data is for a single session of a Network Service or Protocol</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Network Services | OSPF Session</p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>Hostnames of the two devices in a session. Arrow points in the direction of the session.</p></td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 45 %}}</p>
,
<p>{{% imgOld 46 %}}</p></td>
<td><p>Current state of OSPF.</p>
<p>{{% imgOld 47 %}}</p>
Full or
<p>{{% imgOld 48 %}}</p>
2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.</td>
</tr>
<tr class="even">
<td><p>Time period for chart</p></td>
<td><p>Time period for the chart data</p></td>
</tr>
<tr class="odd">
<td><p>Session State Changes Chart</p></td>
<td><p>Heat map of the state of the given session over the given time period. The status is sampled at a rate consistent with the time period. For example, for a 24 hour period, a status is collected every hour. Refer to <a href="#src-12321517_MonitortheOSPFService-TimePerGran">Granularity of Data Shown Based on Time Period</a>.</p></td>
</tr>
<tr class="even">
<td><p>Ifname</p></td>
<td><p>Interface name on or hostname for host device where session resides</p></td>
</tr>
<tr class="odd">
<td><p>Peer Address</p></td>
<td><p>IP address of the peer device</p></td>
</tr>
<tr class="even">
<td><p>Peer ID</p></td>
<td><p>IP address of router with access to the peer device</p></td>
</tr>
</tbody>
</table>

The large OSPF Session card contains two tabs.

The *Session Summary* tab displays:

<span style="color: #000000;"> </span>

{{% imgOld 49 %}}

<table class="confluenceTable">

<thead class=" ">

<tr>

<td class="confluenceTh" rowspan="1" colspan="1">

Item

</td>

<td class="confluenceTh" rowspan="1" colspan="1">

Description

</td>

</tr>

</thead>

<tfoot class=" ">

</tfoot>

<tbody class=" ">

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Time period

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Range of time in which the displayed data was collected; applies to all
card sizes

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

{{% imgOld 50 %}}

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Indicates data is for a single session of a Network Service or Protocol

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Title

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Session Summary (Network Services | OSPF Session)

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="2" colspan="1">

Summary bar

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Hostnames of the two devices in a session. Arrow points in the direction
of the session.

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Current state of OSPF.

{{% imgOld 51 %}}

Full or

{{% imgOld 52 %}}

2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Session State Changes Chart

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Heat map of the state of the given session over the given time period.
The status is sampled at a rate consistent with the time period. For
example, for a 24 hour period, a status is collected every hour. Refer
to [Granularity of Data Shown Based on Time
Period](#src-12321517_MonitortheOSPFService-TimePerGran) .

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Alarm Count Chart

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Distribution and count of OSPF alarm events over the given time period

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Info Count Chart

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Distribution and count of OSPF info events over the given time period

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Ifname

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Name of the interface on the host device where the session resides

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

State

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Current state of OSPF.

{{% imgOld 53 %}}

Full or

{{% imgOld 54 %}}

2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Is Unnumbered

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Indicates if the session is part of an unnumbered OSPF configuration
(*true*) or part of a numbered OSPF configuration (*false*)

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Nbr Count

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Number of routers in the OSPF configuration

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Is Passive

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Indicates if the host is in a passive state (*true*) or active state
(*false*).

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Peer ID

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

IP address of router with access to the peer device

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Is IPv6

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Indicates if the IP address of the host device is IPv6 (*true*) or IPv4
(*false*)

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

If Up

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Indicates if the interface on the host is up (true) or down (false)

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Nbr Adj Count

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Number of adjacent routers for this host

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

MTU

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Maximum transmission unit (MTU) on shortest path between the host and
peer

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Peer Address

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

IP address of the peer device

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Area

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Routing domain of the host device

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Network Type

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Architectural design of the network. Values include Point-to-Point and
Broadcast.

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Cost

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Shortest path through the network between the host and peer devices

</td>

</tr>

<tr>

<td class="confluenceTd" rowspan="1" colspan="1">

Dead Time

</td>

<td class="confluenceTd" rowspan="1" colspan="1">

Countdown timer, starting at 40 seconds, that is constantly reset as
messages are heard from the neighbor. If the dead time gets to zero, the
neighbor is presumed dead, the adjacency is torn down, and the link
removed from SPF calculations in the OSPF database.

</td>

</tr>

</tbody>

</table>

The *Configuration File Evolution* tab displays:

{{% imgOld 55 %}}

{{% imgOld 56 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 57 %}}</p></td>
<td><p>Indicates data is for a single session of a Network Service or Protocol</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>(Network Services | OSPF Session) <strong>Configuration File Evolution</strong></p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 58 %}}</p></td>
<td><p>Device identifiers (hostname, IP address, or MAC address) for host and peer in session. Click on <span style="color: #353744;"> </span></p>
<p>{{% imgOld 59 %}}</p>
to open associated device card.</td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 60 %}}</p>
,
<p>{{% imgOld 61 %}}</p></td>
<td><p>Current state of OSPF.</p>
<p>{{% imgOld 62 %}}</p>
Full or
<p>{{% imgOld 63 %}}</p>
2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.</td>
</tr>
<tr class="even">
<td><p>Timestamps</p></td>
<td><p>When changes to the configuration file have occurred, the date and time are indicated. Click the time to see the changed file.</p></td>
</tr>
<tr class="odd">
<td><p>Configuration File</p></td>
<td><p>When <strong>File</strong> is selected, the configuration file as it was at the selected time is shown. When <strong>Diff</strong> is selected, the configuration file at the selected time is shown on the left and the configuration file at the previous timestamp is shown on the right. Differences are highlighted.</p>
<p><strong>Note</strong>: If no configuration file changes have been made, the card shows no results at all.</p></td>
</tr>
</tbody>
</table>

The full screen OSPF Session card provides tabs for all OSPF sessions
and all events.

{{% imgOld 64 %}}

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Item</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Network Services | OSPF</p></td>
</tr>
<tr class="odd">
<td><p>All OSPF Sessions tab</p></td>
<td><p>Displays all OSPF sessions running on the host device. The session list is sorted by <strong>hostname</strong> by default. This tab provides the following additional data about each session:</p>
<ul>
<li><p><strong>Area</strong>: Routing domain for this host device. Example values include 0.0.0.1, 0.0.0.23.</p></li>
<li><p><strong>DB State</strong>: Session state of DB</p></li>
<li><p><strong>Ifname</strong>: Name of the interface on host device where session resides. Example values include swp5, peerlink-1.</p></li>
<li><p><strong>Is IPv6</strong>: Indicates whether the address of the host device is IPv6 (true) or IPv4 (false)</p></li>
<li><p><strong>Peer</strong></p>
<ul>
<li><p>Address: IPv4 or IPv6 address of the peer device</p></li>
<li><p>Hostname: User-defined name for peer device</p></li>
<li><p>ID: Network subnet address of router with access to the peer device</p></li>
</ul></li>
<li><p><strong>State</strong>: Current state of OSPF. Values include Full, 2-way, Attempt, Down, Exchange, Exstart, Init, and Loading.</p></li>
<li><p><strong>Timestamp</strong>: Date and time session was started, deleted, updated or marked dead (device is down)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>All Events tab</p></td>
<td><p>Displays all events network-wide. By default, the event list is sorted by <strong>time</strong>, with the most recent events listed first. The tab provides the following additional data about each event:</p>
<ul>
<li><p><strong>Message</strong>: Text description of a OSPF-related event. Example: OSPF session with peer tor-1 swp7 vrf default state changed from failed to Established</p></li>
<li><p><strong>Source</strong>: Hostname of network device that generated the event</p></li>
<li><p><strong>Severity</strong>: Importance of the event. Values include critical, warning, info, and debug.</p></li>
<li><p><strong>Type</strong>: Network protocol or service generating the event. This always has a value of OSPF in this card workflow.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Export</p></td>
<td><p>Enables export of all or selected items in a CSV or JSON formatted file</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 65 %}}</p></td>
<td><p>Enables manipulation of table display; choose columns to display and reorder columns</p></td>
</tr>
</tbody>
</table>

### View Session Status Summary</span>

A summary of the OSPF session is available from the OSPF Session card
workflow, showing the node and its peer and current status.

To view the summary:

1.  Add the Network Services | All OSPF Sessions card.

2.  Switch to the full screen card.

3.  Click the **All Sessions** tab.

4.  Double-click the session of interest. The full screen card closes
    automatically.

5.  Optionally, switch to the small OSPF Session card.  
    
    {{% imgOld 66 %}}
    
    {{% imgOld 67 %}}

### View OSPF Session State Changes</span>

You can view the state of a given OSPF session from the medium and large
OSPF Session Network Service cards. For a given time period, you can
determine the stability of the OSPF session between two devices. If you
experienced connectivity issues at a particular time, you can use these
cards to help verify the state of the session. If it was not established
more than it was established, you can then investigate further into
possible causes.

To view the state transitions for a given OSPF session, on the *medium*
OSPF Session card:

1.  Add the Network Services | All OSPF Sessions card.

2.  Switch to the full screen card.

3.  Open the large OSPF Service card.

4.  Click the **All Sessions** tab.

5.  Double-click the session of interest. The full screen card closes
    automatically.
    
    {{% imgOld 68 %}}

The heat map indicates the status of the session over the designated
time period. In this example, the session has been established for the
entire time period.

From this card, you can also view the interface name, peer address, and
peer id identifying the session in more detail.

To view the state transitions for a given OSPF session on the large OSPF
Session card, follow the same steps to open the medium OSPF Session card
and then switch to the large card.

{{% imgOld 69 %}}

From this card, you can view the alarm and info event counts, interface
name, peer address and peer id, state, and several other parameters
identifying the session in more detail.

### View Changes to the OSPF Service Configuration File</span>

Each time a change is made to the configuration file for the OSPF
service, NetQ logs the change and enables you to compare it with the
last version. This can be useful when you are troubleshooting potential
causes for alarms or sessions losing their connections.

To view the configuration file changes:

1.  Open the large OSPF Session card.

2.  Hover over the card and click
    
    {{% imgOld 70 %}}
    
    to open the **Configuration File Evolution** tab.

3.  Select the time of interest on the left; when a change may have
    impacted the performance. Scroll down if needed.
    
    {{% imgOld 71 %}}

4.  Choose between the **File** view and the **Diff** view (selected
    option is dark; File by default).  
    The File view displays the content of the file for you to review.
    
    {{% imgOld 72 %}}
    
    The Diff view displays the changes between this version (on left)
    and the most recent version (on right) side by side. The changes are
    highlighted in red and green. In this example, we don't have a
    change to highlight, so it shows the same file on both sides.
    
    {{% imgOld 73 %}}

### View All OSPF Session Details</span>

You can view all stored attributes of all of the OSPF sessions
associated with the two devices on this card.

To view all session details, open the full screen OSPF Session card, and
click the **All OSPF Sessions** tab.

{{% imgOld 74 %}}

To return to your workbench, click

{{% imgOld 75 %}}

in the top right corner.

### View All Events</span>

You can view all of the alarm and info events for the two devices on
this card.

To view all events, open the full screen OSPF Session card, and click
the **All Events** tab.

{{% imgOld 76 %}}

To return to your workbench, click

{{% imgOld 77 %}}

in the top right corner.

<article id="html-search-results" class="ht-content" style="display: none;">

</article>

<footer id="ht-footer">

</footer>
