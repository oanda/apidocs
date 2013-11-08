---
title: Accounts | OANDA API
---

# Account Endpoints

* TOC
{:toc}

## Get accounts for a user

Return a list of accounts owned by user

    GET /v1/accounts

#### Input Query Parameters

username
: _Optional_ Name of the user

#### Example
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts?username=fxtrader"

#### Response

~~~json
[
   85454,
   95666,
   23633
]
~~~

----

## Create a test account
Create a new account.  This call is only available on our sandbox system.  Please create accounts on fxtrade.oanda.com on our production system.

    POST /v1/accounts

#### Input Query Parameters

currency
: _Optional_ Home currency of the newly created account


#### Example
    curl -X POST "http://api-sandbox.oanda.com/v1/accounts"

#### Response

~~~json
{
  "username" : "keith",
  "password" : "Rocir~olf4",
  "accountId" : 8954947
}
~~~

----

## Get account information

    GET /v1/accounts/:account_id

#### Request
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/8954947"

#### Response

~~~json
{
  "accountId" : 8954947,
  "accountName" : "Primary",
  "balance" : 100000,
  "unrealizedPl" : 0,
  "realizedPl" : 0,
  "marginUsed" : 0,
  "marginAvail" : 100000,
  "openTrades" : 0,
  "openOrders" : 0,
  "marginRate" : 0.05,
  "accountCurrency" : "USD"
}
~~~
