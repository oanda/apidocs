---
title: 注文 | OANDA API
---

# 注文 エンドポイント

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
  "time" : "2013-12-06T20:36:06Z", // 注文が作成された時間
  "price" : 1.37041,               // 注文のトリガー価格
  "tradeOpened" : {
    "id" : 175517237,              // 注文ID
    "units" : 2,                   // 注文単位
    "side" : "sell",               // 売買区別
    "takeProfit" : 0,              // テイクプロフィット価格　－　注文時に設定した場合
    "stopLoss" : 0,                // ストップロス価格　－　注文時に設定した場合
    "trailingStop" : 0             // トレーリングストップディスタンス　－　注文時に設定した場合
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
  "time" : "2013-01-01T20:36:06Z",      // 注文が作成された時間
  "price" : 1.2,                        // 注文のトリガー価格
  "orderOpened" : {             
    "id" : 175517237,                   // 注文ID
    "units" : 2,                        // 注文単位
    "side" : "sell",                    // 売買区別
    "takeProfit" : 0,                   // テイクプロフィット価格　－　注文時に設定した場合
    "stopLoss" : 0,                     // ストップロス価格　－　注文時に設定した場合
    "expiry" : "2013-02-01T00:00:00Z",  // 注文が失効する時間 (RFC3339フォーマット)
    "upperBound" : 0,                   // 成立上限価格　－　注文時に設定した場合
    "lowerBound" : 0,                   // 成立下限価格　－　注文時に設定した場合
    "trailingStop" : 0                  // トレーリングストップディスタンス　－　注文時に設定した場合
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
  "id" : 43211,                        // 注文ID
  "instrument" : "EUR_USD",            // 銘柄
  "units" : 5,                         // 注文単位
  "side" : "buy",                      // 売買区別
  "type" : "limit",                    // 注文のタイプ
  "time" : "2013-01-01T00:00:00Z",     // 注文が最後に変更された時間、もしくは一度も変更がなかった場合は作成された時間 (RFC3339フォーマット)
  "price" : 1.45123,                   // 注文のトリガー価格
  "takeProfit" : 1.7,                  // テイクプロフィット価格　－　注文時に設定した場合
  "stopLoss" : 1.4,                    // ストップロス価格　－　注文時に設定した場合
  "expiry" : "2013-02-01T00:00:00Z",   // 注文が失効する時間 (RFC3339フォーマット)
  "upperBound" : 0,                    // 成立上限価格　－　注文時に設定した場合
  "lowerBound" : 0,                    // 成立下限価格　－　注文時に設定した場合
  "trailingStop" : 10                  // トレーリングストップディスタンス　－　注文時に設定した場合
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
  "id" : 43211,                        // 注文ID
  "instrument" : "EUR_USD",            // 銘柄
  "units" : 5,                         // 注文単位
  "side" : "buy",                      // 売買区別
  "type" : "limit",                    // 注文のタイプ
  "time" : "2013-01-01T00:00:00Z",     // 注文が変更された時間 (RFC3339フォーマット)
  "price" : 1.45123,                   // 注文のトリガー価格
  "takeProfit" : 1.7,                  // テイクプロフィット価格　－　注文時に設定した場合
  "stopLoss" : 1.3,                    // ストップロス価格　－　注文時に設定した場合
  "expiry" : "2013-02-01T00:00:00Z",   // 注文が失効する時間 (RFC3339フォーマット)
  "upperBound" : 0,                    // 成立上限価格　－　注文時に設定した場合
  "lowerBound" : 0,                    // 成立下限価格　－　注文時に設定した場合
  "trailingStop" : 10                  // トレーリングストップディスタンス　－　注文時に設定した場合
}
~~~

----

## 注文をキャンセルする

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
  "id" : 54332,                   // キャンセルする注文のID
  "instrument" : "EUR_USD",       // 銘柄
  "units" : 2,
  "side" : "sell",
  "price" : 1.30601,              // 注文のトリガー価格
  "time" : "2013-01-01T00:00:00Z" // 注文がキャンセルされた時間
}
~~~

