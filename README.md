![](https://raw.github.com/oanda/apidocs/master/images/oanda_header.png)
=========

**Disclaimer**: The OANDA API is currently available for use in our developer sandbox, where you are free to develop and test your apps.  To use the API with production accounts, please email us at api@oanda.com.

<table>
	<tr>
		<td>
			We want to make it easy for software developers to tap into the forex market.  There are a lot of financial API's out there that leave us scratching our heads.  Enter the OANDA API.  This API will empower you to do all things forex.  Want an exchange rate?  Easy.  A list of open trades?  Done.  Notifications?  Childs play.
			<br/><br/>
			This guide will lay out for you, step by step, what you need to get started trading forex with the OANDA API.
		</td>
		<td><img src="https://raw.github.com/oanda/apidocs/master/images/box.png" /></td>
	</tr>
</table>

Your first request
------------------

Before getting into the details below, let's try using the API.  Issue the following GET request using your favourite HTTP client.  The response will tell you what price EUR/USD is currently trading at.  Seriously, try it out.

    http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price

You'll get back some JSON that looks like:

    {
	    "instrument":"EUR_USD",
	    "time":1353974468.336059,
	    "bid":1.29838,
	    "ask":1.29844
	}

This response tells us that at the time we made the request (epoch time), the bid price of EUR/USD is 1.29838, and the ask price is 1.29844.

Getting Started
---------------
* Check out our [getting started guide](https://github.com/oanda/apidocs/blob/master/sections/getting_started.md)
* Check out a few [example apps](https://github.com/oanda/apidocs/blob/master/sections/getting_started.md#examples) that use the API
* Check out our [documentation](#overview) to see what we offer

Describing the API
------------------
All requests will use http://api-sandbox.oanda.com as the base url, and follow the convention in the diagram below.

![](https://raw.github.com/oanda/apidocs/master/images/api_url.png)

* An action (CRUD) can be performed on a resource
* A resource belongs to a collection
* A collection exists under a version

One or more CRUD (Create, Read, Update, Delete) operations can be performed on a single URL, depending on the action desired.

<table>
	<tr>
		<td>POST (Create)</td>
		<td>POST requests are used to submit new data</td>
	</tr>
	<tr>
		<td>GET (Read)</td>
		<td>GET requests are used to read data</td>
	</tr>
	<tr>
		<td>PUT (Update)</td>
		<td>PUT requests are used to modify existing data</td>
	</tr>
	<tr>
		<td>DELETE (Delete)</td>
		<td>DELETE requests are used to delete data</td>
	</tr>
</table>

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

In order to trade, you will need an OANDA account id.  Creating a new user will automatically generate a new account id for you.  While our API is still being drafted, you will not need your username/password for any purpose other than creating an account.  Please be respectful of others by not trading on accounts that you don't own.

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
-->

Request and Response
------------------
<!--
OAuth token to be part of the HTTP header in all requests

    GET /accounts/1/trades HTTP/1.1
    Accept: */*
    Connection: close
    User-Agent: OAuth gem v0.4.4
    Content-Type: application/x-www-form-urlencoded
    Host: api.oanda.com
-->
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
<!--
Rate limiting
-------------
-->

Trading API Overview
---

| Resource | URI | Methods | Description |
| -------- | -------- | ------- | ----------- |
| [user][users]| /users/:username  | [POST](sections/users.md#post-users)    | User registration, user profile |
| [account][accounts]| /accounts/:account_id  | [GET](sections/accounts.md#get-accountsaccount_id)    | Contains account information for a specific account |
| [account collection][accounts] | /accounts | [GET](sections/accounts.md#get-accounts) | Contains list of accounts for a specific user |
| [trade][trades] | /accounts/:account_id/trades/:trade_id | [GET](sections/trades.md#get-tradestrade_id), [PUT](sections/trades.md#put-tradestrade_id), [DELETE](sections/trades.md#delete-tradestrade_id) | Contains info of a specific trade. |
| [trade collection][trades] | /accounts/:id/trades | [GET](sections/trades.md#get-accountsaccount_idtrades), [POST](sections/trades.md#post-accountsaccount_idtrades) | Contain a list of trade for a specific account. Use POST to create new trades |
| [order][orders] | /accounts/:account_id/orders/:order_id | [GET](sections/orders.md#get-accountsaccount_idorders), [PUT](sections/orders.md#put-accountsaccount_idorders), [DELETE](sections/orders.md#delete-accountsaccount_idorders) | Contains info of a specific order. GET to retrieve info. PUT to change, DELETE to delete.|
| [order collection][orders] | /accounts/:account_id/orders | [GET](), POST | Contain a list of trade for a specific account. Use POST to create new trades |
| [position collection][positions] | /accounts/:account_id/position | GET, DELETE | Contain a list of positions for a specific account. Use GET to retrieve. DELTE to delete existing position. |
| [transaction][transactions] | /accounts/:account_id/transactions/:trans_id | GET | Contains info of a specific transaction. |
| [transaction collection][transactions] | /accounts/:account_id/transaction | GET | Contains info of a list transactions. |
| [rates][rates] | | | Market rates data. |

<!--
| [price alert][alerts] | /accounts/:account_id/alerts/:alert_id | GET, DELETE | Contains info of a specific transaction. |
| [price alert collection][alerts] | /accounts/:account_id/alerts | GET | Contains info of a list transactions. |
| [news][news] | /news/:article_id | GET | Retrieves the body of a news item. |
| [news collection][news] | /news | GET | Contains a list of news items. |
| [notification collection][notifications] | /users/:username/notifications | POST, DELETE | Contains a list of devices registered for notification for :username's accounts |
-->


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
[quick_start]: https://github.com/oanda/apidocs/blob/master/sections/getting_started.md
[examples]: https://github.com/oanda/apidocs/blob/master/sections/getting_started.md#examples


