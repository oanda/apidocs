---
title: アカウント | OANDA API
---

# アカウントエンドポイント

* TOC
{:toc}

----------------------------

## ユーザーのアカウントを取得する

ユーザーの保持しているアカウントのリストを取得します

    GET /v1/accounts

#### 入力クエリパラメータ

username
: _任意_ ユーザーの名前。　注: このパラメータはsandbox環境でのみ必要です。　それ以外の環境ではアクセストークンが貴方を識別します。

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts?username=fxtrader"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 128
~~~

###### ボディ

~~~json
{
  "accounts": [
      {
        "accountId" : 8954947,
        "accountName" : "Primary",
        "accountCurrency" : "USD",
        "marginRate" : 0.05
      },
      {
        "accountId" : 8954950,
        "accountName" : "SweetHome",
        "accountCurrency" : "CAD",
        "marginRate" : 0.02
      }
  ]
}
~~~

----

## テストアカウントの作成
新しいアカウントを作成します。このリクエストはsandbox環境でのみ有効です。　その他の環境においてはfxtrade.oanda.comからアカウントを作成してください。

    POST /v1/accounts

#### 入力クエリパラメータ

currency
: _任意_ 新しいアカウントのデフォルト通貨ペア。


#### 例
    $curl -X POST "http://api-sandbox.oanda.com/v1/accounts"

#### レスポンス

###### ヘッダ

~~~header
HTTP/1.1 201 Created
Content-Type: application/json
Content-Length: 77
Location: https://api-sandbox.com/v1/accounts/8954947
~~~

###### ボディ

~~~json
{
  "username" : "keith",
  "password" : "Rocir~olf4",
  "accountId" : 8954947
}
~~~

----

## アカウント情報の取得

    GET /v1/accounts/:account_id

#### 例
    $curl -X GET "http://api-sandbox.oanda.com/v1/accounts/8954947"

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
  "accountId" : 8954947,
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
