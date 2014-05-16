---
title: Authentication | OANDA API
---

# Authentication

* TOC
{:toc}

## Overview

Authentication is turned off on our sandbox system (http://api-sandbox.oanda.com)  You don't have to worry about credentials, session tokens, OAuth, etc.  Just make your requests and enjoy the API.

Authentication is required to access your live accounts. Application developers will need to use the OAuth 2.0 flow [described below](#third-party-applications), while personal traders can request a personal access token.

----------------

## Personal Strategy Traders

A personal access token can be used to access your account through the OANDA Open API. Once created, a token will grant access all of your sub-accounts. Please note, your personal access token is like a password, so you should guard it carefully. These tokens are unique to an OANDA account and should be stored securely.

#### Obtaining a Personal Access Token

Members of the Open API private beta will be provided with a link on their OANDA fxTrade account profile page titled "Manage API Access" (My Account -> My Services -> Manage API Access). From there, you can generate a personal access token to use with the OANDA Open API, as well as revoke a token you may currently have. To apply for the private beta program, [sign up here](/beta-signup).

#### Using a Personal Access Token

After generating your token, you should keep it somewhere secure. OANDA does not retain your token so if it is lost or forgotten you must revoke it and generate a new one to keep API access.

In order to use a token to access API resources, you must include the token as a Bearer token in the HTTP `Authorization` header. As an example:

~~~
curl -H "Authorization: Bearer 12345678900987654321-abc34135acde13f13530"
    https://api-fxpractice.oanda.com/v1/accounts
~~~

If you open new subaccounts or change your password, you should revoke and regenerate your access token to ensure proper access to all your accounts. 


----------------

## Third Party Applications

OANDA supports web based third party applications to access OANDA API on behalf of OANDA users.  OANDA's API uses the [OAuth 2.0 protocol](http://tools.ietf.org/html/draft-ietf-oauth-v2-31) to provide this capability.  It is the responsibility of the third party application to successfully complete the server-side flow to obtain the required access token.

Once the access token has been obtained, your application can use it in the same manner a [personal access token](#personal-access-token) is used.

### Register Your Application

Contact api@oanda.com to register your application with OANDA.  Please clearly state in the email that you would like to register your application with the OANDA API and provide the following information.

* Application name
* Application description
* Authorized redirect URI (The HTTP redirect URIs must be protected with TLS security)

Once all required information is received and approved by OANDA*, we will then provide you with the following credentials. 

* Client Application Id
* Client Application Secret

Please treat the client application secret as a password and keep it in a secure location.

*Please note: OANDA does not guarantee that your application will be accepted. All applications are subjected to OANDA due diligence review. If you are successful in your application, please note that you will be bound by the relevant terms and conditions stipulated by OANDA.
  
### Server-side flow

Obtaining an access token is a three step process.


1. [Direct user to the OANDA OAuth authorization endpoint.  The user will be prompted to login to OANDA and grant permission for your application to access their accounts.](#step1)  

2. [Upon completion of the above step, OANDA servers will redirect the user to your application's registered redirect URI.  Assuming the above step was successful, OANDA will include a unique authorization code with the redirect request.](#step2)  

3. [Your application will then make a request to OANDA's access token endpoint to exchange the authorization code for an access token.  Your application will use the access token to act on behalf of the user.](#step3)  


####Subdomain

The subdomain for the request is dependent on the environment you wish to obtain access tokens for.

|Environment|Subdomain|
|---|---|
|fxTrade Practice|api-fxpractice.oanda.com|
|fxTrade|api-fxtrade.oanda.com|


####Step 1: Direct the user's browser to OANDA's authorization endpoint<a name="step1"></a>

~~~
GET /v1/oauth2/authorize
~~~  


#####Request Query Parameters  

client_id
: The client application id as provided when registering the application with OANDA.

redirect_uri
: The redirect URI must exactly match the value that the application was registered with.

response_type
: Specify '**code**' to request server-side flow.

state
: A unique token to maintain application state between the request and callback. This parameter and token value will be included in the OANDA redirect response.  Your application must verify that the token returned matches the token that you have specified.   OANDA recommends that this token be generated using a high-quality random-number generator.

scope
: A list of permission that your application requires.  Permissions are separated by the '+' character.  See [here](#permissions) for full list and description.
  
#####Example

~~~
https://api-fxpractice.oanda.com/v1/oauth2/authorize?client_id=CLIENT_ID&redirect_uri=https://oanda-oauth-example.com/acceptcode&state=STATE_TOKEN&response_type=code&scope=read+trade+marketdata+stream
~~~


####Step 2: Receive redirect from OANDA<a name="step2"></a>

If the user consents to grant access to your application, the user will then be redirected to the `redirect_uri` with the following parameters appended.

#####Redirect Query Parameters

state
: The unique token that your application specified in the original request.  Your application must verify that this token matches what was specified before continuing to the next step.

code
: A one-time `authorization code` that is exchanged for an access token in the next step.

#####Example
~~~
  https://oanda-oauth-example.com/acceptcode?state=STATE_TOKEN&code=AUTH_CODE
~~~  

If your authorization request is denied by the user, OANDA will redirect the user to the `redirect_uri` with the following parameter appended.

#####Redirect Query Parameters

error
: reason for error

##### Example
~~~
  https://oanda-oauth-example.com/acceptcode?error=access_denied
~~~

####Step 3: Exchange authorization code for access token<a name="step3"></a>

Perform an HTTPS POST request to OANDA's /v1/oauth2/access_token endpoint with the required parameters in the request body.

~~~
POST /v1/oauth2/access_token

client_id=<client_application_id> \
client_secret=<client_application_secret> \
grant_type=authorization_code \
redirect_uri=<redirect_uri> \
code=<authorization_code> \
~~~

#####Request Body Parameters

client_id
: The client application id as provided when registering the application with OANDA.

client_secret
: The application secret as provided when registering the application with OANDA.

grant_type
: Specify '**authorization_code**' for this parameter.

code
: The `authorization code` received in the previous message.

redirect_uri
: The redirect URI must exactly match the value that the application was registered with.


#####Example

~~~

curl -X POST "https://api-fxpractice.oanda.com/v1/oauth2/access_token" -d "client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTH_CODE&redirect_uri=https://oanda-oauth-example.com/acceptcode"
~~~

#####Response


######Response Body Parameters

access_token
: The access token your application will need to submit when making authenticated requests to the OANDA API on behalf of the user.

token_type
: The type of token returned.  This value must be specified along with the access token when making authenticated requests.

expires_in
: The time in seconds when the access token will expire.  A value of 0 denotes that the access token will not expire.

#####Example

~~~json
{
  "access_token": "ACCESS-TOKEN",
  "token_type": "Bearer",
  "expires_in": 0
}
~~~

----------------

###Permissions

The following table lists available permissions and describes the type of access it provides.  Note, permissions are in lower case.

|Permission|Description|
|---|---|
|read|This permission grants access to view account information and transaction history on all accounts of the user|
|trade|This permission grants access to perform trading activity on all accounts of the user|
|marketdata|This permission grants access to view market data associated to all accounts of the user|
|stream|This permission grants access to streams associated to all accounts of the user|


