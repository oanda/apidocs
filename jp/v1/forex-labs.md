---
title: OANDA FXラボ（ForexLabs）| OANDA API
---

# OANDA FXラボ（ForexLabs）エンドポイント

* TOC
{:toc}

----------------------------

## OANDA FXラボ（ForexLabs）

OANDA APIの一環として、弊社はFXの分析、シグナル、ツールを提供するOANDA FXラボ（ForexLabs）へのアクセスを提供します。　これらのAPIリクエストにより取得できる情報の詳細は、[OANDA FXラボ](http://fxtrade.oanda.ca/analysis/labs)をご覧ください。

本ページの全てのエンドポイントは認証が必要です。　従ってsandbox環境からはアクセスすることはできません。　本ページの例はapi-fxpractice環境を利用しています。

<p style="color: red;font-style: italic;">このサービスのご提供はまだ開発段階ですので、今後幾つかの機能に変更がある可能性があります。　最近の変更については弊社の<a href="/docs/jp/v1/release-notes">リリースノート</a>のページをご参照ください。</p> 

-----------------

## カレンダー

銘柄に関する一年前までの経済カレンダー情報を取得できます。 例えば、もし銘柄が EUR_USD　だった場合、ユーロとUSドルに関連する全ての経済情報が含まれます。 情報には、重要な会議などのニュースや、経済指標データなどがあります。


    GET /labs/v1/calendar


#### 入力クエリパラメータ

instrument
: _必須_ カレンダー取得対象の銘柄名。　全ての取引可能な銘柄が指定可能です。

period
: _必須_ カレンダーデータを取得する期間（秒単位）。 以下の有効な値のいずれかに合致しない値を設定した場合は、一番近い有効な値に自動的に修正されます。

	有効な値:

    * __3600__     - 1時間
    * __43200__    - 12時間
    * __86400__    - 1日
    * __604800__   - 1週間
    * __2592000__  - 1ヶ月
    * __7776000__  - 3ヶ月
    * __15552000__ - 6ヶ月
    * __31536000__ - 1年

#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/calendar?instrument=EUR_USD&period=2592000" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
[
    {
        "title": "Building Permits",
        "timestamp": 1379507400,
        "unit": "k",
        "currency": "USD",
        "forecast": "910",
        "previous": "954",
        "actual": "926",
        "market": "950"
    },
    {
        "title": "FOMC - Fed Funds Rate",
        "timestamp": 1379527200,
        "unit": "%",
        "currency": "USD",
        "forecast": "0.25",
        "previous": "0.25",
        "actual": "0.25",
        "market": "0.25"
    },
    {
        "title": "Fed Chairman Bernanke holds press conference following FOMC meeting on interest rate policy",
        "timestamp": 1379529000,
        "unit": "",
        "currency": "USD"
    }
]
~~~

#### JSON レスポンス フィールド

title
: イベントのタイトル。

timestamp
: イベントのタイムスタンプ（unix timestamp形式）。

unit
: __forecast__, __previous__, __actual__ and __market__の各フィールドにおけるデータの形式。　設定される値の例としては: % の場合はデータがパーセント表示、 k の場合はデータが1,000単位、　もしくは本イベントに関連するデータがない場合は空白。

currency
: ニュースイベントに関連する通貨。

forecast
: フォーキャストの値。

previous
: 同イベントの前回リリース時の値。

actual
: 実際値。　イベントが実際に起こった後でのみ、取得可能になります。

market
: 市場が期待した値。

-----------------------

## ヒストリカルポジションレシオ

銘柄の1年前までのヒストリカルポジションレシオを取得します。　詳細は[OANDA FXラボ](http://fxtrade.oanda.ca/analysis/historical-positions)をご覧ください。


    GET /labs/v1/historical_position_ratios


#### 入力クエリパラメータ

instrument
: _必須_ ヒストリカルポジションレシオ取得対象の銘柄名。 取得可能な銘柄: AUD_JPY, AUD_USD, EUR_AUD, EUR_CHF, EUR_GBP, EUR_JPY, EUR_USD, GBP_CHF, GBP_JPY, GBP_USD, NZD_USD, USD_CAD, USD_CHF, USD_JPY, XAU_USD, XAG_USD.

period
: _必須_ ヒストリカルポジションレシオのデータを取得する期間（秒単位）。 以下の有効な値のいずれかに合致しない値を設定した場合は、一番近い有効な値に自動的に修正されます。

	有効な値:

    * __86400__    - 1日      - 20分毎スナップショット
    * __172800__   - 2日    　- 20分毎スナップショット
    * __604800__   - 1週間    - 1時間毎スナップショット
    * __2592000__  - 1ヶ月  　- 3時間毎スナップショット
    * __7776000__  - 3ヶ月　  - 3時間毎スナップショット
    * __15552000__ - 6ヶ月    - 3時間毎スナップショット
    * __31536000__ - 1年   　 - 1日毎スナップショット

#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/historical_position_ratios?instrument=EUR_USD&period=86400" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
{
  "data": {
    "EUR_USD": {
      "data": [
        [
          1382026800,
          26.11,
          1.3663
        ],
        [
          1382028001,
          26.11,
          1.3666
        ],
        [
          1382029200,
          26.11,
          1.3662
        ],
        [
          1382030401,
          26.11,
          1.3665
        ],
        [
          1382031600,
          26.11,
          1.3666
        ]
      ],
      "label": "EUR/USD"
    }
  }
}
~~~

#### JSON レスポンス フィールド

timestamp
: 配列の最初の項目。　タイムスタンプはunix timestamp形式です。

long position ratio
: 配列の2番目の項目。 このスナップショットのロングポジションのパーセンテージ。　45.66%のロングポジションのレシオは45.66と設定されます。

exchange rate
: 配列の3番目の項目。 銘柄のその時点での為替レート。

label
: 通貨の表示名。

-----------------------

## スプレッド

銘柄の1年前までのスプレッド情報を取得します。　返信されるデータは15分間隔のインターバルに分割されています。　それぞれのインターバルにおいて時間加重平均、最小、最大スプレッドが取得できます。　詳細は[OANDA FXラボ](http://fxtrade.oanda.ca/why/spreads/recent)をご覧ください。 


    GET /labs/v1/spreads


#### 入力クエリパラメータ

instrument
: _必須_ スプレッドデータ取得対象の銘柄名。　全ての取引可能な銘柄が指定可能です。

period
: _必須_ スプレッドデータを取得する期間（秒単位）。 以下の有効な値のいずれかに合致しない値を設定した場合は、一番近い有効な値に自動的に修正されます。

	有効な値:

    * __3600__     - 1時間
    * __43200__    - 12時間
    * __86400__    - 1日
    * __604800__   - 1週間
    * __2592000__  - 1ヶ月
    * __7776000__  - 3ヶ月
    * __15552000__ - 6ヶ月
    * __31536000__ - 1年

unique
: _任意_ このパラメータはインターバルの隣同士でスプレッド値が完全に同じ値である場合に、それでも全てのスプレッドを省略せずに取得するかどうかを決定します。　"0"の場合は、全てのスプレッドが返信されます。　"1"の場合は、隣同士の重複するスプレッドは省略されます。　これはデータ送信量を抑えるためのパラメータです。

    デフォルトは "1" です。

#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/spreads?instrument=EUR_USD&period=3600" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
{
  "max": [
    [
      1381996800,
      2.8
    ],
    [
      1381997700,
      2.3
    ],
    [
      1381998600,
      2.1
    ]
  ]
  "min": [
    [
      1381996800,
      0.8
    ],
    [
      1382001300,
      0.7
    ],
    [
      1382002200,
      0.8
    ]
  ]
  "avg": [
    [
      1381996800,
      1.15367
    ],
    [
      1381997700,
      1.15878
    ],
    [
      1381998600,
      1.09433
    ]
  ]
}
~~~

#### JSON レスポンス フィールド

メインのレスポンスハッシュには、3つのキーがあります: max, min, avg。　スプレッド情報の配列は、これらの3つのキーに対応しています。

max
: 15分インターバルの最大のスプレッド

min
: 15分インターバルの最小のスプレッド

avg
: 15分インターバルの平均スプレッド

timestamp
: 配列の最初の項目で、15分インターバルの終了の時間が設定されます（unix timestamp形式）。

spread
: 配列の2番目の項目で、15分インターバルのスプレッドが設定されます。

-----------------------

## CFTC（全米先物取引委員会）IMM通貨先物ポジション

サポートされている特定の通貨ペアに対して、CFTC（全米先物取引委員会）が発表するIMM通貨先物ポジションを4年前までさかのぼる事ができます。[CFTC](http://www.cftc.gov/MarketReports/CommitmentsofTraders/index.htm)。　詳細は[OANDA FXラボ](http://fxtrade.oanda.ca/analysis/commitments-of-traders)をご覧ください。


    GET /labs/v1/commitments_of_traders


#### 入力クエリパラメータ

instrument
: _必須_ ポジション取得対象の銘柄名。　取得可能な銘柄: AUD_USD, GBP_USD, USD_CAD, EUR_USD, USD_JPY, USD_MXN, NZD_USD, USD_CHF, XAU_USD, XAG_USD.

#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/commitments_of_traders?instrument=EUR_USD" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
{
  "EUR_USD": [
    {
      "oi": "179512",
      "ncl": "85915",
      "price": "1.4643725",
      "date": 1199750400,
      "ncs": "34013",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "177003",
      "ncl": "81812",
      "price": "1.478495",
      "date": 1200355200,
      "ncs": "36830",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "184289",
      "ncl": "65581",
      "price": "1.464425",
      "date": 1200960000,
      "ncs": "41836",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "187780",
      "ncl": "62078",
      "price": "1.459195",
      "date": 1201564800,
      "ncs": "39622",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "199494",
      "ncl": "60106",
      "price": "1.479215",
      "date": 1202169600,
      "ncs": "47542",
      "unit": "Contracts Of EUR 125,000"
    },
    {
      "oi": "207601",
      "ncl": "55016",
      "price": "1.466445",
      "date": 1202774400,
      "ncs": "44721",
      "unit": "Contracts Of EUR 125,000"
    },


...

    {
      "oi": "218469",
      "ncl": "63355",
      "price": "1.3044125",
      "date": 1362441600,
      "ncs": "89471",
      "unit": "Contracts Of EUR 125,000"
    }
  ]
}
~~~

#### JSON レスポンス フィールド

レスポンスは一つのキー・値ペアで返信されます。　値はハッシュの配列です。　このモデルには期間のパラメータがないため、4年間分の全てのデータが一挙に返信されます。

oi
: グロスポジション

ncl
: 投機筋ではないロングポジション

price
: 為替レート

date
: unix timestamp形式の日時

ncs
: 投機筋ではないショートポジション

unit
: 各ポジション（グロス、ロング、ショート）における一枚あたりの単位

-----------------

## オーダーブック

OANDAの一年前までのオーダーブックデータを取得できます。　詳細は[OANDA FXラボ](http://fxtrade.oanda.ca/analysis/forex-order-book)をご覧ください。


    GET /labs/v1/orderbook_data


#### 入力クエリパラメータ

instrument
: _必須_ オーダーブックデータ取得対象の銘柄名。　取得可能銘柄: AUD_JPY, AUD_USD, EUR_AUD, EUR_CHF, EUR_GBP, EUR_JPY, EUR_USD, GBP_CHF, GBP_JPY, GBP_USD, NZD_USD, USD_CAD, USD_CHF, USD_JPY, XAU_USD, XAG_USD.

period
: _必須_ オーダーブックデータを取得する期間（秒単位）。 以下の有効な値のいずれかに合致しない値を設定した場合は、一番近い有効な値に自動的に修正されます。

	有効な値:

    * __3600__     - 1時間  　- 20分毎スナップショット
    * __21600__    - 6時間  　- 20分毎スナップショット
    * __86400__    - 1日   　 - 2時間毎スナップショット
    * __604800__   - 1週間  　- 1日2回(04:00 と 16:00 GMT)のスナップショット
    * __2592000__  - 1ヶ月 　 - 1日1回(16:00 GMT)のスナップショット
    * __31536000__ - 1年      - 1ヶ月１回、月初の日(12:00 GMT)のスナップショット

#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/orderbook_data?instrument=EUR_USD&period=3600" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
{
  "1382042401": {
    "price_points": {
      "1.359": {
        "os": 0.638,
        "ps": 0.2173,
        "pl": 0.67,
        "ol": 0.1535
      },
      "1.3365": {
        "os": 0.0512,
        "ps": 0.4346,
        "pl": 0.0905,
        "ol": 0.4435
      },
      "1.348": {
        "os": 0.0546,
        "ps": 1.8109,
        "pl": 0.1449,
        "ol": 0.3992
      },
      "1.4285": {
        "os": 0.0068,
        "ps": 0,
        "pl": 0,
        "ol": 0.0273
      },

...

      "1.335": {
        "os": 0.1126,
        "ps": 0.5433,
        "pl": 0.0362,
        "ol": 0.7779
      },
      "1.3705": {
        "os": 0.1126,
        "ps": 0,
        "pl": 0,
        "ol": 0.0614
      },
      "1.317": {
        "os": 0.0444,
        "ps": 0.1992,
        "pl": 0.0724,
        "ol": 0.5664
      }
    },
    "rate": 1.3676
  },
  "1382037600": {
    "price_points": {
      "1.359": {
        "os": 0.638,
        "ps": 0.2173,
        "pl": 0.67,
        "ol": 0.1535
      },
      "1.3365": {
        "os": 0.0512,
        "ps": 0.4346,
        "pl": 0.0905,
        "ol": 0.4435
      },
      "1.348": {
        "os": 0.0546,
        "ps": 1.8109,
        "pl": 0.1449,
        "ol": 0.3992
      },


...

      "1.381": {
        "os": 0.0614,
        "ps": 0,
        "pl": 0,
        "ol": 0.058
      },
      "1.335": {
        "os": 0.1126,
        "ps": 0.5433,
        "pl": 0.0362,
        "ol": 0.7779
      },
      "1.3705": {
        "os": 0.1126,
        "ps": 0,
        "pl": 0,
        "ol": 0.0614
      },
      "1.317": {
        "os": 0.0444,
        "ps": 0.1992,
        "pl": 0.0724,
        "ol": 0.5664
      }
    },
    "rate": 1.3677
  }
}
~~~

#### JSON レスポンス フィールド

レスポンスは複数のキー・値ペアで返信されます。　それぞれのキーはスナップショットのunix timestampです。　それぞれのスナップショットはプライスポイントとレートのハッシュです。　それぞれのプライスポイントは一つのキー・値ペアです。　レートはスナップショット時の市場レートです。

os
: ショート注文のパーセンテージ

ol
: ロング注文のパーセンテージ

ps
: ショートポジションのパーセンテージ

pl
: ロングポジションのパーセンテージ

rate entry
: その時刻における市場レート

-----------------

## Autochartistパターン

Autochartistからの'Our Favourites'シグナルを取得します。　これらのシグナルは、チャートパターンもしくはキーレベルです。

チャートパターンはパターンを構成する上下の線、パターンの終了時間、そして予想レートボックスから成り立っています。　

キーレベルはプライスレベルのみです。

#### 入力クエリパラメータ

以下のパラメータは全て任意です。　もしパラメータを何も設定しなかった場合は、現在の全ての'Our Favourites'パターンと、キーレベルが返信されます。　これらのパターンは１５分毎に更新されます。

instrument:
: _任意_ パターン取得対象の銘柄名。

period:
: _任意_ パターンを取得する期間（秒単位）。

	有効な値:

    * __14400__    - 4時間
    * __28800__    - 8時間
    * __43200__    - 12時間
    * __86400__    - 1日
    * __604800__   - 1週間

quality:
: _任意_ 検索するチャートパターンのクオリティ。

    有効な値: 1, 2, 3 .. 10

type:
: _任意_ 検索するパターンのタイプ。

    有効な値:

    * __chartpattern__ - ウェッジ、チャネルなど、チャートパターンのみを返信する
    * __keylevel__     - レジスタンス、サポートラインなど、キーレベルのみを返信する

direction:
: _任意_ 検索するパターンの方向。

    有効な値:

    * __bullish__
    * __bearish__


#### 例

    curl "https://api-fxpractice.oanda.com/labs/v1/signal/autochartist?instrument=EUR_CAD&period=3600" -H "Authorization: Bearer <access-token>"

#### レスポンス

###### ヘッダ

~~~
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 264
~~~

------------------

###### ボディ

~~~json
{
  "signals": [
    {
      "meta": {
        "completed": 1,
        "scores": {
          "uniformity": 1,
          "quality": 5,
          "breakout": 5,
          "initialtrend": 7,
          "clarity": 6
        },
        "probability": 63.09,
        "interval": 60,
        "direction": 1,
        "pattern": "Falling Wedge",
        "length": 35,
        "historicalstats": {
          "hourofday": {
            "total": 969,
            "percent": 55.93,
            "correct": 542
          },
          "pattern": {
            "total": 2917,
            "percent": 64.45,
            "correct": 1880
          },
          "symbol": {
            "total": 390,
            "percent": 64.36,
            "correct": 251
          }
        },
        "trendtype": "Continuation"
      },
      "id": 123456789,
      "instrument": "EUR_CAD",
      "type": "chartpattern",
      "data": {
        "patternendtime": 1412600400,
        "points": {
          "resistance": {
            "x0": 1412298000,
            "x1": 1412578800,
            "y0": 1.4146,
            "y1": 1.40981
          },
          "support": {
            "x0": 1412323200,
            "x1": 1412589600,
            "y0": 1.40567,
            "y1": 1.40452
          }
        },
        "prediction": {
          "timeto": 1412784000,
          "timefrom": 1412600400,
          "pricehigh": 1.4152,
          "pricelow": 1.4118
        }
      }
    }
  ],
  "provider": "autochartist"
}
~~~

#### JSON レスポンス フィールド

###### チャートパターンの場合

~~~json
{
  "signals": [
    {                                   // それぞれのエントリーはシグナルオブジェクトです
      "meta": {                         // シグナルのメタデータ・セクション
        "completed": 1,                 // シグナルがすでに完結している場合は1、今現れてきている場合は0
        "scores": {                     // このシグナルのクオリティに関するAutochartistの測定評価
          "uniformity": 1,              // 値は 1 .. 10
          "quality": 5,                 // 値は 1 .. 10
          "breakout": 5,                // 値は 1 .. 10
          "initialtrend": 7,            // 値は 1 .. 10
          "clarity": 6                  // 値は 1 .. 10
        },
        "probability": 63.09,           // このシグナルの成功の可能性。　最大値は 100
        "interval": 60,                 // このパターンにおけるキャンドルスティックの精度（間隔）。　分単位で、60は１時間。
        "direction": 1,                 // このパターンの方向。 1 は bullish、 -1 は bearish。
        "pattern": "Falling Wedge",     // パターンの名前
        "length": 35,                   // パターンを形成しているキャンドルの本数
        "historicalstats": {            // 時間帯、パターンタイプ、銘柄ベースのヒストリカルな統計
          "hourofday": {                // 時間帯ベースのヒストリカルな統計
            "total": 969,               // 時間帯ベースのヒストリカルな統計に関する総パターン数
            "percent": 55.93,           // 時間帯ベースの成功のパーセンテージ。　最大値は 100
            "correct": 542              // 時間帯ベースの正確な予測数
          },
          "pattern": {                  // パターンタイプベースのヒストリカルな統計
            "total": 2917,              // パターンタイプベースのヒストリカルな統計に関する総パターン数
            "percent": 64.45,           // パターンタイプベースの成功のパーセンテージ。　最大値は 100
            "correct": 1880             // パターンタイプベースの正確な予測数
          },
          "symbol": {                   // 銘柄ベースのヒストリカルな統計
            "total": 390,               // 銘柄ベースのヒストリカルな統計に関する総パターン数
            "percent": 64.36,           // 銘柄ベースの成功のパーセンテージ。　最大値は 100
            "correct": 251              // 銘柄ベースの正確な予測数
          }
        },
        "trendtype": "Continuation"     // トレンドのタイプ：　'Continuation' もしくは 'Reversal'
                                        // チャートパターンシグナルの場合にのみ設定されます。
      },
      "id": 423422911,                  // AutochartistパターンID
      "instrument": "EUR_CAD",          // 銘柄名
      "type": "chartpattern",           // パターンのタイプ：　'chartpattern' もしくは 'keylevel'
      "data": {
        "patternendtime": 1412600400,   // パターンの完了時刻のタイムスタンプ（unix timestamp形式）。
        "points": {
          "resistance": {               // パターンの抵抗線
            "x0": 1412298000,           // x0、 x1はunix timestamp形式
            "x1": 1412578800,           // y0、y1は価格
            "y0": 1.4146,
            "y1": 1.40981
          },
          "support": {                  //  パターンの支持線
            "x0": 1412323200,           // x0、 x1はunix timestamp形式
            "x1": 1412589600,           // y0、y1は価格
            "y0": 1.40567,
            "y1": 1.40452
          }
        },
        "prediction": {                 // 予測に関する情報。　パターンが完了している場合にのみ設定されます。
          "timefrom": 1412600400,       // 予測プライスボックスの最も早いunix timestamp形式のタイムスタンプ (左辺)
          "timeto": 1412784000,         // 予測プライスボックスの最も遅いunix timestamp形式のタイムスタンプ (右辺)
          "pricehigh": 1.4152,          // 予測プライスボックスの上辺
          "pricelow": 1.4118            // 予測プライスボックスの底辺
        }
      }
    }
  ],
  "provider": "autochartist"            // シグナルプロバイダ名
}

~~~

###### キーレベルの場合

~~~json
{
  "signals": [
    {                                   // それぞれのエントリーはシグナルオブジェクトです
      "meta": {                         // シグナルのメタデータ・セクション
        "completed": 1,                 // シグナルがすでに完結している場合は1、今現れてきている場合は0
        "scores": {                     // このシグナルのクオリティに関するAutochartistの測定評価
          "quality": 4                  // 値は 1 .. 10
        },
        "patterntype": "Approaching",   // パターンのタイプ：　'Approaching' もしくは 'Breakout'
        "probability": 80.3,            // このシグナルの成功の可能性。　最大値は 100
        "interval": 240,                // このパターンにおけるキャンドルスティックの精度（間隔）。　分単位で、240は6時間。

        "direction": 1,                 // このパターンの方向。 1 は bullish、 -1 は bearish。
        "pattern": "Resistance",        // パターンの名前
        "length": 128,                  // パターンを形成しているキャンドルの本数
        "historicalstats": {            // 時間帯、パターンタイプ、銘柄ベースのヒストリカルな統計（チャートパターンの場合、を参照してください）。
          "hourofday": {
            "total": 47,
            "percent": 70.21,
            "correct": 33
          },
          "pattern": {
            "total": 640,
            "percent": 81.56,
            "correct": 522
          },
          "symbol": {
            "total": 86,
            "percent": 82.56,
            "correct": 71
          }
        }
      },
      "id": 123456789,                  // AutochartistパターンID
      "instrument": "SPX500_USD",       // 銘柄名
      "type": "keylevel",               // パターンのタイプ：　'chartpattern' もしくは 'keylevel'
      "data": {
        "price": 2002.35,               // このキーレベルの価格
        "patternendtime": 1412352000,   // パターンの完了時刻のタイムスタンプ（unix timestamp形式）。
        "points": {
          "keytime": {                  // このパターンの最大10個までのunix timestamp形式のタイムスタンプ
            "1": 1409097600,            // この例では、4つのキータイムスタンプがあります。
            "2": 1411560000,
            "3": 1410494400,
            "4": 1409112000,
            "5": 0,
            "6": 0,
            "7": 0,
            "8": 0,
            "9": 0,
            "10": 0
          }
        },
        "prediction": {                 // 予測に関する情報。　パターンが完了している場合にのみ設定されます。
          "timefrom": 1412600400,       // 予測プライスボックスの最も早いunix timestamp形式のタイムスタンプ (左辺)
          "timeto": 1412784000,         // 予測プライスボックスの最も遅いunix timestamp形式のタイムスタンプ (右辺)
          "timebars": 56,               // パターン認識後、予想価格に到達するまでのキャンドルの本数
          "pricehigh": 2003.4152,       // 予測プライスボックスの上辺
          "pricelow": 2002.4118         // 予測プライスボックスの底辺
        }
      }
    }
}
~~~


