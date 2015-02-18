---

title: リリースノート | OANDA API
---

# リリースノート

<!-- Template for adding new notes

## Version 1.1.0
- Released to Sandbox on Feb 21, 2014
- Released to fxTrade Practice on Feb 26, 2014
- Release to fxTrade pending  
<br/>

##### Compatibility Changes:

- None because we don't mess with that much

##### New Features:

- Modified the thing to do the stuff
- More modifications to the thing

##### Bug Fixes:

- Stopped the other thing from breaking on sundays
:
-------------------------------------

Template ends -->

## バージョン 1.3.7
- 2014年9月29日にリリースされました

##### 新機能:

- Autochartistからのシグナルを提供するため、新たに/labs/v1/signal/autochartist　エンドポイントを導入しました。

-------------------------------------

## バージョン 1.3.6
- 2014年9月26日にリリースされました

##### 互換性に影響する変更点:

- ORDER_UPDATEトランザクションレスポンスにorderIDフィールドを追加しました。
- 注文が最大オープンチケット数超過によりキャンセルされた場合、該当するORDER_CANCELトランザクションが /v1/transactions 及び /v1/events レスポンスにて送信されるようになりました。

##### バグ修正:

- /v1/instrumentリクエストに対して送信されるpip値の誤りを修正しました。　pip値が1.0の銘柄が、0.1として誤送信されていました。

-------------------------------------

## バージョン 1.3.5
- 2014年9月12日にリリースされました

##### 互換性に影響する変更点:

- イベントストリームは全てのトランザクションイベントを送信するようになりました。
　この機能は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54008795/)におけるコメントへの対応です。

##### 新機能:

- PATCHやDELETEメソッドなどをサポートしていないHTTPクライアントのために、X-HTTP-Method-Overrideヘッダを導入しました。

-------------------------------------

## バージョン 1.3.4
- 2014年8月15日にリリースされました

##### 新機能:

- キャンドルリクエストにおいて、キャンドルの区切りの時刻をどのタイムゾーンで指定するかを設定する　alignmentTimezone パラメータを導入しました。 
- /v1/instruments　エンドポイントに　interestRate フィールドを導入しました。 
- ユーザー認証においてクライアントサイドフローを導入しました。 
- 新しいエンドポイント /labs/v1/ の導入により、OANDA FXラボへのAPIアクセスの提供を開始しました。

-------------------------------------

## バージョン 1.3.3
- 2014年8月15日にリリースされました

##### 新機能:

- /v1/instruments　リクエストにおいて、ユーザーが取引停止中の銘柄を識別できるよう halted レスポンスフィールドを導入しました。

-------------------------------------

## バージョン 1.3.2
- 2014年7月25日にリリースされました

##### 新機能:

- トランザクションおよびストリームイベントのレスポンスにおけるORDER_FILLED、 STOP_LOSS_FILLED、 TAKE_PROFIT_FILLED そして TRAILING_STOP_FILLEDレコードにunitsフィールドを追加しました。
- /alltransactions レスポンスにおけるSTOP_LOSS_FILLED、 TAKE_PROFIT_FILLED そして TRAILING_STOP_FILLED レコードにtradeIdフィールドを追加しました。

-------------------------------------

## バージョン 1.3.1
- 2014年8月15日にリリースされました

##### 互換性に影響する変更点:

- "tick"オブジェクト内にストリーミングレートをラッピングしました―[詳細はこちら](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445)。

-------------------------------------

## バージョン 1.3.0
- 2014年7月4日にリリースされました

##### 新機能:

- HTTPイベントストリーミングをfxTrade PracticeとfxTradeに追加しました。
- ユーザーがタイムスタンプを選択できるようX-Accept-Datetime-Format HTTPヘッダーに追加しました。
  この機能は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54007925/)におけるコメントへの対応です。
- 一つのパーソナルアクセストークンで2つのアクティブHTTPストリーミングコネクションを利用できることになりました。
  この変更は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54008535/)におけるコメントへの対応です。
- ユーザーがそれぞれのHTTPストリーミングコネクションを識別できるようsessionIdパラメータを追加しました。
  これはsandbox環境ではサポートしていません。
  この変更は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54007895/?page=1#post-9935825)におけるコメントへの対応です。


