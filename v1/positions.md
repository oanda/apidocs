---
title: Positions | OANDA API
---

# Position Endpoints

* TOC
{:toc}


## Get a list of all open positions

    GET /v1/accounts/:account_id/positions 

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 318
~~~

###### Body

~~~json
{
  "positions" : [
    {
      "instrument" : "EUR_USD",
      "units" : 4741,
      "side" : "buy",
      "avgPrice" : 1.3626
    },
    {
      "instrument" : "USD_CAD",
      "units" : 30,
      "side" : "sell",
      "avgPrice" : 1.11563
    },
    {
      "instrument" : "USD_JPY",
      "units" : 88,
      "side" : "buy",
      "avgPrice" : 102.455
    }
  ]
}
~~~

----

## Get the position for an instrument

    GET /v1/accounts/:account_id/positions/:instrument

#### Example
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR_USD"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85
~~~

###### Body

~~~json
{
  "side" : "sell",
  "instrument" : "EUR_USD",
  "units" : 9,
  "avgPrice" : 1.3093
}
~~~

----

## Close an existing position 

    DELETE /v1/accounts/:account_id/positions/:instrument

#### Example
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/1234/positions/EUR_USD"

#### Response

###### Header

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 293
~~~

###### Body

~~~json
{
  "ids" : [
     12345,
     12346,
     12347
  ], // Contains a list of transaction ids created as a result of the close position, including the id of the trades that were closed
  "instrument" : "EUR_USD",
  "totalUnits": 1234,
  "price" : 1.2345
}
~~~

