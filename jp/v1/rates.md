---
title: レート | OANDA API
---

# レート エンドポイント

* TOC
{:toc}

----------------------------

## 銘柄リストを取得する

指定の口座で取引可能な銘柄のリスト (通貨ペア、 CFD、および貴金属)を取得します。

    GET /v1/instruments

※2014年8月現在日本国内ではCFD、貴金属のお取引は提供しておりません。あらかじめご了承ください。

#### インプットクエリのパラメータ

accountId
: _必須_ 取引可能な銘柄のリストを取得したい口座のID 

fields
: _任意_ レスポンスにて返信されるURLエンコードされ(*%2C*)、コンマで区切られた銘柄フィールドのリスト。
         このクエリパラメータへの入力に関わらず __instrument__フィールドは返信されます。
         可能な値については、以下のレスポンスのパラメータのセクションを参照してください。

instruments
: _任意_ レスポンスにて返信されるURLエンコードされ(*%2C*)、コンマで区切られた銘柄フィールドのリスト。
         もしinstrumentsパラメータが設定されなかった場合、全ての銘柄が返信されます。

#### 例
    curl -X GET "http://api-sandbox.oanda.com/v1/instruments?accountId=12345&instruments=AUD_CAD%2CAUD_CHF"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

###### ボディ

~~~json
{
  "instruments" : [
    {
      "instrument" : "AUD_CAD",
      "displayName" : "AUD\/CAD",
      "pip" : "0.0001",
      "maxTradeUnits" : 10000000
    },
    {
      "instrument" : "AUD_CHF",
      "displayName" : "AUD\/CHF",
      "pip" : "0.0001",
      "maxTradeUnits" : 10000000
    }
  ]

}
~~~


#### レスポンスのパラメータ


instrument
: 銘柄名。　この値をレートの取得や新規注文などに使用してください。

displayName
: エンドユーザーの表示名

pip
: 当該銘柄の1 pipの値。　[pipについての詳細はこちら](http://www.babypips.com/school/pips-and-pipettes.html)

maxTradeUnits
: 当該銘柄の最大取引単位

precision
: The smallest unit of measurement to express the change in value between the instrument pair. 

maxTrailingStop
: 当該銘柄を取引する際に設定できるトレーリングストップの最大値（pips)

minTrailingStop
: 当該銘柄を取引する際に設定できるトレーリングストップの最小値（pips)

marginRate
: 銘柄の必要証拠金率。　3% margin rateは0.03と提示されます。
 
__fields__パラメータがリクエストで設定されていなかった場合は、デフォルトで返信される銘柄フィールドは__instrument__、 __displayName__、 __pip__、 __maxTradeUnits__となります。

----

## 現在のレートを取得する

    GET /v1/prices

OANDAプラットフォームで取得可能な特定の銘柄のライブレートを取得します。

#### 入力クエリパラメータ

instruments
: _必須_  URLエンコードされ(*%2C*)、コンマで区切られたレート取得対象の銘柄のリスト。　値は、/v1/instrumentsのレスポンスで受信した取引可能な銘柄の中のどれかであることが必要です。

since
: _任意_  もし設定された場合、指定されたタイムスタンプ以降に発生したレートのみが配信されます。　値は正しい[日時フォーマット](/docs/jp/v1/guide/#datetime-format)である必要があります。


#### 例
    curl -X GET "http://api-sandbox.oanda.com/v1/prices?instruments=EUR_USD%2CUSD_JPY%2CEUR_CAD"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 379
~~~

###### ボディ

~~~json
{
  "prices": [
    {
      "instrument":"EUR_USD",
      "time":"2013-06-21T17:41:04.648747Z",  // time in RFC3339 format
      "bid":1.31513,
      "ask":1.31528
    },
    {
      "instrument":"USD_JPY",
      "time":"2013-06-21T17:49:02.475381Z",
      "bid":97.618,
      "ask":97.633
    },
    {
      "instrument":"EUR_CAD",
      "time":"2013-06-21T17:51:38.063560Z",
      "bid":1.37489,
      "ask":1.37517,
      "status": "halted"                    // このレスポンスのパラメータは当該銘柄がOANDAプラットフォーム上で現在Halted(停止)状態の場合のみ設定されます。
    }
  ]
}
~~~

----

## 銘柄の過去データの取得

特定銘柄に関する過去データの取得

    GET /v1/candles

#### リクエスト
    curl -X GET "http://api-sandbox.oanda.com/v1/candles?instrument=EUR_USD&count=2&candleFormat=midpoint"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 429
~~~

###### ボディ

~~~json
{
  "instrument" : "EUR_USD",
  "granularity": "S5",
  "candles": [
    {
      "time": "2013-06-21T17:41:00Z",  // time in RFC3339 format
      "openMid": 1.30237,
      "highMid": 1.30237,
      "lowMid": 1.30237,
      "closeMid": 1.30237,
      "volume" : 5000,
      "complete": true
    },
    {
      "time": "2013-06-21T17:41:05Z",  // time in RFC3339 format
      "openMid": 1.30242,
      "highMid": 1.30242,
      "lowMid": 1.30242,
      "closeMid": 1.30242,
      "volume" : 2000,
      "complete": true
    }
  ]
}
~~~

#### リクエスト
    curl "http://api-sandbox.oanda.com/v1/candles?instrument=EUR_USD&start=2014-06-19T15%3A47%3A40Z&end=2014-06-19T15%3A47%3A50Z"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 634
~~~

###### ボディ

~~~json
{
    "instrument" : "EUR_USD",
    "granularity" : "S5",
    "candles" : [
        {
            "time" : "2014-06-19T15:47:40.000000Z",
            "openBid" : 1.25682,
            "openAsk" : 1.25691,
            "highBid" : 1.25682,
            "highAsk" : 1.25691,
            "lowBid" : 1.25642,
            "lowAsk" : 1.25651,
            "closeBid" : 1.25642,
            "closeAsk" : 1.25651,
            "volume" : 9,
            "complete" : true
        },
        {
            "time" : "2014-06-19T15:47:45.000000Z",
            "openBid" : 1.25644,
            "openAsk" : 1.25653,
            "highBid" : 1.25644,
            "highAsk" : 1.25653,
            "lowBid" : 1.25634,
            "lowAsk" : 1.25643,
            "closeBid" : 1.25634,
            "closeAsk" : 1.25643,
            "volume" : 4,
            "complete" : true
        }
    ]
}
~~~

#### 入力クエリのパラメータ

instrument
: _Required_  過去データを取得した銘柄の名前。　値は、/v1/instrumentsのレスポンスで受信した取引可能な銘柄の中のどれかであることが必要です。

granularity<sup>1</sup>
: _Optional_  それぞれのキャンドルスティックがカバーしている時間の範囲。　指定された値が、最初のキャンドルスティックのアラインメントを決定します。　
    
	可能な値は:

	* __１分の初めにアライン__
		* "S5"  - 5 秒
		* "S10" - 10 秒
		* "S15" - 15 秒
		* "S30" - 30 秒
		* "M1"  - 1 分
	* __1時間の初めにアライン__
		* "M2"  - 2 分
		* "M3"  - 3 分
		* "M5"  - 5 分
		* "M10" - 10 分
		* "M15" - 15 分
		* "M30" - 30 分
		* "H1"  - 1 時間
	* __1日の初めにアライン(17:00, 米国東部標準時)__
		* "H2"  - 2 時間
		* "H3"  - 3 時間
		* "H4"  - 4 時間
		* "H6"  - 6 時間
		* "H8"  - 8 時間
		* "H12" - 12 時間
		* "D"   - 1 日
	* __1週間の初めにアライン (土曜日)__
		* "W"   - 1 週
	* __1か月の初めにアライン (その月の最初の日)__
		* "M"   - 1 か月
	

もしgranularityパラメータが設定されなかった場合は、__granularity__ のデフォルト値は"S5"となります。

count
: _任意_  レスポンスで送信するキャンドルの数。　指定された時間範囲によっては、このパラメータはサーバー側で無視されることがあります。　詳細な説明については以下の"時間とカウントの意味"を参照してください。  * 
もし設定されなかった場合、__count__ のデフォルト値は500です。　__count__ に設定できる最大値は5000です。
             
	もし、__start__ と __end__ パラメータが両方設定されていた場合、__count__ は設定されるべきではありません。

start<sup>2</sup>
: _任意_  リクエストされたキャンドルの時間範囲における開始日時のタイムスタンプ。　設定される値は、有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)である必要があります。

end<sup>2</sup>
: _任意_  リクエストされたキャンドルの時間範囲における終了日時のタイムスタンプ。　設定される値は、有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)である必要があります。

candleFormat
: _任意_ ローソク表示 ([ローソク表示について](#about-candlestick-representation))。　これは以下のいずれかの値が設定可能です:
	* "midpoint" - 中値ベースのローソク
	* "bidask" - Bid/Askベースのローソク

	もしcandleFormatパラメータが設定されなかった場合、 __candleFormat__ のデフォルト値は"bidask"です。

includeFirst
: _任意_  "true" もしくは "false"が設定できるboolean型パラメータ。　もしこのパラメータが"true"の場合は、 <i>start</i> タイムスタンプがカバーしているローソクがレスポンスに含まれます。　"false"の場合、このローソクは含まれません。
This field exists to provide clients a mechanism to not repeatedly fetch the most recent candlestick which it is not a "Dancing Bear".
If __includeFirst__ is not specified, the default setting is "true".

<sup>1</sup> ティックがなかったインターバルについてはローソクは送信されませんので、ギャップが発生します。<br>
<sup>2</sup> もし __start__ 及び __end__ の両方のパラメータが設定されなかった場合、 __end__ にはデフォルトとして現在の時刻が設定され、 __count__ 本のローソクが送信されます。<br>

----

## ローソク表示について

__midpoint__ ティックボリュームを含む中値ベースのローソク

~~~json
{
  "time":<TS>,
  "openMid":<O_m>,
  "highMid":<H_m>,
  "lowMid":<L_m>,
  "closeMid":<C_m>,
  "volume":<V>,
  "complete":<DB>
}
~~~

__bidask__ ティックボリュームを含むBID/ASKベースのローソク

~~~json
{
  "time":<TS>,
  "openBid":<O_b>,
  "openAsk":<O_a>,
  "highBid":<H_b>,
  "highAsk":<H_a>,
  "lowBid":<L_b>,
  "lowAsk":<L_a>,
  "closeBid":<C_b>,
  "closeAsk":<C_a>,
  "volume":<V>,
  "complete":<DB>
}
~~~

以上のローソクにおける各フィールドは以下の意味を持っています:

* TS  - ローソクの開始タイムスタンプ
* O_b - Open Tick Bid
* O_a - Open Tick Ask
* O_m - Open Tick Mid
* H_b - High Tick Bid
* H_a - High Tick Ask
* H_m - High Tick Mid
* L_b - Low Tick Bid
* L_a - Low Tick Ask
* L_m - Low Tick Mid
* C_b - Close Tick Bid
* C_a - Close Tick Ask
* C_m - Close Tick Mid
* V   - ローソクのティックボリューム
* DB  - "Dancing Bear". This field indicates that the candlestick
        may still be modified by the creation of ticks in the future because
        the current server time is contained by the time range which the
        candlestick represents.

