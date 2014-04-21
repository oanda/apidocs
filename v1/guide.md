---
title: Development Guide | OANDA API
---

# Development Guide


* TOC
{:toc}



------


API URLs
--------------------

There are 3 different environments available for the REST API.

|Environment|URL|Authentication|Description|
|---|---|---|---|---|
|Sandbox|**http**://api-sandbox.oanda.com|No authentication required|An environment purely for testing; it is not as fast, stable and reliable as the other environments (i.e. it can go down once in a while).|
|fxTrade Practice|**https**://api-fxpractice.oanda.com|Required. [Details Here](/docs/v1/auth/)|A stable environment; recommended for testing with your fxTrade Practice account and your personal access token.|
|fxTrade|**https**://api-fxtrade.oanda.com|Required. [Details Here](/docs/v1/auth/)|A stable environment; recommended for production-ready code to execute with your fxTrade account and your personal access token.|

There are also 3 different environments for our Streaming API.

|Environment|URL|Authentication|Description|
|---|---|---|---|---|
|Sandbox|**http**://stream-sandbox.oanda.com|No authentication required|An environment purely for testing; it is not as fast, stable and reliable as the other environments (i.e. it can go down once in a while).|
|fxTrade Practice|**https**://stream-fxpractice.oanda.com|Required. [Details Here](/docs/v1/auth/)|A stable environment; recommended for testing with your fxTrade Practice account and your personal access token.|
|fxTrade|**https**://stream-fxtrade.oanda.com|Required. [Details Here](/docs/v1/auth/)|A stable environment; recommended for production-ready code to execute with your fxTrade account and your personal access token.|

<br/>

Our documentation uses the sandbox URL for all examples. To use a different environment simply replace the base of the url with the appropriate one listed above and follow any necessary authentication (Note: replace the http or https as necessary as well).

The sandbox environment is a test bed used to showcase our REST API and is open to everyone to use. The sandbox environment currently has the following limitations:

* Generated (fake) market data
* No order monitoring.  Limiting orders will not be triggered.

----

Request and Response Details
--------------------

All requests require `Content-Type: application/x-www-form-urlencoded` unless specified otherwise.

All responses will be in [JSON format](http://www.json.org).

All endpoints also support the HTTP OPTIONS verb, and will respond with a `Access-Control-Allow-Methods` header listing the available verbs for the endpoint.

----

Rate Limiting
-------------

Client is allowed to have no more than 15 requests per second on average, with bursts of no more than 15 requests. Excess requests will be rejected.

----


## Get real time currency prices

Currencies, metals and CFD prices change multiple times per second. To get a price, specify the instrument name you want to retrieve, for
example EUR/USD.  Replace the '/' character with an underscore '_' in currency pair names.

#### Example
Get the current price of EUR/USD

    $curl -X GET "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 139
~~~

###### Body

~~~json
{
  "prices" : [
    {
      "instrument" : "EUR_USD",
      "time" : "2013-09-16T18:59:03.687308Z",
      "bid" : 1.33319,
      "ask" : 1.33326
    }
  ]
}
~~~

----

## Get historical prices and charts

The API can also be used to get both current and historical candles for a variety of uses, including creating your own charts.

#### Example
Get two of the most recent candles for EUR/USD
  
    $curl -X GET "http://api-sandbox.oanda.com/v1/candles?instrument=EUR_USD&count=2"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 621
~~~

###### Body

~~~json
{
  "instrument" : "EUR_USD",
  "granularity" : "S5",
  "candles" : [
    {
      "time" : "2014-02-12T14:50:25Z",
      "openBid" : 1.35757,
      "openAsk" : 1.35768,
      "highBid" : 1.35759,
      "highAsk" : 1.35769,
      "lowBid" : 1.35745,
      "lowAsk" : 1.35757,
      "closeBid" : 1.35745,
      "closeAsk" : 1.35757,
      "volume" : 11,
      "complete" : true
    },
    {
      "time" : "2014-02-12T14:50:30Z",
      "openBid" : 1.35746,
      "openAsk" : 1.35757,
      "highBid" : 1.35746,
      "highAsk" : 1.35758,
      "lowBid" : 1.35742,
      "lowAsk" : 1.35753,
      "closeBid" : 1.35742,
      "closeAsk" : 1.35753,
      "volume" : 8,
      "complete" : false
    }
  ]
}
~~~

#### Sample Code
[Candle Average Price](https://github.com/oanda/cl-restapi-demo) is written in Lisp, and will calculate the average price of a currency pair over the past 'X' days.

#### Reference
[Reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md) for historical prices and charts.

----

## Trade currencies, metals, and CFD's

#### Example
Open a buy EUR/USD trade for 1000 units.  This example uses curl to submit three parameters using POST data.

    $curl -X POST -d "instrument=EUR_USD&units=1000&side=buy&type=market" http://api-sandbox.oanda.com/v1/accounts/6531071/orders

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 265
~~~

###### Body

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z", // Time that order was executed
  "price" : 1.37041,               // Trigger price of the order
  "tradeOpened" : {
    "id" : 175517237,              // Order id
    "units" : 1000,                // Number of units
    "side" : "buy",                // Direction of the order
    "takeProfit" : 0,              // The take-profit associated with the Order, if any
    "stopLoss" : 0,                // The stop-loss associated with the Order, if any
    "trailingStop" : 0             // The trailing stop associated with the rrder, if any
  },
  "tradesClosed" : [],
  "tradeReduced" : {}
}
~~~

#### Sample Code
[Api Trading](https://github.com/oanda/py-api-trading) is written in Python, and demonstrates opening trades and orders.

#### Reference
[Reference documentation](/docs/v1/trades) for opening trades and orders.

----

This is just scratching the surface, check out everything you can do in the sidebar.

