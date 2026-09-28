# Lab 6 – Webex MCP Servers

In this section, you will use **Webex MCP Servers** to work with Webex through an AI assistant. Instead of writing code, you will describe what you want in plain language, and the assistant will call the Webex APIs for you.

Upon completion of this section, you will be able to:

1. Explain what MCP is and how the Webex MCP Servers expose Webex to AI assistants.
2. Use the Webex Messaging MCP Server to work with spaces and messages.
3. Use the Webex Meetings MCP Server to find and schedule meetings.
4. Combine both servers in a single request.
5. Compare MCP with the bots and Service Apps you built in the previous labs.

### Prerequisites

You will quickly notice in the documentation that every official Webex MCP server includes this requirement:

!!! Warning "Prerequisites"
    This MCP server must be enabled by your organization's admin in Webex Control Hub before it can be used. See [Provisioning on Control Hub](https://developer.webex.com/mcp/docs/provisioning-on-control-hub){:target="_blank"} for details.

MCP servers are **NOT** enabled by default in your organization. An administrator needs to enable them before your users can use them.

??? Note "Reference: How to enable MCP servers in your organization"
    These steps have been completed before this lab, since you are all sharing the same organization, but they are relevant for your own organizations. The presenters will demonstrate this process.

    1. If you try to access MCP for the first time, you will see the message **No allowed MCP servers found**.

        ![No allowed MCP servers found](./assets/token_4.png){ width="450" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;"}

    2. To enable them, go to **Collaboration Control Hub** -> **Apps** -> **Agentic Apps** and select the **Webex** tab:

        ![Control Hub Agentic Apps](./assets/controlhub_1.png){ style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;"}

    3. To enable any of them, click **Allowed for all users** and save:

        ![Control Hub allow MCP server](./assets/controlhub_2.png){ style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;"}

    4. You are not done yet. If you try to use an MCP server now, your MCP client will show an error similar to this one:

        ```terminal
        2026-09-14 12:21:38.230 [warning] [server stderr] [63062] Fatal error: SdkHttpError: Error POSTing to endpoint: {"id":0,"jsonrpc":"2.0","error":{"code":-32003,"message":"You don't have access to this MCP server yet. Ask your administrator to enable it for your account or organization.","data":{"reason":"ACCESS_DENIED"}}}
        2026-09-14 12:21:38.230 [warning] [server stderr]     at StreamableHTTPClientTransport._send (XXXX/.npm/_npx/705d23756ff7dacc/node_modules/mcp-remote/dist/chunk-EFMRUNOV.js:31464:15)
        2026-09-14 12:21:38.230 [warning] [server stderr]     at process.processTicksAndRejections (node:internal/process/task_queues:105:5) {
        2026-09-14 12:21:38.231 [warning] [server stderr]   code: 'CLIENT_HTTP_NOT_IMPLEMENTED',
        2026-09-14 12:21:38.231 [warning] [server stderr]   data: {
        2026-09-14 12:21:38.231 [warning] [server stderr]     status: 403,
        2026-09-14 12:21:38.231 [warning] [server stderr]     statusText: 'Forbidden',
        2026-09-14 12:21:38.231 [warning] [server stderr]     text: `{"id":0,"jsonrpc":"2.0","error":{"code":-32003,"message":"You don't have access to this MCP server yet. Ask your administrator to enable it for your account or organization.","data":{"reason":"ACCESS_DENIED"}}}`
        ```

        You also need to allow specific tools for each MCP server.

    5. Go to the MCP server, select the **Tools** tab, and enable the tools you want your users to use. In this case, all of them are enabled:

        ![Control Hub MCP tools](./assets/controlhub_3.png){ style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;"}

        After this change, users will be able to use the server.

## Step 6.1: Understand MCP and Webex MCP Servers

The **Model Context Protocol (MCP)** is an open standard that lets AI assistants use external tools. It has three parts:

- **MCP client:** the AI assistant you talk to. In this lab, it runs in the Webex MCP Lab website.
- **MCP server:** a service that offers a set of **tools** the assistant can call. Webex provides a **Messaging** server (spaces, messages, memberships) and a **Meetings** server (meetings, schedules, join links).
- **Tools:** single actions, such as *list spaces* or *create meeting*. Each tool calls the same Webex REST APIs you used in the previous labs.

When you send a request, the assistant decides which tools to call, calls them, and uses the results to answer you.

Webex MCP Servers act **on behalf of the signed-in user**. This is the main difference with what you built before:

| | Acts as | Permissions |
|---|---|---|
| **Bot** (Lab 3) | The bot itself | Only the spaces the bot is in |
| **Service App** (Lab 4) | The organization | Admin scopes approved in Control Hub |
| **Webex MCP Server** (Lab 6) | You | What you can do in Webex, limited to the tools your admin allowed in Control Hub |

!!! Warning
    The assistant can send messages and schedule meetings as you. Read what it plans to do before you confirm any action that creates, changes, or deletes something.

## Step 6.2: Open the Webex MCP Lab

The Webex MCP Lab website is already connected to the Webex MCP Servers, so you do not need any access token, configuration, or VS Code for this section.

1. Open the [Webex MCP Lab](https://mcp-lab.webexdevs.com/){:target="_blank"} in your browser.

2. Sign in with your lab user (for example, `pod0@webexone-developer.wbx.ai`).

3. Check that the **Webex Messaging** and **Webex Meetings** servers are connected.

4. Ask the assistant what it can do:

    ```text
    Which Webex tools can you use? Group them by server and describe each one in one sentence.
    ```

    You will see the tools available in each server. Take a moment to notice how many of them map to the APIs you have already used: rooms, messages, memberships, and meetings.

## Step 6.3: Webex Messaging

In this step, you will repeat some of the actions from the previous labs, this time without code.

1. List your spaces, just like you did with the Rooms API in Lab 3:

    ```text
    List my 5 most recently active Webex spaces with their titles.
    ```

2. Look at the space your bot created in Step 3.3:

    ```text
    Who are the members of the space "WebexOne Room"?
    ```

    You should see yourself and your bot. If you joined the space again in Lab 5, Exercise 1, this is the same membership the bot created for you.

3. Read a conversation. Ask about your 1:1 space with your bot:

    ```text
    Summarize the last 10 messages in my 1:1 space with my bot WebexOne-USERNAME.
    ```

    The assistant reads the messages as you, so it can see the echo, commands, and cards you tested in Lab 3.

4. Send a message as yourself:

    ```text
    Send a message to the space "WebexOne Room" saying "Hello from the Webex MCP Lab!" in bold.
    ```

    Open Webex and check the space. Notice that the message was sent by **you**, not by your bot.

5. Create a space and invite your bot:

    ```text
    Create a space called "WebexOne MCP Space", add my bot WebexOne-USERNAME to it, and post a welcome message explaining that this space was created with MCP.
    ```

    This single request needs several tools: create the space, add the member, and send the message. Check which tools the assistant used and in what order.

## Step 6.4: Webex Meetings

In Lab 4, your Service App scheduled a meeting on your behalf. Now you will find and schedule meetings as yourself.

1. Find the meeting your Service App created:

    ```text
    List my upcoming Webex meetings for the next 7 days, with title, start time, and join link.
    ```

    You should see the meeting scheduled in Lab 4, with the title you chose in Step 4.4.

2. Get the details of that meeting:

    ```text
    Show me the meeting number and host of the meeting I scheduled with my Service App.
    ```

3. Schedule a new meeting:

    ```text
    Schedule a 30-minute Webex meeting tomorrow at 10:00 in my time zone, called "WebexOne MCP Meeting".
    ```

    !!! Tip
        Always mention the time zone. Without it, the meeting may be scheduled in UTC, just like the Service App script in Lab 4.

4. Open Webex and check that the meeting appears in your calendar.

## Step 6.5: Combine Messaging and Meetings

The real power of MCP is combining tools from different servers in a single request, something that would need several API calls and some code to glue them together.

1. Share your meeting with a space:

    ```text
    Post the join link of "WebexOne MCP Meeting" in the space "WebexOne MCP Space", with a short message inviting everyone to join.
    ```

2. Create a meeting from a conversation:

    ```text
    Read the last messages in "WebexOne MCP Space", schedule a 15-minute follow-up meeting for tomorrow at 11:00 in my time zone, and post the join link in the same space.
    ```

3. Clean up what you created in this lab:

    ```text
    Delete the meetings "WebexOne MCP Meeting" and the follow-up meeting you just created.
    ```

    The assistant should ask you to confirm before deleting. Check that it lists exactly the meetings you expect.

## Step 6.6: Explore the limits

MCP tools can only do what the signed-in user can do. Try these requests and compare the results with what you built in the previous labs:

1. Ask for something that required a Service App in Lab 5:

    ```text
    List all the users in my organization.
    ```

    Your lab user is not an administrator, so the assistant cannot list everyone like the feedback bot did with the `spark-admin:people_read` scope.

2. Ask for something that required a bot:

    ```text
    Reply automatically "Echo:" to every message I receive from now on.
    ```

    MCP tools run only when you ask the assistant. To react to events in real time, you still need a bot listening over WebSockets, like the one you built in Step 3.5.

!!! Info
    **When to use each approach**

    - **Bot:** always-on assistants that react to messages and cards, for any user.
    - **Service App:** organization-wide automation with admin permissions and no user signed in.
    - **Webex MCP Server:** on-demand tasks for a single user, described in plain language.
