---
title: ストリーミング | OANDA API
---

# ストリーミング エンドポイント

* TOC
{:toc}

----------------------------

## ストリーミング

OANDA APIの一環として、弊社はOANDA REST APIの他に選択肢としてリアルタイムデータストリーミング接続をお客様のために提供しています。

ストリーミングAPIは、HTTP 1.1のTransfer-Encoding: chunkedメカニズムに準拠しています。　全てのストリーミング接続は認証が必要です。

-----------------

## レートストリーミング

特定の銘柄に対するリアルタイム相場レートを受信するためのストリーミング接続をオープンします。


    GET /v1/prices


#### 入力クエリパラメータ

accountId
: _必須_ アカウントID

instruments
: _必須_ URLエンコードされ(*%2C*)、コンマで区切られたレート取得対象の銘柄のリスト。 


#### 例

    curl "http://stream-sandbox.oanda.com/v1/prices?accountId=12345&instruments=AUD_CAD%2CAUD_CHF"

#### レスポンス

##### ヘッダ

~~~Header
Transfer-Encoding: chunked
~~~

------------------

### ボディ (ストリーム)

**注:** 本マニュアルではtick情報は"tick"オブジェクトにラップされています。　これはsandbox環境に適用された変更で、fxPractice及びfxTrade環境にも今後のリリースで追加予定です(リリース時期は後日公表されます)。　 この段階的なリリースにより、tickフォーマットが変更になる前にソースコードの変更をテストすることが可能になります。　現在fxTrade及びfxPractice環境では、tickはラップされておらず、以下のようなフォーマットとなっています。
{: style="color:red"}

~~~json
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.855887Z","bid":0.79357,"ask":0.79390}
~~~

ストリームに書き込まれる全てのデータはJSONフォーマットでエンコードされています。　最初に受信するデータは配信登録をした銘柄のレートのスナップショットです。　その後は、新しいレートが発生する毎にレートがストリームに書き込まれます。　HTTP接続が切断されないように、ストリームにはハートビートも書き込まれます。

~~~json
{"tick":{"instrument":"AUD_CAD","time":"2014-01-30T20:47:08.066398Z","bid":0.98114,"ask":0.98139}}
{"tick":{"instrument":"AUD_CHF","time":"2014-01-30T20:47:08.053811Z","bid":0.79353,"ask":0.79382}}
{"tick":{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.493511Z","bid":0.79355,"ask":0.79387}}
{"heartbeat":{"time":"2014-01-30T20:47:11.543511Z"}}
{"tick":{"instrument":"AUD_CHF","time":"2014-01-30T20:47:11.855887Z","bid":0.79357,"ask":0.79390}}
{"tick":{"instrument":"AUD_CAD","time":"2014-01-30T20:47:14.066398Z","bid":0.98112,"ask":0.98138}}
~~~

#### JSONレスポンスフィールド

##### tick:

###### instrument
{: .indent}
銘柄名
{: .double-indent}

###### time
{: .indent}
有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時
{: .double-indent}###### bid
{: .indent}
Bid（買い）価格
{: .double-indent}

###### ask
{: .indent}
Ask（売り）価格
{: .double-indent}


##### heartbeat: 

###### time
{: .indent}
有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時
{: .double-indent}

-----------------------

## イベントストリーミング

アクセス権のあるアカウントに関するリアルタイムイベントを受信するストリーミング接続をオープンします。


    GET /v1/events

-----------------


#### 入力クエリパラメータ

accountIds
: _任意_ URLエンコードされ(*%2C*)、コンマで区切られたイベントを受信したいアカウントIDのリスト。　リストが設定されなかった場合は、当該トークンでアクセス権のある全てのアカウントに対して受信登録します。
注: sandbox環境ではこのパラメータは*必須*です。


#### 例

    curl "http://stream-sandbox.oanda.com/v1/events?accountIds=12345%2C6789"

#### レスポンス

##### ヘッダ

~~~Header
Transfer-Encoding: chunked
~~~

------------------

### ボディ (ストリーム)

ストリームに書き込まれる全てのデータはJSONフォーマットでエンコードされています。
ストリームに書き込まれるイベントは、HTTP接続をアクティブに保つためのハートビート(15秒間隔)か、もしくは以下のイベントのいずれかが起こったことをレポートするトランザクションです:
マージンカット（強制決済）、注文の約定、システムによる注文の取り消し、テイクプロフィット・ストップロス・トレーリングストップ注文の約定、マージンコール（追証）の発生・解消。


~~~json
{"heartbeat":{"time":"2014-05-26T13:58:40Z"}}
{"transaction":{"id":10001,"accountId":12345,"time":"2014-05-26T13:58:41.000000Z","type":"MARGIN_CLOSEOUT","instrument":"EUR_USD","units":10,"side":"sell","price":1,"pl":1.234,"interest":0.034,"accountBalance":10000,"tradeId":1359}}
{"transaction":{"id":10002,"accountId":12345,"time":"2014-05-26T13:58:45.000000Z","type":"ORDER_FILLED","instrument":"EUR_USD","side":"sell","price":1,"pl":1.234,"interest":0.034,"accountBalance":10000,"orderId":0,"tradeReduced":{"id":54321,"units":10,"pl":1.234,"interest":0.034}}}
~~~

#### JSONレスポンスフィールド


##### transaction:

###### id
{: .indent}
トランザクションID
{: .double-indent}

###### accountId
{: .indent}
アカウントID
{: .double-indent}

###### time
{: .indent}
有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時
{: .double-indent}

###### type
{: .indent}
トランザクションのタイプ。　設定される値は次のどれかです: ORDER_FILLED, STOP_LOSS_FILLED, TAKE_PROFIT_FILLED, TRAILING_STOP_FILLED, MARGIN_CLOSEOUT, ORDER_CANCEL, MARGIN_CALL_ENTER, MARGIN_CALL_EXIT
{: .double-indent}

###### instrument
{: .indent}
銘柄名
{: .double-indent}

###### side
{: .indent}
売買区別（buy=買い、sell=売り）
{: .double-indent}

###### units
{: .indent}
通貨単位
{: .double-indent}

###### price
{: .indent}
注文価格もしくは約定価格
{: .double-indent}

###### lowerBound
{: .indent}
成立下限価格
{: .double-indent}

###### upperBound
{: .indent}
成立上限価格
{: .double-indent}

###### takeProfitPrice
{: .indent}
テイクプロフィット価格
{: .double-indent}

###### stopLossPrice
{: .indent}
ストップロス価格
{: .double-indent}

###### trailingStopLossDistance
{: .indent}
トレーリングストップのディスタンス（pipsで小数第一位まで）
{: .double-indent}

###### pl
{: .indent}
P/L（利益または損失）
{: .double-indent}

###### interest
{: .indent}
スワップ金利の積み立て利益
{: .double-indent}

###### accountBalance
{: .indent}
本イベント後の口座残高
{: .double-indent}

###### tradeId
{: .indent}
新規もしくは決済された取引のID
{: .double-indent}

###### orderId
{: .indent}
約定した注文のID
{: .double-indent}

###### tradeOpened
{: .indent}
もし新規取引が成立した場合このオブジェクトがjsonレスポンスに付与されます。　取引に関連するフィールドは: id、 unitsです。
{: .double-indent}

###### tradeReduced
{: .indent}
もし取引が決済されたり、反対売買によりポジションが減少した場合に、このオブジェクトがjsonレスポンスに付与されます。　取引に関連するフィールドは: id、 units、 pl、 interestです。
{: .double-indent}

##### heartbeat:

###### time
{: .indent}
有効な[日時フォーマット](/docs/jp/v1/guide/#datetime-format)による日時
{: .double-indent}

-----------------------

### 接続数制限

#### レートストリーミング

* アクセストークン毎に一つのアクティブレートストリーム接続が可能です。

#### イベントストリーミング

* *Sandbox*: IPアドレス毎の最大接続数は５です。
* *Production environment*: アクセストークン毎に5接続まで可能です。

接続制限を超過した場合、OANDAサーバーは以下のアクションを行います:

*Sandbox*: 当該新規接続要求をステータスコード429でreject（拒否）します。

*Production environment*: 一番古い接続を切断し、そのかわりに新規接続要求による新しい接続を確立します。


### 接続

#### 切断

OANDAは以下のような状況においては、既存のアクティブな接続を切断します。

* OANDAのインフラ整備によるシステムダウンの時間。　整備の時間中にサーバー側の各システムは停止され、アップグレードされます。
* 特定のアクセストークンに許可されたアクティブな接続数の上限を超えた場合。　当該アクセストークンに関連する一番古い接続が切断されます。　切断される接続に対してdisconnect（切断）メッセージが送信されます。

~~~json
{"disconnect":{"code":60,"message":"Access Token connection limit exceeded: This connection will now be disconnected","moreInfo":"http:\/\/developer.oanda.com\/docs\/v1\/troubleshooting"}}
~~~

#### 配信停止

以下のような状況が起こった場合、アプリケーションは当該ストリーム接続を切断し、再接続することを推奨します:

* 10秒以上レートストリームから何のデータ(ティック、ハートビート)も受信しない場合。
* 20秒以上イベントストリームから何のデータ(イベント、ハートビート)も受信しない場合。

#### 再接続

再接続に関してその頻度を制限するメカニズムが導入されています。　この制限を超えた頻度で再接続を行うと、HTTP 429エラーレスポンスを受信します。

そのためアプリケーションは再接続を試みるにあたって、backoffの実装をすることを推奨します。　実装は[exponential backoff](http://en.wikipedia.org/wiki/Exponential_backoff)のような方法で可能です。

* 例えば、もし再接続要求に対してHTTPエラーを受信した場合、次の再接続要求を行う前に1秒間backoff（待機）します。　それでもエラーの場合は、再接続要求間のbackoffの間隔を倍々に増やしていき、最終的に接続が確立できるまで繰り返します。

