---
title: Latest Changes | OANDA API
---

# Release Notes

* TOC
{:toc}

-------

## Full History

To see the full change history, [visit here](/docs/full-history.md).

------------------------


<!-- Template for adding new notes

## Version 1.1.0
- Released to Sandbox on Feb 21, 2014
- Released to fxTrade Practice on Feb 26, 2014
- Release to fxTrade pending  
<br/>

##### Compatibility Changes:

- None because we don't mess with that much

##### New Features:

- Modified the thing to do the stuff
- More modifications to the thing

##### Bug Fixes:

- Stopped the other thing from breaking on sundays

-------------------------------------


Template ends -->


## Version 1.1.0
- Released to Sandbox on Apr 21, 2014
- Released to fxTrade Practice on Apr 25,2014
- Release to fxTrade pending  
<br/>

##### Compatibility Changes:

- Renamed v1/quote REST endpoint to v1/prices to be consistent with the response
- Renamed v1/history REST endpoint to v1/candles to be more clear what the history refers to
- Renamed v1/quote Streaming endpoint to v1/prices to be consistent with the REST API polling endpoint
- Stream prices for tradeable instruments only to enable more tradeable instruments to traders for streaming (as a preparation for public launch)

##### New Features:

- Added support for If-None-Match/ETag headers to enable saving bandwidth by the client
- Added optional `since` parameter to v1/prices endpoint to enable users to see if there has been a new tick since the time specified
- Increased the number of instruments that can be streamed to 130 tradeable instruments for fxTrade Practice (fxTrade will follow for public launch)

-------------------------------------


## Version 1.0.0
- Released to fxTrade Practice on Mar 11, 2014
- Release to fxTrade pending  
<br/>

These are the initial release notes for the Open API!  
As we add new features and fix issues we'll post new updates here.

##### New Features:

- Added Release Notes and Streaming to documentation
- Added HTTP Streaming Rate support

-------------------------------------





