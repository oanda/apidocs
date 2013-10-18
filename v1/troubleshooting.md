---
title: Troubleshooting &amp; Errors
---

# Return Values

## Errors
---------

####Overview

When an error occurs, the applicable HTTP response code is returned as well as an error message in the body in the following format:

	HTTP/1.1 400 Bad Request
	
	{
    	"code" : [OANDA error code, may or may not be the same as the HTTP status code],
    	"message"   : [a description of the error which occurred, intended for developers],
    	"moreInfo"  : [(OPTIONAL)a link to a web page describing the error and possible causes and solutions]
	}
	
####List of errors

|errorCode|HTTP Status Code|HTTP Status Message|message|Detailed description|
|---|---|---|---|---|
|1|400|Bad Request|Invalid or malformed argument: [arg]|The argument specified is not properly formatted or is an unaccepted value|
|2|400|Bad Request|Missing required argument||
|3|401|Unauthorized|This request requires authorization||
|4|401|Unauthorized|The access token provided does not allow this request to be made||
|5|500|Internal Server Error|An internal server error occurred, our engineers have been notified||
|6|503|Service Unavailable|Service Unavailable||
|7|405|Method Not Allowed|Method not allowed||
|8|400|Bad Request|Malformed Authorization header or invalid access token||
|9|400|Bad Request|Currency provided is invalid||
|10|400|Bad Request|Unexpected response from storm||
|11|404|Not Found|Order not found|Invalid order number|
|12|404|Not Found|Trade not found||
|13|404|Not Found|Transaction not found||
|14|404|Not Found|Position not found||
|15|403|Forbidden|Max Open Trades Reached||
|16|403|Forbidden|Max Open Orders Reached||
|17|403|Forbidden|Insufficient Liquidity|Flagged for margin close|
|18|400|Bad Request|Invalid Precision||
|19|403|Forbidden|Insufficient Liquidity||
|20|403|Forbidden|Account in margin call||
|21|403|Forbidden|Exceeds maximum position close||
|22|400|Bad Request|Upper bound exceeded|Current price is above the upperBound|
|23|400|Bad Request|Lower bound exceeded|Current price is below the lowerBound|
|24|403|Forbidden|Instrument trading halted||
|25|403|Forbidden|Account Not Tradable||
|26|403|Forbidden|Account Locked||
|27|403|Forbidden|Insufficient Funds||
|28|403|Forbidden|Trades in the same instrument must be closed in FIFO order||
|29|400|Bad Request|No new position is allowed||
|30|404|Not Found|Order not found|The order id was not found in the account provided|
|31|404|Not Found|Order not found|Invalid order request|
|32|404|Not Found|The account ID provided is invalid||
|33|400|Bad Request|Invalid stopLoss error: requested stopLoss [stopLoss] is [above/below] price [price]|If the user is buying, the stopLoss must be below the bid price. If the user is selling, the stopLoss must be above the bid price. The stopLoss cannot be equal to the bid price|
|34|400|Bad Request|Invalid takeProfit error: requested takeProfit [takeProfit] is [above/below] price [price]|If the user is buying, the takeProfit must be above the bid price. If the user is selling, the takeProfit must be below the bid price. The takeProfit cannot be equal to the bid price|
