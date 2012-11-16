# Rate Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/instruments](#get-v1instruments) | Instrument Discovery |
| [GET /v1/instruments/price](#get-v1instrumentsprice) | Get current price for multiple instruments |
| [GET /v1/instruments/:instrument/price](#get-v1instrumentsinstrumentprice) | Get current price for single instrument |
| [GET /v1/instruments/:instrument/candles](#get-v1instrumentsinstrumentcandles) | Get candlesticks for a single instrument |
| [POST /v1/instruments/poll](#post-v1instrumentspoll) | Create and modify rates/candle polling session ([about rates polling](#aboutratespolling))|
| [GET /v1/instruments/poll](#get-v1instrumentsinstrumentspoll) | Rates/candle polling ([about rates polling](#aboutratespolling))|

## GET /v1/instruments
#### Request
    https://api.oanda.com/v1/instruments

#### Respond
    {
         "instruments" : [
             {"instrument":"AUD_CAD", "displayName" : "AUD/CAD", "pip" : "0.0001", "pipLocation" : -4, "extraPrecision" : 1},
             {"instrument":"AUD_CHF", "displayName" : "AUD/CHF", "pip" : "0.0001", "pipLocation" : -4, "extraPrecision" : 1},
             .
             .
             {"instrument":"ZAR_JPY", "displayName" : "ZAR/JPY", "pip" : "0.0001", "pipLocation" : -4, "extraPrecision" : 1}
         ]
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| visibility | "tradable" (default) or "all". The minimum visibility of the instruments to return. |

#### Required scope
read



## GET /v1/instruments/price
#### Request
    https://api.oanda.com/v1/instruments/price?instruments=EUR/USD,USD/JPY

#### Respond
    {
        "prices": [
            {
                "instrument": "EUR_USD",
                "time": 1350590296,
                "bid": 1.30714,
                "ask": 1.30723
            },
            {
                "instrument": "USD_JPY",
                "time": 1350590296,
                "bid": 79.248,
                "ask": 79.264
            }
        ]
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| instruments | __require___ A comma-separated list of instruments to fetch prices for |

#### Required scope
read


## GET /v1/instruments/:instrument/price
#### Request
    https://api.oanda.com/v1/instruments/EUR_USD/price

#### Respond
    {
        "instrument": "EUR_USD",
        "time": 1350590296,
        "bid": 1.30714,
        "ask": 1.30723
    }

#### Parameters
* __volume__

The volume used to determine which rung is returned. To determine which rung will be returned, the maximum tradeable amount of the current rung must be greater than the volume and the maximum tradeable amount of the previous rung must be less than the volume.

For example, consider an instrument with the following ladder structure:

[0] 1,000,000 bid: 0.0 ask: 0.0
[1] 5,000,000 bid: 0.1 ask: 0.1
[2] 10,000,000 bid: 0.3 ask: 0.3

Requesting the instrument's price with the following volumes will return in the following rungs:

    0 - 999,999             => Rung 0
    1,000,000 - 4,999,999   => Rung 1
    >= 5,000,000            => Rung 2
volume has a default value of 0, meaning that by default only the lowest run will be returned.

#### Required scope
read

## GET /v1/instruments/:instrument/candles
#### Request
    https://api.oanda.com/v1/instruments/EUR_USD/candles?count=2

#### Respond
    {
        "instrument": "EUR_USD",
        "granularity": "S5",
        "candles": [
            {
                "time": 1350683410,
                "open mid": 1.30237,
                "high mid": 1.30237,
                "low mid": 1.30237,
                "close mid": 1.30237,
                "complete": "true"
            },
            {
                "time": 1350684320,
                "open mid": 1.30242,
                "high mid": 1.30242,
                "low mid": 1.30242,
                "close mid": 1.30242,
                "complete": "true"
            }
        ]
    }


#### Parameters

<table class="table table-striped table-bordered table-condensed">
    <tbody>
        <tr>
            <th>Name</th> <th>Description</th>
        </tr>
        <tr>
            <td>gran</td>
            <td>
                    The granularity of the candles to be returned. This must be one of the
                    "named" THS granularities which include:
                    <li>Second-based: S5,S10,S15,S30</li>
                    <li>Minute-based: M1,M2,M3,M4,M5,M10,M15,M30</li>
                    <li>Hour-based: H1,H2,H3,H4,H6,H8,H12</li>
                    <li>Daily: D</li>
                    <li>Weekly: W</li>
                    <li>Monthly: M</li>
                    The default for <i>gran</i> is "S5"
            </td>
        </tr>
        <tr>
            <td>count</td>
            <td>
                The number of candles to return in the response. This paramater may be
                    ignored by the server depending on the time range provided. See "Time
                    and Count Semantics" below for a full description.
                <br>
                    The default for <i>count</i> is 500, and the server will cap this value at 5000.
            </td>
        </tr>
        <tr>
            <td>start</td>
            <td>The start timestamp for the range of candles requested. Default: NULL (unset)</td>
        </tr>
        <tr>
            <td>end</td>
            <td>The end timestamp for the range of candles requested. Default: NULL (unset)</td>
        </tr>
        <tr>
            <td>candleRepr</td>
            <td>
            Candlesticks representation (<a href="index.php?page=about-candle-representation">about candlestick presentation</a>)
            This can be one of:
            <li>"M" - midpoint-based candlesticks</li>
            <li>"BA" - BID/ASK-based candlesticks</li>
            <li>"MV" - midpoint-based candlesticks</li>
            <li>"BAV" - BID/ASK-based candlesticks</li>
            Default: "M"
            </td>
        </tr>
        <tr>
            <td>includeFirst</td>
            <td>
                A boolean field which may be set to "true" or "false".
                If it is set to "true", the candlestick covered by the <i>start</i> timestamp
                will be returned. If it is set to "false", this candlestick will not
                be returned.
                <br/><br/>
                This field exists to provide clients a mechanism to not repeatedly fetch
                the most recent candlestick which it is not a "Dancing Bear".
                <br/><br/>
                Default: true
            </td>
        </tr>
    </tbody>
</table>

#### Required scope
read


## POST /v1/instruments/poll
#### Request
    POST /v1/instruments/poll HTTP/1.1
    Accept: */*
    Connection: close
    User-Agent: OAuth gem v0.4.4
    Content-Type: application/json
    Host: abc.com

    {
        "prices" : [
            "EUR_USD",
            "USD_CAD"
        ],
        "candles" : [
            {
                "instrument" : "EUR_JPY",
                "granularity" : "S5",
                "start" : 1340895300
            },
            {
                "instrument" : "USD_CHF",
                "granularity" : "H1",
                "start" : 1340895300
            }
    }

#### Respond
    {"sessionId":"1234"}

#### Required scope
read

## GET /v1/instruments/poll
#### Request
    https://api.oanda.com/v1/instruments/poll?sessionId=1234&candleFormat=M

#### Respond
    {
        "time": 1353023991
        "prices": [
            {"instrument":"EUR_USD", "time":135023990, "bid":1.21, "ask":1.22},
            {"instrument":"USD_JPY", "time":135023990, "bid":80.122, "ask":80.123}
        ],
        "candles": [
            {
                "instrument": "EUR_USD",
                "granularity": "S5",
                "candles": [
                    {
                        "time": 1350683410,
                        "open mid": 1.30237,
                        "high mid": 1.30237,
                        "low mid": 1.30237,
                        "close mid": 1.30237,
                        "complete": "true"
                    },
                    {
                        "time": 1350684320,
                        "open mid": 1.30242,
                        "high mid": 1.30242,
                        "low mid": 1.30242,
                        "close mid": 1.30242,
                        "complete": "true"
                    }
                ]
            },
            {
                "instrument": "USD_CHF",
                "granularity": "S5",
                "candles": [
                    {
                        "time": 1350683410,
                        "open mid": 1.30237,
                        "high mid": 1.30237,
                        "low mid": 1.30237,
                        "close mid": 1.30237,
                        "complete": "true"
                    },
                    {
                        "time": 1350684320,
                        "open mid": 1.30242,
                        "high mid": 1.30242,
                        "low mid": 1.30242,
                        "close mid": 1.30242,
                        "complete": "true"
                    }
                ]
            }
        ]
    }

#### Parameters

<pre>
sessionId
    The polling session ID for the client. This MUST be provided for the server to be able to return
    the proper polled prices and candles for the client.

candleFormat
    Candlesticks representation (about candlestick presentation) This can be one of:
    "M" - midpoint-based candlesticks
    "BA" - BID/ASK-based candlesticks
    "MV" - midpoint-based candlesticks
    "BAV" - BID/ASK-based candlesticks
    Default: "M"
</pre>

#### Required scope
read

##About rates polling
####Overview
Clients may set up sessions with the server to manage the polling of prices and candlesticks. These polling sessions push the complexity of maintaining state for real-time candlestick graphs down to the server while simultaneously reducing the amount of network traffic between the client and server.

In order to poll for price and chandlestick changes, client need to first setup a session by doing a POST to /instruments/poll with a configuration specifying which instruments the client whiches to get polled prices and candle data from. Once a session is setup, client can do a GET /instruments/poll

Candlestick polling will give you all new candle sticks since last time you poll. Price polling will only return you the latest price if there is an update

Notes: /instruments/poll is only meant to be used to retrieve updates. Please use /instruments/:instrument/candles for historial candles.

####Examples
1.Set up polling session for EUR/USD price changes

    curl -H "Content-Type: application/json" -d '{ "prices": [ "EUR/USD", "USD/CAD" ] }' httpx://api.oanda.com/v1/instruments/poll

    {"sessionId":"123456"}

2.Poll for price changes

    curl http://api.oanda.com/v1/instruments/poll?sessionId=123456

    {
        "time": 1350672751,
        "prices": [
            {
                "instrument": "EUR/USD",
                "time": 1350672733,
                "bid": 1.30206,
                "ask": 1.30215
            },
            {
                "instrument": "USD/CAD",
                "time": 1350672726,
                "bid": 0.99364,
                "ask": 0.99389
            }
        ]
    }

    curl http://api.oanda.com/v1/instruments/poll?sessionId=123456

    {
        "time": 1350672802,
        "prices": [
            {
                "instrument": "EUR/USD",
                "time": 1350672795,
                "bid": 1.30204,
                "ask": 1.30213
            },
            {
                "instrument": "USD/CAD",
                "time": 1350672768,
                "bid": 0.99364,
                "ask": 0.99389
            }
        ]
    }

3.Change config for existing session (if needed)

    curl -H "Content-Type: application/json" -d '{ "sessionId":"123456", "prices": [ "EUR/USD" ] }' httpx://api.oanda.com/v1/instruments/poll



## Candlestick Representation

M" - midpoint-based candlesticks. Each Candle will have the format:

    {
        "timestamp":,
        "open mid":,
        "high mid":,
        "low mid":,
        "close mid":,
        "complete":
    }

"BA" - BID/ASK-based candlesticks

    {
        "timestamp":,
        "open bid":,
        "open ask":,
        "high bid":,
        "high ask":,
        "low bid":,
        "low ask":,
        "close bid":,
        "close ask":,
        "complete":
    }

"MV" midpoint-based candlesticks with tick volume

    {
        "timestamp":,
        "open mid":,
        "high mid":,
        "low mid":,
        "close mid":,
        "volume":,
        "complete":
    }

"BAV" - BID/ASK-based candlesticks with tick volume

    {
        "timestamp":,
        "open bid":,
        "open ask":,
        "high bid":,
        "high ask":,
        "low bid":,
        "low ask":,
        "close bid":,
        "close ask":,
        "volume":,
        "complete":
    }

The fields in the above candlesticks have the following meanings:

* TS  - Start timestamp of the candlestick
* O_b - Open Tick Bid
* O_a - Open Tick Ask
* O_m - Open Tick Mid
* H_b - High Tick Bid
* H_a - High Tick Ask
* H_m - High Tick Mid
* L_b - Low Tick Bid
* L_a - Low Tick Ask
* L_m - Low Tick Mid
* C_b - Close Tick Bid
* C_a - Close Tick Ask
* C_m - Close Tick Mid
* V   - Candlestick tick volume
* DB  - "Dancing Bear". This optional field indicates that the candlestick
        may still be modified by the creation of ticks in the future because
        the current server time is contained by the time range which the
        candlestick represents.
