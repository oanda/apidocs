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
    "errorCode" : [OANDA error code, may or may not be the same as the HTTP status code],
    "message"   : [a description of the error which occurred, intended for developers],
    "moreInfo"  : [a link to a web page describing the error and possible causes and solutions]
}
```

Rate limiting
-------------

API
---
* [User](https://github.com/oanda/openapi/blob/master/sections/users.md)
* [Account](https://github.com/oanda/openapi/blob/master/sections/Accounts.md)
* [Trade](https://github.com/oanda/openapi/blob/master/sections/Trade.md)
* [Order](https://github.com/oanda/openapi/blob/master/sections/Order.md)

