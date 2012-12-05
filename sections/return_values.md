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
|1|403|Unauthorized|access denied||
|2|400|Bad Request|account busy||
|3|403|Forbidden|account locked||
|4|400|Bad Request|account not tradable||
|5|400|Bad Request|bad request|One or more parameters were missing or invalid|
|6|400|Bad Request|data already exists|For user/register, the server was unable to generate an account id. Also returned if user/regiser_zero cannot generate a unique username|
|7|400|Bad Request|insufficient funds||
|8|400|Bad Request|insufficient margin||
|9|500|Internal Server Error|internal server error||
|10|400|Bad Request|invalid account|Account id specified does not exist or is not owned by the user. For user/register, the given username is taken|
|11|403|Forbidden|invalid api key||
|12|400|Bad Request|invalid close arg||
|13|400|Bad Request|invalid currency||
|14|400|Bad Request|invalid duration||
|15|400|Bad Request|invalid id||
|16|400|Bad Request|invalid order id||
|17|400|Bad Request|invalid position number||
|18|400|Bad Request|invalid price||
|19|400|Bad Request|invalid stop loss||
|20|400|Bad Request|invalid symbol||
|21|400|Bad Request|invalid take profit||
|22|400|Bad Request|invalid ticket number||
|23|400|Bad Request|invalid trade id||
|24|400|Bad Request|invalid trailing stop||
|25|400|Bad Request|invalid transaction id||
|26|400|Bad Request|invalid units||
|27|400|Bad Request|invalid user||
|28|400|Bad Request|invalid user id||
|29|400|Bad Request|markup error||
|30|400|Bad Request|max accounts||
|31|400|Bad Request|max trades||
|32|400|Bad Request|no new position||
|33|400|Bad Request|no quotes avaliable||
|34|400|Bad Request|no rate||
|35|404|Not Found|not found||
|36|400|Bad Request|not logged in||
|37|400|Bad Request|profile error|For user/register, one or more of the given fields were not valid|
|38|400|Bad Request|quotes expired||
|39|403|Forbidden|rate limit exceeded||
|40|503|Service Unavailable|service unavailable||
|41|400|Bad Request|symbol halted||
|42|400|Bad Request|too many markups||
|43|400|Bad Request|trade above limit||
|44|400|Bad Request|trade below limit||
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

