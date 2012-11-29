![](https://raw.github.com/oanda/apidocs/master/images/oanda_header.png)
=========

We want to make it easy for software developers to tap into the forex market.  This guide will lay out for you, step by step, what you need to get started trading forex with the OANDA API.

What can I do with the OANDA REST API?
--------------------------------------

Our REST API lets you retrieve currency prices, historical rates and charts, and allows you to create, modify and close trades.

What can I build?
-----------------

[OANDA](http://www.oanda.com) is a leading forex broker enabling you to trade over 90 currency pairs, metals, and CFDs.  We've designed our api to not be restrictive, and so everything you ask for is live and real-time.  The only limit is your imagination.

* Build a full fledged trading app on our platform
* Create a service that provides exchange rates for eCommerce companies
* Start a business to hedge currency risks for other companies
* Implement your own trading algorithms
* Create a social, big data, mobile mashup targeted at disrupting the Gen Y crowd

Try it!
-------

Issue the following GET request using your favourite HTTP client, or just click on the link.  The response will tell you what price EUR/USD is currently trading at.  Seriously, try it out.

[http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price](http://api-sandbox.oanda.com/v1/instruments/EUR_USD/price)

You'll see the currency pair you requested, the time you made the request (in epoch time), the bid price, and the ask price.  All responses are in JSON.

How do I start?
---------------

* Read the [Getting Started Guide](https://github.com/oanda/apidocs/blob/master/sections/getting_started.md)
* Try some [sample code](https://github.com/oanda/apidocs/blob/master/sections/code_samples.md)
* Browse the [reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md)

Disclaimer: The OANDA API is currently available for use in our developer sandbox, where you are free to develop and test your apps.  To use the API with production accounts, please email us at api@oanda.com or visit [developer.oanda.com](http://developer.oanda.com).

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/08c4e77e4cb54028197e21a0923e9311 "githalytics.com")](http://githalytics.com/oanda/apidocs)

