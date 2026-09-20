# Lab 6 - Webex MCP Servers

In this lab section, you will explore **Webex MCP Servers** and learn how they expose Webex capabilities to external AI clients through the Model Context Protocol (MCP).

### Step 7.1: Understand MCP and Webex MCP Servers

### Step 7.2: Prepare your MCP client

**VS Code** (`~/.vscode/mcp.json`):

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

### Step 7.4: Execute Webex actions through MCP tools

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

**Example workflow prompt:**

```text
Create a Webex space called "WebexOne 2026 MCP Onboarding",
add a welcome message explaining what MCP can do in this lab,
and summarize the steps we completed in Labs 1 through 5.
```

Document the tools the client used and the final result in your lab notes.

!!! Note "Screenshot needed"
    Add screenshot of the MCP client executing the workflow and the resulting Webex space message.
