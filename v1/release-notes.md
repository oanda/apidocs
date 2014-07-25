---

title: Latest Changes | OANDA API
---

# Release Notes

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
:
-------------------------------------


Template ends -->

## Version 1.3.4
- Released to Sandbox on August 8, 2014
- Released to fxTrade Practice on August 8, 2014
- Release to fxTrade pending

##### New Features:
- Introduced [timezoneAlignment](/docs/v1/rates/#retrieve-instrument-history) parameter to allow users to specify which timezone to use when delimiting daily candle requests.
- Introduced interestRate field to the [/v1/instruments](/docs/v1/rates/#get-an-instrument-list) endpoint.
- Introduced support for the (Implicit OAuth Flow)[/docs/v1/auth/]. 
------------------------------------

## Version 1.3.3
- Released to Sandbox on July 25, 2014
- Released to fxTrade Practice on July 25, 2014
- Release to fxTrade on pending.

##### New Features:

- Introduced [halted](/docs/v1/rates/#get-an-instrument-list) response field parameter to allow users to identify halted instruments in the /v1/instruments request.
- Released to Sandbox on __date__
- Release to fxTrade Practice pending
- Release to fxTrade pending

-------------------------------------

## Version 1.3.1
- Released to Sandbox on June 30, 2014
- New features released to fxTrade Practice on July 25, 2014.  Compatibility change released to fxTrade Practice on July 23, 2014. 
- New features released to fxTrade pending.  Compatibility change released to fxTrade on August 8, 2014.

##### Compatibility Changes:

- Wrapped streaming rates inside a "tick" object as mentioned [here](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445).

##### New Features:

- Introduced [daily](/docs/v1/rates/#retrieve-instrument-history) and [weekly](/docs/v1/rates/#retrieve-instrument-history) alignment parameter to allow users to specify which hour and day of the week to use when delimiting daily or weekly candle requests.


