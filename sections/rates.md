# Rate Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/instruments](#get-v1instruments) | Instrument Discovery |
| [GET /v1/quote](#get-v1quote) | Get current price for specified instrument(s) |
| [GET /v1/history](#get-v1history) | Get historical rates for an instrument |

## GET /v1/instruments

Return a list of instruments (currency pairs, CFDs, and commodities) that are available on the OANDA platform.

#### Request
    http://api-sandbox.oanda.com/v1/instruments

#### Response
    {
         "instruments" : [
             {"instrument":"AUD_CAD", "displayName" : "AUD/CAD", "pip" : "0.0001", maxTradeUnits: 10000},
             {"instrument":"AUD_CHF", "displayName" : "AUD/CHF", "pip" : "0.0001", maxTradeUnits: 10000},
             .
             .
             {"instrument":"ZAR_JPY", "displayName" : "ZAR/JPY", "pip" : "0.0001", maxTradeUnits: 10000}
         ]
    }

#### Query Parameters
**Optional**

* __fields__: A (URL encoded) comma separated list of instrument fields that are to be returned in the response.
              The __instrument__ field will be returned regardless of the input to this query parameter.
              Please see the Response Parameters section below for a list of valid values.
		


		http://api-sandbox.oanda.com/v1/instruments?fields=precision%2CmaxTrailingStop%2CminTrailingStop%2CmarginRate

   		{
   		  "instruments" : [
   		  {"instrument":"AUD_CAD", "precision" : 0.00001, "maxTrailingStop" : 12, minTrailingStops: 4, marginRate: 0.02},
   		  {"instrument":"AUD_CHF", "precision" : 0.00001, "maxTrailingStop" : 12, minTrailingStops: 4, marginRate: 0.04},
   		  .
   		  .
   		  {"instrument":"ZAR_JPY", "precision" : 0.0001, "maxTrailingStop" : 8, minTrailingStops: 6, marginRate: 0.12},
   		  ]
		}
		

#### Response Parameters


* **instrument**: Name of the instrument.  This value should be use when used to fetch prices and create orders and trades.
* **displayName**: Display name for end user.
* **pip**: Value of 1 pip for the instrument. [More on pip](http://www.babypips.com/school/pips-and-pipettes.html)
* **maxTradeUnits**: The maximum number of units that can be traded for the instrument.
* **precision**: The smallest unit of measurement to express the change in value between the instrument pair. 
* **maxTrailingStop**: The maximum trailing stop value (in pips) that can be set when trading the instrument.
* **minTrailingStop**: The minimum trailing stop value (in pips) that can be set when trading the instrument.
* **marginRate**: The margin requirement for the instrument. A 3% margin rate will be represented as 0.03.
 
If the __fields__ parameter was not specified in the request, the default instrument fields returned are __instrument__, __displayName__, __pip__, __maxTradeUnits__.

## GET /v1/quote

Fetch live prices for specified instruments that are available on the OANDA platform.

#### Request
    http://api-sandbox.oanda.com/v1/quote?instruments=EUR_USD%2CUSD_JPY%2CZAR_CAD

#### Response
	{
		"prices":
		[
			{
				"instrument":"EUR_USD",
				"time":2013-06-21T17:41:04.648747Z,  // time in RFC3339 format
				"bid":1.31513,
				"ask":1.31528
			},
			{
				"instrument":"USD_JPY",
				"time":2013-06-21T17:49:02.475381Z,
				"bid":97.618,
				"ask":97.633
			},
			{
				"instrument":"EUR_CAD",
				"time":2013-06-21T17:51:38.063560Z,
				"bid":1.37489,
				"ask":1.37517,
				"halted": true                    // this response parameter will only appear if the instrument is currently halted on the Oanda platform.
			}
		]
	}

#### Query Parameters

**Required**
* __instruments__:  A (URL encoded) comma separated list of instruments to fetch prices for.  Values should be one of the available instrument from the /v1/instruments response.


## GET /v1/history

#### Request
    http://api-sandbox.oanda.com/v1/history?instrument=EUR_USD&count=2&candleFormat="midpoint"

#### Response
    {
        "instrument" : "EUR_USD",
        "granularity": "S5",
        "candles": [
           {
               "time": 2013-06-21T17:41:00Z,  // time in RFC3339 format
               "openMid": 1.30237,
               "highMid": 1.30237,
               "lowMid": 1.30237,
               "closeMid": 1.30237,
               "volume" : 5000,
               "complete": true
           },
           {
               "time": 2013-06-21T17:41:05Z,  // time in RFC3339 format
               "openMid": 1.30242,
               "highMid": 1.30242,
               "lowMid": 1.30242,
               "closeMid": 1.30242,
               "volume" : 2000,
               "complete": true
           }
        ]
        
    }


#### Query Parameters

**Required**

* __instrument__:  Name of the instrument to retreive history for.  The instrument should be one of the available instrument from the /v1/instruments response.

**Optional**

* __granularity__<sup>1</sup>: The time range represented by each candlestick.  The value specified will determine the alignment of the first candlestick.
    
	Valid values are:

	* __Top of the minute alignment__
		* "S5"  - 5 seconds
		* "S10" - 10 seconds
		* "S15" - 15 seconds
		* "S30" - 30 seconds
		* "M1"  - 1 minute
	* __Top of the hour alignment__
		* "M2"  - 2 minutes
		* "M3"  - 3 minutes
		* "M5"  - 5 minutes
		* "M10" - 10 minutes
		* "M15" - 15 minutes
		* "M30" - 30 minutes
		* "H1"  - 1 hour
	* __Start of day alignment (12 am, Timezone/New York)__
		* "H2"  - 2 hours
		* "H3"  - 3 hours
		* "H4"  - 4 hours
		* "H6"  - 6 hours
		* "H8"  - 8 hours
		* "H12" - 12 hours
		* "D"   - 1 Day
	* __Start of week alignment (Saturday)__
		* "W"   - 1 Week
	* __Start of month alignment (First day of the month)__
		* "M"   - 1 Month
	

The default for __granularity__ is "S5" if the granularity parameter is not provided.

* __count__: The number of candles to return in the response. This paramater may be ignored by the server depending on the time range provided. See "Time and Count Semantics" below for a full description.  * 
If not specified, __count__ will default to 500. The maximum acceptable value for __count__ is 5000.  
             
	__count__ should not be specified if both the __start__ and __end__ parameters are also specified.

* __start__<sup>2</sup>: The start timestamp for the range of candles requested.  Must be specified in RFC3339 format.

* __end__<sup>2</sup>: The end timestamp for the range of candles requested.  Must be specified in RFC3339 format.

* __candleFormat__: Candlesticks representation ([about candestick representation](#candlestick-representation)). This can be one of the following:
	* "midpoint" - Midpoint based candlesticks.
	* "bidask" - Bid/Ask based candlesticks

	The default for __candleFormat__ is "bidask" if the candleFormat parameter is not specified.

* __includeFirst__: A boolean field which may be set to "true" or "false". If it is set to "true", the candlestick covered by the <i>start</i> timestamp will be returned. If it is set to "false", this candlestick will not be returned.  
This field exists to provide clients a mechanism to not repeatedly fetch the most recent candlestick which it is not a "Dancing Bear".  
If __includeFirst__ is not specified, the default setting is "true".
<p><br>
<sup>1</sup> No candles are published for intervals where there are no ticks.  This will result in gaps in between time periods.<br>
<sup>2</sup> If neither __start__ nor __end__ time are specified by the requester, __end__ will default to the current time and __count__ candles will be returned.<br>
<p>


## Candlestick Representation


__midpoint__ midpoint-based candlesticks with tick volume

    {
        "timestamp":<TS>,
        "openMid":<O_m>,
        "highMid":<H_m>,
        "lowMid":<L_m>,
        "closeMid":<C_m>,
        "volume":<V>,
        "complete":<DB>
    }

__bidask__ - BID/ASK-based candlesticks with tick volume

    {
        "timestamp":<TS>,
        "openBid":<O_b>,
        "openAsk":<O_a>,
        "highBid":<H_b>,
        "highAsk":<H_a>,
        "lowBid":<L_b>,
        "lowAsk":<L_a>,
        "closeBid":<C_b>,
        "closeAsk":<C_a>,
        "volume":<V>,
        "complete":<DB>
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
* DB  - "Dancing Bear". This field indicates that the candlestick
        may still be modified by the creation of ticks in the future because
        the current server time is contained by the time range which the
        candlestick represents.

