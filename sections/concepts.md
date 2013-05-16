API Structure
-------------

The [Trading API reference documentation](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview) describes the URI structure as follows:

    http://api-sandbox.oanda.com  /v1       /accounts    /12345     /trades      /987654321
             ----------           --------- ------------ ---------- ------------ ----------
             [Base URL]           [Version] [Collection] [Resource] [Collection] [Resource]

Each URI performs a different function depending on whether you issue a GET, POST, PUT, or DELETE request.

##### Base URL
[http://api-sandbox.oanda.com/v1/](http://api-sandbox.oanda.com/v1/)

All requests will use this as the base URL.

##### Accounts
[http://api-sandbox.oanda.com/v1/accounts/6531071](http://api-sandbox.oanda.com/v1/accounts/6531071)

Your account id is always the base URI to any trading request.  If you issue a GET request with this URL, you'll see details for account 6531071.

##### Collection
[http://api-sandbox.oanda.com/v1/accounts/6531071/trades](http://api-sandbox.oanda.com/v1/accounts/6531071/trades)

Every account has trades, so if you issue this as a GET request, you'll get a list of currently open trades for that account.

##### Resource
[http://api-sandbox.oanda.com/v1/accounts/6531071/trades/177810368](http://api-sandbox.oanda.com/v1/accounts/6531071/trades/177810368)

If you already have a unique id for a trade, you can issue this as a GET request to get more details about trade 177810368.

##### Place a trade
To place a trade, you use the **same** URL as you would to get a list of open trades, but issue a POST request instead.  The POST data will describe details of the trade you wish to open.  The example below uses curl, passing three parameters (instrument, units, and direction) but you can use any HTTP client.

	$ curl -X POST -d "instrument=EUR_USD&units=1000&side=buy" http://api-sandbox.oanda.com/accounts/6531071/trades

Sample response:

	{
		"opened" : 178117474,
		"updated" : 0,
		"closed" : [],
		"interest" : [],
		"instrument" : "EUR_USD",
		"units" : 1000,
		"side" : "buy",
		"price" : 1.28861,
		"marginUsed" : 64.4305,
		"time" : "2013-05-16T19:21:24Z"
	}
