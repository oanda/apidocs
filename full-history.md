## Version 1.1.0
- Released to Sandbox on Apr 21, 2014
- Released to fxTrade Practice on Apr 25, 2014
- Release to fxTrade pending  

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
