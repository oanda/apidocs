---
title: Orders | OANDA API
---

# Order Endpoints

* TOC
{:toc}


## Get orders for an account

    GET /v1/accounts/:account_id/orders

#### Input Query Parameters

maxId
: _Optional_ The server will return orders with id less than or equal to this, in descending order (for pagination)

count
: _Optional_ Maximum number of open orders to return. Default: 50 Max value: 500

instrument
: _Optional_ Retrieve open orders for a specific instrument only Default: all

ids
: _Optional_ A comma separated list of orders to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&count=4"

#### Response

###### Header

~~~
Link: <http://api-sandbox.oanda.com/accounts/12345/orders?count=4&maxId=78>; ref="next"
~~~

###### Body

~~~json
{
  "orders" : [
    { "id" : 12345, "type": "marketIfTouched", "side" : "buy", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 0},
    { "id" : 12344, "type": "marketIfTouched", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
    { "id" : 890, "type": "limit", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
    { "id" : 789, "type": "stop", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1}
  ],
}
~~~

####Pagination

Orders can be paginated with the count and maxId parameters.
At most, a maximum of 50 orders can be returned in one query. 
If more orders exist than specified by the given or default count, a url with maxId set to the next unreturned order will be returned within the Link header.

----

## Create a new order

    POST /v1/accounts/:account_id/orders


#### Input Data Parameters

**Required**

instrument
: _Required_ Instrument to open Order on

units
: _Required_ Number of units to open Order for

side
: _Required_ 'buy' or 'sell'

type
: _Required_ 'limit', 'stop', 'marketIfTouched' or 'market' 

<!--* **type**: entry (default), or limit (More about order types) -->

expiry
: _Required_ If order type is not 'market'. UTC Time (in RFC3339 Format) when order expires

price
: _Required_ If order type is not 'market'. Price where order is set to trigger at

lowerBound
: _Optional_ Minimum execution price

upperBound
: _Optional_ Maximum execution price

stopLoss
: _Optional_ Stop Loss value

takeProfit
: _Optional_ Take Profit value

trailingStop
: _Optional_ Trailing Stop distance in pips, up to one decimal place

#### Example ('market' order)
    curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=market" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### Response

~~~header
HTTP/1.1 200 OK
~~~

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z", // Time that order was executed
  "price" : 1.37041,               // Trigger price of the order
  "tradeOpened" : {
    "id" : 175517237,              // Order id
    "units" : 2,                   // Number of units
    "side" : "sell",               // Direction of the order
    "takeProfit" : 0,              // The take-profit associated with the Order, if any
    "stopLoss" : 0,                // The stop-loss associated with the Order, if any
    "trailingStop" : 0             // The trailing stop associated with the rrder, if any
  },
  "tradesClosed" : [],
  "tradeReduced" : {}
}
~~~

#### Example
    curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=marketIfTouched&price=1.2&expiry=2013-04-01T00%3A00%3A00Z" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### Response

~~~header
HTTP/1.1 201 CREATED
Location: http://api-sandbox.oanda.com/v1/accounts/12345/orders/175517237
~~~

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z",      // Time that order was executed
  "price" : 1.2,                        // Trigger price of the order
  "orderOpened" : {             
    "id" : 175517237,                   // Order id
    "units" : 2,                        // Number of units
    "side" : "sell",                    // Direction of the order
    "takeProfit" : 0,                   // The take-profit associated with the Order, if any
    "stopLoss" : 0,                     // The stop-loss associated with the Order, if any
    "expiry" : "2013-02-01T00:00:00Z",  // The time the rrder expires (in RFC3339 format)
    "upperBound" : 0,                   // The maximum execution price associated with the order, if any
    "lowerBound" : 0,                   // The minimum execution price associated with the order, if any
    "trailingStop" : 0                  // The trailing stop associated with the rrder, if any
  }
}
~~~

----

## Get information for an order

    GET /v1/accounts/:account_id/orders/:order_id

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/1234/orders/43211"

#### Response

~~~json
{
  "id" : 43211,                        // The ID of the Order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the Order
  "units" : 5,                         // The number of units in the Order
  "side" : "buy",                      // The direction of the Order
  "type" : "limit",                    // The type of the Order 
  "time" : "2013-01-01T00:00:00Z",     // The time of the Order (in RFC3339 format)
  "price" : 1.45123,                   // The price the Order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the Order, if any
  "stopLoss" : 1.4,                    // The stop-loss associated with the Order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the Order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the Order, if any
}
~~~

----

## Modify an existing order

    PUT /v1/accounts/:account_id/orders/:order_id

#### Input Data Parameters

units
: _Optional_ Number of units to open Order for 

price
: _Optional_ The price at which the order is set to trigger at

expiry
: _Optional_ UTC time (in RFC3339 format) when order expires

lowerBound
: _Optional_ Minimum execution price

upperBound
: _Optional_ Maximum execution price

stopLoss
: _Optional_ Stop Loss value

takeProfit
: _Optional_ Take Profit value

trailingStop
: _Optional_ Trailing Stop distance in pips, up to one decimal place

#### Example
    curl -X PUT -d "stopLoss=1.3" "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### Response

#####Header

~~~header
HTTP/1.1 201 Created
Location: http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211
~~~

#####Body

~~~json
{
  "id" : 43211,                        // The ID of the Order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the Order
  "units" : 5,                         // The number of units in the Order
  "side" : "buy",                      // The direction of the Order
  "type" : "limit",                    // The type of the Order 
  "time" : "2013-01-01T00:00:00Z",     // The time of the Order (in RFC3339 format)
  "price" : 1.45123,                   // The price the Order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the Order, if any
  "stopLoss" : 1.3,                    // The stop-loss associated with the Order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the Order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the Order, if any
}
~~~

----

## Close an order

    DELETE /v1/accounts/:account_id/orders/:order_id

#### Example
    curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### Response

~~~json
{
  "id" : 54332,                   // The ID of the close Order transaction
  "instrument" : "EUR_USD",       // The symbol of the instrument of the Order
  "units" : 2,
  "side" : "sell",
  "price" : 1.30601,              // The price at which the Order executed
  "time" : "2013-01-01T00:00:00Z" // The time at which the Order executed
}
~~~

