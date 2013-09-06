# Account Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accounts) | Get all accounts associated with a username. |
| [GET /v1/accounts/:account_id](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accountsaccount_id) | Get detailed balance and holding information for :account_id |

## GET /v1/accounts

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts?username=fxtrader"

#### Response
    {
        "accounts" : [
            {
                "id" : 2100493,
                "name" : "Primary",
                "accountCurrency" : "USD",
                "marginRate" : 0.05
            }
        ]
    }


## GET /v1/accounts/:account_id

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/8954947"

#### Response
    {
        "accountId" : 8954947,
        "accountName" : "Primary",
        "balance" : 100000,
        "unrealizedPl" : 0,
        "realizedPl" : 0,
        "marginUsed" : 0,
        "marginAvail" : 100000,
        "openTrades" : 0,
        "openOrders" : 0,
        "marginRate" : 0.05,
        "accountCurrency" : "USD"
    }
