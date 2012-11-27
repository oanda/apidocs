# Position Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /v1/accounts/:account_id/positions | Get a list of open positions |
| GET /1/accounts/:account_id/positions/:instrument | Get a list of open position for :instrument |
| DELETE /v1/accounts/:account_id/positions/:instrument | Close a position |


## GET /v1/accounts/:account_id/positions

#### Request
    https://api-sandbox.oanda.com/v1/accounts/12345/positions

#### Response
    {
      "positions" : [
        { "direction" : "long", "instrument": "EUR/USD", "units": 1000, "avgPrice": 25.23 },
        { "direction" : "short", "instrument": "USD/CAD", "units": 10000, "avgPrice": 325.56 }
     ]
    }

#### Required scope
read


## GET /v1/accounts/:account_id/positions/:instrument
#### Request
    https://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR/USD

#### Response
    {
        "direction" : "short",
        "instrument" : "EUR\/USD",
        "units" : 9,
        "avgPrice" : 1.3093
    }

#### Required scope
read

## DELETE /v1/accounts/:account_id/positions/:instrument

#### Request
    https://api-sandbox.oanda.com/v1/accounts/1234/position/EUR/USD

#### Response
    {
      "ids" : [
         12345, 12346, 12347
      ],
      "instrument" : "EUR/USD",
      "totalUnits": 1234,
      "price" : 1.2345
    }

#### Required scope
trade

