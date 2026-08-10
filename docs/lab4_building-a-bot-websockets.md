# 3 – Building a Bot with WebSockets

This is the **WebSocket-based version** of Lab 3. Steps 3.1 to 3.4 are the same as in [Lab 3 - Building a Bot (Old)](lab4_building-a-bot.md). Steps 3.5 to 3.7 use a **native WebSocket connection to Webex Mercury** instead of the `webex_bot` library.

Upon completion of this section, you will be able to:

1. Create a **Bot** in Webex.
2. Send a message and find people using the Bot using Python.
3. Create a room and add a person using the Bot using Python.
4. Create and send an Adaptive Card.
5. Build an interactive bot using a **WebSocket connection** to receive events in real time.

Reference:

- [Webex Bots Guide](https://developer.webex.com/messaging/docs/bots){:target="_blank"}
- [Webhooks Guide](https://developer.webex.com/messaging/docs/api/guides/webhooks){:target="_blank"}

!!! Note
    The original Lab 3 section using the `webex_bot` library is still available in the guide. This section is the recommended WebSocket-first approach for 2026.

## Why WebSockets?

Webex bots can receive events in two main ways:

- **Webhooks:** Webex sends HTTP callbacks to a public URL. This works well in production, but usually requires a public endpoint or a tunnel such as ngrok during development.
- **WebSockets (Mercury):** Your bot opens a persistent connection to Webex and receives events in real time. This is a better fit for lab environments and corporate networks because no public URL is required.

In this section, you will use the WebSocket approach directly so you can see how event handling, message parsing, and Adaptive Card actions work without a third-party bot framework.

## Step 3.1: Create a Bot

First you need to create your bot:

1. Log into [developer.webex.com](https://developer.webex.com/){:target="_blank"} with credentials that were provided.
2. Up on the top right corner of the page, click your avatar and then select **My Webex Apps**.
3. On the **Create a New App** page, find the Bot card and click the **Create a Bot** button.

![docx-image-018](assets/docx-image-018.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

4. Fill out the webform to register a new bot.

    1. **Bot Name:** WebexOne-*USERNAME*
    2. **Bot Username:** WebexOne-*USERNAME*
    3. **Icon:** Select any color icon
    4. **Description:** Bot for WebexOne

![docx-image-019](assets/docx-image-019.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

!!! Warning
    Copy your **Bot access token** in your `.env` file as **BOT_TOKEN**.

## Step 3.2: Send a message to yourself

In this step, you will send your first 1:1 message using the bot you just created.

1. In VS Code navigate to your `.env` file, and make sure to fill and save the following variables:

    - BOT_TOKEN
    - EMAIL
    - DOMAIN

2. Navigate to `03-bots/01_people.py` and review the code. You will notice that there are two functions **find_people** and **all_people**.

    !!! Warning
        **all_people** will not work if you are using a bot token as it requires admin privileges.

3. Make sure that in your terminal you are in the right folder:

    - cd 03-bots

4. Run your code with the following command:

    - python 01_people.py

5. You should see the following in the console:

    ![docx-image-022](assets/docx-image-022.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

6. Navigate to `03-bots/02_message.py` and review the code. You will use the function you created earlier to find yourself, so you can send a message to your own account.
7. Run your code with the following command:

    - python 02_message.py

You should have received the following message:

![docx-image-023](assets/docx-image-023.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Step 3.3: Create a Room and add yourself

In this step, you will create a room with your bot and add your user to it.

1. Navigate to `03-bots/03_rooms.py` and review the code.

    There are two functions, one to create the room **create_webex_room** and another one to add a person to it **add_person_to_room**.

2. Run your code with the following command:

    - python 03_rooms.py

3. You will see all the steps printed in the console:

    ![docx-image-024](assets/docx-image-024.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

    You should also see the newly created room appear in your Webex App:

    ![docx-image-025](assets/docx-image-025.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Step 3.4: Adaptive card

In this step, you will explore how to create and send an Adaptive Card.

1. Navigate to `03-bots/04_adaptivecard.py` and review the code.

    You will notice a card content example, but ideally you will create your own card.

2. Navigate to the Buttons and Cards Designer:

    - [Buttons and Cards Designer](https://developer.webex.com/buttons-and-cards-designer){:target="_blank"}

3. Use the UI to create your own card. Experiment with the available options to understand how they work. Once you’re ready, your card’s JSON code will appear below in the Card Payload Editor.

    !!! Warning
        When you copy the code from the Card Payload Editor, make sure to change **true** to **True** so it works correctly in Python.

4. Run your code with the following command:

    - python 04_adaptivecard.py

If you have used the example card, you should receive the following:

![docx-image-027](assets/docx-image-027.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Step 3.5: Connect your bot using WebSockets

In this step, you will open a persistent WebSocket connection to Webex Mercury and handle incoming messages without using the `webex_bot` library.

1. Navigate to `03-bots/05_websocket_bot.py` and review the code.

    The sample follows this flow:

    - Register the bot device with Webex
    - Open a Mercury WebSocket connection
    - Listen for `message` events
    - Reply to the user through the REST API

    ```
    import asyncio
    import os

    from dotenv import load_dotenv

    load_dotenv()
    BOT_TOKEN = os.getenv("BOT_TOKEN")

    async def handle_message(event):
        room_id = event["data"]["roomId"]
        person_email = event["data"].get("personEmail")
        text = event["data"].get("text", "")

        print(f"Message from {person_email}: {text}")

        # Example: echo the message back using the REST API
        # send_message(room_id, f"You said: {text}")

    async def main():
        # connect_mercury(BOT_TOKEN, handle_message)
        # await listen_forever()
        pass

    if __name__ == "__main__":
        asyncio.run(main())
    ```

    !!! Note
        The lab repository will provide the full working implementation. The important part is understanding the event loop and where your business logic lives.

2. Execute the code with the following command and let it run:

    - python 05_websocket_bot.py

!!! Warning
    Wait until you see a message such as **WebSocket connected** or **Mercury connected** appear in the console.

3. Send any message to your bot, and it will respond using the handler defined in the script:

    ![docx-image-029](assets/docx-image-029.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

4. Observe how the incoming events are printed in the console:

    ![docx-image-030](assets/docx-image-030.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

5. Trigger a card action or follow the sample prompt and verify that the bot response is sent through the REST API rather than by the framework itself.

## Step 3.6: WebSockets – Create your own handler

In this step, you will replace the generic echo behavior with your own command handler.

1. Navigate to `03-bots/06_websocket_bot-2.py` and review the code.

    !!! Note
        The WebSocket version replaces `webex_bot` concepts with explicit handler logic:

        - **allowed_domains** validates the sender's email domain before executing commands
        - **command_keyword** maps user text such as `message` to a specific handler
        - **send_message()** sends responses through the Webex REST API
        - **delete_message()** optionally removes the previous card before sending a new one

        ![docx-image-035](./assets/docx-image-035.png){ width="700" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

2. Execute the code with the following command and let it run:

    - python 06_websocket_bot-2.py

!!! Warning
    Wait until you see the message **WebSocket connected** appear in the console.

3. Send any message to your bot and you should receive a card with your custom function **Send Hello!**:

    ![docx-image-037](assets/docx-image-037.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

4. Click **Send Hello!**. The card will be deleted, you will receive a confirmation message, and you should also see your **Hello!** message:

    ![docx-image-038](assets/docx-image-038.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

5. To directly invoke this function, text your bot with the **message** keyword:

    ![docx-image-039](assets/docx-image-039.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Step 3.7: WebSockets – Adaptive card processing

In this final step, you will handle Adaptive Card submissions through WebSocket events.

1. Navigate to `03-bots/07_websocket_bot-3.py` and review the code.

    !!! Note
        Instead of `chained_commands` from `webex_bot`, the WebSocket version listens for **`attachmentActions`** events and routes them to a handler:

        - **on_attachment_action()** receives card submission data
        - **extract_input_values()** reads the fields submitted by the user
        - **send_card_response()** sends the confirmation or next step

        ![docx-image-040](./assets/docx-image-040.png){ width="700" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

2. Execute the code with the following command and let it run:

    - python 07_websocket_bot-3.py

!!! Warning
    Wait until you see the message **WebSocket connected** appear in the console.

3. Text **message** to your bot to invoke your function directly:

    ![docx-image-042](assets/docx-image-042.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

4. Enter a message and click **Submit**.

    The previous card will be deleted. You should then receive both your message and a formatted notification confirming that your message has been sent:

    ![docx-image-043](assets/docx-image-043.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Compare with the webex_bot version

| Topic | `webex_bot` version | WebSocket version |
| --- | --- | --- |
| Event transport | WebSocket via library | WebSocket via Mercury directly |
| Command routing | Built-in commands / cards | Your own handler functions |
| Card actions | `chained_commands` | `attachmentActions` event handler |
| Dependencies | `webex_bot`, `webexpythonsdk` | WebSocket client + REST API calls |
| Best for | Fast prototyping | Understanding the underlying platform |

## Repository work still needed

- [ ] Add `05_websocket_bot.py`, `06_websocket_bot-2.py`, and `07_websocket_bot-3.py` to the lab repo
- [ ] Remove `webex_bot` dependency from the WebSocket path in `requirements.txt`
- [ ] Decide whether to keep the legacy Postman and `webex_bot` sections after the 2026 event
