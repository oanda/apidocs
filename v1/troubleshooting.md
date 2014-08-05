---
title: Troubleshooting &amp; Errors
---

# Troubleshooting &amp; Errors

* TOC
{:toc}

--------------------------

## Service Outages

Occasionally the OANDA API might be down or unresponsive (most notably during our maintenance period). The [OANDA API Status Page](http://api-status.oanda.com) is a service hosted at an independent location that will provide up-to-the-minute information about the status of the various platforms. There is even a publicly available API to programatically check the status of the OANDA API with documentation available [here](http://api-status.oanda.com/documentation).

The simplest way to check the current status of all of the services would be to look for the `current_event` information for each service in the `GET services` call outlined below.

	curl http://api-status.oanda.com/api/v1/services

---------------------------

## POST and PATCH Requests

One common problem with POST or PATCH requests is that the input parameters get included in the query string.  For POSTs and PATCHs, input parameters should be included in the body of the request.

Below are examples of how to properly format a POST request and also a common mistake made with POST requests.

#### Correct

	curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=market" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### Incorrect

	curl -X POST "http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&units=2&side=sell&type=market"

If an incorrect request is submitted the following error codes will be returned to the user.

#### POST

	"code" : 65,
	"message" : "Input parameters should be provided in the body of this request",

#### PATCH

	"code" : 42,
	"message" : "Received request with unsupported Content-Type: ''",

---------------------------

## Errors  

####Overview

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

    HTTP/1.1 400 Bad Request

~~~json
{
  "code" : [OANDA error code, may or may not be the same as the HTTP status code],
  "message"   : [a description of the error which occurred, intended for developers],
  "moreInfo"  : [(OPTIONAL)a link to a web page describing the error and possible causes and solutions]
}
~~~

####List of errors

|errorCode|HTTP Status Code|HTTP Status Message|message|Detailed description|
|---|---|---|---|---|
|1|400|Bad Request|Invalid or malformed argument: [arg]|The argument specified is not properly formatted or is an unaccepted value|
|2|400|Bad Request|Missing required argument||
|3|401|Unauthorized|This request requires authorization|Ensure that an access token is supplied with the request.  Please see the [authentication](/docs/v1/auth/#overview) section for more details.|
|4|401|Unauthorized|The access token provided does not allow this request to be made|Ensure that the access token supplied with the request is correct and is valid.  Please see the [authentication](/docs/v1/auth/#overview) section for more details|
|5|500|Internal Server Error|An internal server error occurred, our engineers have been notified||
|6|503|Service Unavailable|Service Unavailable||
|7|405|Method Not Allowed|Method not allowed|The HTTP method specified is not valid for the requested endpoint|
|9|400|Bad Request|Currency provided is invalid|The list of valid instruments for your account is identified by the [/docs/v1/instruments](/docs/v1/rates/#get-an-instrument-list) request|
|11|404|Not Found|Order not found|The order id specified is not valid.<br><br>It is possible that the order being referenced has been filled and has been converted to a trade.<br><br>Please see [here](/docs/v1/orders/) for more details|
|12|404|Not Found|Trade not found|The trade id specified is not valid.<br><br>It is possible that the trade being referenced has been closed due to a stopLoss or takeProfit.<br><br>Please see [here](/docs/v1/trades/) for more details|
|13|404|Not Found|Transaction not found|The transaction id specified is not valid.<br><br>The /v1/transactions returns recent transactions and it is possible that the transaction being referenced is no longer in the recent transactions list.  Please see the [best practices](/docs/v1/best-practices/#transactions) section for the optimal use of the transactions endpoints|
|14|404|Not Found|Position not found|There is no open position for the instrument and account specified|
|15|403|Forbidden|Max Open Trades Reached|There is a limit of 1000 open trades for each sub-account|
|16|403|Forbidden|Max Open Orders Reached|There is a limit of 1000 open orders for each sub-account|
|17|403|Forbidden|Insufficient Liquidity|The account specified is currently in a margin closeout pending state.  Please see [here](http://fxtrade.oanda.com/help/policies/margin-rules) for more details|
|18|400|Bad Request|Invalid Precision|The precision specified in the request is not valid for the instrument.  Please use the correct precision as identified by the [/v1/instruments](/docs/v1/rates/#get-an-instrument-list) request|
|19|403|Forbidden|Insufficient Liquidity|The account specified does not have enough funds to complete the order|
|20|403|Forbidden|Account in margin call|The account specified is currently in a margin closeout pending state.  Please see [here](http://fxtrade.oanda.com/help/policies/margin-rules) for more details|
|21|403|Forbidden|Exceeds maximum position close||
|22|400|Bad Request|Upper bound exceeded|Current price is above the upperBound.  Please see [here](http://fxtrade.oanda.ca/help/advanced-topics/place-market-order) for more details|
|23|400|Bad Request|Lower bound exceeded|Current price is below the lowerBound.  Please see [here](http://fxtrade.oanda.ca/help/advanced-topics/place-market-order) for more details|
|24|403|Forbidden|Instrument trading halted|Currency trading are regularly halted during week-end time. CFDs are halted at their scheduled times.  Please see [this link](http://fxtrade.oanda.ca/help/policies/weekend-exposure-limits) for hours of operation|
|25|403|Forbidden|Account Not Tradable|The account specified can not be used for trading.  Please contact [OANDA support](http://fxtrade.oanda.ca/help/customer-service/) for more details|
|26|403|Forbidden|Account Locked|The account specified is in a locked state.  Please contact [OANDA support](http://fxtrade.oanda.ca/help/customer-service/) for more details|
|27|403|Forbidden|Insufficient Funds||
|28|403|Forbidden|Trades in the same instrument must be closed in FIFO order|Please see [here](http://fxtrade.oanda.com/help/close-trades-fifo) for more details|
|29|400|Bad Request|No new position is allowed||
|30|404|Not Found|Order not found|The order id was not found in the account provided|
|32|404|Not Found|The account ID provided is invalid||
|33|400|Bad Request|Invalid stopLoss error: requested stopLoss [stopLoss] is [above/below] price [price]|If the user is buying, the stopLoss must be below the bid price. If the user is selling, the stopLoss must be above the bid price. The stopLoss cannot be equal to the bid price.  Please see [here](http://fxtrade.oanda.ca/learn/intro-to-currency-trading/first-trade/orders) for more details|
|34|400|Bad Request|Invalid takeProfit error: requested takeProfit [takeProfit] is [above/below] price [price]|If the user is buying, the takeProfit must be above the bid price. If the user is selling, the takeProfit must be below the bid price. The takeProfit cannot be equal to the bid price.  Please see [here](http://fxtrade.oanda.ca/learn/intro-to-currency-trading/first-trade/orders) for more details|
|37|400|Bad Request|Invalid Instrument: This instrument is not tradable with the specified account| The list of valid instruments for your account is identified by the [/v1/instruments](/docs/v1/rates/#get-an-instrument-list) request|
|42|415|Unsupported Media Type|Received request with unsupported Content-Type: [content type]|Please see [here](/docs/v1/guide/#request-and-response-details) for supported content-types|
|53|429|Rate Limit|Rate limit violation. Allowed rate:|Rate limits are applied to various services offered:<br>[REST API](/docs/v1/guide/#rate-limiting)<br>[STREAM API](/docs/v1/stream/#streaming)|
|54|502|Bad Gateway|Bad Gateway|
|55|400|Bad Request|Invalid Username|The username specified in the request is invalid|
|56|400|Bad Request|Exceeded maximum number of instrument subscription|The number of instruments specified in the rate stream request exceeds the maximum allowable subscription limit granted to the specified access token|
|57|401|Unauthorized|You are not authorized to subscribe to instrument|The access token provided does not have access to subscribe to the rate stream of the specified instrument|
|58|403|Forbidden|Application Disabled|The third party application making the request has been disabled by OANDA|
|59|403|Forbidden|The access is forbidden|
|60|-|-|Access Token connection limit exceeded|The number of streaming connections permitted by the specified access token has been exceeded. Connection that receives this message will be disconnected by the server.  Please see the [streaming section](/docs/v1/stream/#streaming) for more details|
|61|411|Length Required|Data is required|PATCH or POST request without any payload set.  Please see [here](/docs/v1/troubleshooting/#post-and-patch-requests) for details|
|62|400|Bad Request|Invalid DateTime Format: choose rfc3339 or unix|Please see the [DateTime Format](/docs/v1/guide/#datetime-format) section for more details|
|63|504|Gateway Timeout|Gateway Timeout||
|64|-|-|Session has been disconnected by a new connection|A new streaming request has been made with the same access token and sessionId. Connections that receive this message will be disconnected by the server.  Please see the [streaming section](/docs/v1/stream/#streaming) for more details|
|65|400|Bad Request|Input parameters should be provided in the body of this request|Will be returned if POST request is sent with parameters in query string instead of the request body.  Please see [here](/docs/v1/troubleshooting/#post-and-patch-requests) for details|
