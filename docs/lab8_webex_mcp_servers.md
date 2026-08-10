# Lab 7 - Webex MCP Servers

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

!!! Note
    Add the exact MCP client, config file path, and sample JSON configuration here once the lab environment is finalized.

Example configuration skeleton:

```json
{
  "mcpServers": {
    "webex": {
      "command": "REPLACE_WITH_COMMAND",
      "args": ["REPLACE_WITH_ARGS"],
      "env": {
        "WEBEX_ACCESS_TOKEN": "REPLACE_WITH_LAB_TOKEN"
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

**Expected outcome:**

- MCP client shows Webex tools/resources
- A basic tool call succeeds without authorization errors

### Step 7.4: Execute Webex actions through MCP tools

**Goal:** Perform common Webex tasks through MCP instead of direct API calls.

- [ ] Use an MCP tool to retrieve user or room information
- [ ] Use an MCP tool to send a message or create a space
- [ ] Use an MCP tool to perform one admin or automation action relevant to the lab
- [ ] Compare the MCP experience with Postman and Python SDK usage from earlier labs

**Suggested exercises:**

- Send a message to your lab space through MCP
- Look up a room or person through MCP
- Trigger one automation action defined for the event

### Step 7.5: Build a small MCP-driven workflow

**Goal:** Combine MCP tools into a simple real-world scenario.

- [ ] Define a user prompt or business task
- [ ] Identify which MCP tools are needed
- [ ] Execute the workflow through the MCP client
- [ ] Verify the outcome in Webex Client or Control Hub

## Suggested final exercise

Create one MCP-driven workflow that helps with a task such as:

- Creating a Webex space and posting an onboarding message
- Checking service status and summarizing the result in a space
- Assisting an admin with a repetitive Webex action using natural language

## Content still to define

- Official Webex MCP Server package or repository URL
- Supported MCP clients for the event
- Required tokens, scopes, and approval steps
- Sample prompts and expected tool invocations
- Screenshots of MCP tool discovery and successful execution
