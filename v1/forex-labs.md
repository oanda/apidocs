---
title: Forex Labs | OANDA API
---

# Forex Labs Endpoints

* TOC
{:toc}

----------------------------

## Forex Labs

As part of our OANDA API offering, we provide access to OANDA FXLabs for forex analysis, signals and FX tools. The information provided by these API requests is available on our [Forex Labs](http://fxtrade.oanda.ca/analysis/labs) website.

This offering is under developement so you can expect changes to some of the resources. Please monitor our [release notes](/docs/v1/release-notes) page for any recent changes. 

__Note:__ All the endpoints on this page require authentication, as such they cannot be access through the sandbox environment and must use either https://api-fxpractice.oanda.com/ or https://api-fxtrade.oanda.com/ as the base url. Examples on this page are shown with api-fxpractice.

-----------------

## Calendar

Returns up to 1 year worth of economic calendar information relevant to an instrument.  For example, if the instrument is EUR_USD, then all economic information relevant to the Euro and the US Dollar will be included.  Some of the entries are strictly news about an important meeting, while other entries may contain economic indicator data. More info [here](link). TODO find a link for this


    GET /labs/v1/calendar


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to for which to retrieve calendar data.

period
: _Required_ Period of time in seconds for which to retrieve calendar data. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * 3600     - 1 hour
    * 43200    - 12 hours
    * 86400    - 1 day
    * 604800   - 1 week
    * 2592000  - 1 month
    * 7776000  - 3 months
    * 15552000 - 6 months
    * 31536000 - 1 year

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/calendar?instrument=EUR_USD&period=2592000"

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
: This field describes the data found in the __forecast__, __previous__, __actual__ and __market__ fields. The possible values are: % meaning the data is a percentage, k meaning an amount, or blank.

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

Returns up to 1 year worth of historical position ratios for a supported instrument. More info [here](http://fxtrade.oanda.ca/analysis/historical-positions).


    GET /labs/v1/historical_positions_ratios


#### Input Query Parameters

instrument
: _Required_ Name of the instrument for which to retreive historical position ratios. Supported instruments: AUD_JPY, AUD_USD, EUR_AUD, EUR_CHF, EUR_GBP, EUR_JPY, EUR_USD, GBP_CHF, GBP_JPY, GBP_USD, NZD_USD, USD_CAD, USD_CHF, USD_JPY, XAU_USD, XAG_USD.

period
: _Required_ Period of time in seconds for which to retrieve calendar data. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * 86400    - 1 day    - 20 minute snapshots
    * 172800   - 2 day    - 20 minute snapshots
    * 604800   - 1 week   - 1 hour snapshots
    * 2592000  - 1 month  - 3 hour snapshots
    * 7776000  - 3 months - 3 hour snapshots
    * 15552000 - 6 months - 3 hour snapshots
    * 31536000 - 1 year   - daily snapshots

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/historical_position_ratios?instrument=EUR_USD&period=3600"

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

Returns up to 1 year worth of spread information a supported instrument.  The returned data is divided in 15 minute intervals.  For each period, we provide the time weighted average, mininum, and maximum spread. More info [here](http://fxtrade.oanda.ca/why/spreads/recent).


    GET /labs/v1/spreads


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to for which to retrieve spread data.

period
: _Required_ Period of time in seconds for which to retrieve calendar data. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * 3600     - 1 hour
    * 43200    - 12 hour
    * 86400    - 1 day
    * 604800   - 1 week
    * 2592000  - 1 month
    * 7776000  - 3 months
    * 15552000 - 6 months
    * 31536000 - 1 year

unique
: _optional_ This parameter sppecifies whether to return identical adjecent spreads. If 0 all spreads are returned, if 1 then adjacent duplicate spreads are omitted. This is used to reduce bandwith consumption.

    The default for __unique__ is "1" if the unique parameter is not specified.

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/spreads?instrument=EUR_USD&period=3600"

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

## Commitments of Traders

Returns up to 4 years worth of Commitments of Traders data from the CFTC for supported currencies.  This is essentially CFTC's Non-Commercial order book data.  More information can be found on the [CFTC's website](http://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm)


    GET /labs/v1/commitments_of_traders


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to for which to retrieve Commitments of Traders data.

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/commitments_of_traders?instrument=EUR_USD"

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
  "EUR_USD": [
    {
      "oi": "179512",
      "ncl": "85915",
      "price": "1.4643725",
      "date": 1199750400,
      "ncs": "34013",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "177003",
      "ncl": "81812",
      "price": "1.478495",
      "date": 1200355200,
      "ncs": "36830",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "184289",
      "ncl": "65581",
      "price": "1.464425",
      "date": 1200960000,
      "ncs": "41836",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "187780",
      "ncl": "62078",
      "price": "1.459195",
      "date": 1201564800,
      "ncs": "39622",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "199494",
      "ncl": "60106",
      "price": "1.479215",
      "date": 1202169600,
      "ncs": "47542",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "207601",
      "ncl": "55016",
      "price": "1.466445",
      "date": 1202774400,
      "ncs": "44721",
      "unit": "Contracts Of EUR 125,000"
    },


...

    {
      "oi": "218469",
      "ncl": "63355",
      "price": "1.3044125",
      "date": 1362441600,
      "ncs": "89471",
      "unit": "Contracts Of EUR 125,000"
    }
  ]
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

## Orderbook

Returns up to 1 year worth of OANDA Order book data. More info [here](http://fxtrade.oanda.ca/analysis/forex-order-book).


    GET /labs/v1/orderbook_data


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to for which to retrieve orderbook data.

period
: _Required_ Period of time in seconds for which to retrieve orderbook data. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * 3600     - 1 hour  - 20 minute snapshots
    * 21600    - 6 hour  - 20 minute snapshots
    * 86400    - 1 day   - 2 hour snapshots
    * 604800   - 1 week  - two daily snapshots at 04:00 and 16:00 GMT
    * 2592000  - 1 month - 1 day snapshots at 16:00 GMT
    * 31536000 - 1 year  - 1 month snapshots first of the month at 12:00 GMT

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/orderbook_data?instrument=EUR_USD&period=3600"

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

