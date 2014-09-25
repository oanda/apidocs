---
title: 導入の手引き | OANDA API
---

# 導入の手引き


* TOC
{:toc}


<!--
## RESTの基本

[トレーディングAPIリファレンスガイド](/docs/jp/v1/reference/#trading-api-overview)ではURI構造を以下のように記述しています:

##### ベースURL
[http://api-sandbox.oanda.com/v1/](http://api-sandbox.oanda.com/v1/)

All requests on the sandbox will use this as the base URL.

##### アカウント
[http://api-sandbox.oanda.com/v1/accounts/6531071](http://api-sandbox.oanda.com/v1/accounts/6531071)

全てのトレーディングのリクエストに関して貴方のアカウントIDは常にベースURIです。　もしあなたが上記のURLに対してGETリクエストを行った場合は、アカウント6531071の情報を受け取ります。

##### コレクション
[http://api-sandbox.oanda.com/v1/accounts/6531071/trades](http://api-sandbox.oanda.com/v1/accounts/6531071/trades)

全てのアカウントはチケットを保持しています。　上記のGETリクエストを行った場合は、アカウント6531071に対する全ての未決済チケットのリストを取得できます。

##### リソース
[http://api-sandbox.oanda.com/v1/accounts/6531071/trades/177810368](http://api-sandbox.oanda.com/v1/accounts/6531071/trades/177810368)

もしあなたがチケットIDを持っている場合は、GETリクエストによりチケットの詳細な情報を取得できます。上記は、チケット177810368の例です。

それぞれのURIはGET、POST、PUT、DELETEのうちどのリクエストを行うかによって、異なったアクションが行われます。
-->

------

## Open API最新情報 

弊社のプライベートベータプログラムへの登録は**ただ今受付中です**! 

登録可能ユーザー数に限りがございますので、ご興味のある方は[こちらのフォーム](http://developer.oanda.com/beta-signup/)ですぐにお申込みください。　Please note that we will start enabling approved applicants around mid-February.


------

REST APIでは主に３つのことが可能です:

1. リアルタイムな相場レートの取得
1. 過去の相場情報とチャートの取得
1. OANDAトレードアカウントで通貨ペア、 CFD、および貴金属をトレードする

※2014年8月現在日本国内ではCFD、貴金属のお取引は提供しておりません。あらかじめご了承ください。

全てのリクエストとレスポンスは[JSONフォーマット](http://www.json.org/)でエンコードされています。

## リアルタイム相場レートの取得

通貨ペア、貴金属、CFDの価格は1秒間に複数回以上更新されます。　レートを取得するためには、例えばEUR/USDなど銘柄を特定してください。 
通貨ペアの場合、通貨ペア名の'/'の区切り文字をアンダースコア'_'文字に置換してください。

#### 例
現在のEUR/USDのレートを取得する

    $curl -X GET "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 139
~~~

###### ボディ

~~~json
{
  "prices" : [
    {
      "instrument" : "EUR_USD",
      "time" : "2013-09-16T18:59:03.687308Z",
      "bid" : 1.33319,
      "ask" : 1.33326
    }
  ]
}
~~~

#### どの通貨ペア、貴金属、CFDの銘柄が取引可能か？

アカウント上で取引可能な銘柄のリストを取得します

    $curl -X GET "http://api-sandbox.oanda.com/v1/instruments?accountId=1"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 11098
~~~

###### ボディ

~~~json
{
  "instruments":
  [
    {
      "instrument":"AUD_CAD",
      "displayName":"AUD/CAD",
      "pip":"0.0001",
      "maxTradeUnits":10000000
    },

    ...

    {
      "instrument":"ZAR_JPY",
      "displayName":"ZAR/JPY",
      "pip":"0.01",
      "maxTradeUnits":10000000
    }
  ]
}
~~~

#### サンプル　コード
[シンプルレートパネル](https://github.com/oanda/simple-rates-panel)はJavascriptで記述されており、特定の通貨ペアの現在のレートを取得できます。　稼働中のバージョンは[こちら](http://oanda.github.com/simple-rates-panel/simplepanel.html)から確認できます。

[アンドロイドレート](https://github.com/oanda/AndroidRatesAPISample)はjavaで記述されており、APIを利用して現在のレートを表示します。

#### リファレンス
リアルタイムの相場レートに関するリファレンスについては[こちら](https://github.com/oanda/apidocs/blob/master/sections/reference.md#price-api-overview)

## 実験的WebSocketストリーミングAPI

#### 例

    var socket = io.connect('http://api-sandbox.oanda.com' , {
      resource : 'ratestream'
    });
    
    socket.emit('subscribe', {'instruments': ['EUR/USD','USD/CAD']});
    
    socket.on('tick', function (data) {
      console.log("Received tick:" + JSON.stringify(data));
    });
    
#### サンプル　コード
[ストリーミングAPIデモ](https://github.com/oanda/streamingapi-demo)

#### リファレンス
ストリーミングAPIに関するリファレンスについては[こちら](https://github.com/oanda/apidocs/blob/master/sections/streaming.md)

## 過去の相場情報とチャートの取得

APIを通じて現在と過去のキャンドルを取得することが可能です。　自分独自のチャートなど様々な用途に利用できます。

#### 例
EUR/USDの直近の2本のキャンドルを取得する
  
    $curl -X GET "http://api-sandbox.oanda.com/v1/candles?instrument=EUR_USD&count=2"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 621
~~~

###### ボディ

~~~json
{
  "instrument" : "EUR_USD",
  "granularity" : "S5",
  "candles" : [
    {
      "time" : "2014-02-12T14:50:25Z",
      "openBid" : 1.35757,
      "openAsk" : 1.35768,
      "highBid" : 1.35759,
      "highAsk" : 1.35769,
      "lowBid" : 1.35745,
      "lowAsk" : 1.35757,
      "closeBid" : 1.35745,
      "closeAsk" : 1.35757,
      "volume" : 11,
      "complete" : true
    },
    {
      "time" : "2014-02-12T14:50:30Z",
      "openBid" : 1.35746,
      "openAsk" : 1.35757,
      "highBid" : 1.35746,
      "highAsk" : 1.35758,
      "lowBid" : 1.35742,
      "lowAsk" : 1.35753,
      "closeBid" : 1.35742,
      "closeAsk" : 1.35753,
      "volume" : 8,
      "complete" : false
    }
  ]
}
~~~

#### サンプル　コード
[Candle Average Price](https://github.com/oanda/cl-restapi-demo)はlispで記述されており、過去 'X' 日間の通貨ペアの平均価格を計算します。

#### リファレンス
過去の相場情報とチャートに関するリファレンスについては[こちら](https://github.com/oanda/apidocs/blob/master/sections/reference.md) 


## 通貨ペア、貴金属、CFDをトレードする

### テストユーザーを作成する

貴方のアプリケーションをAPI sandboxでテストするためには、まず最初にテストユーザーを作成する必要があります。　ユーザーはアカウントを所有しており、アカウントには貴方がトレードで使用する資金が保管されています。　トレードを行う場合、貴方がトレードで使用したい資金を保管しているアカウントを指定する必要があります。

[ユーザーとアカウントの作成](http://oanda.github.com/gen-account.html)を行うことにより、ユーザー名、パスワード、そしてアカウントIDを取得できます。

* アカウントIDはトレードを行ったり、トレードに関する情報を取得するためのあらゆるリクエストにおいて、必須パラメータです。

ユーザー名とパスワードは、ほとんどのケースにおいては必要ありません。　弊社のsandbox環境においては、ユーザーが少しでも簡単にトレードができるようにAPIは認証を行いません。　**唯一アカウントIDだけがトレードを行うために必要です。**

もし貴方が自分で[ユーザーとアカウントを作成](http://oanda.github.com/gen-account.html)したい場合は、[こちらの手順](/docs/v1/accounts/#create-a-test-account)に従ってください。

### 新規のトレードを行う

#### 例
EUR/USDを1000単位買いのチケットを新規に建てます。 こちらの例はcurlを使用してPOSTデータを通じて３つのパラメータを送信しています。

    $curl -X POST -d "instrument=EUR_USD&units=1000&side=buy&type=market" http://api-sandbox.oanda.com/v1/accounts/6531071/orders

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 265
~~~

###### ボディ

~~~json
{

  "instrument" : "EUR_USD",
  "time" : "2013-12-06T20:36:06Z", // Time that order was executed
  "price" : 1.37041,               // Trigger price of the order
  "tradeOpened" : {
    "id" : 175517237,              // Order id
    "units" : 1000,                // Number of units
    "side" : "buy",                // Direction of the order
    "takeProfit" : 0,              // The take-profit associated with the Order, if any
    "stopLoss" : 0,                // The stop-loss associated with the Order, if any
    "trailingStop" : 0             // The trailing stop associated with the rrder, if any
  },
  "tradesClosed" : [],
  "tradeReduced" : {}
}
~~~

#### サンプル　コード
[Apiトレーディング](https://github.com/oanda/py-api-trading)はPythonで記述されており、新規のチケットを建てたり新規注文を発注するサンプルプログラムです。

#### リファレンス
新規のチケット、新規注文に関するリファレンスについては[こちら](/docs/jp/v1/trades)

<!--
## 未決済のチケットを取得する

#### 例
アカウント6531071に関するEUR/USDの未決済チケットのリストを取得します

    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/6531071/trades?instrument=EUR_USD"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1743
X-Result-Count: 8
~~~

###### ボディ

~~~json
{
  "trades" : [
    {
      "id" : 177810427,
      "units" : 1000,
      "side" : "buy",
      "instrument" : "EUR_USD",
      "time" : "2013-01-11T15:57:11Z",
      "price" : 1.29787,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "trailingStop" : 0
    },
    {
      "id" : 177810261,
      "units" : 4,
      "side" : "buy",
      "instrument" : "EUR_USD",
      "time" : "2013-01-11T15:57:11Z",
      "price" : 1.29736,
      "takeProfit" : 0,
      "stopLoss" : 0,
      "trailingStop" : 0
    }
  ]
}
~~~

#### サンプル　コード
[アカウントサーチ](https://github.com/oanda/AccountSearchPHP)はPHPで記述されており、特定のアカウントの未決済のチケットと注文のリストを取得します。 

#### リファレンス
未決済のチケットと注文に関するリファレンスについては[こちら](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview)

### 未決済のポジションを取得する

未決済ポジションは、未決済チケットを集約したものです。　ある通貨ペアに関して自分で全ての未決済チケットを集約しネットポジションを計算する必要はありません。　ネットポジションを直接リクエストできます。

#### 例
アカウント6531071に関する全ての未決済ポジションのリストを取得します

    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/6531071/positions"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 318
~~~

###### ボディ

~~~json
    {
      "positions" : [
        {
          "side" : "buy",
          "instrument" : "EUR_USD",
          "units" : 1004,
          "avgPrice" : 1.29787
        },
        {
          "side" : "buy",
          "instrument" : "USD_CAD",
          "units" : 298,
          "avgPrice" : 0.99287
        }
      ]
    }
~~~

#### リファレンス
未決済ポジションに関するリファレンスについては[こちら](https://github.com/oanda/apidocs/blob/master/sections/reference.md#trading-api-overview)
-->

### トランザクションの履歴を取得する

トランザクション履歴は指定のアカウントに対する全てのアクティビティの記録です。　アクティビティには、例えば新しいチケットが建てられた、注文が発動した、アカウントに入金があったなどのイベントを含みます。

#### 例
アカウント6531071に関する直近の２つのトランザクションを取得します

    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/6531071/transactions?count=2"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 715
Link: <http://api-sandbox.oanda.com/v1/accounts/6531071/transactions?count=2&maxTransId=175427700>; rel="next"
X-Result-Count: 354
~~~

###### ボディ

~~~Body
{
  "transactions" : [
    {
      "id" : 175427704,
      "accountId" : 6531071,
      "time" : "2014-02-12T15:04:13Z",
      "type" : "MARKET_ORDER_CREATE",
      "instrument" : "EUR_USD",
      "units" : 1000,
      "side" : "buy",
      "price" : 1.35723,
      "pl" : 0,
      "interest" : 0,
      "accountBalance" : 1000041.3944,
      "tradeOpened" : {
        "id" : 175427704,
        "units" : 1000
      }
    },
    {
      "id" : 175427703,
      "accountId" : 6531071,
      "time" : "2014-02-12T15:03:27Z",
      "type" : "MARKET_ORDER_CREATE",
      "instrument" : "EUR_USD",
      "units" : 1000,
      "side" : "buy",
      "price" : 1.35728,
      "pl" : 0,
      "interest" : 0,
      "accountBalance" : 1000041.3944,
      "tradeOpened" : {
        "id" : 175427703,
        "units" : 1000
      }
    }
  ]
}
~~~

#### リファレンス
トランザクション履歴に関するリファレンスについては[こちら](/docs/v1/transactions) for viewing transaction history.

----

このページ上の情報は実際にできることのほんの一部でしかありません。　更なる詳細については左側のサイドメニューの各アイテムを参照してください。

