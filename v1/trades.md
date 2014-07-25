---
title: Trades | OANDA API
---

# Trade Endpoints

* TOC
{:toc}

----------------------------

## Get a list of open trades

    GET /v1/accounts/:account_id/trades

#### Input Query Parameters

maxId
: _Optional_  The server will return trades with id less than or equal to this, in descending order (for pagination).

count
: _Optional_ Maximum number of open trades to return. Default: 50 Max value: 500

instrument
: _Optional_ Retrieve open trades for a specific instrument only Default: all

ids
: _Optional_ A (URL encoded) comma separated list of trades to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/trades?instrument=EUR_USD&count=2"

####Response

######Header

~~~Header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 449
Link: <http://api-sandbox.oanda.com/v1/accounts/12345/trades?count=2&instrument=EUR_USD&maxId=175427741>; rel="next"
X-Result-Count: 3
~~~

######Body

~~~json
{
  "trades" : [
    {
            "id" : 175427743,
            "units" : 2,
            "side" : "sell",
            "instrument" : "EUR_USD",
            "time" : "2014-02-13T17:47:57Z",
            "price" : 1.36687,
            "takeProfit" : 0,
            "stopLoss" : 0,
            "trailingStop" : 0
            "trailingAmount" : 0
    },
    {
            "id" : 175427742,
            "units" : 2,
            "side" : "sell",
            "instrument" : "EUR_USD",
            "time" : "2014-02-13T17:47:56Z",
            "price" : 1.36687,
            "takeProfit" : 0,
            "stopLoss" : 0,
            "trailingStop" : 0,
            "trailingAmount" : 0,
    }
  ]
}
~~~

####Pagination

Trades can be paginated with the count and maxId parameters.
At most, a maximum of 50 trades can be returned in one query. 
If more trades exist than specified by the given or default count, a url with maxId set to the next unreturned trade will be returned within the Link header..

----

## Get information on a specific trade

    GET /v1/accounts/:account_id/trades/:trade_id

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### Response

######Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 193
~~~

######Body

~~~json
{
  "id" : 43211,                          // The ID of the trade
  "units" : 5,                           // The number of units in the trade
  "side" : "buy",                        // The direction of the trade
  "instrument" : "EUR_USD",              // The symbol of the instrument of the trade
  "time" : "2013-07-03T14:30:38Z",       // The time of the trade (in RFC3339 format)
  "price" : 1.45123,                     // The price the trade was executed at
  "takeProfit" : 1.7,                    // The take-profit associated with the trade, if any
  "stopLoss" : 1.4,                      // The stop-loss associated with the trade, if any
  "trailingStop" : 50                    // The trailing stop associated with the trade, if any
  "trailingAmount" : 1.44613             // The trailing amount associated with the trade, if any.
                                            Returns -1 if our trailing amount server is unavailable
}
~~~

----

## Modify an existing trade

    PATCH /v1/accounts/:account_id/trades/:trade_id

#### Input Data Parameters (inside body)
Note: Only the specified parameters will be modified. All other parameters will remain unchanged. To remove an optional parameter, set its value to 0.

stopLoss
: _Optional_ Stop Loss value

takeProfit
: _Optional_ Take Profit value

trailingStop
: _Optional_ Trailing Stop distance in pips, up to one decimal place

#### Example
    $curl -X PATCH -d "stopLoss=1.6" "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### Response

######Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 193
~~~

######Body

~~~json
{
  "id" : 43211,                          // The ID of the trade
  "units" : 5,                           // The number of units in the trade
  "side" : "buy",                        // The direction of the trade
  "instrument" : "EUR_USD",              // The symbol of the instrument of the trade
  "time" : "2013-07-03T14:30:38Z",       // The time of the trade (in RFC3339 format)
  "price" : 1.45123,                     // The price the trade was executed at
  "takeProfit" : 1.7,                    // The take-profit associated with the trade, if any
  "stopLoss" : 1.6,                      // The stop-loss associated with the trade, if any
  "trailingStop" : 50                    // The trailing stop associated with the trade, if any
  "trailingAmount" : 1.44613             // The trailing amount associated with the trade, if any.
                                            Returns -1 if our trailing amount server is unavailable
}
~~~

----

## Close an open trade

    DELETE /v1/accounts/:account_id/trades/:trade_id

#### Example
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### Response

######Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 140
~~~

######Body

~~~json
{
  "id" : 54332,                   // The ID of the close trade transaction
  "price" : 1.30601,              // The price the trade was closed at
  "instrument" : "EUR_USD",       // The symbol of the instrument of the trade
  "profit" :  0.005,              // The realized profit of the trade in units of base currency
  "side" : "sell"                 // The direction the trade was in
  "time" : "2013-07-12T16:09:15Z" // The time at which the trade was closed (in RFC3339 format)
}
~~~
