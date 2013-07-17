![](https://raw.github.com/oanda/apidocs/master/images/oanda_header.png)
=========

[Home](https://github.com/oanda/apidocs) |
[Getting Started Guide](https://github.com/oanda/apidocs/blob/master/sections/getting_started.md) | 
[Sample Code](https://github.com/oanda/apidocs/blob/master/sections/code_samples.md)



# API Reference
<!--
Streaming API Overview
---
[Streaming rates](https://github.com/oanda/apidocs/blob/master/sections/streaming.md)
-->

Price API Overview
---
| Resource | URI | Methods | Description |
| -------- | -------- | ------- | ----------- |
| [instruments][rates] | /v1/instruments | GET | Retrieve a list of available currency pairs. |
| [quote][rates] | /v1/quote | GET | Retrieve live prices for specified instrument(s). |
| [history][rates] | /v1/history | GET | Retrieve historical rates for the instrument pair. |

Trading API Overview
---

| Resource | URI | Methods | Description |
| -------- | -------- | ------- | ----------- |
| [account][accounts]| /v1/accounts/:account_id  | [GET](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accountsaccount_id)    | Contains account information for a specific account. |
| [account collection][accounts] | /v1/accounts | [POST](https://github.com/oanda/apidocs/blob/master/sections/accounts.md#get-v1accounts) | Contains list of accounts for a specific user. |
| [trade][trades] | /v1/accounts/:account_id/trades/:trade_id | [GET](https://github.com/oanda/apidocs/blob/master/sections/trades.md#get-v1accountsaccount_idtradestrade_id), [PUT](https://github.com/oanda/apidocs/blob/master/sections/trades.md#put-v1accountsaccount_idtradestrade_id), [DELETE](https://github.com/oanda/apidocs/blob/master/sections/trades.md#delete-v1accountsaccount_idtradestrade_id) | Contains info of a specific trade. |
| [trade collection][trades] | /v1/accounts/:id/trades | [GET](https://github.com/oanda/apidocs/blob/master/sections/trades.md#get-v1accountsaccount_idtrades), [POST](https://github.com/oanda/apidocs/blob/master/sections/trades.md#post-v1accountsaccount_idtrades) | Contains a list of trades for a specific account. Use POST to create new trades. |
| [order][orders] | /v1/accounts/:account_id/orders/:order_id | [GET](https://github.com/oanda/apidocs/blob/master/sections/orders.md#get-v1accountsaccount_idorderorder_id), [PUT](https://github.com/oanda/apidocs/blob/master/sections/orders.md#put-v1accountsaccount_idordersorder_id), [DELETE](https://github.com/oanda/apidocs/blob/master/sections/orders.md#delete-v1accountsaccount_idordersorder_id) | Contains info of a specific order. GET to retrieve info. PUT to change, DELETE to delete.|
| [order collection][orders] | /v1/accounts/:account_id/orders | [GET](https://github.com/oanda/apidocs/blob/master/sections/orders.md#get-v1accountsaccount_idorders), [POST](https://github.com/oanda/apidocs/blob/master/sections/orders.md#post-v1accountsaccount_idorders) | Contains a list of orders for a specific account. Use POST to create new orders |
| [position collection][positions] | /v1/accounts/:account_id/positions | [GET](https://github.com/oanda/apidocs/blob/master/sections/positions.md#get-v1accountsaccount_idpositions), [DELETE](https://github.com/oanda/apidocs/blob/master/sections/positions.md#delete-v1accountsaccount_idpositionsinstrument) | Contains a list of positions for a specific account. Use GET to retrieve. DELETE to delete existing position. |
| [transaction][transactions] | /v1/accounts/:account_id/transactions/:trans_id | [GET](https://github.com/oanda/apidocs/blob/master/sections/transactions.md#get-v1accountsaccount_idtransactionstrans_id) | Contains info of a specific transaction. |
| [transaction collection][transactions] | /v1/accounts/:account_id/transactions | [GET](https://github.com/oanda/apidocs/blob/master/sections/transactions.md#get-v1accountsaccount_idtransactions) | Contains info of a list transactions. |


<!--
| [price alert][alerts] | /accounts/:account_id/alerts/:alert_id | GET, DELETE | Contains info of a specific transaction. |
| [price alert collection][alerts] | /accounts/:account_id/alerts | GET | Contains info of a list transactions. |
| [news][news] | /news/:article_id | GET | Retrieves the body of a news item. |
| [news collection][news] | /news | GET | Contains a list of news items. |
| [notification collection][notifications] | /users/:username/notifications | POST, DELETE | Contains a list of devices registered for notification for :username's accounts |
-->


Request and Response
------------------
<!--
OAuth token to be part of the HTTP header in all requests

    GET /accounts/1/trades HTTP/1.1
    Accept: */*
    Connection: close
    User-Agent: OAuth gem v0.4.4
    Content-Type: application/x-www-form-urlencoded
    Host: api.oanda.com
-->
All requests require <code>Content-Type: application/x-www-form-urlencoded</code> unless specified otherwise.

All responses will be in JSON format.


Handling Errors
----------------

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

```shell
HTTP/1.1 400 Bad Request

{
    "code" : [OANDA error code, may or may not be the same as the HTTP status code],
    "message"   : [a description of the error which occurred, intended for developers],
    "moreInfo"  : [(OPTIONAL) a link to a web page describing the error and possible causes and solutions]
}
```

[More on error codes](https://github.com/oanda/apidocs/blob/master/sections/return_values.md#errors)




[accounts]: https://github.com/oanda/apidocs/blob/master/sections/accounts.md
[trades]: https://github.com/oanda/apidocs/blob/master/sections/trades.md
[orders]: https://github.com/oanda/apidocs/blob/master/sections/orders.md
[positions]: https://github.com/oanda/apidocs/blob/master/sections/positions.md
[transactions]: https://github.com/oanda/apidocs/blob/master/sections/transactions.md
[alerts]: https://github.com/oanda/apidocs/blob/master/sections/alerts.md
[news]: https://github.com/oanda/apidocs/blob/master/sections/news.md
[rates]: https://github.com/oanda/apidocs/blob/master/sections/rates.md
[notifications]: https://github.com/oanda/apidocs/blob/master/sections/notifications.md
[quick_start]: https://github.com/oanda/apidocs/blob/master/sections/getting_started.md
[examples]: https://github.com/oanda/apidocs/blob/master/sections/getting_started.md#examples


##Rate Limiting

Client is allowed to have no more than 4 requests per second on average, with bursts of no more than 5 requests. Excess requests will be delayed on our server.

