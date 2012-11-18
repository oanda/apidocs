OANDA Open API
==============

**Disclaimer**: API is currently in draft and is not open to public access.

API Request Endpoint
--------------------

Making a request
----------------

```shell
$curl -X POST \
    --data-urlencode 'instrument=EUR/USD' \
    --data-urlencode 'uints=1' \
    --data-urlencode 'direction=long' \
    http://api.oanda.com/accounts/6531071/trades

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

##Scope

| scope | description |
| ----- | ----------- |
| read  | Allows access to rates and account information |
| trade | Allows access to open and close trades |

Request and Respond
------------------
OAuth token to be part of the HTTP header in all requests

    GET /accounts/1/trades HTTP/1.1
    Accept: */*
    Connection: close
    User-Agent: OAuth gem v0.4.4
    Content-Type: application/x-www-form-urlencoded
    Authorization: Bearer mF_9.B5f-4.1JqM
    Host: api.oanda.com

All requests require <code>Content-Type: application/x-www-form-urlencoded</code> unless specified otherwise.

All response will be in JSON format.

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

| Resource | URI | Methods | Description |
| -------- | -------- | ------- | ----------- |
| [user][users]| /users/:username  | [POST](https://github.com/oanda/apidocs/blob/master/sections/users.md)    | User registration, user profile |
| [account][accounts]| /accounts/:account_id  | [GET](https://github.com/oanda/apidocs/blob/master/sections/accounts.md)    | Contains account information for a specific account |
| [account collection][accounts] | /accounts | [GET](apidocs/blob/master/sections/accounts.md) | Contains list of accounts for a specific user |
| [trade][trades] | /accounts/:account_id/trades/:trade_id | GET, PUT, DELETE | Contains info of a specific trade. |
| [trade collection][trades] | /accounts/:id/trades | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| [order][orders] | /accounts/:account_id/orders/:order_id | GET, PUT, DELETE | Contains info of a specific order. GET to retrieve info. PUT to change, DELETE to delete.|
| [order collection][orders] | /accounts/:account_id/orders | GET, POST | Contain a list of trade for a specific account. Use POST to create new trades |
| [position collection][positions] | /accounts/:account_id/position | GET, DELETE | Contain a list of positions for a specific account. Use GET to retrieve. DELTE to delete existing position. |
| [transaction][transactions] | /accounts/:account_id/transactions/:trans_id | GET | Contains info of a specific transaction. |
| [transaction collection][transactions] | /accounts/:account_id/transaction | GET | Contains info of a list transactions. |
| [price alert][alerts] | /accounts/:account_id/alerts/:alert_id | GET, DELETE | Contains info of a specific transaction. |
| [price alert collection][alerts] | /accounts/:account_id/alerts | GET | Contains info of a list transactions. |
| [rates][rates] | | | Market rates data. |
| [news][news] | /news/:article_id | GET | Retrieves the body of a news item. |
| [news collection][news] | /news | GET | Contains a list of news items. |
| [notification collection][notifications] | /users/:username/notifications | POST, DELETE | Contains a list of devices registered for notification for :username's accounts |


[users]:(https://github.com/oanda/apidocs/blob/master/sections/users.md)
[accounts]:(https://github.com/oanda/apidocs/blob/master/sections/accounts.md)
[trades]:(https://github.com/oanda/apidocs/blob/master/sections/trades.md)
[orders]:(https://github.com/oanda/apidocs/blob/master/sections/orders.md)
[positions]:(https://github.com/oanda/apidocs/blob/master/sections/positions.md)
[transactions]:(https://github.com/oanda/apidocs/blob/master/sections/transactions.md)
[alerts]:(https://github.com/oanda/apidocs/blob/master/sections/alerts.md)
[news]:(https://github.com/oanda/apidocs/blob/master/sections/news.md)
[rates]:(https://github.com/oanda/apidocs/blob/master/sections/rates.md)
[notifications]:(https://github.com/oanda/apidocs/blob/master/sections/notifications.md)


