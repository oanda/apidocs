---
title: Reference | OANDA API
---

# API Reference
<!--
Streaming API Overview
---
[Streaming rates](https://github.com/oanda/apidocs/blob/master/sections/streaming.md)
-->

<!--
Price API Overview
---

| Resource | URI | Methods | Description |
| -------- | -------- | ------ | ---------- |
| [instruments][rates] | /v1/instruments | GET | Retrieve a list of available currency pairs. |
| [quote][rates] | /v1/quote | GET | Retrieve live prices for specified instrument(s). |
| [history][rates] | /v1/history | GET | Retrieve historical rates for the instrument pair. |
-->


<!--
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


    HTTP/1.1 400 Bad Request

~~~json
{
  "code" : [OANDA error code, may or may not be the same as the HTTP status code],
  "message"   : [a description of the error which occurred, intended for developers],
  "moreInfo"  : [(OPTIONAL) a link to a web page describing the error and possible causes and solutions]
}
~~~

[More on error codes](/docs/v1/troubleshooting)


[accounts]: /docs/v1/accounts
[trades]: /docs/v1/trades
[orders]: /docs/v1/orders
[positions]: /docs/v1/positions
[transactions]: /docs/v1/transactions
[alerts]: /docs/v1/alerts
[news]: /docs/v1/news
[rates]: /docs/v1/rates
[notifications]: /docs/v1/notifications
[quick_start]: /docs/v1/getting_started

##Rate Limiting

Client is allowed to have no more than 4 requests per second on average, with bursts of no more than 5 requests. Excess requests will be delayed on our server.

