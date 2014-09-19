---
title: Transactions | OANDA API
---

# Transaction Endpoints

* TOC
{:toc}

----------------------------

## Get transaction history

    GET /v1/accounts/:account_id/transactions

#### Input Query Parameters

maxId
: _Optional_ The first transaction to get. The server will return transactions with id less than or equal to this, in descending order. 

minId
: _Optional_ The last transaction to get. The server will return transactions with id greater or equal to this, in descending order.

count
: _Optional_ The maximum number of transactions to return.  The maximum value that can be specified is 500. By default, if count is not specified, a maximum of 50 transactions will be fetched.
             __Note__: Transactions requests with the count parameter specified is rate limited to 1 per every 60 seconds.

instrument
: _Optional_ Retrieve transactions for a specific instrument only. Default: all.

ids
: _Optional_ An URL encoded comma (*%2C*) separated list of transaction ids to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&count=1"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 196
X-Result-Count: 50 
~~~

###### Body

~~~json
{
  "transactions" : [
    {
      "id" : 175427739,
      "accountId" : 6531071,
      "time" : "2014-02-12T21:16:46Z",
      "type" : "ORDER_CANCEL",
      "orderId" : 175427728,
      "reason" : "CLIENT_REQUEST"
    }
  ]
}
~~~

#### Response Parameters

Transactions returned may be of different types describing an event that happened to an account. Transaction of each type contains only relevant sub-set of fields. The following fields are the most common:

id
: The transaction ID.

accountId
: The account ID of the user.

time
: Time in a valid [datetime format](/docs/v1/guide/#datetime-format).

type
: The possible types are:

~~~ 
MARKET_ORDER_CREATE, STOP_ORDER_CREATE, LIMIT_ORDER_CREATE, MARKET_IF_TOUCHED_ORDER_CREATE,
ORDER_UPDATE, ORDER_CANCEL, ORDER_FILLED, TRADE_UPDATE, TRADE_CLOSE, MIGRATE_TRADE_OPEN,
MIGRATE_TRADE_CLOSE, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED, TRAILING_STOP_FILLED, MARGIN_CALL_ENTER,
MARGIN_CALL_EXIT, MARGIN_CLOSEOUT, SET_MARGIN_RATE, TRANSFER_FUNDS, DAILY_INTEREST, FEE
~~~

instrument
: The name of the instrument.

side
: The direction of the action performed on the account, possible values are: buy, sell

units
: The amount of units involved.

price
: The execution or requested price.

lowerBound
: The minimum execution price.

upperBound
: The maximum execution price.

takeProfitPrice
: The price of the take profit.

stopLossPrice
: The price of the stop loss.

trailingStopLossDistance
: The distance of the trailing stop in pips, up to one decimal place.

pl
: The profit and loss value.

interest
: The interest accrued.

accountBalance
: The balance on the account after the event.

tradeId
: ID of a trade that has been closed or open

orderId
: ID of a filled order.

tradeOpened
: This object is appended to the json response if a new trade has been created. Trade related fields are: id, units.

tradeReduced
: This object is appended to the json response if a trade has been closed or reduced. Trade related fields are: id, units, pl, interest.


---------------------------

## Transaction types and a sub-set of corresponding parameters

##### MARKET_ORDER_CREATE
A transaction of this type is created when a user has successfully traded a specified number of units of an instrument at the current market price.

Required Fields
: id, accountId, time, type, instrument, side, units, price, pl, interest, accountBalance

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

###### Trade information
Transaction of a MARKET_ORDER_CREATE type also contains information about associated trade

tradeOpened
: This object is appended to the json response if a new trade has been created. Trade related fields are: id, units.

tradeReduced
: This object is appended to the json response if a trade has been closed or reduced. Trade related fields are: id, units, pl, interest.

~~~json
{
  "id" : 176403879,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:31:05Z",
  "type" : "MARKET_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1.25325,
  "pl" : 0,
  "interest" : 0,
  "accountBalance" : 100000,
  "tradeOpened" : {
    "id" : 176403879,
    "units" : 2
  }
}
~~~


##### STOP_ORDER_CREATE
A transaction of this type is created when a user has successfully placed a Stop order on his/her account.
A Stop order is an order which buys or sells a specified number of units of an instrument when the market 
price for that instrument first becomes equal to or worse than the price threshold specified.

Required Fields
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403886,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:20:05Z",
  "type" : "STOP_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### LIMIT_ORDER_CREATE
A transaction of this type is created when a user has successfully placed a Limit order on his/her account.
A Limit order is an order which buys or sells a specified number of units of an instrument when the market price
for that instrument first becomes equal to or better than the price threshold specified.

Required Fields
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403880,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:34:32Z",
  "type" : "LIMIT_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### MARKET_IF_TOUCHED_ORDER_CREATE
A transaction of this type is created when a user has successfully placed a MarketIfTouched order on his/her account.
A MarketIfTouched order is an order which buys or sells a specified number of units of an instrument when the market price
for that instrument first touches or crosses the price threshold specified. This order replaces what is historically known
as the "OANDA Limit Order".

Required Fields
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403882,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:40:42Z",
  "type" : "MARKET_IF_TOUCHED_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### ORDER_UPDATE
A transaction of this type is created when a user has successfully updated any of the LimitOrder, StopOrder, MarketIfTouched orders on his/her account.

Required Fields
: id, accountId, time, type, instrument, units, price, reason (REPLACES_ORDER)

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403883,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:07:13Z",
  "type" : "ORDER_UPDATE",
  "instrument" : "EUR_USD",
  "units" : 3,
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "REPLACES_ORDER"
  "orderId" : 1234567
}
~~~


##### ORDER_CANCEL
A transaction of this type is created when an order is being closed due to one of the reasons:
CLIENT_REQUEST, TIME_IN_FORCE_EXPIRED, ORDER_FILLED, MIGRATION.
Order fill may result in a failure due to the following reasons:
INSUFFICIENT_MARGIN, BOUNDS_VIOLATION, UNITS_VIOLATION, STOP_LOSS_VIOLATION, TAKE_PROFIT_VIOLATION, TRAILING_STOP_VIOLATION,
MARKET_HALTED, ACCOUNT_NON_TRADABLE, NO_NEW_POSITION_ALLOWED, INSUFFICIENT_LIQUIDITY.

Required Fields
: id, accountId, time, type, orderId, reason

~~~json
{
  "id" : 176403881,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:37:27Z",
  "type" : "ORDER_CANCEL",
  "orderId" : 176403880,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### ORDER_FILLED
A transaction of this type is created when an order has been filled.

Required Fields
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, orderId

Optional Fields
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

###### Trade information
Transaction of an ORDER_FILLED type also contains information about an associated trade.

tradeOpened
: This object is appended to the json response if a new trade has been created. Trade related fields are: id, units

tradeReduced
: This object is appended to the json response if a trade has been closed or reduced. Trade related fields are: id, units, pl, interest

~~~json
{
  "id" : 175685908,
  "accountId" : 2610411,
  "time" : "2014-04-14T20:32:34Z",
  "type" : "ORDER_FILLED",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1.3821,
  "pl" : 0,
  "interest" : 0,
  "accountBalance" : 100000,
  "orderId" : 175685907,
  "tradeOpened" : {
          "id" : 175685908,
          "units" : 2
  }
}
~~~


##### TRADE_UPDATE
A transaction of this type is created when a user has successfully updated a trade.

Required Fields
: id, accountId, time, type, instrument, units, side, tradeId

Optional Fields
: takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403884,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:09:38Z",
  "type" : "TRADE_UPDATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "",
  "stopLossPrice" : 1.1,
  "tradeId" : 176403879
}
~~~


##### TRADE_CLOSE
A transaction of this type is created when a user has successfully closed a trade.

Required Fields
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403885,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### MIGRATE_TRADE_CLOSE
A transaction of this type is created when a trade has been closed due to user migration to another division.

Required Fields
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403885,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "MIGRATE_TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### MIGRATE_TRADE_OPEN
A transaction of this type is created when a trade is reopened on the account if the user migrated to another division.

Required Fields
: id, accountId, time, type, instrument, side, units, price

Optional Fields
: takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 175685908,
  "accountId" : 2610411,
  "time" : "2014-04-14T20:32:34Z",
  "type" : "MIGRATE_TRADE_OPEN",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1.3821,
  "tradeOpened" : {
          "id" : 175685908,
          "units" : 2
  }
}
~~~


###### Trade information
Transaction of a MIGRATE_TRADE_OPEN type also contains information about associated trade

tradeOpened
: This object is appended to the json response. Trade related fields are: id, units.


##### TAKE_PROFIT_FILLED
A transaction of this type is created when a Take Profit order has been filled on user's account.

Required Fields
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175685954,
  "accountId" : 1491998,
  "time" : "2014-04-15T14:12:47Z",
  "type" : "TAKE_PROFIT_FILLED",
  "units" : 10,
  "tradeId" : 175685930,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.38231,
  "pl" : 0.0001,
  "interest" : 0,
  "accountBalance" : 100000.0001
}
~~~


##### STOP_LOSS_FILLED
A transaction of this type is created when a Stop Loss order has been filled on user's account.

Required Fields
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175685918,
  "accountId" : 1403479,
  "time" : "2014-04-14T20:41:41Z",
  "type" : "STOP_LOSS_FILLED",
  "units" : 10,
  "tradeId" : 175685917,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.3821,
  "pl" : -0.0003,
  "interest" : 0,
  "accountBalance" : 99999.9997
}
~~~


##### TRAILING_STOP_FILLED
A transaction of this type is created when a Trailing Stop Loss order has been filled on user's account.

Required Fields
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175739353,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "TRAILING_STOP_FILLED",
  "units" : 10,
  "tradeId" : 175739352,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.38137,
  "pl" : -0.0009,
  "interest" : 0,
  "accountBalance" : 99999.9992
}
~~~


##### MARGIN_CALL_ENTER
A transaction of this type is created administratively to mark user's account as being in a margin call.

Required Fields
: id, accountId, time, type

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "MARGIN_CALL_ENTER"
}
~~~


##### MARGIN_CALL_EXIT
A transaction of this type is created administratively to mark user's account exiting margin call state.

Required Fields
: id, accountId, time, type

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "MARGIN_CALL_EXIT"
}
~~~


##### MARGIN_CLOSEOUT
A transaction of this type is created automatically by the system when the margin closeout value in user's
account declines to half or less than half of the margin used. A MARGIN_CLOSEOUT represents the closing
of all open trades in user's account.

Required Fields
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403889,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "MARGIN_CLOSEOUT",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### SET_MARGIN_RATE
A transaction of this type is created whenever user modifies margin rate for the account.

Required Fields
: id, accountId, time, type, marginRate

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "SET_MARGIN_RATE",
  "rate" : 0.02
}
~~~


##### TRANSFER_FUNDS
A transaction of this type is created whenever user successfully deposited or withdrew funds from the account.
Amount specified is positive in case of deposit and negative in case of withdrawal. 

Required Fields
: id, accountId, time, type, amount, accountBalance, reason (CLIENT_REQUEST, ADJUSTMENT, MIGRATION)

~~~json
{
  "id" : 176403878,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:29:25Z",
  "type" : "TRANSFER_FUNDS",
  "amount" : 100000,
  "accountBalance" : 100000,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### DAILY_INTEREST
A transaction of this type is created automatically by the system to pay/collect the daily position and
balance interest for an account. It is generated every day at 4pm America/New_York for accounts which have
had a balance or open position during the previous day.
The transaction itself is not guaranteed to be created exactly at 4pm each day, however the interest paid/collected
is based only on the positions and balance that existed up to 4pm of the previous day.

Required Fields
: id, accountId, time, type, instrument, interest, accountBalance

~~~json
{
  "id" : 175739363,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "DAILY_INTEREST",
  "instrument" : "EUR_USD",
  "interest" : 10.0414,
  "accountBalance" : 99999.9992
}
~~~


##### FEE
A transaction of this type is created automatically by the system to collect a fee if applicable.

Required Fields
: id, accountId, time, type, amount, accountBalance, reason(FUNDS, CURRENSEE_MONTHLY, CURRENSEE_PERFORMANCE, SDR_REPORTING)

~~~json
{
  "id" : 175739369,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "FEE",
  "amount" : -10.0414,
  "accountBalance" : 99999.9992,
  "reason" : "FUNDS"
}
~~~


#### Pagination

Transactions can be paginated with the count and maxId parameters.
At most, a maximum of 50 transactions can be returned in one query. 
If more transactions exist than specified by the given or default count, a URL with maxId set to the next unreturned transaction will be returned within the Link header.

------------------------------

## Get information for a transaction

    GET /v1/accounts/:account_id/transactions/:transaction_id

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions/1170980"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 269
~~~

###### Body

~~~json
{
  "id" : 1170980,
  "accountId" : 12345,
  "time" : "2014-02-12T20:02:45Z",
  "type" : "TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 1000,
  "side" : "sell",
  "price" : 1.35921,
  "pl" : 1.93,
  "interest" : 0,
  "accountBalance" : 1000026.0723,
  "tradeId" : 175427703 //Closed trade id
}
~~~

----------------------------

## Get full account history

Submit a request for a full transaction history of a given account.
A successfully accepted submission results in a response containing a URL in the `Location` header to a file that will be available once the request is served.
Response for the URL will be HTTP 404 until the file is ready. Once served the URL will be valid for a certain amount of time.

    GET /v1/accounts/:account_id/alltransactions

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/alltransactions"

#### Response

###### Header

~~~header
HTTP/1.1 202 Accepted
Content-Type: application/json
Content-Length: 0
Location: https://fxtrade.oanda.com/53e58d3509cef65f33998c879dcccbd9.json.zip
~~~

