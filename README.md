OANDA Open API
==============

**Disclaimer**: API is currently in draft and is not open to public access.

##API Request URL

	https://api-test.oanda.com

Please use this URL for all requests docuemented.


##Quick Start Guide

#### Step 1: Create a new user
	$curl -X POST -d "currency=USD" "http://api-test.oanda.com/users"

	{
    	"username" : "willymoth",
    	"password" : "balvEdayg"
	}
#### Step 2: Get account belongs to user
	$ curl "http://api-test.oanda.com/users/willymoth/accounts"

	[
    	{
        	"id" : 6531071,
        	"name" : "Primary",
        	"homecurr" : "USD",
        	"marginRate" : 0.05,
        	"accountPropertyName" : []
    	}
	]

#### Step 3: Start Trading
	$ curl -X POST -d "instrument=EUR/USD&units=1&direction=long" "http://api-test.oanda.com/accounts/6531071/trades

	{
    	"ids" : [177715575],
    	"instrument" : "EUR\/USD",
    	"units" : 2,
    	"price" : 1.30582,
    	"marginUsed" : 0.1306,
    	"direction" : "short"
	}
   

<!--##Getting started g
###Step 1: Register your application

* Go to developer.oanda.com and sign up for a developer acount.
* Register your applications on developer.oanda.com. We will assign OAuth client_id and client_secret for each of your applications. 

###Step 2: Authenticate
All requests beside rates require authentcation.  Authentication required requests require an OAuth `access_token` which can be obtained by following the [authenication guide](#Authentication) below.

###Step 3: Start Making request

```shell
$curl -X POST \
    -H "Authorization: Bearer some_access_token_mF_9.B5f-4.1JqM"
    --data-urlencode 'instrument=EUR/USD' \
    --data-urlencode 'uints=1' \
    --data-urlencode 'direction=long' \
    http://api.oanda.com/accounts/6531071/trades

{
    "ids" : [177715575],
    "instrument" : "EUR\/USD",
    "units" : 2,
    "price" : 1.30582,
    "marginUsed" : 0.1306,
    "direction" : "short"
}
```
-->

##Authentication

**Authentication is not currently not required.** No OAuth, no login, no permission required. Please don't trade with accounts that you don't own.
<!--
OANDA's API uses the [OAuth 2.0 protocol](http://tools.ietf.org/html/draft-ietf-oauth-v2-12). A successful authentication flow results in the application obtaining a user access token which can be used to make requests to OANDA's APIs.
-->

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

	GET /accounts/1/trades HTTP/1.1
	Accept: */*
	Connection: close
	User-Agent: OAuth gem v0.4.4
	Content-Type: application/x-www-form-urlencoded
	Authorization: Bearer Asf9e9f30u909u
	Host: api.oanda.com

##Scope (Permissions)

* __read__: Allows access to rates and account information
* __trade__: Allows access to open and close trades

Request and Response
------------------
OAuth token to be part of the HTTP header in all requests

    GET /accounts/1/trades HTTP/1.1
    Accept: */*
    Connection: close
    User-Agent: OAuth gem v0.4.4
    Content-Type: application/x-www-form-urlencoded
    Authorization: Bearer mF_9.B5f-4.1JqM
    Host: api.oanda.com

All requests require <code>Content-Type: application/x-www-form-urlencoded</code> unless specified otherwise.

All response will be in JSON format.

Handling errors
----------------

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

```shell
{
    "errorCode" : [OANDA error code, may or may kot be the same as the HTTP status code],
    "message"   : [a description of the error which occurred, intended for developers],
    "moreInfo"  : [a link to a web page describing the error and possible causes and solutions]
}
```

[More on error codes]()

Rate limiting
-------------

API Resources and Collections
---

| Resource | URI | Methods | Description |
| -------- | -------- | ------- | ----------- |
| [user][users]| /users/:username  | [POST](https://github.com/oanda/apidocs/blob/master/sections/users.md)    | User registration, user profile |
| [account][accounts]| /accounts/:account_id  | [GET](https://github.com/oanda/apidocs/blob/master/sections/accounts.md)    | Contains account information for a specific account |
| [account collection][accounts] | /accounts | [GET](apidocs/blob/master/sections/accounts.md) | Contains list of accounts for a specific user |
| [trade][trades] | /accounts/:account_id/trades/:trade_id | GET, PUT, DELETE | Contains info of a specific trade. |
| [trade collection][trades] | /accounts/:id/trades | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| [order][orders] | /accounts/:account_id/orders/:order_id | GET, PUT, DELETE | Contains info of a specific order. GET to retrieve info. PUT to change, DELETE to delete.|
| [order collection][orders] | /accounts/:account_id/orders | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| [position collection][positions] | /accounts/:account_id/position | GET, DELETE | Contain a list of positions for a specific account. Use GET to retrieve. DELTE to delete existing position. |
| [transaction][transactions] | /accounts/:account_id/transactions/:trans_id | GET | Contains info of a specific transaction. |
| [transaction collection][transactions] | /accounts/:account_id/transaction | GET | Contains info of a list transactions. |
| [price alert][alerts] | /accounts/:account_id/alerts/:alert_id | GET, DELETE | Contains info of a specific transaction. |
| [price alert collection][alerts] | /accounts/:account_id/alerts | GET | Contains info of a list transactions. |
| [rates][rates] | | | Market rates data. |
| [news][news] | /news/:article_id | GET | Retrieves the body of a news item. |
| [news collection][news] | /news | GET | Contains a list of news items. |
| [notification collection][notifications] | /users/:username/notifications | POST, DELETE | Contains a list of devices registered for notification for :username's accounts |


[users]: https://github.com/oanda/apidocs/blob/master/sections/users.md
[accounts]: https://github.com/oanda/apidocs/blob/master/sections/accounts.md
[trades]: https://github.com/oanda/apidocs/blob/master/sections/trades.md
[orders]: https://github.com/oanda/apidocs/blob/master/sections/orders.md
[positions]: https://github.com/oanda/apidocs/blob/master/sections/positions.md
[transactions]: https://github.com/oanda/apidocs/blob/master/sections/transactions.md
[alerts]: https://github.com/oanda/apidocs/blob/master/sections/alerts.md
[news]: https://github.com/oanda/apidocs/blob/master/sections/news.md
[rates]: https://github.com/oanda/apidocs/blob/master/sections/rates.md
[notifications]: https://github.com/oanda/apidocs/blob/master/sections/notifications.md


