---
title: 開発ガイド | OANDA API
---

# 開発ガイド


* TOC
{:toc}



------


APIのURL
--------------------

REST APIには三つの異なった環境が用意されています。

|環境|URL|認証|詳細|
|---|---|---|---|---|
|Sandbox|**http**://api-sandbox.oanda.com|認証は必要ありません。|テスト専用の環境。　他の環境に比べると反応速度、安定度、信頼度の面で劣ります(時々環境が使用不可能になることもあります)。|
|fxTrade Practice|**https**://api-fxpractice.oanda.com|必要です。[詳細はこちら](/docs/jp/v1/auth/)|安定した環境。　fxTrade Practiceアカウントとパーソナルアクセストークンでのテスト時に推奨の環境です。|
|fxTrade|**https**://api-fxtrade.oanda.com|必要です。[詳細はこちら](/docs/jp/v1/auth/)|安定した環境。　fxTradeアカウントとパーソナルアクセストークンによりアプリケーションを本番稼働させる際の推奨環境です。|

Streaming APIにも同じく三つの異なった環境が用意されています。

|環境|URL|認証|詳細|
|---|---|---|---|---|
|Sandbox|**http**://stream-sandbox.oanda.com|認証は必要ありません。|テスト専用の環境。　他の環境に比べると反応速度、安定度、信頼度の面で劣ります(時々環境が使用不可能になることもあります)。|
|fxTrade Practice|**https**://stream-fxpractice.oanda.com|必要です。[詳細はこちら](/docs/jp/v1/auth/)|安定した環境。　fxTrade Practiceアカウントとパーソナルアクセストークンでのテスト時に推奨の環境です。|
|fxTrade|**https**://stream-fxtrade.oanda.com|必要です。[詳細はこちら](/docs/jp/v1/auth/)|安定した環境。　fxTradeアカウントとパーソナルアクセストークンによりアプリケーションを本番稼働させる際の推奨環境です。|

<br/>

本マニュアルにおける例では常にsandbox URLを使用しています。　他の環境を利用する場合は、URLのベースを上の表の適当なURLに置き換え、必要な認証ステップをクリアしてください。 (注意: http、httpsも必要に応じて置換してください)。

Sandbox環境はREST APIのテスト用の環境で、誰でも使用可能です。　Sandbox環境には現在以下の制限があります:

* Generated (fake) market data
* No order monitoring.  Limiting orders will not be triggered.

----

リクエストとレスポンスの詳細
--------------------

全てのリクエストは不必要と明記されている場合を除き`Content-Type: application/x-www-form-urlencoded`が必要です。

全てのレスポンスは[JSONフォーマット](http://www.json.org)です。

全てのエンドポイントはHTTP OPTIONSメソッドに対応しており、`Access-Control-Allow-Methods`のヘッダにより、そのエンドポイントで可能なメソッドの一覧をレスポンスで返します。

------


DateTimeフォーマット
------------

OANDA APIはRFC3339とUnixのdatetimeフォーマットをリクエスト、レスポンスでサポートしています。

リクエストはHTTPヘッダの`X-Accept-Datetime-Format`を利用して、使用するdatetimeフォーマットを指定するべきです。

このヘッダにおける設定可能な値は:

* "UNIX" - 全てのタイムスタンプはUnix timeフォーマットになります。
    * 入力については、OANDAサーバーは秒単位まで認識します。
    * 出力については, OANDAサーバーはマイクロ秒単位まで表示します。
* "RFC3339" - 全てのタイムスタンプはRFC3339フォーマットになります。
    * 入力については、OANDAサーバーは秒単位まで認識します。
    * 出力については, OANDAサーバーはマイクロ秒単位まで表示します。

もし`X-Accept-Datetime-Format`ヘッダが設定されていない場合、リクエスト、レスポンスにおけるデフォルトのdatetimeフォーマットはRFC3339となります。

注意: 本セクションを例外として、OANDA開発者ポータルにおける全ての例はデフォルトdatetimeフォーマットのRFC3339を使用しています。

#### 例: UNIX datetimeフォーマットを設定する。

リクエスト:

    curl -i -H "X-Accept-Datetime-Format: UNIX" "http://api-sandbox.oanda.com/v1/candles?instrument=EUR_USD&start=137849394&count=1"

レスポンス:

~~~json
{
	"instrument" : "EUR_USD",
	"granularity" : "S5",
	"candles" : [
		{
			"time" : "1378493950000000",
			"openBid" : 1.31807,
			"openAsk" : 1.31817,
			"highBid" : 1.31807,
			"highAsk" : 1.31817,
			"lowBid" : 1.31806,
			"lowAsk" : 1.31817,
			"closeBid" : 1.31806,
			"closeAsk" : 1.31817,
			"volume" : 2,
			"complete" : true
		}
	]
}
~~~

------


ETag
------------


OANDA REST APIは全てのGETリクエストにおいて、ETagをサポートしています。　ETagを使用することにより、データトラフィックとレイテンシーの削減の効果があります。

ETagの使用:

1. 成功したGETリクエストのレスポンスにはETagヘッダが含まれています。　このETagはレスポンスボディのハッシュです。　もしGETリクエストが繰り返される場合は、この値を保存してください。

2. もしGETリクエストを実行する場合は、前回のGETレスポンスから保存しておいたETagをﾂ**If-None-Match**ﾂヘッダと一緒に設定してリクエストを送信してください。 

    * もしデータが変更されていなかった場合、HTTPコード304がボディなしでレスポンスされます。
    * もしデータが変更されていた場合は、レスポンスは通常通り返ってきます。　新しいETagが送信されてきますので、今後のリクエストのためにこの値を保存することを推奨します。

#### 例: ETagでレートを取得する

##### ステップ 1: EUR/USDの現在のレートを取得する

Request:

    curl -i "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD"

Response:

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 139
ETag: "76674ac46b624e70fc24e176d56c224bad85bc65"
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

##### ステップ 2: 同じGETレートリクエストをIf-None-Matchヘッダと前回のETagを付与して行います

Request:

    curl -i -H "If-None-Match: \"76674ac46b624e70fc24e176d56c224bad85bc65\"" "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD"
    curl -i -H 'If-None-Match: "76674ac46b624e70fc24e176d56c224bad85bc65"' "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD"

##### データに変更がなかった場合

###### ヘッダ

~~~header
HTTP/1.1 304 NOT_MODIFIED
Content-Type: application/json
Content-Length: 139
ETag: "76674ac46b624e70fc24e176d56c224bad85bc65"                      // 同じETagの値が返されます
~~~

##### データに変更があった場合

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 139
ETag: "12984044501813201567f11908be9643f56cc2ee"                      // 新しいETagの値
~~~

###### ボディ

~~~json
{
  "prices" : [
    {
      "instrument" : "EUR_USD",
      "time" : "2013-09-16T18:59:05.687308Z",
      "bid" : 1.33321,
      "ask" : 1.33328
    }
  ]
}
~~~

------


X-HTTP-Method-Override
------------

HTTPクライアントによってはOANDA REST APIで使用しているHTTPメソッドの全てをサポートしていない場合があります。 この課題をクリアするため、OANDA REST APIはX-HTTP-Method-Override HTTP ヘッダを利用してHTTPリクエストに指定されたメソッドをバイパスします。 

X-HTTP-Method-Override HTTP ヘッダは、ベース HTTP メソッドが GETかPOSTとして指定されたリクエストでのみサポートされます。　それぞれのベース HTTP メソッドにおいて、値の一部のみが　X-HTTP-Method-Override HTTP ヘッダを通じてサポートされます。

以下が有効な HTTP メソッドと X-HTTP-Method-Override の値の一覧です。

|HTTP メソッド|X-HTTP-Method-Override の値|
|---|---|
|GET|GET, OPTIONS|
|POST|POST, PATCH, DELETE|

#### 例 １: X-HTTP-Method-Overrideを利用してリクエストをOPTIONSリクエストにオーバーライドする。

###### リクエスト: 

~~~
curl -i -X GET -H "X-HTTP-Method-Override: OPTIONS" "https://api-fxtrade.oanda.com/v1/instruments"
~~~

#### 例 ２: X-HTTP-Method-Overrideを利用してリクエストをPATCHリクエストにオーバーライドする。

###### リクエスト: 

~~~
curl -i -X POST -H "X-HTTP-Method-Override: PATCH" -d "stopLoss=1.334" "https://api-fxtrade.oanda.com/v1/accounts/6531071/orders/12345"
~~~

----

レート制限
-------------

ユーザーは平均毎秒15リクエストまで送信することができ、そして一度に送信できる（バースト）リクエスト数は15以下です。　過剰なリクエストは拒否（Reject)されます。

----


## リアルタイムレートの取得

外国為替、貴金属、CFDの価格は一秒間に複数回更新されます。　レートを取得するには、取得したい通貨ペアを指定し（例えばEUR/USD）、通貨ペア名の'/'の区切り記号を'_'に置換してください。

※2015年1月現在日本国内ではCFD、貴金属のお取引は提供しておりません。あらかじめご了承ください。

#### 例
EUR/USDの現在のレートを取得する

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

----

## ヒストリカルレートとチャートを取得

APIによって現在と過去のキャンドルを、自分独自のチャートの作成など様々な用途のために、取得することも可能です。

#### 例
最も直近のEUR/USDのキャンドルを2本取得する
  
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

#### サンプルコード
[キャンドル平均レート](https://github.com/oanda/cl-restapi-demo)はLispで書かれており、通貨ペアの'X'日間の平均レートを計算します。

#### リファレンス
ヒストリカルレートとチャートに関する[リファレンスマニュアル](https://github.com/oanda/apidocs/blob/master/sections/reference.md)。

----

## 通貨ペア、貴金属、CFDを取引する

※2015年1月現在日本国内ではCFD、貴金属のお取引は提供しておりません。あらかじめご了承ください。

#### 例
EUR/USDの1000通貨買い注文を入れます。 この例はcurlを利用し、POSTデータで3つのパラメーターを送信しています。

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

#### サンプルコード
[APIトレーディング](https://github.com/oanda/py-api-trading)はPythonで書かれており、発注などトレードの例を見ることができます。

#### リファレンス
注文、取引に関する[リファレンスマニュアル](/docs/jp/v1/trades)。

----

このページ上の情報は実際にできることのほんの一部でしかありません。　更なる詳細については左側のサイドメニューの各アイテムを参照してください。

