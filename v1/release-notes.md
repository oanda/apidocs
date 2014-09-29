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

## Version 1.3.6
- Released to Sandbox on September 19, 2014
- Released to fxTrade Practice on September 19, 2014
- Released to fxTrade on September 26, 2014

##### Compatibility Changes:
- Added *orderId* field to the *ORDER_UPDATE* transaction response.
- When an order is cancelled due to the maximum open trades limit being reached, a corresponding *ORDER_CANCEL* transaction will be reported in the /v1/transactions and the /v1/events responses.
- When a user resets PL within their fxPractice account, a corresponding *PROFIT_LOSS_RESET* transaction will be reported in the /v1/transactions and the /v1/events responses.

##### Bug Fixes:
- Fixed incorrect pip values reported by the /v1/instrument request.  Instruments that have pip value of 1.0 was incorrectly reported as 0.1

