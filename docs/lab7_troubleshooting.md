# 6 – Troubleshooting

During this section, you will be presented with different exercises to apply the concepts you learn and use different Webex APIs to get relevant information for troubleshooting.

You can use any of the tools—either Postman, Webex for Developers, or Python scripts—to resolve them.

You will also be presented with the steps for resolution.

Upon completion of this section, you will be able to:

1. Check general Webex Status
2. Check different Audit events
3. Create reports and download them
4. Check Events as a Compliance Officer
5. Get Call History and statistics
6. Check Meeting statistics

Reference:

<https://developer.webex.com/>

## Exercise 1: Webex Status API

In this exercise, you will use the Webex Status API to retrieve and interpret key information about the current operational status of Webex services.

This API can be useful for quickly identifying service outages or degradations, troubleshooting reported issues, informing users about ongoing incidents, and deciding when to escalate or communicate about disruptions.

1. Get the general Webex status indicator.
2. Check if there are any current unresolved incidents.
3. Check if there is any ongoing maintenance.

References:

<https://developer.webex.com/calling/docs/webex-status-api>

<https://status.webex.com/api/>

### Solution

In this section, you do not need an access token to access the Webex Status API.

Similarly, you cannot access this information through the Webex Developer Portal, so you will need to create a query using either Postman or Python.

To get a full overview of available status endpoints and summary information, perform a GET request to:

<https://status.webex.com/index.json>

This endpoint provides a summary of the overall status, unresolved incidents, and both active and upcoming scheduled maintenances. It serves as a central resource for quickly accessing all key status information in one response.  
  
![](assets/docx-image-067.png)

1. Indicator  
     
   To get the status indicator, perform a GET request to:  
   <https://status.webex.com/status.json>  
     
   ![](assets/docx-image-068.png)  
     
   The possible values are green, yellow, and red, which indicate the overall status of Webex services:  
   * Green: All systems operational
   * Yellow: Degraded performance or partial outage
   * Red: Major outage

In this case, there are no issues reported.

1. Incidents  
   To check for any current unresolved incidents, perform a GET request to:

<https://status.webex.com/unresolved-incidents.json>  
  
This endpoint returns a list of incidents that are not yet resolved.  
  
![](assets/docx-image-069.png)  
  
In this case, there are no incidents reported at this time.

1. Active Maintenance:  
     
   To check for any ongoing maintenance activities, perform a GET request to:

<https://status.webex.com/active-scheduled-maintenances.json>

This endpoint returns a list of all currently active scheduled maintenances  
  
![](assets/docx-image-070.png)  
  
In this case, there are no active maintenance activities at this time.

## Exercise 2: Admin Audit Events

In this exercise, you need to access the Admin Audit Events to identify different types of events. If you run this with your current user, you will receive a "Forbidden" error.

You can use the following credentials for this activity:

|  |  |
| --- | --- |
| User | Username |
| esteele@cb127.dc-02.com | dCloud2856! |

During this exercise, you need to:

1. List all admin audit event categories available
2. Get all the logins from the last 5 days
3. Get all the organization configuration changes for the last 5 days

Reference:

<https://developer.webex.com/admin/docs/api/v1/admin-audit-events>

### Solution

1. List admin audit categories:  
     
   To list all available admin audit event categories, send a GET request to:  
   <https://webexapis.com/v1/adminAuditEventCategories>  
     
   ![](assets/docx-image-071.png)  
     
   This will give you an overview of all the types of admin activities you can monitor and query, helping you understand the full range of auditing possibilities available.
2. Get all the logins:  
     
   To retrieve all login events from the last 5 days, send a GET request to the admin audit events endpoint with the following parameters:
   * from: 2026-09-24T00:00:00.000Z
   * to: 2026-09-29T00:00:00.000Z
   * orgId: cf0be353-2dd4-463f-9843-4a3c27a9a9d3
   * eventCategories: LOGINS

![](assets/docx-image-072.png)

1. Organization changes  
     
   To retrieve all organization configuration change events from the last 5 days, send a GET request to the admin audit events endpoint with the following parameters:
   * from: 2026-09-24T00:00:00.000Z
   * to: 2026-09-29T00:00:00.000Z
   * orgId: cf0be353-2dd4-463f-9843-4a3c27a9a9d3
   * eventCategories: ORG_SETTINGS

![](assets/docx-image-073.png)

## Exercise 3: Reports

Create and check a report for **User Activity Summary**.

1. Use the Reports API to create a report with the type user_activity_summary.
2. Monitor the status of the report until it is complete.
3. Download and review the report once it’s ready.

Reference:  
<https://developer.webex.com/docs/api/v1/reports>

### Solution

To complete this activity, follow these four steps:

1. Get the report templates to identify the desired report.
2. Create the report.
3. Retrieve the download link for the report.
4. Download the report.

Let’s walk through each step:

1. First of all, you need to get the report templates to find the report and its Id:  
   <https://developer.webex.com/admin/docs/api/v1/report-templates/list-report-templates>

![](assets/docx-image-074.png)  
  
Look for the report “User Activity Summary”:  
  
![](assets/docx-image-075.png)  
  
It has the Id: **115**

1. Now that you have the report ID, you can generate the report.

The startDate and endDate fields are mandatory. For example, to generate the report for the whole month of September, use:

* + startDate: 2026-09-01
  + endDate: 2026-09-28

![](assets/docx-image-076.png)  
  
After submitting your request, you will receive the report ID in the response.  
  
![](assets/docx-image-077.png)

1. Using the previously created report ID, get the report details:  
   <https://developer.webex.com/admin/docs/api/v1/reports/get-report-details>

![](assets/docx-image-078.png)  
  
In the response, you will find the downloadURL for your report:  
  
![](assets/docx-image-079.png)

1. To download the report, you can use the "Send and Download" option in Postman with the downloadURL you received:

![](assets/docx-image-080.png)  
  
![](assets/docx-image-081.png)

## Exercise 4: Events

To access the Events API, you need to use a user account with the "Compliance Officer" role. If you do not have this role, you will receive the following error when trying to use the API:

* "Compliance role required to get events."

![](assets/docx-image-082.png)

You can use the following credentials for this activity:

|  |  |
| --- | --- |
| User | Username |
| aperez@cb127.dc-02.com | dCloud2856! |

You have to:

1. Use the Events API to retrieve events filtered by the resource memberships.

Reference:

<https://developer.webex.com/admin/docs/api/v1/events>

### Solution

You can list all the events from this API  
<https://developer.webex.com/admin/docs/api/v1/events/list-events>

![](assets/docx-image-083.png)

To track activity related to room memberships—such as when users join or leave rooms—filter by the resource memberships.

This will show all events involving changes to room participation.

![](assets/docx-image-084.png)

## Exercise 5: Calling Troubleshooting

In this exercise, you will work with the Webex Calling APIs to troubleshoot and review call activity.

You will need to use a user account with the “Webex Calling Detailed Call History API access” role:

![](assets/docx-image-085.png)  
  
You can use the following credentials for this activity:

|  |  |
| --- | --- |
| User | Username |
| esteele@cb127.dc-02.com | dCloud2856! |

References:

<https://developer.webex.com/blog/webex-detailed-call-history-api>

<https://developer.webex.com/blog/exploring-the-webex-calling-reports-and-analytics-apis>  
<https://developer.webex.com/calling/docs/api/v1/reports-detailed-call-history/get-detailed-call-history>

### Solution

Before checking the detailed call history, you will explore the following Calling APIs related to calls:

* List Calls: Retrieves details for all active calls associated with the user<https://developer.webex.com/calling/docs/api/v1/call-controls/list-calls>  
    
  ![](assets/docx-image-086.png)

You may not see any calls at the moment, but this is how the response will look:  
  
![](assets/docx-image-087.png)

* List Call History: Retrieves up to 20 call history records for the user.  
  <https://developer.webex.com/calling/docs/api/v1/call-controls/list-call-history>  
    
  ![](assets/docx-image-088.png)  
    
  This response won’t contain much detailed information about the calls.  
    
  ![](assets/docx-image-089.png)

These APIs provide only basic information and are limited to the user making the API call.

1. Detail Call history - Provides Webex Calling Detailed Call History data for your organization.  
   <https://developer.webex.com/calling/docs/api/v1/reports-detailed-call-history/get-detailed-call-history>  
     
   ![](assets/docx-image-090.png)

Use the following parameters for your request:

* + endTime: 2026-09-28T22:10:00.000Z
  + startTime: 2026-09-28T00:00:00.000Z

The result contains very detailed information about each call.

![](assets/docx-image-091.png)

The information provided by this call is equivalent to the information provided in CH:  
  
![](assets/docx-image-092.png)  
  
The response from the Webex Calling Detailed Call History API includes:

* + **Call direction**: Originating or terminating
  + **Call type:** For example, SIP_ENTERPRISE or SIP_NATIONAL
  + **Timestamps**: Start time, release time, and answer time
  + **User information**: Name, UUID, and user type
  + **Calling and called numbers**: Both internal and external numbers
  + **Device and client details:** Device MAC, OS type, client type, and version
  + **Call metrics**: Ring duration, call duration, and answer indicator
  + **Call outcome**: Success, refusal, or temporarily unavailable, with outcome reasons provided
  + **Session and network data**: Session IDs, correlation IDs, and location information

This detailed information helps you analyze user activity, troubleshoot call issues, trace call flows, and identify patterns or recurring problems within your organization.

## Exercise 6: Meetings Troubleshooting

In this exercise, you will use the Meetings APIs to retrieve meeting details and analyze meeting quality metrics.

You have to:

1. Get Meeting IDs for your organization’s meetings.
2. Retrieve meeting quality data for a specific meeting.

You can use the following credentials for this activity:

|  |  |
| --- | --- |
| User | Username |
| esteele@cb127.dc-02.com | dCloud2856! |

References:

<https://developer.webex.com/meeting/docs/api/v1/meetings/list-meetings>

<https://analytics.webexapis.com/v1/meeting/qualities?meetingId=><meetingId>

**![](assets/docx-image-093.png) Warning:** You need to use Postman to access Meeting Quality data.

### Solution

1. Get MeetingId:  
     
   Use the following endpoint to list meetings and obtain the Meeting ID:  
   <https://developer.webex.com/meeting/docs/api/v1/meetings/list-meetings>

Make sure to filter by meetingType set to "meeting".  
  
![](assets/docx-image-094.png)  
  
You will receive a list of meetings:  
  
![](assets/docx-image-095.png)  
  
Pick one and ensure the ID follows a format similar to:

* + 11556412d0114b1ab6e95f4de6b22f4b_I_678181572335116526

1. Get Meeting Qualities:  
     
   Retrieve quality metrics for a specific meeting using this endpoint:   
   <https://analytics.webexapis.com/v1/meeting/qualities?meetingId=><meetingId>  
     
   ![](assets/docx-image-096.png)  
     
   The response from the Meetings API includes detailed information for each meeting participant:
   * **Identification:** Meeting and participant IDs, user name, and email
   * **Timing:** Join and leave times, and time taken to join the meeting
   * **Client and Device:** Client type and version, OS type and version, hardware details, camera, microphone, and speakers used
   * **Network Info:** Network type, masked local and public IP addresses, server region
   * **Media Quality Metrics:** Audio, video, and content sharing statistics (packet loss, latency, jitter, frame rate, resolution, codec, bitrate, and transport type for both inbound and outbound streams)
   * **Resource Usage:** Average and maximum CPU usage during the meeting

These details help you analyze participant experience, troubleshoot meeting quality issues, and identify patterns related to devices, networks, or client versions.
