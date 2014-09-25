---
title: クイックスタートトレーディング | OANDA API
---

# トレーディングクイックスタートガイド

OANDAでトレードするためには、まずユーザーを作成する必要があります。　ユーザーはアカウントを所有しており、アカウントには貴方がトレードで使用する資金が保管されています。　トレードを行う場合、貴方がトレードで使用したい資金を保管しているアカウントを指定する必要があります。

[ユーザーとアカウントの作成](http://oanda.github.com/gen-account.html)を行うことにより、ユーザー名、パスワード、そしてアカウントIDを取得できます。

* アカウントIDはトレードを行ったり、トレードに関する情報を取得するためのあらゆるリクエストにおいて、必須パラメータです。

ユーザー名とパスワードは、ほとんどのケースにおいては必要ありません。　弊社のsandbox環境においては、ユーザーが少しでも簡単にトレードができるようにAPIは認証を行いません。　**唯一アカウントIDだけがトレードを行うために必要です。**

もしあなたが[自動的にテストアカウントの作成](http://oanda.github.com/gen-account.html)を望まない場合、curlもしくは別のHTTPクライアントで以下の手順を踏むことで作成することも可能です。

#### ステップ 1: 新しいユーザーを作成する
	curl -X POST -H "Content-Type: application/x-www-form-urlencoded" "http://api-sandbox.oanda.com/v1/accounts"

レスポンスの例:


~~~json
{
  "username" : "willymoth",
  "password" : "balvEdayg",
  "accountId" : 3563320
}
~~~
    
#### ステップ 2: アカウント情報を取得する
	curl -X GET "http://api-sandbox.oanda.com/v1/accounts/3563320"

レスポンスの例:

~~~json
{
  "accountId" : 3563320,
  "accountName" : "Primary",
  "balance" : 100000,
  "unrealizedPl" : 0,
  "realizedPl" : 0,
  "marginUsed" : 0,
  "marginAvail" : 100000,
  "openTrades" : 0,
  "openOrders" : 0,
  "marginRate" : 0.05,
  "accountCurrency" : "USD"
}
~~~

#### ステップ 3: トレーディングの開始
	curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "instrument=EUR_USD&units=1&side=buy" "http://api-sandbox.oanda.com/v1/accounts/3563320/trades"

レスポンスの例:

~~~json
{
  "ids" : [
    178690627
  ],
  "instrument" : "EUR_USD",
  "units" : 1,
  "price" : 1.28485,
  "marginUsed" : 0.0642,
  "side" : "buy"
}
~~~
