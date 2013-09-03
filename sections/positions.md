# Position Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/positions](https://github.com/oanda/apidocs/blob/master/sections/positions.md#get-v1accountsaccount_idpositions) | Get a list of open positions |
| [GET /1/accounts/:account_id/positions/:instrument](https://github.com/oanda/apidocs/blob/master/sections/positions.md#get-v1accountsaccount_idpositionsinstrument) | Get information about an open position |
| [DELETE /v1/accounts/:account_id/positions/:instrument](https://github.com/oanda/apidocs/blob/master/sections/positions.md#delete-v1accountsaccount_idpositionsinstrument) | Close a position |


## GET /v1/accounts/:account_id/positions
Get a list of all open positions for :account_id. 

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions"

#### Response
    {
      "positions" : [
        { "side" : "buy", "instrument": "EUR_USD", "units": 1000, "avgPrice": 25.23 },
        { "side" : "sell", "instrument": "USD_CAD", "units": 10000, "avgPrice": 325.56 }
     ]
    }



## GET /v1/accounts/:account_id/positions/:instrument
Get an open position for :instrument in :account_id if it exists.
#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR_USD"

#### Response
    {
        "side" : "sell",
        "instrument" : "EUR_USD",
        "units" : 9,
        "avgPrice" : 1.3093
    }


## DELETE /v1/accounts/:account_id/positions/:instrument
Close out an existing position for :instrument.  

#### Request
    curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/1234/positions/EUR_USD"

#### Response
    {
      "ids" : [
         12345, 12346, 12347
      ],   // Contains a list of transaction ids created as a result of the close position, including the id of the trades that were closed
      "instrument" : "EUR_USD",
      "totalUnits": 1234,
      "price" : 1.2345
    }


