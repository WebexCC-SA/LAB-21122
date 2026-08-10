# 3 – Building a Bot

Upon completion of this section, you will be able to:

1. Create a **Bot** in Webex.
2. Send a message and find people using the Bot using Python.
3. Create a room and add a person using the Bot using Python.
4. Create and send and Adaptive Card.
5. Create an interactive bot using webex_bot python library.

Reference:  
<https://developer.webex.com/messaging/docs/bots>

## Step 3.1: Create a Bot

First you need to create your bot:

1. Log into [developer.webex.com](https://developer.webex.com/) with credentials that were provided.
2. Up on the top right corner of the page, click your avatar and then select ‘My Webex Apps’.
3. On the ‘Create a New App’ page, find the Bot card and click the ‘Create a Bot’ button.

![](assets/docx-image-018.png)

1. Fill out the webform to register a new service app.  
   1. **Bot Name:** WebexOne-*USERNAME*
   2. **Bot Username:** WebexOne-*USERNAME*
   3. **Icon:** *Select any color icon*.
   4. **Description**: “Bot for WebexOne”

![](assets/docx-image-019.png)

**![](assets/docx-image-020.png) Warning:** Copy your **Bot access token** in your .env file as **BOT_TOKEN**.

## Step 3.2: Send a message to yourself

In this step, you will send your first 1:1 message using the bot you just created. There are two options to achieve this:

1. In VS Code navigate to your .env file, and make sure to fill and save the following variables:
   * BOT_TOKEN
   * EMAIL
   * DOMAIN
2. Navigate to 03-bots/01_people.py and review the code. You will notice that there are two functions **find_people** and **all_people**.   
     
   **![](assets/docx-image-021.png) Warning: all_people** will not work if you are using a bot token as it requires admin privileges.
3. Make sure that in your terminal you are in the right folder:  
   * cd 03-bots
4. Run your code with the following command:  
   * python 01_people.py
5. You should see the following in the console:  
     
   ![](assets/docx-image-022.png)
6. Navigate to 03-bots/02_message.py and review the code. You will be using the function you created earlier to find yourself, so you can send a message to your own account.
7. Run your code with the following command:  
   * python 02_message.py

You should have received now the following message:

![](assets/docx-image-023.png)

## Step 3.3: Create a Room and add yourself

In this step, you will create a room with your bot and add your user to it.

1. Navigate to 03-bots/03_rooms.py and review the code.  
     
   There are two functions, one to create the room **create_webex_room** and another one to add a person to it **add_person_to_room**.
2. Run your code with the following command:  
   * python 03_rooms.py
3. You will see all the steps printed in the console:  
     
   ![](assets/docx-image-024.png)  
     
   You should also see the newly created room appear in your Webex App:  
     
   ![](assets/docx-image-025.png)

## Step 3.4: Adaptive card

In this step, you will explore how to create and send an Adaptive Card.

1. Navigate to 03-bots/04_adaptivecard and review the code.  
     
   You will notice that card content example, but ideally you will create your own card.
2. Navigate to the Buttons and Cards Designer:  
   * <https://developer.webex.com/buttons-and-cards-designer>
3. Use the UI to create your own card. Experiment with the available options to understand how they work. Once you’re ready, your card’s JSON code will appear below in the Card Payload Editor.  
     
   **![](assets/docx-image-026.png) Warning:** When you copy the code from the Card Payload Editor, make sure to change **true** to **True** so it works correctly in Python.
4. Now, run your code with the following command:  
   * python 04_adaptivecard.py

If you have used the example card, you should receive the following:  
  
![](assets/docx-image-027.png)

## Step 3.5: webex_bot

In this step and the following ones, you will work with the webex_bot library to get familiar with all the available options. You’ll start with example cards and functions, and then learn how to add your own.

1. Navigate to 03-bots/05_webex_bot.py and review the code.  
     
   In this first example, you will use the default cards and functions provided to understand how the process works.
2. Execute the code with the following command and let it run:  
   * python 05_webex_bot.py

**![](assets/docx-image-028.png) Warning:** Wait until you see the message “WebSocket Opened.” appear in the console.

1. Send any message to your bot, and it will respond with the following card using the default function:  
     
   ![](assets/docx-image-029.png)
2. It is interesting to see how all the events are reflected in the console:  
     
   ![](assets/docx-image-030.png)
3. Once you click “Echo Words Back to You!”, an action will be executed and you will receive the following cards:  
     
   ![](assets/docx-image-031.png)
4. New event is also reflected in the console:  
     
   ![](assets/docx-image-032.png)
5. Type something in the box and click Submit. You should receive a new message, and the previous card will be deleted:   
     
   ![](assets/docx-image-033.png)
6. You should be able to see the message you sent in the console:  
     
   ![](assets/docx-image-034.png)

## Step 3.6: webex_bot – Create your own function

In this step, you will learn how to create your own function and add it to the bot.

1. Navigate to 03-bots/06_webex_bot-2.py and review the code.  
     
   You will be using WebexPythonSDK to send a message as shown previously.  
     
   **![](assets/docx-image-035.png) Note:** Some changes have been made:  
   * **approved_domains** is used to restrict bot access to people inside your domain.
   * **include_demo_commands** is set to False to hide the Echo function.
   * **delete_previous_message** is set to True to delete the previous card and avoid duplicate actions.
   * **bot_name** is the name that the bot will display on the cards.
   * **command_keyword** is the text that you can use to invoke a function instead of the assistant.
   * **help_message** will be the name of your function.

You can also access the information previously displayed in the console from the **attachment_actions** variable.

1. Execute the code with the following command and let it run:  
   * python 06_webex_bot-2.py

**![](assets/docx-image-036.png) Warning:** Wait until you see the message “WebSocket Opened.” appear in the console.

1. Send any message to your bot and you will get the following card with your new function **Send Hello!**:  
     
   ![](assets/docx-image-037.png)
2. Click **Send Hello!**. The card will be deleted, you will receive a new message confirming that the message has been sent, and you should also see your **Hello!** message:  
     
   ![](assets/docx-image-038.png)
3. To directly invoke this function, you can text your bot with **message** keyword. This will automatically send you the **Hello!** message:  
     
   ![](assets/docx-image-039.png)

## Step 3.7: webex_bot – Adaptive card processing

In this final step, you will send an Adaptive Card with multiple fields and use the submitted data to trigger an action.

1. Navigate to 03-bots/07_webex_bot-3.py and review the code.  
     
   **![](assets/docx-image-040.png) Note:** Some changes have been added:  
   * **chained_commands** are the functions that will trigger this funcion when card data is submitted.
   * **quote_info** formats your response.
   * **Response** object allows you to send the Adaptive Card.
2. Execute the code with the following command and let it run:  
   * python 07_webex_bot-3.py

**![](assets/docx-image-041.png) Warning:** Wait until you see the message “WebSocket Opened.” appear in the console.

1. Text **message** to your bot to invoke your function directly:  
     
   ![](assets/docx-image-042.png)
2. Enter a message and click Submit.  
     
   The previous card will be deleted. You should then receive both your message and a formatted notification confirming that your message has been sent:

![](assets/docx-image-043.png)
