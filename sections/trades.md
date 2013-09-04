# Trade Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/trades](https://github.com/oanda/apidocs/blob/master/sections/trades.md#get-v1accountsaccount_idtrades) | Get a list of open trades |
| [POST /v1/accounts/:account_id/trades](https://github.com/oanda/apidocs/blob/master/sections/trades.md#post-v1accountsaccount_idtrades) | Open a new trade |
| [GET /v1/accounts/:account_id/trades/:trade_id](https://github.com/oanda/apidocs/blob/master/sections/trades.md#get-v1accountsaccount_idtradestrade_id) | Get information about an open trade |
| [PUT /v1/accounts/:account_id/trades/:trade_id](https://github.com/oanda/apidocs/blob/master/sections/trades.md#put-v1accountsaccount_idtradestrade_id) | Modify stop loss, take profit, trailing stop on an open trade |
| [DELETE /v1/accounts/:account_id/trades/:trade_id](https://github.com/oanda/apidocs/blob/master/sections/trades.md#delete-v1accountsaccount_idtradestrade_id) | Close an open trade |


## GET /v1/accounts/:account_id/trades

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/trades?instrument=EUR_USD&count=4"

#### Response
    {
      "trades" : [
        { "id" : 12345, "units" : 5, "side" : "buy", "instrument" : "EUR_USD", "time" : "2013-07-03T14:30:38Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 50 },
        { "id" : 12344, "units" : 100, "side" : "sell", "instrument" : "EUR_USD", "time" : "2013-07-03T14:30:38Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 50 },
        { "id" : 890, "units" : 100, "side" : "sell", "instrument" : "EUR_USD", "time" : "2013-07-03T14:30:38Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 50 },
        { "id" : 789, "units" : 100, "side" : "sell", "instrument" : "EUR_USD", "time" : "2013-07-03T14:30:38Z", "price" : 1.45123, "stopLoss" : 1.2, "takeProfit" : 1.7, "trailingStop" : 50 }    
      ],
      "nextPage" : "http:\/\/api-sandbox.oanda.com\/v1\/accounts\/1\/trades?count=4&maxId=788"
    }

#### Query Parameters

**Optional**

* **maxId**:  The server will return trades with id less than or equal to this, in descending order (for pagination).
* **count**: Maximum number of open trades to return. Default: 50 Max value: 500
* **instrument**: Retrieve open trades for a specific instrument only Default: all
* **ids**: A (URL encoded) comma separated list of trades to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.

####Pagination

Trades can be paginated with the count and maxId parameters.
At most, a maximum of 50 trades can be returned in one query. 
If more trades exist than specified by the given or default count, a url with maxId set to the next unreturned trade will be constructed.

## POST /v1/accounts/:account_id/trades
#### Request
    curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "instrument=EUR_USD&units=2&side=sell" "http://api-sandbox.oanda.com/v1/accounts/12345/trades"

#### Response
    {
        "opened" : 12355                // New trade that was opened by the request.
        "updated": 0                    // Trade that was partially closed by the request. Oldest trades updated first.
        "closed": []                    // List of trades closed by the request
        "interest": []                  // List of trades that have incurred interest since the last request.
        "units" : 2,                    // Number of units traded
        "side" : "sell"                 // Direction trade was executed in
        "instrument" : "EUR_USD",       // Symbol of instrument trade was created with
	"time" : "2013-07-11T15:21:19Z" // Execution time of trade (in RFC3339 format)
        "price" : 1.25955,              // Price the trade was executed at
        "marginUsed" : 0.063,           // Percentage of available margin used	
	"takeProfit" : 0,               // The take-profit associated with the trade, if any
        "stopLoss" : 0,                 // The stop-loss associated with the trade, if any
        "trailingStop" : 0              // The trailing stop associated with the trade, if any
    }

#### Data Parameters
**Required**

* **instrument**: Instrument to open trade on
* **units**: Number of units to open trade for
* **side**: buy or sell

**Optional**

* **lowerBound**: Minimum execution price
* **upperBound**: Maximum execution price
* **takeProfit**: Take Profit price
* **stopLoss**: Stop Loss price
* **trailingStop**: Trailing Stop distance in pipettes

[Learn more about order types, stop loss, take profit, and trailing stop](http://fxtrade.oanda.com/learn/intro-to-currency-trading/first-trade/orders)


## GET /v1/accounts/:account_id/trades/:trade_id

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/1234/trades/43211"

#### Response
    {
      "id" : 43211,                          // The ID of the trade
      "units" : 5,                           // The number of units in the trade
      "side" : "buy",                        // The direction of the trade
      "instrument" : "EUR_USD",              // The symbol of the instrument of the trade
      "time" : "2013-07-03T14:30:38Z",       // The time of the trade (in RFC3339 format)
      "price" : 1.45123,                     // The price the trade was executed at
      "takeProfit" : 1.7,                    // The take-profit associated with the trade, if any
      "stopLoss" : 1.4,                      // The stop-loss associated with the trade, if any
      "trailingStop" : 50                    // The trailing stop associated with the trade, if any
    }



## PUT /v1/accounts/:account_id/trades/:trade_id

#### Request
    curl -X PUT -H "Content-Type: application/x-www-form-urlencoded" -d "stopLoss=1.6" "http://api-sandbox.oanda.com/v1/accounts/1234/trades/43211"

#### Response
    {
      "id" : 43211,                          // The ID of the trade
      "units" : 5,                           // The number of units in the trade
      "side" : "buy",                        // The direction of the trade
      "instrument" : "EUR_USD",              // The symbol of the instrument of the trade
      "time" : "2013-07-03T14:30:38Z",       // The time of the trade (in RFC3339 format)
      "price" : 1.45123,                     // The price the trade was executed at
      "takeProfit" : 1.7,                    // The take-profit associated with the trade, if any
      "stopLoss" : 1.6,                      // The stop-loss associated with the trade, if any
      "trailingStop" : 50                    // The trailing stop associated with the trade, if any
    }

#### Parameters
**Optional**

* __stopLoss__: Stop Loss value
* __takeProfit__: Take Profit value
* __trailingStop__: Trailing Stop distance in pipettes



## DELETE /v1/accounts/:account_id/trades/:trade_id

#### Request
    curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/1234/trades/43211"

#### Response
    {
      "id" : 54332,                   // The ID of the close trade transaction
      "price" : 1.30601,              // The price the trade was closed at
      "instrument" : "EUR_USD",       // The symbol of the instrument of the trade
      "profit" :  0.005,              // The realized profit of the trade in units of base currency
      "side" : "sell"                 // The direction the trade was in
      "time" : "2013-07-12T16:09:15Z" // The time at which the trade was closed (in RFC3339 format)
    }

