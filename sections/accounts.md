# Account Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /accounts | Get information about an account |
| GET /accounts/:account_id | Get detailed balance and holding information for :account_id |

## GET /accounts

#### Request
    http://api.oanda.com/v1/accounts

#### Respond
    [
        {"id":1, name:"Primary", homecurr:"USD", marginRate:0.20, "accountPropertyName":[]},
        {"id":2, name:"Subaccount 1", homecurr:"CAD", marginRate:0.30, "accountPropertyName":[]},
        {"id":3, name:"Subaccount 2", homecurr:"EUR", marginRate:0.50, "accountPropertyName":[]},
        {"id":4, name:"MT4 Account", homecurr:"EUR", marginRate:0.50, "accountPropertyName":["MT4"]},
        {"id":4, name:"Beginner Account", homecurr:"EUR", marginRate:0.50, "accountPropertyName":["BEGINNER"]}
    ]

#### Parameters
none

#### Required scope
read

## GET /accounts/:account_id
#### Request
    http://api.oanda.com/v1/accounts/:account_id

#### Respond
    {
        "accountId":1, name:"Primary", "balance":99967.16, "unrealizedPl":1629.21, "nav":101660.07, "realizedPl":-36830.09, "marginUsed":3160.62, "marginAvail":98499.45, "openTrades": 10, "openOrders": 7, "marginRate": 0.05, "homecurr": "USD" 
    }

#### Required scope
read



## GET template
#### Request
#### Respond
#### Parameters
#### Required scope
