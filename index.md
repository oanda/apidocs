---
title: Home | OANDA API
---

# API Home

* TOC
{:toc}

Now with the open OANDA REST API you can build applications, or even entire companies, on top of our [award winning](http://www.forexcrunch.com/forex-magnates-summit-oanda-wins-best-forex-broker-award/) 
currency trading platform, [fxTrade](http://fxtrade.com). 

What can I do with the OANDA REST API?
--------------------------------------

Our REST API gives you full access to our online Forex trading platform in real time.  You can:

* Get real-time (up to the microsecond) currency rates on over 90 pairs, including precious metals and CFDs
* Retrieve historical currency rates going back over ten years
* Open, close and modify trades on a customer trading account

[OANDA](http://www.oanda.com) is a leading forex broker enabling you to trade over 90 currency pairs, metals, and CFDs. Everything you ask for is live and real-time.  The only limit is your imagination.  If you have an idea for a product or company
built on top of our platform we want to help!  You could:

* Write automated trading strategies in any programming language 
* Create a service that provides exchange rates for eCommerce companies
* Start a business to hedge currency risks for other companies
* Download Trading Account History to generate performance reports and trading analytics

This documentation is focussed on quickly getting developers up to speed with our sandbox system.
To use our API with production systems, please send us an email at [api@oanda.com](mailto:api@oanda.com).  

Try it!
-------

<table style="border:1px solid lightgray">
  <tr>
    <td style="background-color:#EEEEEE; padding:15px">
    Issue the following GET request using your favourite HTTP client, or just click on the link.  The response will tell you what price EUR/USD is currently trading at.  Seriously, try it out.
    <br/><br/>
    <a href="http://api-sandbox.oanda.com/v1/quote?instruments=EUR_USD">http://api-sandbox.oanda.com/v1/quote?instruments=EUR_USD</a>
    <br/><br/>
    You'll see the currency pair you requested, the time the tick was published (in epoch time), the bid price, and the ask price.  All responses are in JSON.
    </td>
    <td style="background-color:#C0C0C0"><img src="https://raw.github.com/oanda/apidocs/master/images/box.png" /></td>
  </tr>
</table>

Request and Response
--------------------

Every request will use [http://api-sandbox.oanda.com/v1/](http://api-sandbox.oanda.com/v1/) as its base url; this is the endpoint for our sandbox system. If you would like to use the API on production systems, please email us at [api@oanda.com](mailto:api@oanda.com).

All requests require <code>Content-Type: application/x-www-form-urlencoded</code> unless specified otherwise.

All responses will be in [JSON format](http://www.json.org).


Errors
------

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

    HTTP/1.1 400 Bad Request

~~~json
{
    "code" : [OANDA error code, may or may not be the same as the HTTP status code],
    "message"   : [a description of the error which occurred, intended for developers],
    "moreInfo"  : [(OPTIONAL)a link to a web page describing the error and possible causes and solutions]
}
~~~

[Click here for more on error codes.](/docs/v1/troubleshooting)


Rate Limiting
-------------

Client is allowed to have no more than 4 requests per second on average, with bursts of no more than 5 requests. Excess requests will be delayed on our server.

How do I start?
---------------

* Read the [Getting Started Guide](/docs/v1/getting_started/)
* Try some [Sample Code](/docs/v1/code-samples/)
* Try the API using the [API Console](https://apigee.com/oandapoc/embed/console/oanda)
* Follow [@oandaapi](http://twitter.com/oandaapi) on Twitter
* Email us at api@oanda.com with any questions 

Disclaimer: The OANDA API is currently available for use in our developer sandbox, where you are free to develop and test your apps.  To use the API with production accounts, please email us at [api@oanda.com](mailto:api@oanda.com) or visit [developer.oanda.com](http://developer.oanda.com).

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/08c4e77e4cb54028197e21a0923e9311 "githalytics.com")](http://githalytics.com/oanda/apidocs)

