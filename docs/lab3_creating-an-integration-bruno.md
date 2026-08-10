# 2 – Creating an Integration

In Chapter 1, you used a temporary developer token for quick API tests. While convenient, this isn't how real-world applications securely access user data. This chapter introduces **Webex Integrations** and the **OAuth 2.0 Authorization Code Flow**, the standard for third-party applications to get secure, user-consented access to Webex APIs. You'll create your own integration, perform a manual OAuth flow, and then configure **Bruno** to handle it seamlessly.

Upon completion of this section, you will be able to:

1. Create a Webex Integration
2. Perform a manual OAuth 2.0 authorization flow
3. Configure OAuth 2.0 authentication in Bruno
4. Make API requests with the obtained token in Bruno

Reference:

- [Bruno OAuth 2.0](https://docs.usebruno.com/auth/oauth2-2.0/authentication-oauth2){:target="_blank"}
- Webex collection in the lab repo: `bruno/Webex Messaging/`

## Step 2.1: Create Your Webex Integration on the Developer Portal

First, you need to register your "application" (in this case, our Bruno setup) with Webex. This process gives you a Client ID and Client Secret, which are essential for the OAuth flow.

1. **Navigate to "My Apps" on the Developer Portal:**
   * Open your browser and go to https://developer.webex.com/my-apps.
   * Ensure you are logged in with your **lab credentials**.
2. **Create a New Integration:**
   * Click the **"Create an Integration"** button.
3. **Fill in Integration Details:**
   * **Integration Name:** [Your Name/ID] - Lab Integration (e.g., JaneDoe-Lab Integration)
   * **Description:** Webex One Lab Integration for API testing.
   * **Icon:** Pick your favorite color icon.
   * **App Hub Description:** "Bruno Integration for the Webex One 2026 Lab"
   * **Redirect URI(s):** This is critical! For Bruno's OAuth helper, register the callback URL shown in Bruno's OAuth configuration panel when you configure collection auth (typically a localhost callback such as `http://127.0.0.1:6274/callback`). Confirm the exact URL in Bruno before saving your integration.
   * **Scopes:** Check the following boxes:
     * spark:messages_write
     * spark:messages_read
     * spark:people_read
     * spark:rooms_read
   * Click **"Add Integration"**.
4. **Record Your Client ID and Client Secret:**
   * After creation, you'll see your integration's details. **Copy and save your Client ID, Client Secret, and OAuth Authorization URL** to a temporary text file or notepad.
   * For best results, leave this page open for the rest of the lab.
   * *Note:* The Client Secret is only shown once. If you lose it, you'll have to regenerate it.

## Step 2.2: Manual OAuth 2.0 Authorization Code Flow (Browser + API Call)

Now, let's manually walk through the steps a user and an application would take to get an access token using the Authorization Code flow. This will help you understand the underlying mechanics.

1. **Use the Authorization URL:**
   * Copy the **"OAuth Authorization URL"** from the Integration Details page  
     ![docx-image-013](assets/docx-image-013.png){ width="850" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }  
     Paste this URL into your browser's address bar.
   * *Query Params Explanation:*
     * response_type=code: We want an authorization code.
     * client_id: Your integration's unique ID.
     * redirect_uri: Where Webex sends the user back after authorization. Must match what you registered.
     * scope: The permissions we're requesting.
     * state: An optional parameter for security, usually a random string.
2. **Authorize the Integration (User Consent):**
   * Press Enter to navigate to the constructed URL.
   * You'll be prompted to log in to Webex (if not already) and then asked to grant permission to your [Your Name/ID] - Lab Integration for the requested scopes.  
     ![docx-image-014](assets/docx-image-014.png){ width="700" style="display: block; margin: 0 auto; border: 1px solid lightgray; border-radius: 8px;" }
   * Uncheck the **"Only ask when requesting new permissions."** checkbox. This way, when we authorize again later, we will see the same dialog.
   * Click **"Accept"**
3. **Capture the Authorization Code:**
   * Webex will redirect your browser to your registered callback URL with `?code=YOUR_AUTHORIZATION_CODE&state=...` in the address bar.
   * **Copy the authorization code value** from the URL in your browser's address bar. This is your temporary authorization code.
4. **Exchange the Code for an Access Token (Bruno API Call):**
   * Open Bruno.
   * Create a new **POST** request (or use a temporary request in the collection).
   * Set the URL to: `https://webexapis.com/v1/access_token`
   * Set the body type to **Form URL Encoded** and add the following key-value pairs:

     | Key | Value |
     | --- | --- |
     | grant_type | authorization_code |
     | client_id | YOUR_CLIENT_ID |
     | client_secret | YOUR_CLIENT_SECRET |
     | code | YOUR_AUTHORIZATION_CODE |
     | redirect_uri | YOUR_REGISTERED_REDIRECT_URI |

   * Send the request.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the **POST /access_token** request with form-urlencoded body fields filled in and a **200 OK** response containing `access_token`.

5. **Observe the Token Response:**
   * You should receive a 200 OK response containing your access_token, refresh_token, expires_in, and scope.
   * **Copy your new access_token**. This token was generated via your integration and user consent, making it a more secure way to access Webex APIs on behalf of a user.
6. **Update Bruno Collection with New Token:**
   * In Bruno, open the **Webex Messaging** collection.
   * Open the lab environment (for example **Webex Prod US1**).
   * Paste the access token into the **`webex_token`** variable.
7. **Make an API Call with the Integration Token (List Rooms):**
   * Open **Rooms > List Rooms**.
   * Send the request.
   * You should receive a 200 OK response. Verify that the rooms listed are correct for your lab account. This call was made using the access_token obtained through your integration!

!!! Note "Screenshot needed"
    Add Bruno screenshot showing **GET /rooms** executed successfully using the integration token from the environment variable.

## Step 2.3: Configure OAuth 2.0 in Bruno for Your Integration

Manually performing the OAuth flow is cumbersome. Bruno has built-in support to automate much of this. Let's configure the **Webex Messaging** collection to use your new integration.

1. **Open the Webex Messaging Collection:**
   * In Bruno, open `bruno/Webex Messaging/` from the cloned repository.
2. **Access Collection Authorization Settings:**
   * Select the collection in the sidebar.
   * Open the **Auth** tab for the collection.
3. **Configure OAuth 2.0:**
   * Set **Auth Type** to **OAuth 2.0**.
   * Fill in the following details:

     | Field | Value |
     | --- | --- |
     | Grant Type | Authorization Code |
     | Callback URL | Must match your registered Redirect URI |
     | Auth URL | https://webexapis.com/v1/authorize |
     | Access Token URL | https://webexapis.com/v1/access_token |
     | Client ID | YOUR_CLIENT_ID |
     | Client Secret | YOUR_CLIENT_SECRET |
     | Scope | spark:people_read spark:rooms_read spark:messages_write spark:messages_read |
     | Client Authentication | Send client credentials in body |

   * Click **Get Access Token** (or equivalent) to start the flow.
4. **Authorize in Browser:**
   * Bruno will open a browser window. You'll be prompted to log in to Webex (if necessary) and grant permission to your [Your Name/ID] - Lab Integration.
   * Click **"Accept"** or **"Authorize"**.
5. **Bruno Retrieves Token:**
   * The browser will redirect, and Bruno will automatically capture the authorization code and exchange it for an access token.
   * Confirm the token is stored and associated with the collection.
6. **Verify Collection Authorization:**
   * Confirm the collection auth shows the OAuth token is active.
   * Save the collection if prompted.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing the collection **Auth** tab with OAuth 2.0 configured and an active access token for **Webex Lab Integration Token**.

## Step 2.4: Make API Requests with Your Integration Token

Now that your Bruno collection is configured to use the OAuth 2.0 flow, requests within it will use the access token obtained through your integration.

1. **Retrieve Your Own User Details (GET /v1/people/me):**
   * Open **People > Get My Own Details**.
   * Confirm auth is inherited from the collection OAuth configuration.
   * Send the request.
   * You should receive a 200 OK response with your user details. This time, the token came from your integration!
2. **Send a Message to Your Lab Space (POST Create a Message):**
   * Open **Messages > Create a Message**.
   * Replace the room ID with your lab space ID.
   * Change the text field to: `"Hello from my Webex Integration via Bruno!"`.
   * Send the request.
   * Check your Webex client – the message should appear, sent using your integration's permissions.

!!! Note "Screenshot needed"
    Add Bruno screenshot showing **POST /messages** sent successfully with the integration OAuth token.

Congratulations! You've successfully created a Webex Integration, understood the OAuth 2.0 flow, and used it to securely make API requests with Bruno. This is a fundamental concept for building robust Webex applications.
