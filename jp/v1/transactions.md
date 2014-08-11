---
title: トランザクション | OANDA API
---

# トランザクションエンドポイント

* TOC
{:toc}

----------------------------

## トランザクションの履歴を取得する

    GET /v1/accounts/:account_id/transactions

#### 入力クエリパラメータ

maxId
: _任意_ 取得する最初のトランザクションのID。　サーバーは、このIDとそれ以下のトランザクションを降順で返信します。 

minId
: _任意_ 取得する最後のトランザクションのID。　サーバーは、このIDとそれ以上のトランザクションを降順で返信します。

count
: _任意_ 取得するトランザクションの最大数。　設定できる最大値は５００です。　もしこのパラメータが設定されなかった場合は、最大50件のトランザクションを取得します。
             __注__: このパラメータが設定されたトランザクションの履歴リクエストの送信については、60秒毎に1件という送信制限があります。

instrument
: _任意_ 特定の銘柄に関するトランザクションのみを取得します。　デフォルトは全て（all）です。

ids
: _任意_ URLエンコードされ(*%2C*)、コンマで区切られた取得対象のトランザクションIDのリスト。　リストに設定できるIDの最大数は50です。　このパラメータが設定された場合は、他のパラメータは設定できません。

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&count=1"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 196
X-Result-Count: 50 
~~~

###### ボディ

~~~json
{
  "transactions" : [
    {
      "id" : 175427739,
      "accountId" : 6531071,
      "time" : "2014-02-12T21:16:46Z",
      "type" : "ORDER_CANCEL",
      "orderId" : 175427728,
      "reason" : "CLIENT_REQUEST"
    }
  ]
}
~~~

#### レスポンスのパラメータ

アカウントに対するイベントの種類によって、返信されるトランザクションのタイプが異なります。　それぞれのトランザクションのタイプによって、設定されるフィールドが異なります。　以下のフィールドは最も一般的なフィールドです:

id
: トランザクションID

accountId
: ユーザーのアカウントID

time
: 有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時

type
: 有効な値:

~~~ 
MARKET_ORDER_CREATE , STOP_ORDER_CREATE, LIMIT_ORDER_CREATE, MARKET_IF_TOUCHED_ORDER_CREATE,
ORDER_UPDATE, ORDER_CANCEL, ORDER_FILLED, TRADE_UPDATE, TRADE_CLOSE, MIGRATE_TRADE_OPEN,
MIGRATE_TRADE_CLOSE, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED, TRAILING_STOP_FILLED, MARGIN_CALL_ENTER,
MARGIN_CALL_EXIT, MARGIN_CLOSEOUT, SET_MARGIN_RATE, TRANSFER_FUNDS, DAILY_INTEREST, FEE
~~~

instrument
: 銘柄名

side
: アカウントに対するアクションの売買区別（buy=買い、sell=売り）

units
: 通貨単位

price
: 注文価格もしくは約定価格

lowerBound
: 成立下限価格

upperBound
: 成立上限価格

takeProfitPrice
: テイクプロフィット価格

stopLossPrice
: ストップロス価格

trailingStopLossDistance
: トレーリングストップのディスタンス（pipsで小数第一位まで）

pl
: P/L（利益または損失）

interest
: スワップ金利の積み立て利益

accountBalance
: 本イベント後の口座残高


---------------------------

## トランザクションのタイプとそれぞれのパラメータ

##### MARKET_ORDER_CREATE
このタイプのトランザクションは、ユーザーが指定単位の特定の銘柄を現在の市場レートで取引することに成功した時に作成されます。

必須フィールド
: id, accountId, time, type, instrument, side, units, price, pl, interest, accountBalance

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

###### チケット情報
MARKET_ORDER_CREATEタイプのトランザクションは、関連するチケット情報も含んでいます。

tradeOpened
: もし新しいチケットが作成された場合このオブジェクトがjsonレスポンスに付与されます。　チケットに関連するフィールドは: id、 unitsです。

tradeReduced
: もしチケットが決済されたり、反対売買により減少した場合に、このオブジェクトがjsonレスポンスに付与されます。　チケットに関連するフィールドは: id、 units、 pl、 interestです。

~~~json
{
  "id" : 176403879,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:31:05Z",
  "type" : "MARKET_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1.25325,
  "pl" : 0,
  "interest" : 0,
  "accountBalance" : 100000,
  "tradeOpened" : {
    "id" : 176403879,
    "units" : 2
  }
}
~~~


##### STOP_ORDER_CREATE
このタイプのトランザクションは、ユーザーが自分の口座に対するストップ注文の発注に成功した時に作成されます。
ストップ注文は、銘柄の市場レートが指定された価格と同じか不利になった場合に、銘柄を指定単位売買する注文です。

必須フィールド
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403886,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:20:05Z",
  "type" : "STOP_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### LIMIT_ORDER_CREATE
このタイプのトランザクションは、ユーザーが自分の口座に対するリミット注文の発注に成功した時に作成されます。
リミット注文は、銘柄の市場レートが指定された価格と同じか有利になった場合に、銘柄を指定価格で指定単位売買する注文です。

必須フィールド
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403880,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:34:32Z",
  "type" : "LIMIT_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### MARKET_IF_TOUCHED_ORDER_CREATE
このタイプのトランザクションは、ユーザーが自分の口座に対するマーケット・イフ・タッチド注文の発注に成功した時に作成されます。
マーケット・イフ・タッチド注文は、銘柄の市場レートが指定された価格と同じかその価格を超えた場合に、銘柄を成行で指定単位売買する注文です。
この注文は、以前は"OANDAリミット・オーダー"という名前でした。

必須フィールド
: id, accountId, time, type, instrument, side, units, price, expiry, reason (CLIENT_REQUEST, MIGRATION)

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403882,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:40:42Z",
  "type" : "MARKET_IF_TOUCHED_ORDER_CREATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### ORDER_UPDATE
このタイプのトランザクションは、ユーザーが自分の口座に対するリミット注文、ストップ注文、マーケット・イフ・タッチド注文の変更に成功した時に作成されます。

必須フィールド
: id, accountId, time, type, instrument, units, price, reason (REPLACES_ORDER)

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403883,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:07:13Z",
  "type" : "ORDER_UPDATE",
  "instrument" : "EUR_USD",
  "units" : 3,
  "price" : 1,
  "expiry" : 1398902400,
  "reason" : "REPLACES_ORDER"
}
~~~


##### ORDER_CANCEL
このタイプのトランザクションは、注文が以下の理由でクローズされる際に作成されます:
CLIENT_REQUEST, TIME_IN_FORCE_EXPIRED, ORDER_FILLED, MIGRATION.
注文の約定は以下の理由で無効になる可能性があります:
INSUFFICIENT_MARGIN, BOUNDS_VIOLATION, UNITS_VIOLATION, STOP_LOSS_VIOLATION, TAKE_PROFIT_VIOLATION, TRAILING_STOP_VIOLATION,
MARKET_HALTED, ACCOUNT_NON_TRADABLE, NO_NEW_POSITION_ALLOWED, INSUFFICIENT_LIQUIDITY.

必須フィールド
: id, accountId, time, type, orderId, reason

~~~json
{
  "id" : 176403881,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:37:27Z",
  "type" : "ORDER_CANCEL",
  "orderId" : 176403880,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### ORDER_FILLED
このタイプのトランザクションは、注文が約定した時に作成されます。

必須フィールド
: id, accountId, time, type, instrument, side, price, pl, interest, accountBalance, orderId

任意フィールド
: lowerBound, upperBound, takeProfitPrice, stopLossPrice, trailingStopLossDistance

###### チケット情報
ORDER_FILLEDタイプのトランザクションは、関連するチケット情報も含んでいます。

tradeOpened
: もし新しいチケットが作成された場合このオブジェクトがjsonレスポンスに付与されます。　チケットに関連するフィールドは: id、 unitsです。

tradeReduced
: もしチケットが決済されたり、反対売買により減少した場合に、このオブジェクトがjsonレスポンスに付与されます。　チケットに関連するフィールドは: id、 units、 pl、 interestです。

~~~json
{
  "id" : 175685908,
  "accountId" : 2610411,
  "time" : "2014-04-14T20:32:34Z",
  "type" : "ORDER_FILLED",
  "instrument" : "EUR_USD",
  "side" : "buy",
  "price" : 1.3821,
  "pl" : 0,
  "interest" : 0,
  "accountBalance" : 100000,
  "orderId" : 175685907,
  "tradeOpened" : {
          "id" : 175685908,
          "units" : 2
  }
}
~~~


##### TRADE_UPDATE
このタイプのトランザクションは、ユーザーがチケットの更新に成功した時に作成されます。

必須フィールド
: id, accountId, time, type, instrument, units, side, tradeId

任意フィールド
: takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 176403884,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:09:38Z",
  "type" : "TRADE_UPDATE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "",
  "stopLossPrice" : 1.1,
  "tradeId" : 176403879
}
~~~


##### TRADE_CLOSE
このタイプのトランザクションは、ユーザーがチケットの決済に成功した時に作成されます。

必須フィールド
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403885,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### MIGRATE_TRADE_CLOSE
このタイプのトランザクションは、ユーザーの別のDivisionへの移動によりチケットが決済された場合に作成されます。

必須フィールド
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403885,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "MIGRATE_TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### MIGRATE_TRADE_OPEN
このタイプのトランザクションは、ユーザーの別のDivisionへの移動後チケットが再度オープンされた場合に作成されます。

必須フィールド
: id, accountId, time, type, instrument, side, units, price

任意フィールド
: takeProfitPrice, stopLossPrice, trailingStopLossDistance

~~~json
{
  "id" : 175685908,
  "accountId" : 2610411,
  "time" : "2014-04-14T20:32:34Z",
  "type" : "MIGRATE_TRADE_OPEN",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "buy",
  "price" : 1.3821,
  "tradeOpened" : {
          "id" : 175685908,
          "units" : 2
  }
}
~~~


###### チケット情報
MIGRATE_TRADE_OPENタイプのトランザクションは、関連するチケット情報も含んでいます。

tradeOpened
: このオブジェクトがjsonレスポンスに付与されます。　チケットに関連するフィールドは: id、 unitsです。


##### TAKE_PROFIT_FILLED
このタイプのトランザクションは、ユーザーのアカウントに対するテイクプロフィット注文が約定した時に作成されます。

必須フィールド
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175685954,
  "accountId" : 1491998,
  "time" : "2014-04-15T14:12:47Z",
  "type" : "TAKE_PROFIT_FILLED",
  "tradeId" : 175685930,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.38231,
  "pl" : 0.0001,
  "interest" : 0,
  "accountBalance" : 100000.0001
}
~~~


##### STOP_LOSS_FILLED
このタイプのトランザクションは、ユーザーのアカウントに対するストップロス注文が約定した時に作成されます。

必須フィールド
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175685918,
  "accountId" : 1403479,
  "time" : "2014-04-14T20:41:41Z",
  "type" : "STOP_LOSS_FILLED",
  "tradeId" : 175685917,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.3821,
  "pl" : -0.0003,
  "interest" : 0,
  "accountBalance" : 99999.9997
}
~~~


##### TRAILING_STOP_FILLED
このタイプのトランザクションは、ユーザーのアカウントに対するトレーリングストップロス注文が約定した時に作成されます。

必須フィールド
: id, accountId, time, type, tradeId, instrument, units, side, price, pl, interest, accountBalance

~~~json
{
  "id" : 175739353,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "TRAILING_STOP_FILLED",
  "tradeId" : 175739352,
  "instrument" : "EUR_USD",
  "side" : "sell",
  "price" : 1.38137,
  "pl" : -0.0009,
  "interest" : 0,
  "accountBalance" : 99999.9992
}
~~~


##### MARGIN_CALL_ENTER
このタイプのトランザクションは、ユーザーのアカウントに対してマージンコール（追証）が発生中の時に作成されます。

必須フィールド
: id, accountId, time, type

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "MARGIN_CALL_ENTER"
}
~~~


##### MARGIN_CALL_EXIT
このタイプのトランザクションは、ユーザーのアカウントに対してのマージンコール（追証）が解消した時に作成されます。

必須フィールド
: id, accountId, time, type

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "MARGIN_CALL_EXIT"
}
~~~


##### MARGIN_CLOSEOUT
このタイプのトランザクションは、ユーザーのアカウントにおける清算証拠金が必要証拠金の50％以下になった場合にシステムによって自動的に作成されます。 
MARGIN_CLOSEOUTは、全ての決済注文をキャンセルし、ポジションの反対売買を行うことを意味します。 

必須フィールド
: id, accountId, time, type, instrument, units, side, price, pl, interest, accountBalance, tradeId

~~~json
{
  "id" : 176403889,
  "accountId" : 6765103,
  "time" : "2014-04-07T19:11:14Z",
  "type" : "MARGIN_CLOSEOUT",
  "instrument" : "EUR_USD",
  "units" : 2,
  "side" : "sell",
  "price" : 1.25918,
  "pl" : 0.0119,
  "interest" : 0,
  "accountBalance" : 100000.0119,
  "tradeId" : 176403879
}
~~~


##### SET_MARGIN_RATE
A transaction of this type is created whenever user modifies margin rate for the account.

必須フィールド
: id, accountId, time, type, marginRate

~~~json
{
  "id" : 175739360,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "SET_MARGIN_RATE",
  "rate" : 0.02
}
~~~


##### TRANSFER_FUNDS
このタイプのトランザクションは、ユーザーがアカウントに対して入出金をした場合に作成されます。
入金の場合は金額は正の数字となり、出金の場合は負の数字となります。 

必須フィールド
: id, accountId, time, type, amount, accountBalance, reason (CLIENT_REQUEST, ADJUSTMENT, MIGRATION)

~~~json
{
  "id" : 176403878,
  "accountId" : 6765103,
  "time" : "2014-04-07T18:29:25Z",
  "type" : "TRANSFER_FUNDS",
  "amount" : 100000,
  "accountBalance" : 100000,
  "reason" : "CLIENT_REQUEST"
}
~~~


##### DAILY_INTEREST
このタイプのトランザクションは、アカウントに対してその日のポジションと口座残高による金利の調整（入金、出金）を行った場合にシステムにより自動的に作成されます。
前日に残高もしくは未決済のポジションがあるアカウントに対して米国東部標準時午後4時に毎日自動的に作成されます。
このトランザクション自体は毎日必ずしも午後4時きっかりに作成されるとは限りませんが、金利の入出金は前日の午後4時の時点のポジションと残高をベースに計算されます。

必須フィールド
: id, accountId, time, type, instrument, interest, accountBalance

~~~json
{
  "id" : 175739363,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "DAILY_INTEREST",
  "instrument" : "EUR_USD",
  "interest" : 10.0414,
  "accountBalance" : 99999.9992
}
~~~


##### FEE
このタイプのトランザクションは、何らかの料金を徴収する必要がある場合にシステムにより自動的に作成されます。

必須フィールド
: id, accountId, time, type, amount, accountBalance, reason(FUNDS, CURRENSEE_MONTHLY, CURRENSEE_PERFORMANCE, SDR_REPORTING)

~~~json
{
  "id" : 175739369,
  "accountId" : 1491998,
  "time" : "2014-04-15T15:21:21Z",
  "type" : "FEE",
  "amount" : -10.0414,
  "accountBalance" : 99999.9992,
  "reason" : "FUNDS"
}
~~~


#### Pagination（ページ割り）

トランザクションはcountとmaxIdパラメータによりページ割りができます。
一つのクエリで最大50件のトランザクションが返信されます。 
もしトランザクションの件数がデフォルトもしくは指定された件数を上回っていた場合は、Linkヘッダと一緒にmaxIdが次のまだ未送信のトランザクションのIDに設定されたURLが返信されます。

------------------------------

## トランザクションに関する情報を取得する

    GET /v1/accounts/:account_id/transactions/:transaction_id

#### 例
    curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/transactions/1170980"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 269
~~~

###### ボディ

~~~json
{
  "id" : 1170980,
  "accountId" : 12345,
  "time" : "2014-02-12T20:02:45Z",
  "type" : "TRADE_CLOSE",
  "instrument" : "EUR_USD",
  "units" : 1000,
  "side" : "sell",
  "price" : 1.35921,
  "pl" : 1.93,
  "interest" : 0,
  "accountBalance" : 1000026.0723,
  "tradeId" : 175427703 //Closed trade id
}
~~~

----------------------------

## アカウントの完全な履歴を取得する

指定のアカウントのトランザクションの完全な履歴をリクエストできます。
リクエストが成功した場合、`Location`ヘッダにリクエスト完了後に取得可能なファイルのURLがレスポンスに含まれます。
ファイルの作成が完了するまでは、URLへのアクセスに対してはHTTP 404のレスポンスとなります。　作成完了後、URLは一定時間アクセス可能です。

    GET /v1/accounts/:account_id/alltransactions

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/12345/alltransactions"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 202 Accepted
Content-Type: application/json
Content-Length: 0
Location: https://fxtrade.oanda.com/53e58d3509cef65f33998c879dcccbd9.json.zip
~~~

