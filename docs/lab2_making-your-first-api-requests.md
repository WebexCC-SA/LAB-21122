# 1 – Making Your First API Requests (Postman)

!!! Note
    This is the **legacy Postman version**. For the 2026 lab, use **[Lab 1 - Making Your First API Requests](lab2_making-your-first-api-requests-bruno.md)** with Bruno instead.

In this chapter, you'll dive into the exciting world of Webex APIs by making your very first requests. We'll explore different methods, starting with a quick test on the Webex Developer Portal, then moving to the powerful Postman environment, and finally, making a direct call using cURL.

Upon completion of this section, you will be able to:

1. Test Webex API requests on the Webex Developer Portal
2. Send Webex API requests using Postman
3. Execute Webex API calls with cURL from the command line

Reference:  
<https://www.postman.com/webexdev/webex-public-workspace-beta>

## Step 1.1: Quick Test: Webex Developer Portal "Try It"

The Webex Developer Portal is an excellent resource for documentation and quick, one-off API tests. Let's get a temporary access token and make a simple call directly from the browser.

1. **Open the Webex Developer Portal:**
   * Navigate to <https://developer.webex.com/> in your browser.
2. **Get Your Temporary Access Token:**
   * On the developer portal homepage, locate the "**Profile**" section (usually at the top right).
   * Ensure you are logged in with your **lab credentials**. If not, click "Login" and use the provided Webex email and password.
   * Your temporary **Bearer Token** will be displayed. **Copy this token** to your clipboard. This token is valid for 12 hours.

![docx-image-007](assets/docx-image-007.png){ width="700" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

1. **Navigate to the Webex Messaging documentation area:**
   * In the header of the portal, click on the “**Documentation**” drop down to expand the “Mega Nav” bar.
   * Under the “**SUITE**” section, select “**Webex Messaging**”.

![docx-image-008](assets/docx-image-008.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

1. **Retrieve Your Own User Details (GET /people/me):**
   * In the sidebar navigation, under "**API Reference**", expand the “**All APIs**” section.
   * Then expand "**People**" from the left-hand menu.
   * Scroll down to find the ‘**Get My Own Details**” GET endpoint and select it.
   * On the right side of the page, in the "**Try It**" section, paste your copied **Bearer** Token into the "Authorization" field (if it's not already populated with the “**Use personal access token**” toggle).
   * Click the "**Run**" button.
   * You should see a 200 OK status and a JSON response containing your Webex user details.
   * Congratulations! You just made your first API request!
2. **Find Your Lab Space ID (GET /rooms):**
   * In the left-hand menu, expand "**Rooms**".
   * Scroll down to find the “**List Rooms**” GET endpoint and select it.
   * Click the "**Run**" button.
   * You'll get a 200 OK response with a list of your Webex spaces. **Carefully locate your personal lab space** (e.g., [Your Name/ID] - API Lab Space) and **copy its id value**. You'll need this in the next step!
3. **Send a Message to Your Lab Space (POST /v1/messages):**
   * In the left-hand menu, select "**Messages**".
   * Scroll down to find the “**Create a Message**” POST endpoint and select it.
   * In the "Request Body" section, you'll see a JSON structure.
   * Replace the placeholder for roomId with the id of your lab space that you just copied.
   * Change the text field to a message like: "text": "Hello from the Webex Developer Portal 'Try It' tool!"
   * Click the "**Run**" button.
   * You should see a 200 OK response.
4. **Verify Message in Webex Client:**
   * Switch back to your Webex client. You should see the message you just sent appear in your personal lab space!
   * *Note:* While great for quick tests, this "Try It" feature doesn't save your token or allow for complex workflows, which is why we'll use Postman next!

## Step 1.2: Explore the Webex Public Workspace in Postman

Now, let's explore the Webex Public Workspace in Postman, which provides a much richer environment for API development.

1. **Open the Webex Public Workspace:**
   * In your Postman application (where you logged in with lab credentials), navigate to the Webex Public Workspace by searching for **“Webex Public Workspace”**  
     ![docx-image-009](assets/docx-image-009.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }
2. **Observe the Workspace Structure:**
   * On the left sidebar, you'll see the **"Webex Public Workspace"** listed.
   * Below it, notice the various **Collections** available (e.g., "Webex Messaging", "Webex Meetings", "Webex Calling", etc.). These collections organize different sets of Webex APIs.
   * *Remember:* A Postman Workspace can contain multiple Collections, Environments, and other elements.

![docx-image-010](assets/docx-image-010.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

## Step 1.3: Use the "Webex Messaging" Collection in Postman

We'll now fork the "Webex Messaging" collection into your personal Postman space, set up your authentication, and make some practical API calls.

1. **Fork the "Webex Messaging" Collection:**
   * In the Webex Public Workspace, locate the **"Webex Messaging"** collection in the left sidebar.
   * Hover over the collection name and click the **"..." (ellipsis)** icon, then select **"Fork"**.
   * In the dialog box, give the **Fork label** a name of **“Webex API Fork”**.
   * Choose your **personal workspace** (it should be selected by default if you're in your own workspace).
   * Also include the **“Webex Prod US1”** environment in the fork (we will use this environment later).
   * Click **"Fork Collection"**.
   * You should now see a copy of "Webex Messaging" in your personal workspace (usually listed under "Collections" in the left sidebar).
2. **Configure Collection Authentication and Variable:**
   * Click on your forked **"Webex Messaging"** collection in the left sidebar.
   * In the main Postman window, click the **"Variables"** tab.
   * Add a new variable:
   * **VARIABLE:** webex_token
   * **VALUE:** Paste the **Bearer Token** you copied from the Webex Developer Portal in Step 1.1.2 here.
   * Now, click the **"Authorization"** tab for the collection.
   * For the **TYPE**, select **"Bearer Token"**.
   * In the **Token** field, enter *`{{webex_token}}`*.
   * *Explanation:* This sets up the collection to automatically use your *webex_token* variable for all requests within it!
3. **Make Your First API Call with Postman (Get My Own Details):**
   * In your forked "Webex Messaging" collection, expand the **"People"** folder.
   * Click on the **GET Get My Own Details** request (which corresponds to GET /v1/people/me).
   * In the main request window, ensure the "Authorization" tab shows "Inherit auth from parent" or "Bearer Token" and that `{{access_token}}` is in the Token field (if not inheriting).
   * Click the blue **"Send"** button.
   * You should receive a 200 OK response with your user details, just like in the Developer Portal.
4. **List Your Webex Spaces (Rooms):**
   * In your forked "Webex Messaging" collection, expand the **"Rooms"** folder.
   * Click on the **GET List Rooms** request (which corresponds to GET /v1/rooms).
   * Uncheck all the **Query Params** in the **Params** tab.
   * Click the blue **"Send"** button.
   * You should receive a 200 OK response. In the JSON body, you'll see a list of your Webex spaces. **Carefully locate your personal lab space** (e.g., [Your Name/ID] - API Lab Space) and **copy its id value**. You'll need this in the next steps!
5. **Send a Message to Your Lab Space (POST Create a Message):**
   * In your forked "Webex Messaging" collection, expand the **"Messages"** folder.
   * Click on the **POST Create a Message** request (which corresponds to POST /v1/messages).
   * In the main request window, click the **"Body"** tab.
   * You'll see a JSON body. Remove all fields except for **“roomId”** and **“text”**.
   * Replace `{{roomId}}` with the actual id of your lab space (e.g., "roomId": "c2lkOi8v...").
   * Change the text field to something personal, like "text": "Hello from Postman - this is much easier!".

![docx-image-011](assets/docx-image-011.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

* + Click the blue **"Send"** button.
  + You should receive a 200 OK response. Now, check your Webex Client – you should see the message appear in your lab space!

1. **Read Messages in Your Lab Space (GET List Messages):**
   * Click on the **GET List Messages** request (which corresponds to GET /v1/messages).
   * In the main request window, click the **"Params"** tab.
   * Uncheck all **Query Params**.
   * Check the **roomId** parameter, enter the id of your lab space that you copied earlier into the **Value**.
   * Click the blue **"Send"** button.
   * You should receive a 200 OK response with a list of messages in that space. You should see the message you just sent via Postman, and potentially the one you sent via the Developer Portal "Try It" tool!

## Step 1.4: Make an API Request with cURL

Finally, let's see how to make an API call directly from your command line using cURL. Postman can even generate the cURL command for you!

1. **Generate cURL from Postman:**
   * Go back to the **GET Get My Own Details** request you successfully ran in Postman (under "People API").
   * On the right side of the request window, to the right of the "Send" button, click the **"Code"** link.
   * A "Generate Code Snippets" window will appear. From the dropdown menu, select **"cURL"**.
   * **Copy the entire cURL command** that is displayed.

![docx-image-012](assets/docx-image-012.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

1. **Open Your Terminal/Command Prompt:**
   * On your lab workstation, open a **Terminal** (macOS/Linux) or **Command Prompt/PowerShell** (Windows).
2. **Execute the cURL Command:**
   * Paste the copied cURL command into your terminal/command prompt.
   * **Important:** The cURL command generated by Postman will likely include your actual access_token value directly since it was set as the current value. If it still shows a placeholder, manually replace it with your **Bearer Token** that you copied in Step 1.1.2.
   * *Example:* curl --location --request GET 'https://api.webex.com/v1/people/me' \ --header 'Authorization: Bearer YOUR_ACTUAL_BEARER_TOKEN'
   * Press **Enter** to execute the command.
3. **Observe the Output:**
   * The JSON response containing your user details will be printed directly in your terminal.

Congratulations! You've now made Webex API requests using the Developer Portal, Postman, and cURL. You're well on your way to becoming a Webex API pro!
