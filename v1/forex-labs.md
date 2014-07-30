---
title: Forex Labs | OANDA API
---

# Forex Labs Endpoints

* TOC
{:toc}

----------------------------

## Forex Labs

Description.

More Description.

-----------------

## Calendar

Returns up to 1 year worth of economic calendar information relevant to an instrument.  For example, if the instrument is EUR_USD, then all economic information relevant to the Euro and the US Dollar will be included.  Some of the entries are strictly news about an important meeting, while other entries may contain economic indicator data. More info [here](link).


    GET /labs/v1/calendar


#### Input Query Parameters

instrument
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

period
: _Required_ Period of time in seconds to retrieve calendar information for.
Say something about supported period

#### Example

    curl "http://api-sandbox.oanda.com/labs/v1/calendar?instrument=AUD_CAD&period=2950000"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### Body

~~~json
[
    {
        "title": "Building Permits",
        "timestamp": 1379507400,
        "unit": "k",
        "currency": "USD",
        "forecast": "910",
        "previous": "954",
        "actual": "926",
        "market": "950"
    },
    {
        "title": "FOMC - Fed Funds Rate",
        "timestamp": 1379527200,
        "unit": "%",
        "currency": "USD",
        "forecast": "0.25",
        "previous": "0.25",
        "actual": "0.25",
        "market": "0.25"
    },
    {
        "title": "Fed Chairman Bernanke holds press conference following FOMC meeting on interest rate policy",
        "timestamp": 1379529000,
        "unit": "",
        "currency": "USD"
    }
]
~~~

#### JSON Response Fields

title
: The title of the event, breifly describing what the event was

timestamp
: Time of the event. This time will always be returned as a unix timestamp.

unit
: This field describes the data found in the [forecast], [previous], [actual] and [market] fields. The possible values are: % meaning the data is a percentage, k meaning an amount, or blank.

currency
: This is the currency that is effected by the news event.

forecast
: Description

previous
: Description

actual
: Description

market
: Description

-----------------------

## Historical Position Ratios

Returns up to 1 year worth of historical position ratios for a supported instrument. More info [here](link).


    GET /labs/v1/historical_positions_ratios


#### Input Query Parameters

instrument
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

period
: _Required_ Period of time in seconds to retrieve calendar information for.
Say something about supported period

#### Example

    curl "http://api-sandbox.oanda.com/labs/v1/historical_position_ratios?instrument=AUD_CAD&period=3600"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### Body

~~~json
{
  "data": {
    "EUR_USD": {
      "data": [
        [
          1382026800,
          26.11,
          1.3663
        ],
        [
          1382028001,
          26.11,
          1.3666
        ],
        [
          1382029200,
          26.11,
          1.3662
        ],
        [
          1382030401,
          26.11,
          1.3665
        ],
        [
          1382031600,
          26.11,
          1.3666
        ]
      ],
      "label": "EUR/USD"
    }
  }
}
~~~

#### JSON Response Fields

timestamp
: This is the first field in the Json array, representing the time at which the position ratio is valid.

percentage
: The second field in the Json array. This is a decimal number representing the percentage of long positions for this 15 minute range.

exchange rate
: The exchange rate for the instrument at that point in time

label
: Display name of currency

-----------------------

## Spreads

Returns up to 1 year worth of spread information a supported instrument.  The returned data is divided in 15 minute intervals.  For each period, we provide the time weighted average, mininum, and maximum spread. More info [here](link).


    GET /labs/v1/spreads


#### Input Query Parameters

instrument
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

period
: _Required_ Period of time in seconds to retrieve calendar information for.
Say something about supported period

#### Example

    curl "http://api-sandbox.oanda.com/labs/v1/spreads?instrument=AUD_CAD&period=3600"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### Body

~~~json
{
  "max": [
    [
      1381996800,
      2.8
    ],
    [
      1381997700,
      2.3
    ],
    [
      1381998600,
      2.1
    ]
  ]
  "min": [
    [
      1381996800,
      0.8
    ],
    [
      1382001300,
      0.7
    ],
    [
      1382002200,
      0.8
    ]
  ]
  "avg": [
    [
      1381996800,
      1.15367
    ],
    [
      1381997700,
      1.15878
    ],
    [
      1381998600,
      1.09433
    ]
  ]
}
~~~

#### JSON Response Fields

max
: Maximum spread for this 15 minute period.

min
: Minimum spread for this 15 minute period.

avg
: Average spread for this 15 minute period.

timestamp
: Time at the end of the 15 minute period, always returned as a unix timestamp

spread
: This is the spread for the 15 minute period.

-----------------------

## commitments of traders

Returns up to 4 years worth of Commitments of Traders data from the CFTC for supported currencies.  This is essentially CFTC's Non-Commercial order book data.  More information can be found on the [CFTC's website](http://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm)


    GET /labs/v1/commitments_of_traders


#### Input Query Parameters

instrument
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

#### Example

    curl "http://api-sandbox.oanda.com/labs/v1/commitments_of_traders?instrument=EUR_USD"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### Body

~~~json
{
    TODO:
    {
        field : "data"
    }
}
~~~

#### JSON Response Fields

The response consists of one key and value pair.  The value is an array of hashes.  Since there is no period parameter for this model, all 4 years worth of data is returned at once.

oi
: Overall Interest

ncl
: Non-Commercial Long

price
: Exchange Rate

date
: Time represented as a unix timestamp

ncs
: Non-Commercial Short

unit
: Gives the sizes of a single contract in Overall Interest, Non-Commercial Long, and Non-Commercial Short

-----------------

## Orderbook data

Returns up to 1 year worth of OANDA Order book data. More info [here](link)


    GET /labs/v1/orderbook_data


#### Input Query Parameters

instrument
: _Required_ An URL encoded comma (*%2C*) separated list of instruments to fetch prices for. 

period
: _Required_ Period of time in seconds to retrieve calendar information for.
Say something about supported period

#### Example

    curl "http://api-sandbox.oanda.com/labs/v1/orderbook_data?instrument=EUR_USD&period=3600"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### Body

~~~json
{
  "1382042401": {
    "price_points": {
      "1.359": {
        "os": 0.638,
        "ps": 0.2173,
        "pl": 0.67,
        "ol": 0.1535
      },
      "1.3365": {
        "os": 0.0512,
        "ps": 0.4346,
        "pl": 0.0905,
        "ol": 0.4435
      },
      "1.348": {
        "os": 0.0546,
        "ps": 1.8109,
        "pl": 0.1449,
        "ol": 0.3992
      },
      "1.4285": {
        "os": 0.0068,
        "ps": 0,
        "pl": 0,
        "ol": 0.0273
      },

...

      "1.335": {
        "os": 0.1126,
        "ps": 0.5433,
        "pl": 0.0362,
        "ol": 0.7779
      },
      "1.3705": {
        "os": 0.1126,
        "ps": 0,
        "pl": 0,
        "ol": 0.0614
      },
      "1.317": {
        "os": 0.0444,
        "ps": 0.1992,
        "pl": 0.0724,
        "ol": 0.5664
      }
    },
    "rate": 1.3676
  },
  "1382037600": {
    "price_points": {
      "1.359": {
        "os": 0.638,
        "ps": 0.2173,
        "pl": 0.67,
        "ol": 0.1535
      },
      "1.3365": {
        "os": 0.0512,
        "ps": 0.4346,
        "pl": 0.0905,
        "ol": 0.4435
      },
      "1.348": {
        "os": 0.0546,
        "ps": 1.8109,
        "pl": 0.1449,
        "ol": 0.3992
      },


...

      "1.381": {
        "os": 0.0614,
        "ps": 0,
        "pl": 0,
        "ol": 0.058
      },
      "1.335": {
        "os": 0.1126,
        "ps": 0.5433,
        "pl": 0.0362,
        "ol": 0.7779
      },
      "1.3705": {
        "os": 0.1126,
        "ps": 0,
        "pl": 0,
        "ol": 0.0614
      },
      "1.317": {
        "os": 0.0444,
        "ps": 0.1992,
        "pl": 0.0724,
        "ol": 0.5664
      }
    },
    "rate": 1.3677
  }
}
~~~

#### JSON Response Fields

os
: Percentage short orders

ol
: Percentage long orders

ps
: Percentage short positions

pl
: Percentage long positions

rate
: This is the rate at that specific time

