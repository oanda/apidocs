# Position Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /accounts/:account_id/positions | Get a list of open positions |
| GET /accounts/:account_id/positions/:instrument | Get a list of open position for :instrument |
| DELETE /accounts/:account_id/positions/:instrument | Close a position |


## GET /accounts/:account_id/positions

#### Request
    https://api.oanda.com/v1/accounts/12345/positions

#### Respond
    {
      "positions" : [
        { "direction" : "long", "instrument": "EUR/USD", "units": 1000, "avgPrice": 25.23 },
        { "direction" : "short", "instrument": "USD/CAD", "units": 10000, "avgPrice": 325.56 }
     ]
    }

#### Required scope
read


## GET /accounts/:account_id/positions/:instrument
#### Request
    https://api.oanda.com/v1/accounts/12345/positions/EUR/USD

#### Respond
    {
        "direction" : "short",
        "instrument" : "EUR\/USD",
        "units" : 9,
        "avgPrice" : 1.3093
    }

#### Required scope
read

## DELETE /accounts/:account_id/positions/:instrument

#### Request
    https://api.oanda.com/v1/accounts/1234/position/EUR/USD

#### Respond
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

