---
title: Streaming | OANDA API
---

# Streaming

As part of our Open API offering, we provide real time data streaming connections for customers that require an alternative to the OANDA REST API. 

The streaming API adheres to the chunked transfer encoding data transfer mechanism of HTTP 1.1.  All streaming connections are authenticated.

* TOC
{:toc}

## Rates Streaming

Open a streaming connection to receive real time market prices for specified instruments.  There is a limit of 1 rate streaming connection per access token.


    GET /v1/ratestream

### Limits

There is a limit of 1 rate streaming connection per access tokens.
In the event that a new rate stream request with the access token of an existing rate stream connection, OANDA servers will disconnect the older connection without warning.

A rate stream connection may subscribe up to a maximum of 10 instruments.

### Input Query Parameters

accountId
: _Required_ The account that prices are applicable for.

Instruments
: _Required_ A (URL encoded) comma separated list of instruments to fetch prices for. 


### Example
    curl -H "Authorization: Bearer ACCESS-TOKEN" "https://fxtrade-api.oanda.com/v1/ratestream?accountId=12345&instruments=AUD_CAD%2CAUD_CHF"

### Response

####Header

~~~Header
Transfer-Encoding: chunked
~~~

####Body (Stream)

All data written to the stream are encoded in the JSON format.
The initial data returned are price snapshots of the subscribed instruments.  Subsequent price data will be written to the stream whenever new prices are avaliable.
Heartbeats are written to the stream at set intervals to ensure the HTTP connection remains active.

~~~json
{"instrument":"AUD_CAD","time":"2014-01-30T20:47:08.066398Z","bid":0.98114,"ask":0.98139}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:08.053811Z","bid":0.79353,"ask":0.79382}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.493511Z","bid":0.79355,"ask":0.79387}
{"Heartbeat":{"time":"2014-01-30T20:47:11.543511Z"}}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.855887Z","bid":0.79357,"ask":0.79390}
{"instrument":"AUD_CAD","time":"2014-01-30T20:47:14.066398Z","bid":0.98112,"ask":0.98138}
~~~


##### JSON Response Fields

instrument
: Name of the instrument.

time
: Time in RFC3339 format

bid
: Bid price

ask
: Ask price
