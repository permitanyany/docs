---
title: Validate Network Protocol and Service Operations
author: Cumulus Networks
weight: 141
aliases:
 - /display/NETQ21/Validate+Network+Protocol+and+Service+Operations
 - /pages/viewpage.action?pageId=12321612
pageID: 12321612
product: Cumulus NetQ
version: '2.1'
imgData: cumulus-netq-21
siteSlug: cumulus-netq-21
---
With the NetQ UI, you can validate the operation of the network
protocols and services running in your network either on demand or on a
scheduled basis. There are three card workflows to perform this
validation: one for creating the validation request (either on-demand or
scheduled) and two validation results (one for on-demand and one for
scheduled).

This release supports validation of the following network protocols and
services: Agents, BGP, CLAG, EVPN, Interfaces, License, MTU, NTP, OSPF,
Sensors, VLAN, and VXLAN.

For a more general understanding of how well your network is operating,
refer to the [Monitor Network
Health](/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-the-Network/Monitor-Network-Health)
topic.

## Create Validation Requests</span>

The Validation Request card workflow is used to create on-demand
validation requests to evaluate the health of your network protocols and
services.

### Validation Request Card Workflow</span>

The small Validation Request card displays:

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
<td><p><span style="color: #222222;"> </span></p>
<p>{{% imgOld 1 %}}</p></td>
<td><p>Indicates a validation request</p></td>
</tr>
<tr class="even">
<td><p>Validation</p></td>
<td><p>Select a scheduled request to run that request on-demand. A default validation is provided for each supported network protocol and service, which runs a network-wide validation check. These validations run every 60 minutes, but you may run them on-demand at any time.</p>
<p><strong>Note</strong>: No new requests can be configured from this size card.</p></td>
</tr>
<tr class="odd">
<td><p>GO</p></td>
<td><p>Start the validation request. The corresponding On-demand Validation Result cards are opened on your workbench, one per protocol and service.</p></td>
</tr>
</tbody>
</table>

The medium Validation Request card displays:

{{% imgOld 2 %}}

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
<p>{{% imgOld 3 %}}</p></td>
<td><p>Indicates a validation request</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Validation Request</p></td>
</tr>
<tr class="odd">
<td><p>Validation</p></td>
<td><p>Select a scheduled request to run that request on-demand. A default validation is provided for each supported network protocol and service, which runs a network-wide validation check. These validations run every 60 minutes, but you may run them on-demand at any time.</p>
<p><strong>Note</strong>: No new requests can be configured from this size card.</p></td>
</tr>
<tr class="even">
<td><p>Protocols</p></td>
<td><p>The protocols included in a selected validation request are listed here.</p></td>
</tr>
<tr class="odd">
<td><p>Schedule</p></td>
<td><p>For a selected scheduled validation, the schedule and the time of the last run are displayed.</p></td>
</tr>
<tr class="even">
<td><p>Run Now</p></td>
<td><p>Start the validation request</p></td>
</tr>
</tbody>
</table>

The large Validation Request card displays:

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
<td><p><span style="color: #222222;"> </span></p>
<p>{{% imgOld 5 %}}</p></td>
<td><p>Indicates a validation request</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Validation Request</p></td>
</tr>
<tr class="odd">
<td><p>Validation</p></td>
<td><p>Depending on user intent, this field is used to:</p>
<ul>
<li><p>Select a scheduled request to run that request on-demand. A default validation is provided for each supported network protocol and service, which runs a network-wide validation check. These validations run every 60 minutes, but you may run them on-demand at any time.</p></li>
<li><p>Leave as is to create a new scheduled validation request</p></li>
<li><p>Select a scheduled request to modify</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Protocols</p></td>
<td><p>For a selected scheduled validation, the protocols included in a validation request are listed here. For new on-demand or scheduled validations, click these to include them in the validation.</p></td>
</tr>
<tr class="odd">
<td><p>Schedule:</p></td>
<td><p>For a selected scheduled validation, the schedule and the time of the last run are displayed. For new scheduled validations, select the frequency and starting date and time.</p>
<ul>
<li><p>Run Every: Select how often to run the request. Choose from 30 minutes, 1, 3, 6, or 12 hours, or 1 day.</p></li>
<li><p>Starting: Select the date and time to start the first request in the series</p></li>
<li><p>Last Run: Timestamp of when the selected validation was started</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Run Now</p></td>
<td><p>Start the validation request</p></td>
</tr>
<tr class="odd">
<td><p>Update</p></td>
<td><p>When changes are made to a selected validation request, <strong>Update</strong> becomes available so that you can save your changes.</p>
<p>{{%notice info%}}</p>
<p>Be aware, that if you update a previously saved validation request, the historical data collected will no longer match the data results of future runs of the request. If your intention is to leave this request unchanged and create a new request, click <strong>Save As New</strong> instead.</p>
<p>{{%/notice%}}</p></td>
</tr>
<tr class="even">
<td><p>Save As New</p></td>
<td><p>When changes are made to a previously saved validation request, <strong>Save As New</strong> becomes available so that you can save the modified request as a new request.</p></td>
</tr>
</tbody>
</table>

The full screen Validation Request card displays all scheduled
validation requests.

{{% imgOld 6 %}}

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
<td><p>Validation Request</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 7 %}}</p></td>
<td><p>Closes full screen card and returns to workbench</p></td>
</tr>
<tr class="odd">
<td><p>Default Time</p></td>
<td><p>No time period is displayed for this card as each validation request has its own time relationship.</p></td>
</tr>
<tr class="even">
<td><p>Results</p></td>
<td><p>Number of results found for the selected tab</p></td>
</tr>
<tr class="odd">
<td><p>Validation Requests</p></td>
<td><p>Displays all <em>scheduled</em> validation requests. By default, the requests list is sorted by the date and time that it was originally created (<strong>Created At</strong>). This tab provides the following additional data about each request:</p>
<ul>
<li><p><strong>Name</strong>: Text identifier of the validation</p></li>
<li><p><strong>Type</strong>: Name of network protocols and/or services included in the validation</p></li>
<li><p><strong>Start Time</strong>: Data and time that the validation request was run</p></li>
<li><p><strong>Last Modified</strong>: Date and time of the most recent change made to the validation request</p></li>
<li><p><strong>Cadence (Min)</strong>: How often, in minutes, the validation is scheduled to run. This is empty for new on-demand requests.</p></li>
<li><p><strong>Is Active</strong>: Indicates whether the request is currently running according to its schedule (<em>true</em>) or it is not running (<em>false</em>)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Export</p></td>
<td><p>Enables export of all or selected items in a CSV or JSON formatted file</p></td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 8 %}}</p></td>
<td><p>Enables manipulation of table display; choose columns to display and reorder columns</p></td>
</tr>
</tbody>
</table>

### Creating Requests</span>

There are several types of validation requests that a user can make.
Each has a slightly different flow through the Validation Request card,
and is therefore described separately. The types are based on the intent
of the request:

  - [Run an Existing Scheduled Validation Request On
    Demand](#src-12321612_ValidateNetworkProtocolandServiceOperations-SchValNow)

  - [Create a New On-demand Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-OndNow)

  - [Modify an On-demand Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-ModOndNow)

  - [Save and Schedule an On-demand Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-SchOnd)

  - [Create a New Scheduled Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-SchNew)

  - [Modify a Scheduled Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-EditSchedVal)

### <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jU2NoVmFsTm93" class="confluence-anchor-link"></span>Run an Existing Scheduled Validation Request On Demand</span>

You may find that although you have a validation scheduled to run at a
later time, you would like to run it now.

To run a scheduled validation now:

1.  Open either the small or medium Validation Request card.

2.  Select the validation from the **Validation** dropdown list.  
    
    {{% imgOld 9 %}}
    
    {{% imgOld 10 %}}

3.  Click **Go** or **Run Now**.  
    The associated Validation Result card is opened on your workbench.
    Refer to [View On-demand Validation
    Results](#src-12321612_ValidateNetworkProtocolandServiceOperations-OndValRes).
    
    {{% imgOld 11 %}}

### <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jT25kTm93" class="confluence-anchor-link"></span>Create a New On-demand Validation Request</span>

When you want to validate the operation of one or more network protocols
and services right now, you can create and run an on-demand validation
request using the large Validation Request card.

To create and run a request for *a single* protocol or service:

1.  Open the small, medium or large Validation Request card.

2.  Select the validation from the **Validation** dropdown list.
    
    {{% imgOld 12 %}}

3.  Click **Go** or **Run Now**.  
    The associated Validation Result card is opened on your workbench.
    Refer to [View On-demand Validation
    Results](#src-12321612_ValidateNetworkProtocolandServiceOperations-OndValRes).

To create and run a request for *more than one* protocol and/or service,
essentially a custom validation request, you must first save it as a
scheduled request. After which you can run it on demand.

1.  Open the large Validation Request card.
    
    {{% imgOld 13 %}}

2.  Click the names of the protocols and services you want to validate.
    We selected BGP and EVPN in this example.
    
    {{% imgOld 14 %}}

3.  Enter the schedule frequency (30 min, 1 hour, 3 hours, 6 hours, 12
    hours, or 1 day) by selecting it from the **Run every** field.
    
    {{% imgOld 15 %}}

4.  Select the starting day and click **Next**, then select the starting
    time and click **OK**.  
    
    {{% imgOld 16 %}}
    
    {{% imgOld 17 %}}

5.  Click **Save As New**.

6.  Give the validation a unique name, and click **Save**.
    
    {{% imgOld 18 %}}

7.  Select the newly create validation from the **Validation** list.

8.  Click **Run Now** to start the validation.  
    The associated on-demand validation result cards (one per protocol
    or service selected) are opened on your current workbench. Refer to
    [View On-demand Validation
    Results](#src-12321612_ValidateNetworkProtocolandServiceOperations-OndValRes).
    
    {{% imgOld 19 %}}

### <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jU2NoTmV3" class="confluence-anchor-link"></span>Create a New Scheduled Validation Request</span>

When you want to see validation results on a regular basis, it is useful
to configure a scheduled validation request to avoid re-creating the
request each time.

To create and run a new scheduled validation:

1.  Open the large Validation Request card.

2.  Select the protocols and/or services you want to include in the
    validation. In this example we have chosen the Agents and NTP
    services.
    
    {{% imgOld 20 %}}

3.  Enter the schedule frequency (30 min, 1 hour, 3 hours, 6 hours, 12
    hours, or 1 day) by selecting it from the **Timeframe** list.
    
    {{% imgOld 21 %}}

4.  Select the starting day and click **Next**, then select the starting
    time and click **OK**.  
    
    {{% imgOld 22 %}}
    
    {{% imgOld 23 %}}

5.  Verify the selections were made correctly.

6.  Click **Save As New**.
    
    {{% imgOld 24 %}}

7.  Enter a name for the validation.
    
    {{% imgOld 25 %}}

8.  Click **Save**.

The validation can now be selected from the Validation listing (on the
small, medium or large size card) and run immediately using **Run Now**,
or you can wait for it to run the first time according to the schedule
you specified. Refer to [View Scheduled Validation
Results](#src-12321612_ValidateNetworkProtocolandServiceOperations-SchValRes).

{{% imgOld 26 %}}

{{% imgOld 27 %}}

### <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jRWRpdFNjaGVkVmFs" class="confluence-anchor-link"></span>Modify an Existing Scheduled Validation Request</span>

At some point you might want to change the schedule or validation types
that are specified in a scheduled validation request.

{{%notice info%}}

When you update a scheduled request, the results for all future runs of
the validation will be different than the results of previous runs of
the validation.

{{%/notice%}}

To modify a scheduled validation:

1.  Open the large Validation Request card.

2.  Select the validation from the **Validation** dropdown list.

3.  Edit the schedule or validation types.

4.  Click **Update**.

The validation can now be selected from the Validation listing (on the
small, medium or large size card) and run immediately using **Run Now**,
or you can wait for it to run the first time according to the schedule
you specified. Refer to [View Scheduled Validation
Results](#src-12321612_ValidateNetworkProtocolandServiceOperations-SchValRes).

## <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jT25kVmFsUmVz" class="confluence-anchor-link"></span>View On-demand Validation Results</span>

The On-demand Validation Result card workflow enables you to view the
results of on-demand validation requests. When a request has started
processing, the associated medium Validation Result card is displayed on
your workbench. When multiple network protocols or services are included
in a validation, a validation result card is opened for each protocol
and service.

### On-Demand Validation Result Card Workflow</span>

The small Validation Result card displays:

{{% imgOld 28 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 29 %}}</p></td>
<td><p>Indicates an on-demand validation result</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>On-demand Result &lt;Network Protocol or Service Name&gt; Validation</p></td>
</tr>
<tr class="odd">
<td><p>Timestamp</p></td>
<td><p>Date and time the validation was completed</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 30 %}}</p>
,
<p>{{% imgOld 31 %}}</p></td>
<td><p>Status of the validation job, where:</p>
<ul>
<li><p><strong>Good</strong>: Job ran successfully. One or more warnings may have occurred during the run.</p></li>
<li><p><strong>Failed</strong>: Job encountered errors which prevented the job from completing, or job ran successfully, but errors occurred during the run.</p></li>
</ul></td>
</tr>
</tbody>
</table>

The medium Validation Result card displays:

{{% imgOld 32 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 33 %}}</p></td>
<td><p>Indicates an on-demand validation result</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>On-demand Validation Result | &lt;Network Protocol or Service Name&gt;</p></td>
</tr>
<tr class="odd">
<td><p>Timestamp</p></td>
<td><p>Date and time the validation was completed</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 34 %}}</p>
, <span style="color: #222222;"> </span>
<p>{{% imgOld 35 %}}</p>
, </span>
<p>{{% imgOld 36 %}}</p></td>
<td><p>Status of the validation job, where:</p>
<ul>
<li><p><strong>Good</strong>: Job ran successfully.</p></li>
<li><p><strong>Warning</strong>: Job encountered issues, but it did complete its run.</p></li>
<li><p><strong>Failed</strong>: Job encountered errors which prevented the job from completing.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Devices Tested</p></td>
<td><p>Chart with the total number of devices included in the validation and the distribution of the results.</p>
<ul>
<li><p><strong>Pass</strong>: Number of devices tested that had successful results</p></li>
<li><p><strong>Warn</strong>: Number of devices tested that had successful results, but also had at least one warning event</p></li>
<li><p><strong>Fail</strong>: Number of devices tested that had one or more protocol or service failures</p></li>
</ul>
<p>Hover over chart to view the number of devices and the percentage of all tested devices for each result category.</p></td>
</tr>
<tr class="even">
<td><p>Sessions Tested</p></td>
<td><p>For BGP, chart with total number of BGP sessions included in the validation and the distribution of the overall results.</p>
<p>For EVPN, chart with total number of BGP sessions included in the validation and the distribution of the overall results.</p>
<p>For Interfaces, chart with total number of ports included in the validation and the distribution of the overall results.</p>
<p>In each of these charts:</p>
<ul>
<li><p><strong>Pass</strong>: Number of sessions or ports tested that had successful results</p></li>
<li><p><strong>Warn</strong>: Number of sessions or ports tested that had successful results, but also had at least one warning event</p></li>
<li><p><strong>Fail</strong>: Number of sessions or ports tested that had one or more failure events</p></li>
</ul>
<p>Hover over chart to view the number of devices, sessions, or ports and the percentage of all tested devices, sessions, or ports for each result category.</p>
<p>This chart does not apply to other Network Protocols and Services, and thus is not displayed for those cards.</p></td>
</tr>
<tr class="odd">
<td><p>Open &lt;Network Protocol or Service Name&gt; Service Card</p></td>
<td><p>Click to open the corresponding medium Network Services card, where available. Refer to <a href="/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-the-Network/Monitor-Network-Protocols-and-Services/">Monitor Network Protocols and Services</a> for details about these cards and workflows.</p></td>
</tr>
</tbody>
</table>

The large Validation Result card contains two tabs.

The *Summary* tab displays:

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 38 %}}</p></td>
<td><p>Indicates an on-demand validation result</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>On-demand Validation Result | Summary | &lt;Network Protocol or Service Name&gt;</p></td>
</tr>
<tr class="odd">
<td><p>Date</p></td>
<td><p>Day and time when the validation completed</p></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 39 %}}</p>
, <span style="color: #222222;"> </span>
<p>{{% imgOld 40 %}}</p>
, </span>
<p>{{% imgOld 41 %}}</p></td>
<td><p>Status of the validation job, where:</p>
<ul>
<li><p><strong>Good</strong>: Job ran successfully.</p></li>
<li><p><strong>Warning</strong>: Job encountered issues, but it did complete its run.</p></li>
<li><p><strong>Failed</strong>: Job encountered errors which prevented the job from completing.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Devices Tested</p></td>
<td><p>Chart with the total number of devices included in the validation and the distribution of the results.</p>
<ul>
<li><p><strong>Pass</strong>: Number of devices tested that had successful results</p></li>
<li><p><strong>Warn</strong>: Number of devices tested that had successful results, but also had at least one warning event</p></li>
<li><p><strong>Fail</strong>: Number of devices tested that had one or more protocol or service failures</p></li>
</ul>
<p>Hover over chart to view the number of devices and the percentage of all tested devices for each result category.</p></td>
</tr>
<tr class="even">
<td><p>Sessions Tested</p></td>
<td><p>For BGP, chart with total number of BGP sessions included in the validation and the distribution of the overall results.</p>
<p>For EVPN, chart with total number of BGP sessions included in the validation and the distribution of the overall results.</p>
<p>For Interfaces, chart with total number of ports included in the validation and the distribution of the overall results.</p>
<p>For OSPF, chart with total number of OSPF sessions included in the validation and the distribution of the overall results.</p>
<p>In each of these charts:</p>
<ul>
<li><p><strong>Pass</strong>: Number of sessions or ports tested that had successful results</p></li>
<li><p><strong>Warn</strong>: Number of sessions or ports tested that had successful results, but also had at least one warning event</p></li>
<li><p><strong>Fail</strong>: Number of sessions or ports tested that had one or more failure events</p></li>
</ul>
<p>Hover over chart to view the number of devices, sessions, or ports and the percentage of all tested devices, sessions, or ports for each result category.</p>
<p>This chart does not apply to other Network Protocols and Services, and thus is not displayed for those cards.</p></td>
</tr>
<tr class="odd">
<td><p>Open &lt;Network Protocol or Service Name&gt; Service Card</p></td>
<td><p>Click to open the corresponding medium Network Services card, when available. Refer to <a href="/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-the-Network/Monitor-Network-Protocols-and-Services/">Monitor Network Protocols and Services</a> for details about these cards and workflows.</p></td>
</tr>
<tr class="even">
<td><p>Table/Filter options</p></td>
<td><p>When the <strong>Most Active</strong> filter option is selected, the table displays switches and hosts running the given service or protocol in decreasing order of alarm counts—devices with the largest number of warnings and failures are listed first.</p>
<p>When the <strong>Most Recent</strong> filter option is selected, the table displays switches and hosts running the given service or protocol sorted by <strong>timestamp</strong>, with the device with the most recent warning or failure listed first. The table provides the following additional information:</p>
<ul>
<li><p><strong>Hostname</strong>: User-defined name for switch or host</p></li>
<li><p><strong>Message Type</strong>: Network protocol or service which triggered the event</p></li>
<li><p><strong>Message</strong>: Short description of the event</p></li>
<li><p><strong>Severity</strong>: Indication of importance of event; values in decreasing severity include critical, warning, error, info, debug</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Show All Devices</p></td>
<td><p>Click to open the full screen card with all on-demand validation results sorted by timestamp.</p></td>
</tr>
</tbody>
</table>

The *Configuration* tab displays:

{{% imgOld 42 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 43 %}}</p></td>
<td><p>Indicates an on-demand validation request configuration</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>On-demand Validation Result | Configuration | &lt;Network Protocol or Service Name&gt;</p></td>
</tr>
<tr class="odd">
<td><p>Validations</p></td>
<td><p>List of network protocols or services included in the request that produced these results</p></td>
</tr>
<tr class="even">
<td><p>Schedule</p></td>
<td><p>Not relevant to on-demand validation results. Value is always N/A.</p></td>
</tr>
</tbody>
</table>

The full screen Validation Result card provides a tab for all on-demand
validation results.

{{% imgOld 44 %}}

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
<td><p>Validation Results | On-demand</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 45 %}}</p></td>
<td><p>Closes full screen card and returns to workbench</p></td>
</tr>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes; select an alternate time period by clicking <span style="color: #353744;"> </span></p>
<p>{{% imgOld 46 %}}</p></td>
</tr>
<tr class="even">
<td><p>Results</p></td>
<td><p>Number of results found for the selected tab</p></td>
</tr>
<tr class="odd">
<td><p>On-demand Validation Result | &lt;network protocol or service&gt;</p></td>
<td><p>Displays all unscheduled validation results. By default, the results list is sorted by <strong>Timestamp</strong>. This tab provides the following additional data about each result:</p>
<ul>
<li><p><strong>Validation Label</strong>: Does not apply to on-demand validation results and can be ignored</p></li>
<li><p><strong>Job ID</strong>: Internal identifier of the validation job that produced the given results</p></li>
<li><p><strong>Timestamp</strong>: Date and time the validation completed</p></li>
<li><p><strong>Type</strong>: Network protocol or service type</p></li>
<li><p><strong>Total Node Count</strong>: Total number of nodes running the given network protocol or service</p></li>
<li><p><strong>Checked Node Count</strong>: Number of nodes on which the validation ran</p></li>
<li><p><strong>Failed Node Count</strong>: Number of checked nodes that had protocol or service failures</p></li>
<li><p><strong>Rotten Node Count</strong>: Number of nodes that could not be reached during the validation</p></li>
<li><p><strong>Unknown Node Count</strong>: Applies only to the Interfaces service. Number of nodes with unknown port states.</p></li>
<li><p><strong>Total Session Count</strong>: Total number of sessions running for the given network protocol or service</p></li>
<li><p><strong>Failed Session Count</strong>: Number of sessions that had session failures</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Export</p></td>
<td><p>Enables export of all or selected items in a CSV or JSON formatted file</p></td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 47 %}}</p></td>
<td><p>Enables manipulation of table display; choose columns to display and reorder columns</p></td>
</tr>
</tbody>
</table>

### View On-demand Validation Results</span>

Once an on-demand validation request has completed, the results are
available in the corresponding Validation Result card.

{{%notice tip%}}

It may take a few minutes for all results to be presented if the load on
the NetQ Platform is heavy at the time of the run.

{{%/notice%}}

To view the results:

1.  Locate the medium on-demand Validation Result card on your workbench
    for the protocol or service that was run.  
    You can identify it by the on-demand result icon,
    <span style="color: #353744;"> </span>
    
    {{% imgOld 48 %}}
    
    , protocol or service name, and the date and time that it was run.  
    **Note:** You may have more than one card open for a given protocol
    or service, so be sure to use the date and time on the card to
    ensure you are viewing the correct card.  
    
    {{% imgOld 49 %}}
    
    {{% imgOld 50 %}}

2.  Note the total number and distribution of results for the tested
    devices and sessions (when appropriate). Are there many failures?

3.  Hover over the charts to view the total number of warnings or
    failures and what percentage of the total results that represents
    for both devices and sessions.

4.  Switch to the large on-demand Validation Result card.
    
    {{% imgOld 51 %}}

5.  If there are a large number of device warnings or failures, view the
    devices with the most issues in the table on the right. By default,
    this table displays the **Most Active** devices.

6.  To view the most recent issues, select **Most Recent** from the
    filter above the table.

7.  If there are a large number of devices or sessions with warnings or
    failures, the protocol or service may be experiencing issues. View
    the health of the protocol or service as a whole by clicking
    **Open** \<*network service*\> **Card** when available.

8.  To view all data available for all on-demand validation results for
    a given protocol, switch to the full screen card.
    
    {{% imgOld 52 %}}
    
    You may find that comparing various results gives you a clue as to
    why certain devices are experiencing more warnings or failures. For
    example, more failures occurred between certain times or on a
    particular device.

## <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jU2NoVmFsUmVz" class="confluence-anchor-link"></span>View Scheduled Validation Results</span>

The Scheduled Validation Result card workflow enables you to view the
results of scheduled validation requests. When a request has completed
processing, you can access the Validation Result card from the full
screen Validation Request card. Each protocol and service has its own
validation result card, but the content is similar on each.

### Scheduled Validation Result Card Workflow Summary</span>

The small Validation Result card displays:

{{% imgOld 53 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 54 %}}</p></td>
<td><p>Indicates a scheduled validation result</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Scheduled Result &lt;Network Protocol or Service Name&gt; Validation</p></td>
</tr>
<tr class="odd">
<td><p>Results</p></td>
<td><p>Summary of validation results:</p>
<ul>
<li><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 55 %}}</p>
<p>Number of validation runs completed in the designated time period</p></li>
<li><p><span style="color: #353744;"> <span style="color: #222222;"> </span></span></p>
<p>{{% imgOld 56 %}}</p>
<p>Number of runs with warnings</p></li>
<li><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 57 %}}</p>
<p>Number of runs with errors</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 58 %}}</p>
,
<p>{{% imgOld 59 %}}</p></td>
<td><p>Status of the validation job, where:</p>
<ul>
<li><p><strong>Pass</strong>: Job ran successfully. One or more warnings may have occurred during the run.</p></li>
<li><p><strong>Failed</strong>: Job encountered errors which prevented the job from completing, or job ran successfully, but errors occurred during the run.</p></li>
</ul></td>
</tr>
</tbody>
</table>

The medium Validation Result card displays:

{{% imgOld 60 %}}

{{% imgOld 61 %}}

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
<p>{{% imgOld 62 %}}</p></td>
<td><p>Indicates a scheduled validation result</p></td>
</tr>
<tr class="odd">
<td><p>Title</p></td>
<td><p>Scheduled Validation Result | &lt;Network Protocol or Service Name&gt;</p></td>
</tr>
<tr class="even">
<td><p>Summary</p></td>
<td><p>Summary of validation results:</p>
<ul>
<li><p>Name of scheduled validation</p></li>
<li><p>Status of the validation job, where:</p>
<ul>
<li><p><strong></strong></p>
<p><strong>{{% imgOld 63 %}}</strong></p>
<p><strong>Pass</strong>: Job ran successfully. One or more warnings may have occurred during the run.</p></li>
<li><p><strong></strong></p>
<p><strong>{{% imgOld 64 %}}</strong></p>
<p><strong>Failed</strong>: Job encountered errors which prevented the job from completing, or job ran successfully, but errors occurred during the run.</p></li>
</ul></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Chart</p></td>
<td><p>Validation results, where:</p>
<ul>
<li><p><strong>Time period:</strong> Range of time in which the data on the heat map was collected</p></li>
<li><p><strong>Heat map: A time segmented view of the results</strong>. For each time segment, the color represents the percentage of warning, passing, and failed results. Refer to <a href="#src-12321612_ValidateNetworkProtocolandServiceOperations-DataGran">Granularity of Data Shown Based on Time Period</a> for details on how to interpret the results.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Open &lt;Network Protocol or Service Name&gt; Service Card</p></td>
<td><p>Click to open the corresponding medium Network Services card, when available. Refer to <a href="/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-the-Network/Monitor-Network-Protocols-and-Services/">Monitor Network Protocols and Services</a> for details about these cards and workflows.</p></td>
</tr>
</tbody>
</table>

The large Validation Result card contains two tabs.

  - The *Summary* tab displays:

  - {{% imgOld 65 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 66 %}}</p></td>
<td><p>Indicates a scheduled validation result</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Validation Summary (Scheduled Validation Result | &lt;Network Protocol or Service Name&gt;)</p></td>
</tr>
<tr class="odd">
<td><p>Summary</p></td>
<td><p>Summary of validation results:</p>
<ul>
<li><p>Name of scheduled validation</p></li>
<li><p>Status of the validation job, where:</p>
<ul>
<li><p><strong></strong></p>
<p><strong>{{% imgOld 67 %}}</strong></p>
<p><strong>Pass</strong>: Job ran successfully. One or more warnings may have occurred during the run.</p></li>
<li><p><strong></strong></p>
<p><strong>{{% imgOld 68 %}}</strong></p>
<p><strong>Failed</strong>: Job encountered errors which prevented the job from completing, or job ran successfully, but errors occurred during the run.</p></li>
</ul></li>
<li><p>{{% imgOld 69 %}}</p>
<p>: Expand the heat map to full width of card, collapse the heat map to the left</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Chart</p></td>
<td><p>Validation results, where:</p>
<ul>
<li><p><strong>Time period:</strong> Range of time in which the data on the heat map was collected</p></li>
<li><p><strong>Heat map: A time segmented view of the results</strong>. For each time segment, the color represents the percentage of warning, passing, and failed results. Refer to <a href="#src-12321612_ValidateNetworkProtocolandServiceOperations-DataGran">Granularity of Data Shown Based on Time Period</a> for details on how to interpret the results.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Open &lt;Network Protocol or Service Name&gt; Service Card</p></td>
<td><p>Click to open the corresponding medium Network Services card, when available. Refer to <a href="/version/cumulus-netq-21/Cumulus-NetQ-UI-User-Guide/Monitor-the-Network/Monitor-Network-Protocols-and-Services/">Monitor Network Protocols and Services</a> for details about these cards and workflows.</p></td>
</tr>
<tr class="even">
<td><p>Table/Filter options</p></td>
<td><p>When the <strong>Most Active</strong> filter option is selected, the table displays switches and hosts running the given service or protocol in decreasing order of alarm counts—devices with the largest number of warnings and failures are listed first.</p>
<p>When the <strong>Most Recent</strong> filter option is selected, the table displays switches and hosts running the given service or protocol sorted by <strong>timestamp</strong>, with the device with the most recent warning or failure listed first. The table provides the following additional information:</p>
<ul>
<li><p><strong>Hostname</strong>: User-defined name for switch or host</p></li>
<li><p><strong>Message Type</strong>: Network protocol or service which triggered the event</p></li>
<li><p><strong>Message</strong>: Short description of the event</p></li>
<li><p><strong>Severity</strong>: Indication of importance of event; values in decreasing severity include critical, warning, error, info, debug</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Show All Results</p></td>
<td><p>Click to open the full screen card with all scheduled validation results sorted by timestamp.</p></td>
</tr>
</tbody>
</table>

The *Configuration* tab displays:

{{% imgOld 70 %}}

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
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 71 %}}</p></td>
<td><p>Indicates a scheduled validation configuration</p></td>
</tr>
<tr class="even">
<td><p>Title</p></td>
<td><p>Configuration (Scheduled Validation Result | &lt;Network Protocol or Service Name&gt;)</p></td>
</tr>
<tr class="odd">
<td><p>Name</p></td>
<td><p>User-defined name for this scheduled validation</p></td>
</tr>
<tr class="even">
<td><p>Validations</p></td>
<td><p>List of validations included in the validation request that created this result</p></td>
</tr>
<tr class="odd">
<td><p>Schedule</p></td>
<td><p>User-defined schedule for the validation request that created this result</p></td>
</tr>
<tr class="even">
<td><p>Edit Config</p></td>
<td><p>Opens the large Validation Request card for editing this configuration</p></td>
</tr>
</tbody>
</table>

The full screen Validation Result card provides tabs for all scheduled
validation results for the service.

{{% imgOld 72 %}}

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
<td><p>Scheduled Validation Results | &lt;Network Protocol or Service&gt;</p></td>
</tr>
<tr class="even">
<td><p>{{% imgOld 73 %}}</p></td>
<td><p>Closes full screen card and returns to workbench</p></td>
</tr>
<tr class="odd">
<td><p>Time period</p></td>
<td><p>Range of time in which the displayed data was collected; applies to all card sizes; select an alternate time period by clicking <span style="color: #353744;"> </span></p>
<p>{{% imgOld 74 %}}</p></td>
</tr>
<tr class="even">
<td><p>Results</p></td>
<td><p>Number of results found for the selected tab</p></td>
</tr>
<tr class="odd">
<td><p>Scheduled Validation Result | &lt;network protocol or service&gt;</p></td>
<td><p>Displays all unscheduled validation results. By default, the results list is sorted by timestamp. This tab provides the following additional data about each result:</p>
<ul>
<li><p><strong>Validation Label</strong>: Does not apply to on-demand validation results and can be ignored</p></li>
<li><p><strong>Job ID</strong>: Internal identifier of the validation job that produced the given results</p></li>
<li><p><strong>Timestamp</strong>: Date and time the validation completed</p></li>
<li><p><strong>Type:</strong> Protocol of Service Name</p></li>
<li><p><strong>Total Node Count</strong>: Total number of nodes running the given network protocol or service</p></li>
<li><p><strong>Checked Node Count</strong>: Number of nodes on which the validation ran</p></li>
<li><p><strong>Failed Node Count</strong>: Number of checked nodes that had protocol or service failures</p></li>
<li><p><strong>Rotten Node Count</strong>: Number of nodes that could not be reached during the validation</p></li>
<li><p><strong>Unknown Node Count</strong>: Applies only to the Interfaces service. Number of nodes with unknown port states.</p></li>
<li><p><strong>Total Session Count</strong>: Total number of sessions running for the given network protocol or service</p></li>
<li><p><strong>Failed Session Count</strong>: Number of sessions that had session failures</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Export</p></td>
<td><p>Enables export of all or selected items in a CSV or JSON formatted file</p></td>
</tr>
<tr class="odd">
<td><p><span style="color: #353744;"> </span></p>
<p>{{% imgOld 75 %}}</p></td>
<td><p>Enables manipulation of table display; choose columns to display and reorder columns</p></td>
</tr>
</tbody>
</table>

### <span id="src-12321612_safe-id-VmFsaWRhdGVOZXR3b3JrUHJvdG9jb2xhbmRTZXJ2aWNlT3BlcmF0aW9ucy0jRGF0YUdyYW4" class="confluence-anchor-link"></span>Granularity of Data Shown Based on Time Period</span>

On the medium and large Validation Result cards, the status of the runs
is represented in heat maps stacked vertically; one for passing runs,
one for runs with warnings, and one for runs with failures. Depending on
the time period of data on the card, the number of smaller time blocks
used to indicate the status varies. A vertical stack of time blocks, one
from each map, includes the results from all checks during that time.
The results are shown by how saturated the color is for each block. If
all validations during that time period pass, then the middle block is
100% saturated (white) and the warning and failure blocks are zero %
saturated (gray). As warnings and errors increase in saturation, the
passing block is proportionally reduced in saturation. An example heat
map for a time period of 24 hours is shown here with the most common
time periods in the table showing the resulting time blocks and regions.

{{% imgOld 76 %}}

| Time Period | Number of Runs | Number Time Blocks | Amount of Time in Each Block |
| ----------- | -------------- | ------------------ | ---------------------------- |
| 6 hours     | 18             | 6                  | 1 hour                       |
| 12 hours    | 36             | 12                 | 1 hour                       |
| 24 hours    | 72             | 24                 | 1 hour                       |
| 1 week      | 504            | 7                  | 1 day                        |
| 1 month     | 2,086          | 30                 | 1 day                        |
| 1 quarter   | 7,000          | 13                 | 1 week                       |

### View Scheduled Validation Results</span>

Once a scheduled validation request has completed, the results are
available in the corresponding Validation Result card.

To view the results:

1.  Open the full size Validation Request card to view all scheduled
    validations.
    
    {{% imgOld 77 %}}

2.  Select the validation results you want to view by clicking in the
    first column of the result and clicking the check box.

3.  On the Edit Menu that appears at the bottom of the window, click
    <span style="color: #353744;"> </span>
    
    {{% imgOld 78 %}}
    
    (Open Cards). This opens the medium Scheduled Validation Results
    card(s) for the selected items.  
    
    {{% imgOld 79 %}}
    
    {{% imgOld 80 %}}

4.  Note the distribution of results. Are there many failures? Are they
    concentrated together in time? Has the protocol or service recovered
    after the failures?

5.  Hover over the heat maps to view the status numbers and what
    percentage of the total results that represents for a given region.
    The tooltip also shows the number of devices included in the
    validation and the number with warnings and/or failures. This is
    useful when you see the failures occurring on a small set of
    devices, as it might point to an issue with the devices rather than
    the network service.
    
    {{% imgOld 81 %}}

6.  Optionally, click **Open***\<network service\>***Card** to open the
    medium individual Network Services card. Your current card is not
    closed.

7.  Switch to the large Scheduled Validation card.

8.  Click
    
    {{% imgOld 82 %}}
    
    to expand the chart.
    
    {{% imgOld 83 %}}

9.  Collapse the heat map by clicking
    
    {{% imgOld 84 %}}
    
    .
    
    {{% imgOld 85 %}}

10. If there are a large number of warnings or failures, view the
    devices with the most issues by clicking **Most Active** in the
    filter above the table. This might help narrow the failures down to
    a particular device or small set of devices that you can investigate
    further.

11. Select the **Most Recent** filter above the table to see the events
    that have occurred in the near past at the top of the list.

12. Optionally, view the health of the protocol or service as a whole by
    clicking **Open** \<*network service*\> **Card** (when available).

13. You can view the configuration of the request that produced the
    results shown on this card workflow, by hovering over the card and
    clicking <span style="color: #333c4e;"> </span>
    
    {{% imgOld 86 %}}
    
    . If you want to change the configuration, click **Edit Config** to
    open the large Validation Request card, pre-populated with the
    current configuration. Follow the instructions in [Modify an
    Existing Scheduled Validation
    Request](#src-12321612_ValidateNetworkProtocolandServiceOperations-EditSchedVal)
    to make your changes.

14. To view all data available for all scheduled validation results for
    the given protocol or service, click **Show All Results** or switch
    to the full screen card.
    
    {{% imgOld 87 %}}

15. Look for changes and patterns in the results. Scroll to the right.
    Are there more failed sessions or nodes during one or more
    validations?

16. Return to the full screen Validation Results card to view another
    Scheduled Validation Result.

<article id="html-search-results" class="ht-content" style="display: none;">

</article>

<footer id="ht-footer">

</footer>
