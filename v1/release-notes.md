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

## Version 1.3.2
- Released to Sandbox on July 11, 2014
- Released to fxTrade Practice July 11, 2014
- Release to fxTrade pending  

##### Bug Fixes:

- Added units field into ORDER_FILLED, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED and TRAILING_STOP_FILLED records of transactions and stream events response
- Added tradeId field into STOP_LOSS_FILLED, TAKE_PROFIT_FILLED and TRAILING_STOP_FILLED records of /alltransactions response 

-------------------------------------

## Version 1.3.1
- Released to Sandbox on June 30, 2014
- Release to fxTrade Practice pending.  Compatibility change is scheduled to be released on July 23, 2014. 
- Release to fxTrade pending

##### Compatibility Changes:

- Wrapped streaming rates inside a "tick" object as mentioned [here](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445).  This change is scheduled to be released to fxPractice on July 23, 2014.

##### New Features:

- Introduced [daily](/docs/v1/rates/#retrieve-instrument-history) and [weekly](/docs/v1/rates/#retrieve-instrument-history) alignment parameter to allow users to specify which hour and day of the week to use when delimiting daily or weekly candle requests.


