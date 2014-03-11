---
title: Authentication | OANDA API
---

# Authentication

* TOC
{:toc}

## Overview

Authentication is turned off on our sandbox system (http://api-sandbox.oanda.com)  You don't have to worry about credentials, session tokens, OAuth, etc.  Just make your requests and enjoy the API.

Authentication is required to access your live accounts. Application developers will need to use the OAuth 2.0 flow [described below](#oauth-authentication), while personal traders can request a personal access token.

----------------

## Personal Strategy Traders

A personal access token can be used to access your account through the OANDA Open API. Once created, a token will grant access all of your sub-accounts. Please note, your personal access token is like a password, so you should guard it carefully. These tokens are unique to an OANDA account and should be stored securely.

#### Obtaining a Personal Access Token

Members of the Open API private beta will be provided with a link on their OANDA fxTrade account profile page titled "Manage API Access" (My Account -> My Services -> Manage API Access). From there, you can generate a personal access token to use with the OANDA Open API, as well as revoke a token you may currently have. To apply for the private beta program, [sign up here](/docs/beta-signup).

#### Using a Personal Access Token

After generating your token, you should keep it somewhere secure. OANDA does not retain your token so if it is lost or forgotten you must revoke it and generate a new one to keep API access.

In order to use a token to access API resources, you must include the token as a Bearer token in the HTTP `Authorization` header. As an example:

~~~
curl -H "Authorization: Bearer 12345678900987654321-abc34135acde13f13530"
    https://api-fxpractice.oanda.com/v1/accounts
~~~

If you open new subaccounts or change your password, you should revoke and regenerate your token to ensure proper access to your accounts. 

<!--
---------------------


## Partners

OANDA's API uses the [OAuth 2.0 protocol](http://tools.ietf.org/html/draft-ietf-oauth-v2-31). A successful authentication flow results in the application obtaining a user access token which can be used to make requests to OANDA's APIs.

#### Registering Your Application

OAuth is not available for the initial start of the private beta program yet (we might be able to add it closer to the end of the private beta). However, OAuth will be a certain feature for the public launch.

In the meantime, you can get yourself familiar with the protocol by reading the following:


#### Obtaining an Access Token

1. Direct user to our authourization URL.  User will be asked to log in if they are not logged in. The user will be prompt if he/she would like to give you application access to their account.

2. Our server will redirect the user to a URI of your choice. Take the provided code parameter and exchange it for an access_token by POSTing the code to our access_token URL.

##### Server-side flow

######Step 1: Direct user to OANDA for authorization

Direct OANDA account holder to the following URL to obtain authorization from user:

~~~
https://api-fxpractice.oanda.com/oauth2/authorize?client_id=$APP_ID&\
                                        redirect_uri=$APP_REDIRECT_URL&\
                                        state=$UNIQUE_STRING&\
                                        response_type=code
~~~

**Parameters**

* **client_id**: **required** The Application ID as provided when registering the application with OANDA.
* **redirect_url**: **required** The URL to redirect to after the user finishes the authorization flow. The URL specified must match exactly as specified in the application settings.
* **response_type**: **required** Specify **code** to request server-size flow.
* **state**: **required** A unique string used to maintain application state between the request and callback. When OANDA redirects the user back to the application redirect_uri, this parameter's value will be included in the response. This parameter is used to protect against Cross-Site Request Forgery.

######Step 2: Receive redirect from OANDA 

OANDA will provide you with authentication code by redirecting to your `redirect_url` specified in step 1.

  https://your-redirect-url?state=$UNIQUE_STR&code=$AUTH_CODE
  
**Parameters**

* **code**: The authorization code, that can be used to obtain an access token.
* **state**: The unique string that was originally specified.

If your authorization request is denied by the user, then we will redirect the user to your `redirect_uri` with error parameters:

  http://your-redirect-uri?error=access_denied&error_reason=user_denied&error_description=The+user+denied+your+request

**Parameters**

* **error**: access_denied
* **error_reason**: user_denied
* **error_description**: The user denied your request


######Step 3: 

In order to obtain an `access_token`, you need to POST your `client_id`, `client_secret`, and `code` (authorization code obtained in step 2) to the access_token end point.

~~~
curl \-d 'client_id=CLIENT-ID' \
    -d 'client_secret=CLIENT-SECRET' \
    -d 'grant_type=authorization_code' \
    -d 'redirect_uri=YOUR-REDIRECT-URI' \
    -d 'code=CODE' \https://api-fxpractice.oanda.com/oauth/access_token
~~~

**Parameters**

* **client_id**: *required* The Application ID as provided when registering the application with OANDA.
* **client_secret**: *required* The application secret as provided when registering the application with OANDA.
* **code**: *required* The authorization code received in the previous message.
* **grant_type**: Should always be `authorization_code`
* **redirect_uri**: The `redirect_uri` you used in the authorization request.

If succeed, access_token will be provide in the following format:

~~~json
{
  "access_token": "Asf9e9f30u909u",
  "token_type": "Bearer",
  "expires_in": 0
}  
~~~

#### Using an Access Token

`access_token` need to be provide in the HTTP `Authorization` header. For example:

    curl -H "Authorization: Bearer Asf9e9f30u909u" https://api-fxpractice.oanda.com/v1/instruments?accountId=12345

-->