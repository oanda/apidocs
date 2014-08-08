## Version 1.3.4
- Released to Sandbox on August 8, 2014
- Released to fxTrade Practice on August 8, 2014
- Release to fxTrade pending

##### New Features:
- Introduced [timezoneAlignment](/docs/v1/rates/#retrieve-instrument-history) parameter to allow users to specify which timezone to use when delimiting daily candle requests.
- Introduced interestRate field to the [/v1/instruments](/docs/v1/rates/#get-an-instrument-list) endpoint.
- Introduced support for the [client side OAuth Flow](/docs/v1/auth/#third-party-applications). 

------------------------------------

## Version 1.3.3
- Released to Sandbox on July 25, 2014
- Released to fxTrade Practice on July 25, 2014
- Release to fxTrade on pending.

##### New Features:

- Introduced [halted](/docs/v1/rates/#get-an-instrument-list) response field parameter to allow users to identify halted instruments in the /v1/instruments request.

-------------------------------------

## Version 1.3.1
- Released to Sandbox on June 30, 2014
- New features released to fxTrade Practice on July 25, 2014.  Compatibility change released to fxTrade Practice on July 23, 2014. 
- New features released to fxTrade pending.  Compatibility change released to fxTrade on August 8, 2014.

##### Compatibility Changes:

- Wrapped streaming rates inside a "tick" object as mentioned [here](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445).

##### New Features:

- Introduced [daily](/docs/v1/rates/#retrieve-instrument-history) and [weekly](/docs/v1/rates/#retrieve-instrument-history) alignment parameter to allow users to specify which hour and day of the week to use when delimiting daily or weekly candle requests.

-------------------------------------


## Version 1.3.0
- Released to Sandbox on June 27, 2014
- Released to fxTrade Practice on June 27, 2014
- Released to fxTrade on July 4, 2014

##### New Features:

- Introduced HTTP events streaming to fxTrade Practice.
- Introduced the X-Accept-Datetime-Format HTTP header to allow users to specify the timestamp of choice.
- Each personal access token is now permitted to have 2 active HTTP streaming connections.
- Introduced the sessionId parameter to allow users to uniquely identify an HTTP streaming connection.
  This is not applicable to the sandbox environment.

-------------------------------------


## Version 1.2.2
- Released to Sandbox on June 12, 2014
- Released to fxTrade Practice on June 13, 2014
- Released to fxTrade on June 13, 2014


##### New Features:

- Previously generated Personal Access Tokens are now automatically useable for any newly created subaccounts

-------------------------------------


## Version 1.2.1
- Released to Sandbox on May 21, 2014
- Released to fxTrade Practice on May 22, 2014
- Release to fxTrade on May 23, 2014

##### New Features:

- The REST API is now open to everyone

##### Bug Fixes:

- Changed RFC3339 timestamp format to always contain micro-seconds, even when it's 0


-------------------------------------


## Version 1.2.0
- Released to Sandbox on May 9, 2014
- Released to fxTrade Practice on May 9, 2014
- Release to fxTrade May 16, 2014 

##### Compatibility Changes:

- v1/quote REST requests will no longer be automatically rerouted to v1/prices
- v1/history REST requests will no longer be automatically rerouted to v1/candles
- v1/quote Streaming requests will no longer be automatically rerouted to v1/prices (Streaming)

##### New Features:

- v1/transactions now supports the retrieval of up to a maximum of 500 transactions

##### Bug Fixes:

- For v1/candles, gaps within candle history is now considered when determining if the candles resultset exceeds the 5000 candles limit


-------------------------------------


## Version 1.1.0
- Released to Sandbox on Apr 21, 2014
- Released to fxTrade Practice on Apr 25, 2014
- Release to fxTrade on May 2, 2014

##### Compatibility Changes:

- Renamed v1/quote REST endpoint to v1/prices to be consistent with the response
- Renamed v1/history REST endpoint to v1/candles to be more clear what the history refers to
- Renamed v1/quote Streaming endpoint to v1/prices to be consistent with the REST API polling endpoint
- Stream prices for tradeable instruments only to enable more tradeable instruments to traders for streaming (as a preparation for public launch)

##### New Features:

- Added support for If-None-Match/ETag headers to enable saving bandwidth by the client
- Added optional `since` parameter to v1/prices endpoint to enable users to see if there has been a new tick since the time specified


-------------------------------------


## Version 1.0.0
- Released to fxTrade Practice on Mar 11, 2014
- Released to fxTrade on Mar 18, 2014

These are the initial release notes for the Open API!
As we add new features and fix issues we'll post new updates here.

##### New Features:

- Added Release Notes and Streaming to documentation
- Added HTTP Streaming Rate support

-------------------------------------
