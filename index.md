---
title: Home | OANDA API
---

---------


What can I do with the OANDA REST API?
--------------------------------------

We developed our API on top of our [award winning](http://www.forexcrunch.com/forex-magnates-summit-oanda-wins-best-forex-broker-award/) 
currency trading platform, [fxTrade](http://fxtrade.com). 

#### Market Data

Get real-time currency rates on over 90 currency pairs. Monitor the forex market for changes in real-time, 24 hours a day. You will have access to historical currency rates, dating back over 10 years.

#### Trading

Place trades and orders with our trading API.  You can fetch account activites, balance, trades and orders.

----

API URLs
--------------------

There are 3 different environments available for the REST API.

|Environment|URL|Authentication|
|---|---|---|---|
|Sandbox|http://api-sandbox.oanda.com|No authentication required|
|FxTrade Practice|https://api-fxpractice.oanda.com|Required. [Details Here](/docs/v1/auth/)|
|FxTrade|https://api-fxtrade.oanda.com|Required. [Details Here](/docs/v1/auth/)|

<br/>

Our documentation uses the sandbox URL for all examples. To use a different environment simply replace the base of the url with the appropriate one listed above and follow any necessary authentication.

The sandbox environment is a test bed used to showcase our upcoming REST API and is open to everyone to use. The sandbox environment currently has the following limitation:

* Generated (fake) market data
* No order monitoring.  Limiting orders will not be triggered.

OANDA API on production is currently in a closed beta period with a limited number of slots.  We will be opening up our beta program in the near future.  

----

Request and Response
--------------------

All requests require <code>Content-Type: application/x-www-form-urlencoded</code> unless specified otherwise.

All responses will be in [JSON format](http://www.json.org).

----


Rate Limiting
-------------

Client is allowed to have no more than 15 requests per second on average, with bursts of no more than 5 requests. Excess requests will be delayed on our server.

----


How do I start?
---------------

* Read the [Development Guide](/docs/v1/getting-started/)
* Try some [Sample Code](/docs/v1/code-samples/)
* Follow [@oandaapi](http://twitter.com/oandaapi) on Twitter
* Email us at api@oanda.com with any questions 

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/08c4e77e4cb54028197e21a0923e9311 "githalytics.com")](http://githalytics.com/oanda/apidocs)

