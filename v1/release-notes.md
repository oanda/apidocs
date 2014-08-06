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

## Version <TODO>
- Released to Sandbox <TODO>
- Released to fxTrade Practice on <TODO>
- Release to fxTrade on pending.

##### New Features:

- Introduced new endpoint [/labs/v1/calendar/](/docs/v1/forex-labs/#calendar) providing API access to our [FxLabs](http://fxtrade.oanda.ca/analysis/labs/) data.

-------------------------------------

## Version 1.3.3
- Released to Sandbox on July 25, 2014
- Released to fxTrade Practice on July 25, 2014
- Release to fxTrade on pending.

##### New Features:

- Introduced [halted](/docs/v1/rates/#get-an-instrument-list) response field parameter to allow users to identify halted instruments in the /v1/instruments request.

-------------------------------------

## Version 1.3.2
- Released to Sandbox on July 11, 2014
- Released to fxTrade Practice on July 11, 2014
- Release to fxTrade on July 25, 2014.

##### Bug Fixes:

- Added units field into ORDER_FILLED, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED and TRAILING_STOP_FILLED records of transactions and stream events response
- Added tradeId field into STOP_LOSS_FILLED, TAKE_PROFIT_FILLED and TRAILING_STOP_FILLED records of /alltransactions response 

-------------------------------------

## Version 1.3.1
- Released to Sandbox on June 30, 2014
- New features released to fxTrade Practice on July 25, 2014.  Compatibility change released to fxTrade Practice on July 23, 2014. 
- New features released to fxTrade pending.  Compatibility change is scheduled to be released to fxTrade on August 8, 2014.

##### Compatibility Changes:

- Wrapped streaming rates inside a "tick" object as mentioned [here](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445).

##### New Features:

- Introduced [daily](/docs/v1/rates/#retrieve-instrument-history) and [weekly](/docs/v1/rates/#retrieve-instrument-history) alignment parameter to allow users to specify which hour and day of the week to use when delimiting daily or weekly candle requests.


