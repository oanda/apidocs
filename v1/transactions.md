---
title: Transactions | OANDA API
---

# Transaction Endpoints

* TOC
{:toc}

## Get transaction history

    GET /v1/accounts/:account_id/transactions

#### Input Query Parameters

maxId
: _Optional_ First transaction to get. The server will return transactions with id less than or equal to this, in descending order (for pagination). 

minId
: _Optional_ Last transaction to get. The server will return transactions with id greater or equal to this, in descending order (for pagination).

count
: _Optional_ Maximum number of transactions to return. The default and maximum value for count is 50.
* **instrument**: Retrieve transactions for a specific instrument only Default: all 

ids
: _Optional_ A comma separated list of transactions ids to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.s

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&count=1"

#### Response
###### Header

~~~Header
Link: <http://api-sandbox.oanda.com/v1/accounts/6531071/transactions?count=1&maxId=17780896>; ref="next"
~~~

###### Body

~~~json
{
  "transactions" : [
    {
      "id" : 177808963,
      "accountId" : 6531071,
      "type" : "marketIfTouched",
      "units" : 2,
      "side" : "sell",
      "action" : "open",
      "reason" : "user_submitted",
      "instrument" : "EUR_USD",
      "time" : "2012-07-03T14:30:38Z",
      "price" : 1.2736,
      "balance" : 100000.0815,
      "profitLoss" : 0,
      "marginUsed" : 0.1274
    },
  ]
}
~~~

####Response Fields
type
: The type of transaction. Possible return values are FOK_market, IOC_market, market, marketIfTouched, limit, stop, position, interest, balance, fund, margin, margin_alert, profit_loss, api_fee, api_license_fee, wire_fee, fund_fee, account.

action
: The action performed. Possible return values are open, close, update, flag_to_close, correct, create, enter, resolve, debit, deposit, credit, withdraw, reset

reason
: The reason the transaction was executed. Possible return values are cancelled, stop_loss_cancelled, take_profit_cancelled, market_fill_cancelled, correction, user_submitted, expired, stop_loss, take_profit, margin_closeout, market_filled, order_filled, non_sufficient_funds, bounds_violation, transfer, account_migration, prime_brokerage_giveup, trailing_stop, limit_stop_FOK, limit_stop_IOC, merge, flagged_for_closing, bulk_close, account_created, margin_alert_entered, margin_alert_resolved, account_transfer, invalid_units, stop_loss_violation, take_profit_violation, trailing_stop_violation, instrument_halted, account_non_tradable, no_new_position_allowed, and insufficient_liquidity.

####Pagination

Transactions can be paginated with the count and maxId parameters.
At most, a maximum of 50 transactions can be returned in one query. 
If more transactions exist than specified by the given or default count, a url with maxId set to the next unreturned transaction will be returned within the Link header.

----

## Get information for a transaction

    GET /v1/accounts/:account_id/transactions/:transaction_id

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions/1170980"

#### Response

~~~json
{
  "id" : 177808963,
  "accountId" : 6531071,
  "type" : "marketIfTouched",
  "units" : 2,
  "side" : "sell",
  "action" : "open",
  "reason" : "user_submitted",
  "instrument" : "EUR_USD",
  "time" : "2012-07-03T14:30:38Z",
  "price" : 1.2736,
  "balance" : 100000.0815,
  "profitLoss" : 0,
  "marginUsed" : 0.1274
}
~~~
