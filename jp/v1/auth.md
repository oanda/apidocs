---
title: ユーザー認証 | OANDA API
---

# ユーザー認証

* TOC
{:toc}

----------------------------

## 概要

ユーザー認証は弊社のサンドボックスシステム(http://api-sandbox.oanda.com)では無効となっています。  ユーザー認証に必要な情報、セッショントークン、OAuthなどを気にする必要はありません。　必要なリクエストを実行し、APIを自由にご利用ください。

貴方のライブアカウントにアクセスするには、ユーザ認証が必要です。 アプリケーション開発者は[OAuth 2.0フロー](#third-party-applications)が必要であり, パーソナルトレーダーはパーソナルアクセストークンをリクエストできます。

----------------

## パーソナルストラテジートレーダー

OANDA Open APIに貴方のアカウントを通じてアクセスするためにはパーソナルアクセストークンが必要です。　一度作成されると、トークンは貴方の全てのサブアカウントへのアクセスを可能にします。　貴方のパーソナルアクセストークンはパスワードのようなものですので、注意深く保管してください。　これらのトークンはOANDAのアカウント独自のもので、安全に保管することが重要です。

#### パーソナルアクセストークンの取得

貴方のOANDA fxTradeアカウントプロファイルページには"Manage API Access"というタイトルのリンクがあります(My Account -> My Services -> Manage API Access)。 こちらからOANDA Open APIを利用するためのパーソナルアクセストークンを新規に作成したり、既存のトークンを無効にすることができます。

#### パーソナルアクセストークンの使用

貴方のトークンを作成後、安全な場所に保管してください。　弊社は貴方のトークンを保管していませんので、もしトークンを紛失か保管場所を失念してしまった場合は、APIを継続して利用するために、そのトークンを無効化し、新しいトークンを作成する必要があります。

トークンを使用してAPIリソースにアクセスするためには、HTTPのAuthorizationヘッダにトークンをBearer Tokenとして設定する必要があります。例えば：

~~~
curl -H "Authorization: Bearer 12345678900987654321-abc34135acde13f13530"
    https://api-fxpractice.oanda.com/v1/accounts
~~~


----------------

## サードパーティーアプリケーション

弊社はOANDAユーザーがウェブベースのサードパーティアプリケーションを通じてOANDA APIにアクセスすることをサポートしています。　OANDAのAPIは[OAuth 2.0 プロトコル](http://tools.ietf.org/html/draft-ietf-oauth-v2-31)を利用してこの機能を提供しています。　サーバーサイドフローを完了して必要なアクセストークンを取得することはサードパーティアプリケーションの責任となります。

取得後は、アプリケーションはアクセストークンを[パーソナルアクセストークンの使用](#using-a-personal-access-token)と同じ方法で使用することができます。

### アプリケーションの登録

アプリケーションをOANDAに登録するためにはapi@oanda.comにご連絡ください。　電子メールの本文に貴方のアプリケーションをOANDA APIに登録したいと明記した上で以下の情報をご提示ください：

* アプリケーションの名前
* アプリケーションの説明
* 正当なリダイレクトURI （HTTPリダイレクトURIはTLSセキュリティによって保護されていなくてはなりません）

全ての必要な情報がOANDAに受理され承認された時点で、以下の情報が提供されます。 

* クライアントアプリケーションID
* クライアントアプリケーションシークレット

クライアントアプリケーションシークレットはパスワードとして取扱い、安全な場所に保管してください。

*注意：OANDAは貴方のアプリケーションが受理されることを保証はいたしません。　全てのアプリケーションは弊社の調査レビューの対象となります。　もし貴方のアプリケーションがOANDAに受理された場合は、OANDAの諸条件に従う義務があることをご理解ください。
  
### サーバーサイドフロー

アクセストークンを取得するには3つの段階があります。


1. [ユーザーにOANDA OAuth authorization endpointへアクセスさせてください。　ユーザーはOANDAへのログインを要求され、貴方のアプリケーションがユーザーのアカウントにアクセスする許可を与えます。](#step-1-direct-the-users-browser-to-oandas-authorization-endpoint)  

2. [１のステップ完了後、OANDAサーバーはユーザーを貴方のアプリケーションの登録済みリダイレクトURIにリダイレクトします。 １のステップが成功した場合は、OANDAはユニークなauthorization codeをリダイレクトリクエストに含みます。](#step-2-receive-redirect-from-oanda)

3. [アプリケーションはOANDAのアクセストークンendpointに対してリクエストを行いauthorization codeと引き換えにアクセストークンを取得します。　アプリケーションは、アクセストークンを使用してユーザーの身代わりとして各種オペレーションを行います。](#step-3-exchange-authorization-code-for-access-token)  


####サブドメイン

リクエストのサブドメインはどの環境に対してのアクセストークンを貴方が希望するかにより異なります。

|環境|サブドメイン|
|---|---|
|fxTrade Practice|api-fxpractice.oanda.com|
|fxTrade|api-fxtrade.oanda.com|


####ステップ 1: ユーザーのブラウザをOANDAのauthorization endpointにディレクトする。

~~~
GET /v1/oauth2/authorize
~~~  


#####リクエストクエリパラメータ  

client_id
: OANDAにアプリケーションを登録した際に取得したクライアントアプリケーションID。

redirect_uri
: リダイレクトURIはアプリケーションの登録時のものと完全に一致する必要があります。

response_type
: サーバサイドフローをリクエストするため、**code**を指定してください。

state
: リクエストとコールバック間のアプリケーションの状態（ステート）を保持するためのユニークなトークン。　OANDAからのリダイレクトレスポンスに、このパラメーターとトークンの値が含まれます。　貴方のアプリケーションはこの返り値のトークンが、もともと指定したトークンと同じ値であるかどうか確認する必要があります。　弊社はこのトークンをハイクォリティのランダム数値ジェネレーターで生成することを推奨します。

scope
: アプリケーションが必要なパーミッションのリスト。　パーミッションは'+'記号によって区切られます。 全てのパーミッションとその詳細については[ここ](#permissions)を参照してください。
  
#####例

~~~
https://api-fxpractice.oanda.com/v1/oauth2/authorize?client_id=CLIENT_ID&redirect_uri=https://oanda-oauth-example.com/acceptcode&state=STATE_TOKEN&response_type=code&scope=read+trade+marketdata+stream
~~~


####ステップ 2: OANDAからリダイレクトを受信する。

もしユーザーが貴方のアプリケーションによるアクセスを許可した場合、ユーザーは`redirect_uri`に以下のパラメーターを付与された上でリダイレクトされます。

#####リダイレクトクエリパラメータ

state
: リクエスト時にアプリケーションによって設定されたユニークなトークン。　アプリケーションは、次のステップに進む前に必ずこのトークンが元のトークンと一致しているかを確認する必要があります。

code
: 次のステップでアクセストークンと交換される、一時的に使用される`authorization code`。

#####例
~~~
  https://oanda-oauth-example.com/acceptcode?state=STATE_TOKEN&code=AUTH_CODE
~~~  

もし貴方の認証リクエストがユーザーによって拒否された場合、ユーザーは`redirect_uri`に以下のパラメータと共にリダイレクトされます。

#####リダイレクトクエリパラメーター

state
: リクエスト時にアプリケーションによって設定されたユニークなトークン。　アプリケーションは、次のステップに進む前に必ずこのトークンが元のトークンと一致しているかを確認する必要があります。

error
: エラーのタイプ

error_description
: エラーの理由

#####例
~~~
  https://oanda-oauth-example.com/acceptcode?state=STATE_TOKEN&error=access_denied&error_description=user_denied_access
~~~

####ステップ 3: authorization codeとアクセストークンの交換

OANDAの /v1/oauth2/access_token エンドポイントに対してリクエストボディに必要なパラメーターを付与した上で、HTTPS POSTリクエストを行ってください。

~~~
POST /v1/oauth2/access_token

client_id=<client_application_id> \
client_secret=<client_application_secret> \
grant_type=authorization_code \
redirect_uri=<redirect_uri> \
code=<authorization_code> \
~~~

#####リクエストボディのパラメーター

client_id
: OANDAにアプリケーションを登録した際に取得したクライアントアプリケーションID。

client_secret
: OANDAにアプリケーションを登録した際に取得したアプリケーションシークレット

grant_type
: このパラメーターは'**authorization_code**'を設定してください。

code
: 前のステップで受信した`authorization code`。

redirect_uri
: リダイレクトURIはアプリケーションの登録時のものと完全に一致する必要があります。


#####例

~~~
curl -X POST "https://api-fxpractice.oanda.com/v1/oauth2/access_token" -d "client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTH_CODE&redirect_uri=https://oanda-oauth-example.com/acceptcode"
~~~

#####レスポンス


######レスポンスボディのパラメーター

access_token
: ユーザーの代わりにOANDA APIに対してリクエストを行う場合にアプリケーションとして必要なアクセストークン。

token_type
: 受信したトークンのタイプ。 リクエストを行う場合にアクセストークンと共にこの値を設定する必要があります。

expires_in
: アクセストークンが失効するまでの時間（秒）。　この値が0の場合は、アクセストークンは失効しません。

#####例

~~~json
{
  "access_token": "ACCESS-TOKEN",
  "token_type": "Bearer",
  "expires_in": 0
}
~~~

----------------

### クライアントサイドフロー

アクセストークンを取得するには2つの段階があります。


1. [ユーザーにOANDA OAuth authorization endpointへアクセスさせてください。　ユーザーはOANDAへのログインを要求され、貴方のアプリケーションがユーザーのアカウントにアクセスする許可を与えます。](#step-1-direct-the-users-browser-to-oandas-authorization-endpoint)  

2. [１のステップ完了後、OANDAサーバーはユーザーを貴方のアプリケーションの登録済みリダイレクトURIにリダイレクトします。 １のステップが成功した場合は、OANDAはURIの一部にてアクセストークンを返信します。](#step-2-receive-redirect-from-oanda)

####ステップ 1: ユーザーのブラウザをOANDAのauthorization endpointにディレクトする

~~~
GET /v1/oauth2/authorize
~~~  
#####リクエストクエリパラメータ

client_id
: OANDAにアプリケーションを登録した際に取得したクライアントアプリケーションID。

redirect_uri
: リダイレクトURIはアプリケーションの登録時のものと完全に一致する必要があります。

response_type
: クライアントサイドフローをリクエストするため、**token**を指定してください。

state
: リクエストとコールバック間のアプリケーションの状態（ステート）を保持するためのユニークなトークン。　OANDAからのリダイレクトレスポンスに、このパラメーターとトークンの値が含まれます。　貴方のアプリケーションはこの返り値のトークンが、もともと指定したトークンと同じ値であるかどうか確認する必要があります。　弊社はこのトークンをハイクォリティのランダム数値ジェネレーターで生成することを推奨します。

scope
: アプリケーションが必要なパーミッションのリスト。　パーミッションは'+'記号によって区切られます。 全てのパーミッションとその詳細については[ここ](#permissions)を参照してください。

#####例

~~~
https://api-fxtrade.oanda.com/v1/oauth2/authorize?client_id=CLIENT_ID&redirect_uri=https://oanda-oauth-example.com/acceptcode&state=STATE_TOKEN&response_type=token&scope=read+trade+marketdata+stream
~~~

####ステップ 2: OANDAからリダイレクトを受信する。

もしユーザーが貴方のアプリケーションによるアクセスを許可した場合、ユーザーは`redirect_uri`に以下のパラメーターを付与された上でリダイレクトされます。

#####リダイレクトクエリパラメータ

state
: リクエスト時にアプリケーションによって設定されたユニークなトークン。　アプリケーションは、アクセストークンをアクセプトする前に必ずこのトークンが元のトークンと一致しているかを確認する必要があります。

access_token
: ユーザーの代わりにOANDA APIに対してアプリケーションがリクエストを送信する際に必要なアクセストークン。

token_type
: 受信したアクセストークンのタイプ。　リクエストを送信する際に、アクセストークンと共にこの値を設定する必要があります。

expires_in
: アクセストークンが失効するまでの時間（秒単位）。　この値が0の場合は、アクセストークンは失効しません。

#####例

~~~
https://oanda-oauth-example.com/acceptcode#state=STATE_TOKEN&access_token=ACCESS-TOKEN&expires_in=604800&token_type=BEARER
~~~
もしユーザーが貴方のアプリケーションによるアクセスを拒否した場合、OANDAはユーザーを`redirect_uri`に以下のパラメーターを付与された上でリダイレクトします。

#####リダイレクトクエリパラメータ

state
: リクエスト時にアプリケーションによって設定されたユニークなトークン。　アプリケーションは、アクセストークンをアクセプトする前に必ずこのトークンが元のトークンと一致しているかを確認する必要があります。

error
: エラーのタイプ。

error_description
: エラーの理由。

expires_in
: アクセストークンが失効するまでの時間（秒単位）。　この値が0の場合は、アクセストークンは失効しません。

#####例

~~~
https://oanda-oauth-example.com/acceptcode#state=STATE_TOKEN&error=access_denied&error_description=user_denied_access
~~~

----------------

###パーミッション

以下の表に全ての可能なパーミッションのリストとそれぞれのパーミッション可能にするアクセスのタイプを記述しています。　各パーミッションは小文字であることに留意ください。

|パーミッション|詳細|
|---|---|
|read|このパーミッションはユーザーのアカウントの情報と取引履歴を取得することを可能にします。|
|trade|このパーミッションはユーザーのアカウントを通じて取引を行うことを可能にします。|
|marketdata|このパーミッションはユーザーのアカウントに関連した為替レートを取得することを可能にします。|
|stream|このパーミッションはユーザーのアカウントに関連したストリームにアクセスすることを可能にします。|


