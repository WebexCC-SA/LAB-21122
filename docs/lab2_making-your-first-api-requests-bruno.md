# 1 – Making Your First API Requests

In this chapter, you'll dive into the exciting world of Webex APIs by making your very first requests. We'll explore different methods, starting with a quick test on the Webex Developer Portal, then moving to **Bruno**, and finally making a direct call using cURL.

Upon completion of this section, you will be able to:

1. Test Webex API requests on the Webex Developer Portal
2. Send Webex API requests using Bruno
3. Execute Webex API calls with cURL from the command line

Reference:

- [Bruno](https://www.usebruno.com/){:target="_blank"}
- Webex collection in the lab repo: `bruno/Webex Messaging/`

## Step 1.1: Quick Test: Webex Developer Portal "Try It"

The Webex Developer Portal is an excellent resource for documentation and quick, one-off API tests. Let's get a temporary access token and make a simple call directly from the browser.

1. **Open the Webex Developer Portal:**
   * Navigate to <https://developer.webex.com/> in your browser.
2. **Get Your Temporary Access Token:**
   * On the developer portal homepage, locate the **"Profile"** section (usually at the top right).
   * Ensure you are logged in with your **lab credentials**. If not, click "Login" and use the provided Webex email and password.
   * Your temporary **Bearer Token** will be displayed. **Copy this token** to your clipboard. This token is valid for 12 hours.

![docx-image-007](assets/docx-image-007.png){ width="700" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

3. **Navigate to the Webex Messaging documentation area:**
   * In the header of the portal, click on the **"Documentation"** drop down to expand the **"Mega Nav"** bar.
   * Under the **"SUITE"** section, select **"Webex Messaging"**.

![docx-image-008](assets/docx-image-008.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }

4. **Retrieve Your Own User Details (GET /people/me):**
   * In the sidebar navigation, under **"API Reference"**, expand the **"All APIs"** section.
   * Then expand **"People"** from the left-hand menu.
   * Scroll down to find the **"Get My Own Details"** GET endpoint and select it.
   * On the right side of the page, in the **"Try It"** section, paste your copied **Bearer Token** into the "Authorization" field.
   * Click the **"Run"** button.
   * You should see a 200 OK status and a JSON response containing your Webex user details.
5. **Find Your Lab Space ID (GET /rooms):**
   * Expand **"Rooms"**, open **"List Rooms"**, and click **"Run"**.
   * Copy the `id` of your personal lab space.
6. **Send a Message to Your Lab Space (POST /v1/messages):**
   * Open **"Create a Message"**, replace `roomId`, update the `text` field, and click **"Run"**.
7. **Verify Message in Webex Client:**
   * Switch back to your Webex client and confirm the message appears in your lab space.
   * *Note:* While great for quick tests, this "Try It" feature doesn't save your token or allow for complex workflows, which is why we'll use Bruno next!

## Step 1.2: Explore the Webex Collection in Bruno

Now, let's explore the Webex collection in Bruno, which provides a local, Git-friendly environment for API development.

1. **Open the Webex Messaging collection:**
   * In Bruno, open the collection folder from the cloned repository:

     - `bruno/Webex Messaging/`

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the **Webex Messaging** collection open in the sidebar with folders such as **People**, **Rooms**, and **Messages**.

2. **Observe the collection structure:**
   * Review the folders available in the collection, such as **People**, **Rooms**, and **Messages**.
   * Open the lab environment and confirm variables such as `webex_token` are available.
   * *Remember:* Bruno stores collections locally and works well with source control.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the **Webex Prod US1** environment selected and the collection variables panel with `webex_token`.

## Step 1.3: Use the "Webex Messaging" Collection in Bruno

We'll now configure authentication in Bruno and make practical API calls.

1. **Open the collection and environment:**
   * Open `bruno/Webex Messaging/` in Bruno.
   * Select the lab environment, for example **Webex Prod US1**.
2. **Configure collection authentication and variables:**
   * Open the collection settings or environment variables.
   * Set **`webex_token`** to the Bearer token copied from Step 1.1.
   * Confirm the collection auth type is **Bearer Token** and uses the token variable.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing collection/environment variables with **`webex_token`** populated and Bearer auth configured.

3. **Make your first API call in Bruno (GET /people/me):**
   * Open **People > Get My Own Details**.
   * Send the request.
   * You should receive a 200 OK response with your user details.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing a successful **GET /people/me** request and JSON response.

4. **List your Webex spaces (GET /rooms):**
   * Open **Rooms > List Rooms**.
   * Disable unnecessary query params if present.
   * Send the request and copy your lab space `id`.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing a successful **GET /rooms** response with the lab space visible.

5. **Send a message to your lab space (POST /messages):**
   * Open **Messages > Create a Message**.
   * In the body, keep only `roomId` and `text`.
   * Replace the room ID and set a message such as `"Hello from Bruno - this is much easier!"`.
   * Send the request.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the **POST /messages** request body and a successful 200 response.

6. **Read messages in your lab space (GET /messages):**
   * Open **Messages > List Messages**.
   * Set the `roomId` query parameter to your lab space ID.
   * Send the request and confirm your messages appear in the response.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing **GET /messages** filtered by `roomId`.

## Step 1.4: Make an API Request with cURL

Finally, let's make an API call directly from your command line using cURL. Bruno can generate the cURL command for you.

1. **Generate cURL from Bruno:**
   * Go back to the **GET Get My Own Details** request you successfully ran in Bruno.
   * Open the code snippet / generate code option and select **cURL**.
   * Copy the entire cURL command.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the **Generate Code** panel with the cURL snippet for **GET /people/me**.

2. **Open your terminal:**
   * On your lab workstation, open a terminal or PowerShell window.
3. **Execute the cURL command:**
   * Paste the copied cURL command into your terminal.
   * If the token is still a placeholder, replace it with the Bearer token from Step 1.1.
   * Press **Enter** to execute the command.
4. **Observe the output:**
   * The JSON response containing your user details will be printed directly in your terminal.

Congratulations! You've now made Webex API requests using the Developer Portal, Bruno, and cURL.
