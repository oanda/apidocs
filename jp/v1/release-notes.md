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

## Version 1.2.0
- Released to Sandbox on May 9, 2014
- Released to fxTrade Practice on May 9, 2014
- Release to fxTrade pending
<br/>

##### Compatibility Changes:

- v1/quote REST requests will no longer be automatically rerouted to v1/prices
- v1/history REST requests will no longer be automatically rerouted to v1/candles
- v1/quote Streaming requests will no longer be automatically rerouted to v1/prices (Streaming)

##### New Features:

- v1/transactions now supports the retrieval of up to a maximum of 500 transactions

##### Bug Fixes:

- For v1/candles, gaps within candle history is now considered when determining if the candles resultset exceeds the 5000 candles limit





