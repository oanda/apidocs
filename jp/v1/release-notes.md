---

title: リリースノート | OANDA API
---

# リリースノート

## 完全な更新履歴

完全な更新履歴については、[こちらをご覧ください](/docs/full-history.md)。

------------------------


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

## バージョン 1.3.1
- Sandboxに2014年6月30日にリリースされました
- fxTrade Practiceへリリース予定 
- fxTradeへリリース予定

##### 互換性に影響する変更点:

- "tick"オブジェクト内にストリーミングレートをラッピングしました―[詳細はこちら](https://fxtrade.oanda.com/community/forex-forum/topic/54007715/?page=3#post-9934445)。

-------------------------------------

## バージョン 1.3.0
- Sandboxに2014年6月27日にリリースされました
- fxTrade Practiceに2014年6月27日にリリースされました
- fxTradeへリリース予定

##### 新機能:

- HTTPイベントストリーミングをfxTrade PracticeとfxTradeに追加しました
- ユーザーがタイムスタンプを選択できるようX-Accept-Datetime-Format HTTPヘッダーに追加しました
  この機能は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54007925/)におけるコメントへの対応です。
- 一つのパーソナルアクセストークンで2つのアクティブHTTPストリーミングコネクションを利用できることになりました。
  この変更は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54008535/)におけるコメントへの対応です。
- ユーザーがそれぞれのHTTPストリーミングコネクションを識別できるようsessionIdパラメータを追加しました。
  これはsandbox環境ではサポートしていません。
  この変更は[こちらのディスカッション](https://fxtrade.oanda.com/community/forex-forum/topic/54007895/?page=1#post-9935825)におけるコメントへの対応です。


