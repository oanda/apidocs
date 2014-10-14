---
title: ポジション | OANDA API
---

# ポジション（通貨ペア毎のポジション）エンドポイント

* TOC
{:toc}

----------------------------

## 全ての未決済ポジションのリストを取得する

    GET /v1/accounts/:account_id/positions 

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 318
~~~

###### ボディ

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

## 特定の銘柄に対するポジションを取得する

    GET /v1/accounts/:account_id/positions/:instrument

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/positions/EUR_USD"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85
~~~

###### ボディ

~~~json
{
  "side" : "sell",
  "instrument" : "EUR_USD",
  "units" : 9,
  "avgPrice" : 1.3093
}
~~~

----

## 未決済のポジションをクローズする 

    DELETE /v1/accounts/:account_id/positions/:instrument

#### 例
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/1234/positions/EUR_USD"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 293
~~~

###### ボディ

~~~json
{
  "ids" : [
     12345,
     12346,
     12347
  ], // ポジションのクローズにより作成されたトランザクションのIDとクローズされたチケットのIDのリスト
  "instrument" : "EUR_USD",
  "totalUnits": 1234,
  "price" : 1.2345
}
~~~

