---
title: Orders | OANDA API
---

# Order Endpoints

* TOC
{:toc}


## Get orders for an account

This will return all pending orders for an account. Note: pending take profit or stop loss orders are recorded in the open [trade](/docs/v1/trades#get-a-list-of-open-trades) object, and will not be returned in this request.

    GET /v1/accounts/:account_id/orders

#### Input Query Parameters

maxId
: _Optional_ The server will return orders with id less than or equal to this, in descending order (for pagination).

count
: _Optional_ Maximum number of open orders to return. Default: 50. Max value: 500.

instrument
: _Optional_ Retrieve open orders for a specific instrument only. Default: all.

ids
: _Optional_ A comma separated list of orders to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&count=2"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 660
Link: <http://api-sandbox.oanda.com/v1/accounts/12345/orders?count=2&instrument=EUR_USD&maxId=175427625>; rel="next"
X-Result-Count: 8
~~~

###### Body

~~~json
{
  "orders" : [
    {
      "id" : 175427639,
      "instrument" : "EUR_USD",
      "units" : 20,
      "side" : "buy",
      "type" : "marketIfTouched",
      "time" : "2014-02-11T16:22:07Z",
      "price" : 1,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "expiry" : "2014-02-15T16:22:07Z",
      "upperBound" : 0,
      "lowerBound" : 0,
      "trailingStop" : 0
    },
    {
      "id" : 175427637,
      "instrument" : "EUR_USD",
      "units" : 10,
      "side" : "sell",
      "type" : "marketIfTouched",
      "time" : "2014-02-11T16:22:07Z",
      "price" : 1,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "expiry" : "2014-02-12T16:22:07Z",
      "upperBound" : 0,
      "lowerBound" : 0,
      "trailingStop" : 0
    }
  ]
}
~~~

#### Pagination

Orders can be paginated with the count and maxId parameters.
At most, a maximum of 50 orders can be returned in one query. 
If more orders exist than specified by the given or default count, a URL with maxId set to the next unreturned order will be returned within the Link header.

----

## Create a new order

    POST /v1/accounts/:account_id/orders


#### Input Data Parameters

instrument
: _Required_ Instrument to open the order on.

units
: _Required_ The number of units to open order for.

side
: _Required_ Direction of the order, either 'buy' or 'sell'.

type
: _Required_ The type of the order 'limit', 'stop', 'marketIfTouched' or 'market'.

expiry
: _Required_ If order type is 'limit', 'stop', or 'marketIfTouched'. The order expiration time in UTC (RFC3339 Format).

price
: _Required_ If order type is 'limit', 'stop', or 'marketIfTouched'. The price where the order is set to trigger at.

lowerBound
: _Optional_ The minimum execution price.

upperBound
: _Optional_ The maximum execution price.

stopLoss
: _Optional_ The stop loss value.

takeProfit
: _Optional_ The take profit value.

trailingStop
: _Optional_ The trailing stop distance in pips, up to one decimal place.

#### Example ('market' order)
    $curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=market" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### Response


###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 204
~~~

###### Body

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z", // Time that order was executed
  "price" : 1.37041,               // Trigger price of the order
  "tradeOpened" : {
    "id" : 175517237,              // Order id
    "units" : 2,                   // Number of units
    "side" : "sell",               // Direction of the order
    "takeProfit" : 0,              // The take-profit associated with the order, if any
    "stopLoss" : 0,                // The stop-loss associated with the order, if any
    "trailingStop" : 0             // The trailing stop associated with the order, if any
  },
  "tradesClosed" : [],
  "tradeReduced" : {}
}
~~~

#### Example ('marketIfTouched' order)
    $curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=marketIfTouched&price=1.2&expiry=2013-04-01T00%3A00%3A00Z" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### Response

###### Header

~~~header
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 292
Location: http://api-sandbox.oanda.com/v1/accounts/12345/orders/175517237
~~~

###### Body

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-01-01T20:36:06Z",      // Time that order was executed
  "price" : 1.2,                        // Trigger price of the order
  "orderOpened" : {             
    "id" : 175517237,                   // Order id
    "units" : 2,                        // Number of units
    "side" : "sell",                    // Direction of the order
    "takeProfit" : 0,                   // The take-profit associated with the order, if any
    "stopLoss" : 0,                     // The stop-loss associated with the order, if any
    "expiry" : "2013-02-01T00:00:00Z",  // The time the order expires (in RFC3339 format)
    "upperBound" : 0,                   // The maximum execution price associated with the order, if any
    "lowerBound" : 0,                   // The minimum execution price associated with the order, if any
    "trailingStop" : 0                  // The trailing stop associated with the order, if any
  }
}
~~~

----

## Get information for an order

    GET /v1/accounts/:account_id/orders/:order_id

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/1234/orders/43211"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 290
~~~

###### Body

~~~json
{
  "id" : 43211,                        // The ID of the order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the order
  "units" : 5,                         // The number of units in the order
  "side" : "buy",                      // The direction of the order
  "type" : "limit",                    // The type of the order
  "time" : "2013-01-01T00:00:00Z",     // The time of the order (in RFC3339 format)
  "price" : 1.45123,                   // The price the order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the order, if any
  "stopLoss" : 1.4,                    // The stop-loss associated with the order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the order, if any
}
~~~

----

## Modify an existing order

    PATCH /v1/accounts/:account_id/orders/:order_id

#### Input Data Parameters
Note: Only the specified parameters will be modified. All other parameters will remain unchanged. To remove an optional parameter, set its value to 0.

units
: _Optional_ The number of units to open order for.

price
: _Optional_ The price at which the order is set to trigger at.

expiry
: _Optional_ The order expiration time in UTC (RFC3339 format).

lowerBound
: _Optional_ The minimum execution price.

upperBound
: _Optional_ The maximum execution price.

stopLoss
: _Optional_ The stop loss value.

takeProfit
: _Optional_ The take profit value.

trailingStop
: _Optional_ The trailing stop distance in pips, up to one decimal place.

#### Example
    $curl -X PATCH -d "stopLoss=1.3" "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Server: nginx/1.2.9
Content-Type: application/json
Content-Length: 284

~~~

###### Body

~~~json
{
  "id" : 43211,                        // The ID of the order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the order
  "units" : 5,                         // The number of units in the order
  "side" : "buy",                      // The direction of the order
  "type" : "limit",                    // The type of the order
  "time" : "2013-01-01T00:00:00Z",     // The time of the order (in RFC3339 format)
  "price" : 1.45123,                   // The price the order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the order, if any
  "stopLoss" : 1.3,                    // The stop-loss associated with the order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the order, if any
}
~~~

----

## Close an order

    DELETE /v1/accounts/:account_id/orders/:order_id

#### Example
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 130
~~~

###### Body

~~~json
{
  "id" : 54332,                   // The ID of the close order transaction
  "instrument" : "EUR_USD",       // The symbol of the instrument of the order
  "units" : 2,
  "side" : "sell",
  "price" : 1.30601,              // The price at which the order executed
  "time" : "2013-01-01T00:00:00Z" // The time at which the order executed
}
~~~

