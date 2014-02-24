---
title: Accounts | OANDA API
---

# Account Endpoints

* TOC
{:toc}

## Get accounts for a user

Get a list of accounts owned by the user

    GET /v1/accounts

#### Input Query Parameters

username
: _Optional_ The name of the user. Note: This is only required on the sandbox, on our production systems your access token will identify you.

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts?username=fxtrader"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 128
~~~

###### Body

~~~json
[
  {
    "accountId" : 8954947,
    "accountName" : "Primary",
    "accountCurrency" : "USD",
    "marginRate" : 0.05
  },
  {
    "accountId" : 8954950,
    "accountName" : "SweetHome",
    "accountCurrency" : "CAD",
    "marginRate" : 0.02
  }
]
~~~

----

## Create a test account
Create a new account.  This call is only available on our sandbox system.  Please create accounts on fxtrade.oanda.com on our production system.

    POST /v1/accounts

#### Input Query Parameters

currency
: _Optional_ The home currency of the newly created account.


#### Example
    $curl -X POST "http://api-sandbox.oanda.com/v1/accounts"

#### Response

###### Header

~~~header
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 77
Location: https://api-sandbox.com/v1/accounts/8954947
~~~

###### Body

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

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/8954947"

#### Response

###### Header

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 269
~~~

###### Body

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
