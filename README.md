![](https://raw.github.com/oanda/apidocs/master/images/oanda_header.png)
=========

**Disclaimer**: The OANDA API is currently available for use in our developer sandbox, where you are free to develop and test your apps.  To use the API with production accounts, please email us at api@oanda.com.

<table>
	<tr>
		<td>
			We want to make it easy for software developers to tap into the forex market.  There are a lot of financial API's out there that leave us scratching our heads.  Enter the OANDA API.  This API will empower you to do all things forex.  Want an exchange rate?  Easy.  A list of open trades?  Done.  Notifications?  You get the idea.
			<br/><br/>
			This guide will lay out for you, step by step, what you need to get started trading forex with the OANDA API.
		</td>
		<td style="background-color:#e4e4e4"><img src="https://raw.github.com/oanda/apidocs/master/images/box.png" /></td>
	</tr>
</table>

Your first request
------------------

Before getting into the details below, let's try using the API.  Issue the following GET request using your favourite HTTP client, or just click on the link.  The response will tell you what price EUR/USD is currently trading at.  Seriously, try it out.

[http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price](http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price)

You'll get back some JSON that looks like:

    {
	    "instrument":"EUR_USD",
	    "time":1353974468.336059,
	    "bid":1.29838,
	    "ask":1.29844
	}

This response tells us that at the time we made the request (epoch time), the bid price of EUR/USD is 1.29838, and the ask price is 1.29844.

Getting started
---------------
* Check out our [getting started guide](https://github.com/oanda/apidocs/blob/master/sections/getting_started.md)
* Check out a few [example apps](https://github.com/oanda/apidocs/blob/master/sections/code_samples.md) that use the API
* Try an [API wrapper](https://github.com/oanda/apidocs/blob/master/sections/code_samples.md) for your favorite language 
* Check out our [documentation](#overview) to see what we offer
* [Generate an account](http://oanda.github.com/gen-account.html) and start hacking

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

On api-sandbox.oanda.com, there is no authentication.  You don't have to worry about credentials, session tokens, OAuth, etc.  Just make your requests and enjoy the API.

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
[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/08c4e77e4cb54028197e21a0923e9311 "githalytics.com")](http://githalytics.com/oanda/apidocs)
