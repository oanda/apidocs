---
title: Forex Labs | OANDA API
---

# Forex Labs Endpoints

* TOC
{:toc}

----------------------------

## Forex Labs

As part of our OANDA API offering, we provide access to OANDA fxLabs for forex analysis, signals and tools. The information provided by these API requests is available on our [Forex Labs](http://fxtrade.oanda.ca/analysis/labs) website.

All the endpoints on this page require authentication, as such they cannot be accessed through the sandbox environment. Examples on this page are shown using the api-fxpractice environment.

<p style="color: red;font-style: italic;">This offering is under development so you can expect changes to some of the resources. Please monitor our <a href="/docs/v1/release-notes">release notes</a> page for any recent changes.</p> 

-----------------

## Calendar

Returns up to 1 year worth of economic calendar information relevant to an instrument.  For example, if the instrument is EUR_USD, then all economic information relevant to the Euro and the US Dollar will be included.  Some of the entries are strictly news about an important meeting, while other entries may contain economic indicator data.


    GET /labs/v1/calendar


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to retrieve calendar data for. All tradable instruments are supported.

period
: _Required_ Period of time in seconds to retrieve calendar data for. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * __3600__     - 1 hour
    * __43200__    - 12 hours
    * __86400__    - 1 day
    * __604800__   - 1 week
    * __2592000__  - 1 month
    * __7776000__  - 3 months
    * __15552000__ - 6 months
    * __31536000__ - 1 year

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/calendar?instrument=EUR_USD&period=2592000" -H "Authorization: Bearer <access-token>"

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
: The title of the event.

timestamp
: Time of the event, returned as a unix timestamp.

unit
: This field describes the data found in the __forecast__, __previous__, __actual__ and __market__ fields. Some possible values are: % meaning the data is a percentage, k meaning thousands of units, or blank if there is no data associated with the event.

currency
: This is the currency that is affected by the news event.

forecast
: The forecasted value.

previous
: Shows the value of the previous release of the same event.

actual
: The actual value, this is only available after the event has taken place.

market
: The market expectation of what the value was.

-----------------------

## Historical Position Ratios

Returns up to 1 year worth of historical position ratios for a supported instrument. More info at our [Forex Labs](http://fxtrade.oanda.ca/analysis/historical-positions) page.


    GET /labs/v1/historical_position_ratios


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to retrieve historical position ratios for. Supported instruments: AUD_JPY, AUD_USD, EUR_AUD, EUR_CHF, EUR_GBP, EUR_JPY, EUR_USD, GBP_CHF, GBP_JPY, GBP_USD, NZD_USD, USD_CAD, USD_CHF, USD_JPY, XAU_USD, XAG_USD.

period
: _Required_ Period of time in seconds to retrieve historical position ratios data for. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * __86400__    - 1 day    - 20 minute snapshots
    * __172800__   - 2 day    - 20 minute snapshots
    * __604800__   - 1 week   - 1 hour snapshots
    * __2592000__  - 1 month  - 3 hour snapshots
    * __7776000__  - 3 months - 3 hour snapshots
    * __15552000__ - 6 months - 3 hour snapshots
    * __31536000__ - 1 year   - daily snapshots

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/historical_position_ratios?instrument=EUR_USD&period=86400" -H "Authorization: Bearer <access-token>"

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
: The first field in the array. The time returned as a unix timestamp.

long position ratio
: The second field in the array. The percentage of long positions for this snapshot. A 45.66% long position ratio is represented as 45.66.

exchange rate
: The third field in the array. The exchange rate for the instrument at that point in time.

label
: Display name of currency.

-----------------------

## Spreads

Returns up to 1 year worth of spread information for a supported instrument.  The returned data is divided in 15 minute intervals.  For each period, we provide the time weighted average, minimum, and maximum spread. More info at our [Forex Labs](http://fxtrade.oanda.ca/why/spreads/recent) page.


    GET /labs/v1/spreads


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to retrieve spread data for. All tradable instruments are supported.

period
: _Required_ Period of time in seconds to retrieve spread data for. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * __3600__     - 1 hour
    * __43200__    - 12 hour
    * __86400__    - 1 day
    * __604800__   - 1 week
    * __2592000__  - 1 month
    * __7776000__  - 3 months
    * __15552000__ - 6 months
    * __31536000__ - 1 year

unique
: _Optional_ This parameter specifies whether to return identical adjacent spreads. If "0", all spreads are returned. If "1", adjacent duplicate spreads are omitted. This is used to reduce bandwidth consumption.

    The default for __unique__ is "1" if the unique parameter is not specified.

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/spreads?instrument=EUR_USD&period=3600" -H "Authorization: Bearer <access-token>"

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

The main response hash has 3 keys: max, min, and avg.  Corresponding to these 3 keys are arrays of spread entries.

max
: Maximum spreads for the 15 minute intervals.

min
: Minimum spreads for the 15 minute intervals.

avg
: Average spreads for the 15 minute intervals.

timestamp
: This is the first field of the array, representing the time at the end of the 15 minute interval, returned as a unix timestamp.

spread
: This is the second field of the array, representing the spread for the 15 minute interval.

-----------------------

## Commitments of Traders

Returns up to 4 years worth of Commitments of Traders data from the [CFTC](http://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm) for supported currencies.  This is essentially CFTC's Non-Commercial order book data. More info at our [Forex Labs](http://fxtrade.oanda.ca/analysis/commitments-of-traders) page.


    GET /labs/v1/commitments_of_traders


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to retrieve Commitments of Traders data for. Supported instruments: AUD_USD, GBP_USD, USD_CAD, EUR_USD, USD_JPY, USD_MXN, NZD_USD, USD_CHF, XAU_USD, XAG_USD.

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/commitments_of_traders?instrument=EUR_USD" -H "Authorization: Bearer <access-token>"

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

The response consists of one key value pair.  The value is an array of hashes.  Since there is no period parameter for this model, all 4 years worth of data is returned at once.

oi
: Overall Interest

ncl
: Non-Commercial Long

price
: Exchange Rate

date
: Time returned as a unix timestamp

ncs
: Non-Commercial Short

unit
: Gives the sizes of a single contract in Overall Interest, Non-Commercial Long, and Non-Commercial Short

-----------------

## Orderbook

Returns up to 1 year worth of OANDA Order book data. More info at our [Forex Labs](http://fxtrade.oanda.ca/analysis/forex-order-book) page.


    GET /labs/v1/orderbook_data


#### Input Query Parameters

instrument
: _Required_ Name of the instrument to retrieve orderbook data for. Supported instruments: AUD_JPY, AUD_USD, EUR_AUD, EUR_CHF, EUR_GBP, EUR_JPY, EUR_USD, GBP_CHF, GBP_JPY, GBP_USD, NZD_USD, USD_CAD, USD_CHF, USD_JPY, XAU_USD, XAG_USD.

period
: _Required_ Period of time in seconds to retrieve orderbook data for. Values not in the following list will be automatically adjusted to the nearest valid value.

	Valid values are:

    * __3600__     - 1 hour  - 20 minute snapshots
    * __21600__    - 6 hour  - 20 minute snapshots
    * __86400__    - 1 day   - 2 hour snapshots
    * __604800__   - 1 week  - two daily snapshots at 04:00 and 16:00 GMT
    * __2592000__  - 1 month - 1 day snapshots at 16:00 GMT
    * __31536000__ - 1 year  - 1 month snapshots first of the month at 12:00 GMT

#### Example

    curl "https://api-fxpractice.oanda.com/labs/v1/orderbook_data?instrument=EUR_USD&period=3600" -H "Authorization: Bearer <access-token>"

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

The response consists of key value pairs.  Each key is a unix timestamp of a snapshot.  Each snapshot consists of a hash of price points and a rate entry.  Each price point is a key value pair.  The rate entry is the market price at the snapshot time.

os
: Percentage short orders

ol
: Percentage long orders

ps
: Percentage short positions

pl
: Percentage long positions

rate entry
: This is the rate at that specific time

