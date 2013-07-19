# Account Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accounts) | Get all accounts associated with a username. |
| [GET /v1/accounts/:account_id](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accountsaccount_id) | Get detailed balance and holding information for :account_id |

## GET /v1/accounts

#### Request
    GET /v1/accounts
    Authorization: Bearer <access_token>
    User-Agent: curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3
    Host: api-sandbox.oanda.com
    Accept: */*

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
    GET /v1/accounts/8954947 HTTP/1.1
    User-Agent: curl/7.22.0 (x86_64-pc-linux-gnu) libcurl/7.22.0 OpenSSL/1.0.1 zlib/1.2.3.4 libidn/1.23 librtmp/2.3
    Host: api-sandbox.oanda.com
    Accept: */*

#### Response
    HTTP/1.1 200 OK
    Server: nginx/1.2.0
    Date: Wed, 03 Apr 2013 15:52:09 GMT
    Content-Type: application/json
    Content-Length: 240
    Connection: keep-alive
    Access-Control-Allow-Headers: Origin, Content-Type, Accept, Authorization
    Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, DELETE
    Access-Control-Allow-Origin: *

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
