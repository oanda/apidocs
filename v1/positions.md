---
title: Positions | OANDA API
---

# Position Endpoints

* TOC
{:toc}


## Get a list of all open positions
GET /v1/accounts/:account_id/positions 

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions"

#### Response
    {
      "positions" : [
        { "side" : "buy", "instrument": "EUR_USD", "units": 1000, "avgPrice": 25.23 },
        { "side" : "sell", "instrument": "USD_CAD", "units": 10000, "avgPrice": 325.56 }
     ]
    }



## Get the position for an instrument
GET /v1/accounts/:account_id/positions/:instrument

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR_USD"

#### Response
    {
        "side" : "sell",
        "instrument" : "EUR_USD",
        "units" : 9,
        "avgPrice" : 1.3093
    }


## Close an existing position 
DELETE /v1/accounts/:account_id/positions/:instrument

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


