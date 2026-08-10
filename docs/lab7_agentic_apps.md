# Lab 6 - Agentic Apps

In this lab section, you will explore **Agentic Apps** within the Webex Developer Ecosystem and learn how AI-driven applications can automate tasks, respond to user intent, and integrate with Webex workflows.

## Learning Objectives

Upon completion of this section, you will be able to:

- Understand what Agentic Apps are and how they fit into the Webex Developer Ecosystem
- Identify when to use an Agentic App instead of a bot, integration, or service app
- Configure and test an Agentic App in your lab environment
- Connect an Agentic App to a practical automation scenario

## Prerequisites

Before starting this section, make sure you have completed:

- Getting Started
- Lab 1 – Making Your First API Requests
- Lab 2 – Creating an Integration
- Lab 3 – Building a Bot

## Lab skeleton

### Step 6.1: Understand Agentic Apps in Webex

**Goal:** Learn what Agentic Apps are and where they fit in the platform.

- [ ] Review the Agentic Apps documentation on [Webex for Developers](https://developer.webex.com/){:target="_blank"}
- [ ] Compare Agentic Apps with bots, integrations, and service apps
- [ ] Identify 2–3 use cases in your organization where an Agentic App would be a good fit

**Notes to capture:**

- What triggers an Agentic App?
- What data or context does it need?
- What actions can it perform inside Webex?

### Step 6.2: Access the lab Agentic App

**Goal:** Log in and locate the Agentic App provided for this lab.

- [ ] Log into [Webex for Developers](https://developer.webex.com/){:target="_blank"} with your lab credentials
- [ ] Open the Agentic App created for WebexOne 2026
- [ ] Review its configuration: name, description, scopes, and allowed actions
- [ ] Save any credentials or configuration values required for later steps

!!! Note
    Add screenshots, app names, and credential tables here once the lab environment is finalized.

### Step 6.3: Configure the Agentic App

**Goal:** Apply the minimum configuration needed to run the exercise.

- [ ] Update environment variables or configuration files in VS Code
- [ ] Verify required scopes and permissions are enabled
- [ ] Confirm the app is available in the expected Webex space or user context
- [ ] Run a basic health check or validation command

**Suggested files to add to the repo:**

- `07-agentic/01_configure_agent.py`
- `.env` variables such as `AGENTIC_APP_ID`, `AGENTIC_APP_TOKEN`, or equivalent

### Step 6.4: Run the first Agentic App scenario

**Goal:** Execute a simple end-to-end scenario.

- [ ] Start the sample Agentic App workflow from VS Code
- [ ] Trigger the app from Webex using the method defined for the lab
- [ ] Observe how the app receives context, decides what to do, and returns a result
- [ ] Verify the outcome in Webex Client

**Expected outcome:**

- The app responds to a user request or automation trigger
- The response is visible in Webex
- Logs in the terminal show the decision/action flow

### Step 6.5: Extend the scenario

**Goal:** Adapt the sample to a real-world use case.

- [ ] Modify the prompt, action, or workflow logic
- [ ] Restrict execution to approved users or domains
- [ ] Add one additional action, such as sending a message, creating a room, or calling an API
- [ ] Document what changed and why

## Suggested final exercise

Build a small Agentic App workflow that helps a user complete one of the following tasks:

- Summarize a Webex space conversation and post the result
- Collect structured input from a user and trigger a backend action
- Assist an admin with a repetitive Webex configuration task

## Content still to define

- Official Agentic App product name and navigation path in the Developer Portal
- Exact scopes and approval process for the lab tenant
- Sample code location in the `WebexOne2026` repository
- Screenshots and expected outputs for each step
