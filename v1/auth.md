---
title: Authentication | OANDA API
---

# Authentication

* TOC
{:toc}

Authentication is turned off on our sandbox system (http://api-sandbox.oanda.com)  You don't have to worry about credentials, session tokens, OAuth, etc.  Just make your requests and enjoy the API.

OANDA's API uses the [OAuth 2.0 protocol](http://tools.ietf.org/html/draft-ietf-oauth-v2-31). A successful authentication flow results in the application obtaining a user access token which can be used to make requests to OANDA's APIs.

## Obtaining an access token

1. Direct user to our authourization URL.  User will be asked to log in if they are not logged in. The user will be prompt if he/she would like to give you application access to their account.

2. Our server will redirect the user to a URI of your choice. Take the provided code parameter and exchange it for an access_token by POSTing the code to our access_token url.

<!--
2. The server will redirect the user in one of two ways that you choose:
  * __Server-side flow (reccommended)__: Redirect the user to a URI of your choice. Take the provided code parameter and exchange it for an access_token by POSTing the code to our access_token url.
  * (TODO more research on this) __Implicit flow__: Instead of handling a code, we include the access_token as a fragment (#) in the URL. This method allows applications without any server component to receive an access_token with ease.
-->

#### Server-side flow

#####Step 1: Direct user to OANDA for authorization

Direct OANDA account holder to the following URL to obtain authorization from user:

<pre><code>
  https://api.oanda.com/oauth/authorize?client_id=$APP_ID&\
                                        redirect_uri=$APP_REDIRECT_URL&\
                                        state=$UNIQUE_STRING&\
                                        response_type=code
</code></pre>

**Parameters**

* **client_id**: **required** The Application ID as provided when registering the application with OANDA.
* **redirect_url**: **required** The URL to redirect to after the user finishes the authorization flow. The URL specified must match exactly as specified in the application settings.
* **response_type**: **required** Specify **code** to request server-size flow.
* **state**: **required** A unique string used to maintain application state between the request and callback. When OANDA redirects the user back to the application redirect_uri, this parameter's value will be included in the response. This parameter is used to protect against Cross-Site Request Forgery.

#####Step 2: Receive redirect from OANDA 

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


#####Step 3: 

In order to obtain an `access_token`, you need to POST your `client_id`, `client_secret`, and `code` (authorization code obtained in step 2) to the access_token end point.

<pre><code>
curl \-F 'client_id=CLIENT-ID' \
    -F 'client_secret=CLIENT-SECRET' \
    -F 'grant_type=authorization_code' \
    -F 'redirect_uri=YOUR-REDIRECT-URI' \
    -F 'code=CODE' \https://api.oanda.com/oauth/access_token
</code></pre>

**Parameters**

* **client_id**: *required* The Application ID as provided when registering the application with OANDA.
* **client_secret**: *required* The application secret as provided when registering the application with OANDA.
* **code**: *required* The authorization code received in the previous message.
* **grant_type**: Should always be `authorization_code`
* **redirect_uri**: The `redirect_uri` you used in the authorization request.

If succeed, access_token will be provide in the following format:

    {
      "access_token": "Asf9e9f30u909u"
    }


## Using access_token

`access_token` need to be provide in the HTTP `Authorization` header. For example:

    curl -H "Authorization: Bearer Asf9e9f30u909u" https://oanda-test.apigee.net/v1/instruments

<!--  
##Scope (Permissions)

* __view_transaction_history__: Allows access to rates and account information
* __trade__: Allows access to open and close trades  
--> 

<!--
OANDA's API uses the [OAuth 2.0 protocol](http://tools.ietf.org/html/draft-ietf-oauth-v2-12). A successful authentication flow results in the application obtaining a user access token which can be used to make requests to OANDA's APIs.


#### Obtaining an access token

1. Direct user to our authourization URL.  User will be asked to log in if they are not logged in. The user will be prompt if he/she would like to give you application access to their account.

2. The server will redirect the user in one of two ways that you choose:
  * __Server-side flow__ (Authorization Code): Redirect the user to a URI of your choice. Take the provided code parameter and exchange it for an access_token by POSTing the code to our access_token url.
  * __Client-side flow__ (Implicit flow): Instead of handling a code, we include the access_token as a fragment (#) in the URL. This method allows applications without any server component to receive an access_token with ease.

#### Server-side flow

#####Step 1: Direct user to OANDA for authorization

Direct OANDA account holder to the following URL to obtain authorization from user:

	http://api.oanda.com/oauth/authorized?client_id=$APP_ID&redirect_url=$APP_REDIRECT_URL&scope=$LIST_OF_PERMISSIONS&response_type=code
	
**Parameters**

* **client_id**: **required** The Application ID as provided when registering the application with OANDA.
* **redirect_url**: **required** The URL to redirect to after the user finishes the authorization flow. The URL specified must be a URL of with the same Base Domain as specified in the application settings.
* **scope**: **required** A comma separated list of the permission being requested.
* **response_type**: **required** Specify **code** to request server-size flow.
* **state**: **optional** A unique string used to maintain application state between the request and callback. When OANDA redirects the user back to the application redirect_uri, this parameter's value will be included in the response. This parameter is used to protect against Cross-Site Request Forgery.

#####Step 2: Receive redirect from OANDA 

OANDA will provide you with authentication code by redirecting to your `redirect_url` specified in step 1.

	https://your-redirect-url?state=$UNIQUE_STR&code=$AUTH_CODE
	
**Parameters**

* **code**: The authorization code, that can be used to obtain an access token.
* **state**: The *optional* unique string that was originally specified.

If your authorization request is denied by the user, then we will redirect the user to your `redirect_uri` with error parameters:

	http://your-redirect-uri?error=access_denied&error_reason=user_denied&error_description=The+user+denied+your+request&state=$UNIQUE_STR

**Parameters**

* **error**: access_denied
* **error_reason**: user_denied
* **error_description**: The user denied your request
* **state**: The *optional* unique string that was originally specified.


#####Step 3: Exchange authentication code for access_token

	http://api.oanda.com/oauth/access_token?client_id=$APP_ID&client_secret=$APP_SECRET&code=$AUTH_CODE
	
**Parameters**

* **client_id**: *required* The Application ID as provided when registering the application with OANDA.
* **client_secret**: *required* The application secret as provided when registering the application with OANDA.
* **code**: *required* The authorization code received in the previous message.

If succeed, access_token will be provide in the following format:

	{
		"access_token": "Asf9e9f30u909u"
	}

#### Client-side flow

#####Step 1: Direct user to OANDA for authorization

Follow same instruction as [above](#step-1-direct-user-to-oanda-for-authorization) but set `response_type=token`

#####Step 2: Receive redirect from OANDA 

OANDA will provide you with access_token by redirecting to your `redirect_url` specified in step 1.

	https://your-redirect-url#state=$UNIQUE_STR&access_token=$ACCESS_TOKEN
	
If your authorization request is denied by the user, then we will redirect the user to your `redirect_uri` with error parameters:

	http://your-redirect-uri?error=access_denied&error_reason=user_denied&error_description=The+user+denied+your+request&state=$UNIQUE_STR


##### Using access_token

`access_token` need to be provide in the HTTP `Authorization` header. For example:
<pre><code>
	GET /accounts/1/trades HTTP/1.1
	Accept: */*
	Connection: close
	User-Agent: OAuth gem v0.4.4
	Content-Type: application/x-www-form-urlencoded
	Authorization: Bearer Asf9e9f30u909u
	Host: api.oanda.com
</code></pre>
##Scope (Permissions)

* __read__: Allows access to rates and account information
* __trade__: Allows access to open and close trades
-->

