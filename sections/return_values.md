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
|35|404|Not Found|not found||
|36|400|Bad Request|not logged in||
|37|400|Bad Request|profile error|For user/register, one or more of the given fields were not valid|
|38|400|Bad Request|quotes expired||
|39|403|Forbidden|rate limit exceeded||
|40|503|Service Unavailable|service unavailable||
|41|400|Bad Request|symbol halted||
|42|400|Bad Request|too many markups||
|43|400|Bad Request|Upper bound exceeded|The trade was not completed because the price had moved above the specified upper bound|
|44|400|Bad Request|Lower bound exceeded|The trade was not completed because the price had moved below the specified lower bound|
|45|403|Maintenance Mode|system in maintenance||
|46|401|Unauthorized|need otp <session_token>|login partially succeeded. the client should re-send the login request with the returned session token and the users otp|
|47|401|Unauthorized|need password <session_token>|login partially succeeded. the client should re-send the login request with the returned session token and the users password|
|48|401|Unauthorized|mfa not configured|The client needs to configure mfa|
|49|401|Unauthorized|mfa not verified|The client needs to verify mfa|
|50|503|Service Unavailable|trade undo temporarily disabled||
|51|403|Forbidden|trade undo limit reached||
|52|403|Forbidden|trade undo time limit exceeded||
|53|400|Bad Request|fifo validation failed. first close ticket <ticket#>||
|54|403|Forbidden|upgrade required. minimum version: XXX|the client version is too old - the client must update to at least the version specified|
|55|400|Bad Request|exposure exceeded|the account has reached the exposure limit (this will typically be enforced for weekend trading)|
|56|403|Forbidden|read access denied|The client does not have a "read" access as a result of an incomplete registration, account being locked or email not validated|
|57|403|Forbidden|create access denied|The client does not have a "create" access as a result of an incomplete registration, account being locked or email not validated|
|58|422|Unprocessable Entity|exceeded maximum position close||
|59|400|Bad Request|account in margin call|The request has been denied because the account is in margin call state. This is currently MyGaika specific|
|60|409|Conflict|transaction reference already exists|The requests contains a reference id that already exists, meaning a previous request has completed successfully|
|61|400|Bad Request|insufficient liquidity|Cannot complete the request due to insufficient liquidity|
|62|403|Forbidden|email not verified||
|63|403|Forbidden|registration not completed||
|64|400|Bad Request|invalid price alert type|The price_type field must be "BID", "ASK", or "MID"|
|65|400|Bad Request|invalid precision|The price specified had more precision than the instrument allows. This could be any of the price fields including stop loss and take profit.|
|66|422|Unprocessable Entity|trade is flagged for margin closeout||
|67|403|Forbidden|max number of price alerts reached||
|68|403|Forbidden|max open trades reached||
|69|403|Forbidden|max open orders reached||
|104|401|Unauthorized|need consent|OANDA Japan requires this error be returned upon update of their terms and conditions|
|104|401|Unauthorized|need consent|OANDA Japan requires this error be returned upon update of their terms and conditions|
|406|406|Invalid "visibility" parameter|The visibility="VISIBILITY" parameter is invalid. Choose one of "tradeable" or "all".||
|406|406|Missing the "instruments" parameter|Missing the "instruments" parameter||
|404|404| Granularity Not Found | "The granularity specified for INSTRUMENT ()GRANULARITY) is not recognized. Please select one of the following granularities: S5, S10, S15, S30, M1, M2, M3, M4, M5, M10, M15, M30, H1, H2, H3, H4, H6, H8, H1, D, W, M" | |
|406|406| Invalid candle representation | The candle representation specified by candleRepr=CANDLE_FORMAT is not recognized. Please select one of the following candle representations: M, MV, BA, BAV | |
|406|406| Could not retrieve ticks | Unable to retrieve ticks for "EURUSD": Instrument not found. | |

