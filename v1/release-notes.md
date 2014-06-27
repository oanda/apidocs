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

## Version 1.3.0
- Released to Sandbox on June 27, 2014
- Released to fxTrade Practice on June 27, 2014
- Released to fxTrade on pending

##### New Features:

- Introduced the X-Accept-Datetime-Format HTTP header to allow users to specify the timestamp of choice.
  This feature addresses the concerns raised in [this discussion](https://fxtrade.oanda.com/community/forex-forum/topic/54007925/).
- Each personal access token is now permitted to have 2 active HTTP streaming connections.
  This change addresses the concern raised [here](https://fxtrade.oanda.com/community/forex-forum/topic/54008535/).
- Introduced the sessionId parameter to allow users to uniquely identify an HTTP streaming connection.
  This is not applicable to the sandbox environment.
  This change addresses the concern raised [here](https://fxtrade.oanda.com/community/forex-forum/topic/54007895/?page=1#post-9935825).


