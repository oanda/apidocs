---
title: Streaming | OANDA API
---

# Streaming

As part of our Open API offering, we provide real time data streaming connections for customers that require an alternative to the OANDA REST API. 

The streaming API adheres to the chunked transfer encoding data transfer mechanism of HTTP 1.1.  All streaming connections are authenticated.

## Rates Streaming

Open a streaming connection to receive real time market prices for specified instruments.


    GET /v1/quote

-----------------

### Limits

* There is a limit of one active rate stream connection per access token.  In the event that a new stream request is made with the access token of an existing rate stream connection, OANDA servers will disconnect the older connection without warning.

* Each rate stream connection may subscribe up to a maximum of 10 instruments.

#### Input Query Parameters

accountId
: _Required_ The account that prices are applicable for.

instruments
: _Required_ An URL encoded (*%2C*) comma separated list of instruments to fetch prices for. 


#### Example

    curl -H "Authorization: Bearer ACCESS-TOKEN" "https://stream-fxpractice.oanda.com/v1/quote?accountId=12345&instruments=AUD_CAD%2CAUD_CHF"

#### Response

##### Header

~~~Header
Transfer-Encoding: chunked
~~~

------------------

### Body (Stream)

All data written to the stream are encoded in the JSON format.
The initial data returned are price snapshots of the subscribed instruments. Subsequent price data will be written to the stream whenever new prices are avaliable.
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
: Time in RFC3339 format

bid
: Bid price

ask
: Ask price

-----------------------

### Connections

#### Disconnection

OANDA will terminate existing active connections under the following scenarios.

* OANDA's infrastructure maintenance downtime. Backend components are disabled and upgraded during maintenance windows.
* The number of active connections have exceeded the limit granted to the specified access token.  The oldest connection with the specified access token will be disconnected.

#### Stalls

In the event that no data is received (no ticks, no heartbeats) from the stream for more than 10 seconds, it is recommended that the client application terminate the connection and re-connect.  

#### Re-connection

There is a re-connection rate limit in place and is enforced.  Clients whose re-connection attempts exceeds this limit will receive HTTP 429 error responses.  

Client applications are recommended to utilize a backoff implementation for re-connection attempts.  Implementation includes the [exponential backoff](http://en.wikipedia.org/wiki/Exponential_backoff).  

* For example, if your re-connection attempt receives a HTTP error, back off for 1 second before initiating the next re-connection attempt.  Double the back off interval until the connection is successfully established.


