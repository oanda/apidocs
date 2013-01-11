# Trade Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /accounts/:account_id/trades | Get a list of open trades |
| POST /accounts/:account_id/trades | Create a open trade |
| GET /accounts/:account_id/trades/:trade_id | Get information of an open trade |
| PUT /accounts/:account_id/trades/:trade_id | Modify stop loss, take profit, trailing stop an open trade |
| DELETE /accounts/:account_id/trades/:trade_id | Close an open trade |


## GET /accounts/:account_id/trades

#### Request
    https://api.oanda.com/v1/accounts/12345/trades?instrument=EUR/USD&maxCount=4

#### Response
    {
      "trades" : [
        { "id" : 12345, "units" : 5, "direction" : "long", "instrument" : "EUR/USD", "time" : "2013-01-09T22:02:46Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 10 },
        { "id" : 12344, "units" : 100, "direction" : "short", "instrument" : "EUR/USD", "time" : "2013-01-12T17:02:46Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 10 },
        { "id" : 890, "units" : 100, "direction" : "short", "instrument" : "EUR/USD", "time" : "2013-01-15T01:02:46Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 10 },
        { "id" : 789, "units" : 100, "direction" : "short", "instrument" : "EUR/USD", "time" : "2013-01-22T03:02:46Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 10 }    
      ],
      "nextPage" : "https:\/\/api.oanda.com\/v1\/accounts\/1\/trades?maxCount=4&maxTradeId=788"
    }

#### Query Parameters

**Optional**

* **maxTradeId**:  The server will return trades with id less than or equal to this, in descending order (about pagination).
* **maxCount**: Maximum number of open trades to return. Default: 50 Max value: 500
* **instrument**: Restrict open trade for a specific instrument. Default: all
* **tradeIds**: A common separated list of trades to retrieve.

#### Required scope
read

## POST /accounts/:account_id/trades
#### Request
    curl -X POST -d 'instrument=EUR/USD&units=2&direction=short' https://api.oanda.com/v1/accounts/12345/trades

#### Response
    {
        "ids" : [207654103, 207654104]   // List of transaction ids resulted from the trade include trades opposition direction that were partially closed
        "units" : 2,
        "direction" : "short",
        "instrument" : "EUR\/USD",
        "time" : "2012-08-13T20:52:16Z",
        "price" : 1.23325,
        "takeProfit" : 0,
        "stopLoss" : 0,
        "trailingStop" : 0
    }

#### Data Parameters
** Required **


* **instrument**: Instrument to open trade on
* **units**: Number of units to open trade for

**Optional**

* **type** market (default), fillOrKill, ImmediateOrCancel (More about order types)
* **direction** long (default) or short
* **price** User price. All trade request will be executed at server price
* **lowPrice** Minimum execution price
* **highPrice** Maximum execution price
* **stopLoss** Stop Loss value
* **takeProfit** Take Profit value
* **trailingStop** Trailing Stop distance in pipettes

#### Required scope
trade

## GET /accounts/:account_id/trade/:trade_id

#### Request
    https://api.oanda.com/v1/accounts/1234/trade/43211

#### Response
    {
      "id" : 43211,                        // The ID of the trade
      "units" : 5,                         // The number of units in the trade
      "direction" : "long",                // The direction of the trade
      "instrument" : "EUR_USD",            // The symbol of the instrument of the trade
      "time" : "2012-06-11T21:22:11Z",     // The time of the trade (in RFC3339 format)
      "price" : 1.45123,                   // The price the trade was executed at
      "takeProfit" : 1.7,                  // The take-profit associated with the trade, if any
      "stopLoss" : 1.4,                    // The stop-loss associated with the trade, if any
      "trailingStop" : 10                  // The trailing stop associated with the trade, if any
    }

#### Required scope
read




## PUT /accounts/:account_id/trade/:trade_id

#### Request
    curl -X PUT -d 'stopLoss=1.6' https://api.aonda.com/v1/trade/43211

#### Response
    {
      "id" : 43211,                       // The ID of the trade
      "units" : 5,                        // The number of units in the trade
      "direction" : "long",               // The direction of the trade
      "instrument" : "EUR/USD",           // The symbol of the instrument of the trade
      "time" : "2012-06-11T21:22:11Z",    // The time of the trade (in RFC3339 format)
      "price" : 1.45123,                  // The price the trade was executed at
      "takeProfit" : 1.7,                 // The take-profit associated with the trade, if any
      "stopLoss" : 1.6,                   // The stop-loss associated with the trade, if any
      "trailingStop" : 10                 // The trailing stop associated with the trade, if any
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| stopLoss | Stop Loss value |
| takeProfit | Take Profit value |
| trailingStop | Trailing Stop distance in pipettes |

#### Required scope
trade



## DELETE /accounts/:account_id/trade/:trade_id

#### Request
    curl -X DELETE https://api.aonda.com/v1/accounts/1234/trade/43211

#### Response
    {
      "id" : 54332,               // The ID of the close trade transaction
      "price" : 1.30601           // The pirce trade executed at
      "instrument" : "EUR/USD",   // The symbol of the instrument of the trade
      "profit" :  0.005           // The profit of the trade in dollar (home cuur??)
      "direction" : "short"
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| price | Price the client would like to close trade at.  This value will NOT be used by the server |

#### Required scope
trade

## GET template
#### Request
#### Response
#### Parameters
#### Required scope
