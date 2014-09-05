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

## Version 1.3.5
- Released to Sandbox on August 22, 2014
- Released to fxTrade Practice on August 22, 2014
- Full events stream to fxTrade on September 5, 2014
- X-HTTP-Method-Override header pending to fxTrade

##### New Features:
- Introduced [X-HTTP-Method-Override](/docs/v1/guide/#x-http-method-override) header in favour of some HTTP clients that do not support PATCH or DELETE methods. 

##### Compatibility Changes:
- Events stream now returns all transaction events as suggested [here](https://fxtrade.oanda.com/community/forex-forum/topic/54008795/).

------------------------------------

## Version 1.3.4
- Released to Sandbox on August 8, 2014
- Released to fxTrade Practice on August 8, 2014
- Released to fxTrade on August 15, 2014

##### New Features:
- Introduced [alignmentTimezone](/docs/v1/rates/#retrieve-instrument-history) parameter to allow users to specify which timezone to use when delimiting daily candle requests.
- Introduced interestRate field to the [/v1/instruments](/docs/v1/rates/#get-an-instrument-list) endpoint.
- Introduced support for the [client side OAuth Flow](/docs/v1/auth/#third-party-applications). 
- Introduced new endpoints [/labs/v1/](/docs/v1/forex-labs/) providing API access to our [FxLabs](http://fxtrade.oanda.ca/analysis/labs/) data. (This is not available on the sandbox environment)