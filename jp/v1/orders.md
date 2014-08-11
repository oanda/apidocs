---
title: 注文 | OANDA API
---

# 注文エンドポイント

* TOC
{:toc}

----------------------------

## 特定の口座における注文を取得する

特定の口座における全ての未決済の注文を取得します。　注: 未決済のテイクプロフィット注文およびストップロス注文は未決済[取引](/docs/v1/trades#get-a-list-of-open-trades)オブジェクトに登録されており、このリクエストでは取得できません。

    GET /v1/accounts/:account_id/orders

#### 入力クエリパラメータ

maxId
: _任意_ サーバーは、このIDとそれ以下の注文を降順で返信します(paginationの為)。

count
: _任意_ 取得する未決済注文の最大数。　デフォルトは50で、最大値は500。

instrument
: _任意_ 特定の銘柄に対する未決済注文のみ取得します。　デフォルトは全ての銘柄(all)です。

ids
: _任意_ URLエンコードされ(*%2C*)、コンマで区切られた取得対象の注文IDのリスト。　リストに設定できるIDの最大数は50です。　このパラメータが設定された場合は、他のパラメータは設定できません。

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&count=2"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 660
Link: <http://api-sandbox.oanda.com/v1/accounts/12345/orders?count=2&instrument=EUR_USD&maxId=175427625>; rel="next"
X-Result-Count: 8
~~~

###### ボディ

~~~json
{
  "orders" : [
    {
      "id" : 175427639,
      "instrument" : "EUR_USD",
      "units" : 20,
      "side" : "buy",
      "type" : "marketIfTouched",
      "time" : "2014-02-11T16:22:07Z",
      "price" : 1,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "expiry" : "2014-02-15T16:22:07Z",
      "upperBound" : 0,
      "lowerBound" : 0,
      "trailingStop" : 0
    },
    {
      "id" : 175427637,
      "instrument" : "EUR_USD",
      "units" : 10,
      "side" : "sell",
      "type" : "marketIfTouched",
      "time" : "2014-02-11T16:22:07Z",
      "price" : 1,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "expiry" : "2014-02-12T16:22:07Z",
      "upperBound" : 0,
      "lowerBound" : 0,
      "trailingStop" : 0
    }
  ]
}
~~~

#### Pagination（ページ割り）

注文はcountとmaxIdパラメータによりページ割りができます。
一つのクエリで最大50件の注文が返信されます。 
もし注文の件数がデフォルトもしくは指定された件数を上回っていた場合は、Linkヘッダと一緒にmaxIdが次のまだ未送信の注文のIDに設定されたURLが返信されます。

----

## 新しい注文を作成する

    POST /v1/accounts/:account_id/orders


#### 入力データパラメータ

instrument
: _必須_ 新規注文の銘柄

units
: _必須_ 注文単位

side
: _必須_ 売買区別（buy=買い、sell=売り）

type
: _必須_ 注文のタイプ　'limit'、 'stop'、 'marketIfTouched'、 'market'のいずれか。

expiry
: _必須_ 注文の有効期限。　注文のタイプが　'limit'、 'stop'、　'marketIfTouched'のいずれかの場合、有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時が設定される必要があります。

price
: _必須_ 注文のタイプが　'limit'、 'stop'、　'marketIfTouched'のいずれかの場合、注文がトリガー（発動）する価格を設定します。

lowerBound
: _任意_ 成立下限価格

upperBound
: _任意_ 成立上限価格

stopLoss
: _任意_ ストップロス価格

takeProfit
: _任意_ テイクプロフィット価格

trailingStop
: _任意_ トレーリングストップのディスタンス（pipsで小数第一位まで）

#### 例 ('market' 注文)
    $curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=market" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### レスポンス


###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 204
~~~

###### ボディ

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z", // Time that order was executed
  "price" : 1.37041,               // Trigger price of the order
  "tradeOpened" : {
    "id" : 175517237,              // 注文ID
    "units" : 2,                   // 注文単位
    "side" : "sell",               // 売買区別
    "takeProfit" : 0,              // テイクプロフィット価格　－　注文時に設定した場合
    "stopLoss" : 0,                // ストップロス価格　－　注文時に設定した場合
    "trailingStop" : 0             // The trailing stop associated with the order, if any
  },
  "tradesClosed" : [],
  "tradeReduced" : {}
}
~~~

#### 例 ('marketIfTouched' 注文)
    $curl -X POST -d "instrument=EUR_USD&units=2&side=sell&type=marketIfTouched&price=1.2&expiry=2013-04-01T00%3A00%3A00Z" "http://api-sandbox.oanda.com/v1/accounts/12345/orders"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 292
Location: http://api-sandbox.oanda.com/v1/accounts/12345/orders/175517237
~~~

###### ボディ

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-01-01T20:36:06Z",      // Time that order was executed
  "price" : 1.2,                        // Trigger price of the order
  "orderOpened" : {             
    "id" : 175517237,                   // Order id
    "units" : 2,                        // Number of units
    "side" : "sell",                    // Direction of the order
    "takeProfit" : 0,                   // The take-profit associated with the order, if any
    "stopLoss" : 0,                     // The stop-loss associated with the order, if any
    "expiry" : "2013-02-01T00:00:00Z",  // The time the order expires (in RFC3339 format)
    "upperBound" : 0,                   // The maximum execution price associated with the order, if any
    "lowerBound" : 0,                   // The minimum execution price associated with the order, if any
    "trailingStop" : 0                  // The trailing stop associated with the order, if any
  }
}
~~~

----

## 注文に関する情報を取得する

    GET /v1/accounts/:account_id/orders/:order_id

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/1234/orders/43211"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 290
~~~

###### ボディ

~~~json
{
  "id" : 43211,                        // The ID of the order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the order
  "units" : 5,                         // The number of units in the order
  "side" : "buy",                      // The direction of the order
  "type" : "limit",                    // The type of the order
  "time" : "2013-01-01T00:00:00Z",     // The time of the order (in RFC3339 format)
  "price" : 1.45123,                   // The price the order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the order, if any
  "stopLoss" : 1.4,                    // The stop-loss associated with the order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the order, if any
}
~~~

----

## 既存の注文を変更する

    PATCH /v1/accounts/:account_id/orders/:order_id

#### 入力データパラメータ
注: 特定したパラメータのみが変更され、残りのパラメータは変更されません。　任意のパラメータを削除したい場合は値に0を設定してください。

units
: _任意_ 注文単位

price
: _任意_ 注文がトリガー（発動）する価格

expiry
: _任意_ 注文の有効期限。　有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時が設定される必要があります。


lowerBound
: _任意_ 成立下限価格

upperBound
: _任意_ 成立上限価格

stopLoss
: _任意_ ストップロス価格

takeProfit
: _任意_ テイクプロフィット価格

trailingStop
: _任意_ トレーリングストップのディスタンス（pipsで小数第一位まで）

#### 例
    $curl -X PATCH -d "stopLoss=1.3" "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Server: nginx/1.2.9
Content-Type: application/json
Content-Length: 284

~~~

###### ボディ

~~~json
{
  "id" : 43211,                        // The ID of the order
  "instrument" : "EUR_USD",            // The symbol of the instrument of the order
  "units" : 5,                         // The number of units in the order
  "side" : "buy",                      // The direction of the order
  "type" : "limit",                    // The type of the order
  "time" : "2013-01-01T00:00:00Z",     // The time of the order (in RFC3339 format)
  "price" : 1.45123,                   // The price the order was executed at
  "takeProfit" : 1.7,                  // The take-profit associated with the order, if any
  "stopLoss" : 1.3,                    // The stop-loss associated with the order, if any
  "expiry" : "2013-02-01T00:00:00Z",   // The time the order expires (in RFC3339 format)
  "upperBound" : 0,                    // The maximum execution price associated with the order, if any
  "lowerBound" : 0,                    // The minimum execution price associated with the order, if any
  "trailingStop" : 10                  // The trailing stop associated with the order, if any
}
~~~

----

## Close an order

    DELETE /v1/accounts/:account_id/orders/:order_id

#### 例
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 130
~~~

###### ボディ

~~~json
{
  "id" : 54332,                   // The ID of the close order transaction
  "instrument" : "EUR_USD",       // The symbol of the instrument of the order
  "units" : 2,
  "side" : "sell",
  "price" : 1.30601,              // The price at which the order executed
  "time" : "2013-01-01T00:00:00Z" // The time at which the order executed
}
~~~

