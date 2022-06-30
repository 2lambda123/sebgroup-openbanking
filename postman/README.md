# SEB PSD2 postman collection
Get started using SEBs PSD2 products with postman collection. Mocked data is available through sandbox apps. Real data is available through production apps.

Read [SEB Developer Portal](https://developer.sebgroup.com) for more information.

### Prerequisite
You need to have your own sandbox credentials to test this collection against our sandbox. Go to [SEB Developer Portal Apps](https://developer.sebgroup.com/apps) to retrieve client ID & client secret.  

## How to use SEB PSD2 postman collection

### Step 1. Importing Collection into postman
1. Open postman
1. Select Import in the left navigation menu.
1. Select the files you want to import.
1. Select Import to bring your data into postman.
 
### Step 2. Importing Environment into postman
1. Click on the Environments tab on the left navbar of postman
1. Click on the Import button
1. Click on the Upload Files button and select 'SEB PSD2 Sandbox.postman_environment.json' file
1. Click on the Import Button
 
### Step 3. Set your own Environment  
1. Click on the Environments tab on the left navbar of postman
2. Select 'SEB PSD2 Sandbox'
3. Add your own variables for below, leave others empty. You will find below values when you create sandbox App from developer portal.  

* clientId
* clientSecret
* redirectUri (should be same one used to create a sandbox app)

![Screenshot environment variables](./images/postman_env.png)
4. Save. 
5. In the top right corner of postman, click the environment selector and select 'SEB PSD2 Sandbox'. 

###  Step 4. Test PSD2 APIs with sandbox
1. Use authorization to collect your access token.
2. Call the API with your access token.

## Authorization
The Authorization APIs provide for an authorization & authentication mechanism for SEBs APIs. These APIs are used to obtain security tokens for protected APIs. We offer the redirect and decoupled authorization and you can test both from our sandbox.
Read [PSD2 Authorization](https://developer.sebgroup.com/products/authorization) for more information.

![Screenshot authorization collection](./images/authorization_collection.png)

### Redirect Authorization 

1. Request approval for an Authorization Code
Obtaining the authorization code is an interactive process, which requires you to log in as a user. **It requires you to execute the request in the browser**   
You could find a uri to copy by clicking [Send] button from [GET] Request approval for an Authorization Code (look at the console window in the bottom of your postman)
  ``` 
  https://api-sandbox.sebgroup.com/mga/sps/oauth/oauth20/authorize?client_id={{clientId}}&scope=psd2_accounts&redirect_uri={{redirectUri}}&response_type=code&state=&brandid
  ``` 
 
Add your **8 digits** sandbox identity number, you can find a list of available identity numbers from [SEB Developer Portal](https://developer.sebgroup.com/products/authorization/redirect-authorization#/authorize-get):

![Screenshot redirect authorization](./images/authorization.png)

You can find authorization code in your browser:

![Screenshot redirect authorization code](./images/authorization_code.png)

2. Request access token for Authorization Code or Refresh Token
Add collected authorization code provided by the /authorize endpoint to body.

![Screenshot add redirect authorization code](./images/authorization_code_form.png)

3. Use the access token to access an API

[Redirect Authorization step by step guide](https://developer.sebgroup.com/products/authorization/redirect-authorization) 

### Decoupled Authorization 

1. Initiate authorization request [Send], no need to change any values. This step returns an authorization registration id: auth_req_id. You can change the preferred start_mode to "qr", where ast returns an autostarttoken (for users with Bank ID on the same device) and qr returns a qr code string.
  ``` 
    {
      "client_id": "{{clientId}}",
      "lang": "en",
      "scope": "{{scope}}",
      "start_mode": "ast"
    }
  ``` 
2. Check status [Send], no need to change any values. You will see response with status "Pending" 
  ``` 
    {
        "status": "PENDING",
        "hint_code": "OUTSTANDING_TRANSACTION",
        "autostart_token": "e0aa878a-cabc-4f3b-b805-0f988ea88b72",
        "poll_delay": 500
    }   
  ``` 
3. Bank ID simulation for sandbox  
Add your **12 digits** sandbox identity number, you can find a list of available identity numbers from [SEB Developer Portal](https://developer.sebgroup.com/products/authorization/decoupled-authorization).
Response code 204 No Content for successful login.

![Screenshot authorization mock login](./images/authorization_mock_login.png)
 
4. Check status [Send] again to see status
  ``` 
    {
        "status": "COMPLETE",
        "poll_delay": 500
    }
  ``` 
5. Retrieve access token [Send]

6. Use the access token to access an API

Read more [Decoupled Authorization step by step guide](https://developer.sebgroup.com/products/authorization/decoupled-authorization) 

Now you are ready to call SEB PSD2 APIs with your access token.
