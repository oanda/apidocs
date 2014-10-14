---
title: チケット | OANDA API
---

# チケット（注文毎のポジション） エンドポイント

* TOC
{:toc}

----------------------------

## 未決済チケットのリストを取得する

    GET /v1/accounts/:account_id/trades

#### 入力クエリパラメータ

maxId
: _任意_  サーバーは、このIDとそれ以下のチケットを降順で返信します(paginationの為)。

count
: _任意_ 取得する未決済チケットの最大数。　デフォルトは50で、最大値は500。

instrument
: _任意_ 特定の銘柄に対する未決済チケットのみ取得します。　デフォルトは全ての銘柄(all)です。

ids
: _任意_ URLエンコードされ、コンマで区切られた取得対象のチケットIDのリスト。　リストに設定できるIDの最大数は50です。　このパラメータが設定された場合は、他のパラメータは設定できません。

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/trades?instrument=EUR_USD&count=2"

####レスポンス

######ヘッダ

~~~Header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 449
Link: <http://api-sandbox.oanda.com/v1/accounts/12345/trades?count=2&instrument=EUR_USD&maxId=175427741>; rel="next"
X-Result-Count: 3
~~~

######ボディ

~~~json
{
  "trades" : [
    {
            "id" : 175427743,
            "units" : 2,
            "side" : "sell",
            "instrument" : "EUR_USD",
            "time" : "2014-02-13T17:47:57Z",
            "price" : 1.36687,
            "takeProfit" : 0,
            "stopLoss" : 0,
            "trailingStop" : 0
            "trailingAmount" : 0
    },
    {
            "id" : 175427742,
            "units" : 2,
            "side" : "sell",
            "instrument" : "EUR_USD",
            "time" : "2014-02-13T17:47:56Z",
            "price" : 1.36687,
            "takeProfit" : 0,
            "stopLoss" : 0,
            "trailingStop" : 0,
            "trailingAmount" : 0,
    }
  ]
}
~~~

####Pagination（ページ割り）

チケットはcountとmaxIdパラメータによりページ割りができます。
一つのクエリで最大50件のチケットが返信されます。 
もしチケットの件数がデフォルトもしくは指定された件数を上回っていた場合は、Linkヘッダと一緒にmaxIdが次のまだ未送信のチケットのIDに設定されたURLが返信されます。

----

## 特定のチケットに関する情報を取得する

    GET /v1/accounts/:account_id/trades/:trade_id

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### レスポンス

######ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 193
~~~

######ボディ

~~~json
{
  "id" : 43211,                          // The ID of the trade
  "units" : 5,                           // The number of units in the trade
  "side" : "buy",                        // The direction of the trade
  "instrument" : "EUR_USD",              // The symbol of the instrument of the trade
  "time" : "2013-07-03T14:30:38Z",       // The time of the trade (in RFC3339 format)
  "price" : 1.45123,                     // The price the trade was executed at
  "takeProfit" : 1.7,                    // The take-profit associated with the trade, if any
  "stopLoss" : 1.4,                      // The stop-loss associated with the trade, if any
  "trailingStop" : 50                    // The trailing stop associated with the trade, if any
  "trailingAmount" : 1.44613             // The trailing amount associated with the trade, if any.
                                            Returns -1 if our trailing amount server is unavailable
}
~~~

----

## 既存のチケットを変更する

    PATCH /v1/accounts/:account_id/trades/:trade_id

#### 入力データパラメータ
注: 特定したパラメータのみが変更され、残りのパラメータは変更されません。　任意のパラメータを削除したい場合は値に0を設定してください。

stopLoss
: _任意_ ストップロス価格

takeProfit
: _任意_ テイクプロフィット価格

trailingStop
: _任意_ トレーリングストップのディスタンス（pipsで小数第一位まで）

#### 例
    $curl -X PATCH -d "stopLoss=1.6" "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### レスポンス

######ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 193
~~~

######ボディ

~~~json
{
  "id" : 43211,                          // チケットID
  "units" : 5,                           // チケットの単位
  "side" : "buy",                        // 売買区別
  "instrument" : "EUR_USD",              // 銘柄
  "time" : "2013-07-03T14:30:38Z",       // チケットが変更された時間 (RFC3339フォーマット)
  "price" : 1.45123,                     // チケットの執行された価格
  "takeProfit" : 1.7,                    // テイクプロフィット価格　－　注文時に設定した場合
  "stopLoss" : 1.6,                      // ストップロス価格　－　注文時に設定した場合
  "trailingStop" : 50                    // トレーリングストップディスタンス　－　注文時に設定した場合
  "trailingAmount" : 1.44613             // トレーリングストップ数量　－　注文時に設定した場合
                                            もし弊社のtrailing amount serverに接続できない場合は-1が設定されます。
}
~~~

----

## 未決済のチケットをクローズする

    DELETE /v1/accounts/:account_id/trades/:trade_id

#### 例
    $curl -X DELETE "http://api-sandbox.oanda.com/v1/accounts/12345/trades/43211"

#### レスポンス

######ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 140
~~~

######ボディ

~~~json
{
  "id" : 54332,                   // キャンセルするチケットのID
  "price" : 1.30601,              // チケットがクローズされた価格
  "instrument" : "EUR_USD",       // 銘柄
  "profit" :  0.005,              // ベース通貨における取引の利益
  "side" : "sell"                 // 売買種別
  "time" : "2013-07-12T16:09:15Z" // チケットがクローズされた時間 RFC3339フォーマット)
}
~~~
