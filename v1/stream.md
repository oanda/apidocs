---
title: Streaming | OANDA API
---

# Streaming Endpoints

* TOC
{:toc}

----------------------------

## Streaming

As part of our OANDA API offering, we provide real time data streaming connections for customers that require an alternative to the OANDA REST API.

The streaming API adheres to the chunked transfer encoding data transfer mechanism of HTTP 1.1.  All streaming connections are authenticated.

-----------------

## Rates Streaming

Open a streaming connection to receive real time market prices for specified instruments.


    GET /v1/prices


#### Input Query Parameters

accountId
: _Required_ The account that prices are applicable for.

instruments
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

sessionId
: _Optional_ A unique session id used to identify the rate stream connection. The value specified must be between 1 to 12 alphanumeric characters. If a request is made with a session id that matches the session id of an existing connection, the older connection will be disconnected. Please see the [best practices](/docs/v1/best-practices/#streaming) section for usage examples.  This parameter is not applicable to the sandbox environment.

#### Example

    curl "http://stream-sandbox.oanda.com/v1/prices?accountId=12345&instruments=AUD_CAD%2CAUD_CHF"

#### Response

##### Header

~~~Header
Transfer-Encoding: chunked
~~~

------------------

### Body (Stream)

All data written to the stream are encoded in the JSON format.
The initial data returned are price snapshots of the subscribed instruments. Subsequent price data will be written to the stream whenever new prices are available.
Heartbeats are written to the stream to ensure the HTTP connection remains active.

~~~json
{"instrument":"AUD_CAD","time":"2014-01-30T20:47:08.066398Z","bid":0.98114,"ask":0.98139}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:08.053811Z","bid":0.79353,"ask":0.79382}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.493511Z","bid":0.79355,"ask":0.79387}
{"heartbeat":{"time":"2014-01-30T20:47:11.543511Z"}}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.855887Z","bid":0.79357,"ask":0.79390}
{"instrument":"AUD_CAD","time":"2014-01-30T20:47:14.066398Z","bid":0.98112,"ask":0.98138}
~~~

###### JSON Response Fields

instrument
: Name of the instrument.

time
: Time in a valid [datetime format](/docs/v1/guide/#datetime-format).


bid
: Bid price

ask
: Ask price


-----------------------

## Events Streaming

Open a streaming connection to receive real time authorized accounts' events.


    GET /v1/events

-----------------


#### Input Query Parameters

accountIds
: _Optional_ A URL encoded comma (*%2C*) separated list of account IDs to subscribe for events. If the list is not provided, subscription will be done for all the authorized accounts associated with the token.
Note: The list of account IDs is *required* on the sandbox.


#### Example

    curl "http://stream-sandbox.oanda.com/v1/events?accountIds=12345%2C6789"

#### Response

##### Header

~~~Header
Transfer-Encoding: chunked
~~~

------------------

### Body (Stream)

All data written to the stream are encoded in the JSON format.
Events sent to the stream are either heartbeats (every 15 seconds) to ensure that HTTP connection remains active or transactions reporting the following events:
margin closeout, order filled, order canceled by the system, take profit/stop loss/ trailing stop filled, margin call entry/exit.


~~~json
{"heartbeat":{"time":"2014-05-26T13:58:40Z"}}
{"transaction":{"id":10001,"accountId":12345,"time":"2014-05-26T13:58:41.000000Z","type":"MARGIN_CLOSEOUT","instrument":"EUR_USD","units":10,"side":"sell","price":1,"pl":1.234,"interest":0.034,"accountBalance":10000,"tradeId":1359}}
{"transaction":{"id":10002,"accountId":12345,"time":"2014-05-26T13:58:45.000000Z","type":"ORDER_FILLED","instrument":"EUR_USD","side":"sell","price":1,"pl":1.234,"interest":0.034,"accountBalance":10000,"orderId":0,"tradeReduced":{"id":54321,"units":10,"pl":1.234,"interest":0.034}}}
~~~

###### JSON Response Fields

id
: Transaction ID

accountId
: Account ID

time
: Time in a valid [datetime format](/docs/v1/guide/#datetime-format).

type
: Transaction type. Possible values: ORDER_FILLED, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED, TRAILING_STOP_FILLED, MARGIN_CLOSEOUT, ORDER_CANCEL, MARGIN_CALL_ENTER, MARGIN_CALL_EXIT.

instrument
: The name of the instrument.

side
: The direction of the action performed on the account, possible values are: buy, sell.

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


-----------------------

### Limits

#### Rates Streaming

* Two active rate stream connections per access token.

#### Events Streaming

* *Sandbox*: current limit of connections per IP is set to 5.
* *Production environment*: 5 connections per access token.

In the event that a limit is reached, OANDA servers will do one of the following:

*Sandbox*: reject a new connection with status code reply 429.

*Production environment*: See [disconnection](#disconnection) section below


### Connections

#### Disconnection

OANDA will terminate existing active connections under the following scenarios.

* OANDA's infrastructure maintenance downtime. Backend components are disabled and upgraded during maintenance windows.
* The number of active connections has exceeded the limit granted to the specified access token.  The oldest connection with the specified access token will be disconnected.  A disconnect message will be sent to the connection to be disconnected.

~~~json
{"disconnect":{"code":60,"message":"Access Token connection limit exceeded: This connection will now be disconnected","moreInfo":"http:\/\/developer.oanda.com\/docs\/v1\/troubleshooting"}}
~~~

* A session id has been specified that matches an existing stream's session id. The existing stream will be disconnected and a new stream with the specified session id will be established. A disconnect message will be sent to the connection to be disconnected. __Note__: This only applies to rates streaming.

~~~json
{"disconnect":{"code":64,"message":"Session has been disconnected by a new connection:  This connection will now be disconnected","moreInfo":"http:\/\/developer.oanda.com\/docs\/v1\/troubleshooting"}}
~~~

#### Stalls

It is recommended that the client application terminates the connection and re-connects its corresponding stream in the event of:

* No data has been received (no ticks, no heartbeats) from the rates stream for more than 10 seconds.
* No data has been received (no events, no heartbeats) from the events stream for more than 20 seconds.

#### Re-connection

There is a re-connection rate limit in place that is enforced.  Clients whose re-connection attempts exceed this limit will receive HTTP 429 error responses.

Client applications are recommended to utilize a backoff implementation for re-connection attempts.  Implementation includes the [exponential backoff](http://en.wikipedia.org/wiki/Exponential_backoff).

* For example, if your re-connection attempt receives an HTTP error, back off for 1 second before initiating the next re-connection attempt.  Double the backoff interval until the connection is successfully established.

