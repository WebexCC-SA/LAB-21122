# Lab 7 - Webex MCP Servers

Lab code repository: [https://github.com/diegomjimenez/WebexOne2026_Developer](https://github.com/diegomjimenez/WebexOne2026_Developer){:target="_blank"}

In this lab section, you will explore **Webex MCP Servers** and learn how they expose Webex capabilities to external AI clients through the Model Context Protocol (MCP).

## Learning Objectives

Upon completion of this section, you will be able to:

- Understand the role of MCP Servers in the Webex Developer Ecosystem
- Connect a supported MCP client to a Webex MCP Server
- Execute Webex actions through MCP tools
- Identify practical automation scenarios using MCP

## Prerequisites

Before starting this section, make sure you have completed:

- Getting Started
- Lab 1 – Making Your First API Requests
- Lab 2 – Creating an Integration
- Lab 6 – Agentic Apps

## Lab skeleton

### Step 7.1: Understand MCP and Webex MCP Servers

**Goal:** Learn how MCP fits between AI clients and Webex.

- [ ] Review what MCP is and why it matters for agent-based automation
- [ ] Review the Webex MCP Server documentation and available tools
- [ ] Identify the difference between direct API calls, bots, and MCP-based interactions

**Questions to answer:**

- What is the MCP client in this lab?
- What Webex actions are exposed as MCP tools?
- What authentication model does the MCP server use?

### Step 7.2: Prepare your MCP client

**Goal:** Configure the client that will talk to the Webex MCP Server.

- [ ] Install or open the MCP client used in the lab (for example, Cursor, Claude Desktop, or another supported client)
- [ ] Locate the MCP server configuration file
- [ ] Add the Webex MCP Server entry provided for the lab
- [ ] Restart or reload the client so the tools become available

**Cursor** (`~/.cursor/mcp.json` on macOS/Linux):

```json
{
  "mcpServers": {
    "webex": {
      "command": "npx",
      "args": ["-y", "@webex/mcp-server"],
      "env": {
        "WEBEX_ACCESS_TOKEN": "YOUR_LAB_ACCESS_TOKEN"
      }
    }
  }
}
```

**Claude Desktop** (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

```json
{
  "mcpServers": {
    "webex": {
      "command": "npx",
      "args": ["-y", "@webex/mcp-server"],
      "env": {
        "WEBEX_ACCESS_TOKEN": "YOUR_LAB_ACCESS_TOKEN"
      }
    }
  }
}
```

Use an integration or bot token with scopes such as `spark:people_read`, `spark:rooms_read`, and `spark:messages_write`.

!!! Note "Screenshot needed"
    Add screenshot of the MCP client showing Webex tools listed after reload.

### Step 7.3: Connect to the Webex MCP Server

**Goal:** Verify the MCP server is reachable and authenticated.

- [ ] Start the Webex MCP Server locally or connect to the hosted lab instance
- [ ] Confirm the MCP client lists the available Webex tools
- [ ] Run a simple discovery or health-check action
- [ ] Verify authentication and permissions are working

Optional local smoke test before using the MCP client:

```bash
export WEBEX_ACCESS_TOKEN="YOUR_LAB_ACCESS_TOKEN"
curl -s -H "Authorization: Bearer $WEBEX_ACCESS_TOKEN" \
  https://webexapis.com/v1/people/me | python -m json.tool
```

In your MCP client, ask:

```text
List the Webex MCP tools you can use and describe what each one does.
```

**Expected outcome:**

- MCP client shows Webex tools/resources
- A basic tool call succeeds without authorization errors

### Step 7.4: Execute Webex actions through MCP tools

**Goal:** Perform common Webex tasks through MCP instead of direct API calls.

- [ ] Use an MCP tool to retrieve user or room information
- [ ] Use an MCP tool to send a message or create a space
- [ ] Use an MCP tool to perform one admin or automation action relevant to the lab
- [ ] Compare the MCP experience with Bruno and Python SDK usage from earlier labs

**Sample prompts to try in your MCP client:**

```text
Look up my Webex profile and tell me my display name and email.
```

```text
List my Webex spaces and show the title and ID for each one.
```

```text
Send a message to room ROOM_ID saying: "Hello from the Webex MCP lab!"
```

Equivalent direct API call for comparison (`08-mcp/01_list_rooms.py`):

```python
"""Compare MCP tool usage with a direct Webex REST call."""

import os

import requests
from dotenv import load_dotenv

load_dotenv()

TOKEN = os.getenv("WEBEX_ACCESS_TOKEN")
response = requests.get(
    "https://webexapis.com/v1/rooms",
    headers={"Authorization": f"Bearer {TOKEN}"},
    timeout=30,
)
response.raise_for_status()

for room in response.json().get("items", []):
    print(f"{room['title']} ({room['id']})")
```

Run it with:

```bash
cd 08-mcp
python 01_list_rooms.py
```

### Step 7.5: Build a small MCP-driven workflow

**Goal:** Combine MCP tools into a simple real-world scenario.

- [ ] Define a user prompt or business task
- [ ] Identify which MCP tools are needed
- [ ] Execute the workflow through the MCP client
- [ ] Verify the outcome in Webex Client or Control Hub

**Example workflow prompt:**

```text
Create a Webex space called "WebexOne 2026 MCP Onboarding",
add a welcome message explaining what MCP can do in this lab,
and summarize the steps we completed in Labs 1 through 5.
```

Document the tools the client used and the final result in your lab notes.

!!! Note "Screenshot needed"
    Add screenshot of the MCP client executing the workflow and the resulting Webex space message.

## Suggested final exercise

Create one MCP-driven workflow that helps with a task such as:

- Creating a Webex space and posting an onboarding message
- Checking service status and summarizing the result in a space
- Assisting an admin with a repetitive Webex action using natural language

## Content still to define

- Official Webex MCP Server package name and install command (replace `@webex/mcp-server` placeholder if different)
- Supported MCP clients for the event
- Required tokens, scopes, and approval steps
- Final tool names exposed by the Webex MCP Server
- Screenshots of MCP tool discovery and successful execution
