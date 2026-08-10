# Lab 6 - Agentic Apps

Lab code repository: [https://github.com/diegomjimenez/WebexOne2026_Developer](https://github.com/diegomjimenez/WebexOne2026_Developer){:target="_blank"}

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

Add the following values to your `.env` file (replace placeholders with lab credentials):

```env
AGENTIC_APP_ID=your_agentic_app_id
AGENTIC_APP_TOKEN=your_agentic_app_token
AGENTIC_APP_BASE_URL=https://webexapis.com/v1
LAB_SPACE_ID=your_lab_space_id
ALLOWED_DOMAIN=example.com
```

Starter configuration script (`07-agentic/01_configure_agent.py`):

```python
"""Validate Agentic App credentials and Webex connectivity."""

import os
import sys

import requests
from dotenv import load_dotenv

load_dotenv()

APP_ID = os.getenv("AGENTIC_APP_ID")
APP_TOKEN = os.getenv("AGENTIC_APP_TOKEN")
BASE_URL = os.getenv("AGENTIC_APP_BASE_URL", "https://webexapis.com/v1")
SPACE_ID = os.getenv("LAB_SPACE_ID")


def check_env() -> None:
    missing = [name for name in ("AGENTIC_APP_ID", "AGENTIC_APP_TOKEN") if not os.getenv(name)]
    if missing:
        print(f"Missing required variables: {', '.join(missing)}")
        sys.exit(1)
    print(f"Agentic App ID configured: {APP_ID}")


def check_webex_token() -> None:
    response = requests.get(
        f"{BASE_URL}/people/me",
        headers={"Authorization": f"Bearer {APP_TOKEN}"},
        timeout=30,
    )
    response.raise_for_status()
    profile = response.json()
    print(f"Authenticated as: {profile.get('displayName')} ({profile.get('emails', [''])[0]})")


def check_space_access() -> None:
    if not SPACE_ID:
        print("LAB_SPACE_ID not set; skipping space check.")
        return
    response = requests.get(
        f"{BASE_URL}/rooms/{SPACE_ID}",
        headers={"Authorization": f"Bearer {APP_TOKEN}"},
        timeout=30,
    )
    response.raise_for_status()
    room = response.json()
    print(f"Space accessible: {room.get('title')}")


if __name__ == "__main__":
    check_env()
    check_webex_token()
    check_space_access()
    print("Agentic App configuration looks good.")
```

Run the health check:

```bash
cd 07-agentic
python 01_configure_agent.py
```

### Step 6.4: Run the first Agentic App scenario

**Goal:** Execute a simple end-to-end scenario.

- [ ] Start the sample Agentic App workflow from VS Code
- [ ] Trigger the app from Webex using the method defined for the lab
- [ ] Observe how the app receives context, decides what to do, and returns a result
- [ ] Verify the outcome in Webex Client

Starter workflow script (`07-agentic/02_run_agent_scenario.py`):

```python
"""Run a simple Agentic App scenario: summarize a prompt and post to a space."""

import os

import requests
from dotenv import load_dotenv

load_dotenv()

APP_TOKEN = os.getenv("AGENTIC_APP_TOKEN")
SPACE_ID = os.getenv("LAB_SPACE_ID")
BASE_URL = os.getenv("AGENTIC_APP_BASE_URL", "https://webexapis.com/v1")

USER_PROMPT = "Summarize what an Agentic App can do inside Webex in one sentence."


def run_agent_prompt(prompt: str) -> str:
    # Replace this stub with the lab Agentic App SDK or REST endpoint when available.
    return f"Agent response for prompt: {prompt}"


def post_to_space(markdown: str) -> None:
    response = requests.post(
        f"{BASE_URL}/messages",
        headers={
            "Authorization": f"Bearer {APP_TOKEN}",
            "Content-Type": "application/json",
        },
        json={"roomId": SPACE_ID, "markdown": markdown},
        timeout=30,
    )
    response.raise_for_status()
    print("Posted agent result to the lab space.")


if __name__ == "__main__":
    result = run_agent_prompt(USER_PROMPT)
    post_to_space(f"**Agentic App result**\n\n{result}")
```

Run the scenario:

```bash
python 02_run_agent_scenario.py
```

**Expected outcome:**

- The app responds to a user request or automation trigger
- The response is visible in Webex
- Logs in the terminal show the decision/action flow

!!! Note "Screenshot needed"
    Add screenshot of the Agentic App response posted in the lab Webex space.

### Step 6.5: Extend the scenario

**Goal:** Adapt the sample to a real-world use case.

- [ ] Modify the prompt, action, or workflow logic
- [ ] Restrict execution to approved users or domains
- [ ] Add one additional action, such as sending a message, creating a room, or calling an API
- [ ] Document what changed and why

Example domain restriction helper to add before executing actions:

```python
def is_allowed_sender(email: str, allowed_domains: list[str]) -> bool:
    if not allowed_domains:
        return True
    domain = email.split("@")[-1].lower() if "@" in email else ""
    return domain in {item.lower() for item in allowed_domains}
```

Example extension: create a room and post the agent summary there:

```python
def create_room_and_post(title: str, markdown: str) -> None:
    room = requests.post(
        f"{BASE_URL}/rooms",
        headers={"Authorization": f"Bearer {APP_TOKEN}", "Content-Type": "application/json"},
        json={"title": title},
        timeout=30,
    ).json()
    requests.post(
        f"{BASE_URL}/messages",
        headers={"Authorization": f"Bearer {APP_TOKEN}", "Content-Type": "application/json"},
        json={"roomId": room["id"], "markdown": markdown},
        timeout=30,
    )
```

## Suggested final exercise

Build a small Agentic App workflow that helps a user complete one of the following tasks:

- Summarize a Webex space conversation and post the result
- Collect structured input from a user and trigger a backend action
- Assist an admin with a repetitive Webex configuration task

## Content still to define

- Official Agentic App product name and navigation path in the Developer Portal
- Exact scopes and approval process for the lab tenant
- Final Agentic App SDK or REST endpoint to replace the `run_agent_prompt()` stub in `07-agentic/02_run_agent_scenario.py`
- Screenshots and expected outputs for each step
