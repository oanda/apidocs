# Position Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/positions](https://github.com/oanda/apidocs/blob/master/sections/positions.md#get-v1accountsaccount_idpositions) | Get a list of open positions |
| [GET /1/accounts/:account_id/positions/:instrument](https://github.com/oanda/apidocs/blob/master/sections/positions.md#get-v1accountsaccount_idpositionsinstrument) | Get a list of open position for :instrument |
| [DELETE /v1/accounts/:account_id/positions/:instrument](https://github.com/oanda/apidocs/blob/master/sections/positions.md#delete-v1accountsaccount_idpositionsinstrument) | Close a position for a particular instrument |


## GET /v1/accounts/:account_id/positions
Get a list of all open positions for :account_id. 

#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/positions

#### Response
    {
      "positions" : [
        { "direction" : "long", "instrument": "EUR_USD", "units": 1000, "avgPrice": 25.23 },
        { "direction" : "short", "instrument": "USD_CAD", "units": 10000, "avgPrice": 325.56 }
     ]
    }



## GET /v1/accounts/:account_id/positions/:instrument
Get a list of open positions in :instrument for :account_id. 
#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR_USD

#### Response
    {
        "direction" : "short",
        "instrument" : "EUR_USD",
        "units" : 9,
        "avgPrice" : 1.3093
    }


## DELETE /v1/accounts/:account_id/positions/:instrument
Close out an existing position for :instrument.  

#### Request
    http://api-sandbox.oanda.com/v1/accounts/1234/position/EUR_USD

#### Response
    {
      "ids" : [
         12345, 12346, 12347
      ],   // Contains a list of transaction ids created as a result of the close position, including the id of the trades that were closed
      "instrument" : "EUR_USD",
      "totalUnits": 1234,
      "price" : 1.2345
    }


