OANDA Open API
==============

API Request Endpoint
--------------------

Making a request
----------------

Create a new user

```shell
$curl -X POST -d "currency=USD" "http://oanda-cs-dev:1341/users"

{
    "username" : "willymoth",
    "password" : "balvEdayg"
}
```

Get account belonging to the user

```shell
$ curl "http://oanda-cs-dev:1341/users/willymoth/accounts"

[
    {
        "id" : 6531071,
        "name" : "Primary",
        "homecurr" : "USD",
        "marginRate" : 0.05,
        "accountPropertyName" : []
    }
]
```

Start trading

```shell
$curl -X POST \
    --data-urlencode 'instrument=EUR/USD' \
    --data-urlencode 'uints=1' \
    --data-urlencode 'direction=long' \
    http://oanda-cs-dev:1341/accounts/6531071/trades

{
    "ids" : [177715575],
    "instrument" : "EUR\/USD",
    "units" : 2,
    "price" : 1.30582,
    "marginUsed" : 0.1306,
    "direction" : "short"
}
```


Authentication
--------------

No XML, just JSON
----------------

Handling errors
----------------

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

```shell
{
    "errorCode" : [OANDA error code, may or may kot be the same as the HTTP status code],
    "message"   : [a description of the error which occurred, intended for developers],
    "moreInfo"  : [a link to a web page describing the error and possible causes and solutions]
}
```

Rate limiting
-------------

API
---

| Resource | Methods | Description |
| -------- | ------- | ----------- |
| /accounts/:id  | [GET](https://github.com/oanda/apidocs/blob/master/sections/accounts.md)    | Contains account information for a specific account |
| /accounts | [GET](apidocs/blob/sections/accounts.md) | Contains list of accounts for a specific user |
| /accounts/:id/trades/:id | GET, PUT, DELETE | Contains info of a specific trade. |
| /accounts/:id/trades | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| /order | GET, PUT, DELETE | Contains info of a specific order. GET to retrieve info. PUT to change, DELETE to delete.|
| /order collection | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| /position collection | GET, DELETE | Contain a list of positions for a specific account. Use GET to retrieve. DELTE to delete existing position. |
| /transaction | GET | Contains info of a specific transaction. |
| /transaction collection | GET | Contains info of a list transactions. |


* [User](https://github.com/oanda/openapi/blob/master/sections/users.md)
* [Account](https://github.com/oanda/openapi/blob/master/sections/Accounts.md)
* [Trade](https://github.com/oanda/openapi/blob/master/sections/Trade.md)
* [Order](https://github.com/oanda/openapi/blob/master/sections/Order.md)

