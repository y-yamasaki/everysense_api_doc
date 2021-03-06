---
search: ja
title: Everysense API ドキュメント
---

# Everysense API ドキュメント
本ドキュメントでは、EverySenseサーバと通信するためのAPI解説を行います

## 概要

デバイスとEverysense Serverとの通信は、デバイスゲートウェイを通じて行われます。

Everysense Serverがデバイスと正しく通信するためには、デバイスゲートウェイは以下のことが出来る必要があります。

1. デバイスより受信したデータをデータサブシステムに送る
2. 受信したセンサーデータの単位系の整合
3. (必要なら)デバイスの認証
4. Everysense Serverの必要に応じて、デバイスへのpush

<br>

## Everysense APIの基本
デバイスゲートウェイとEverysense Serverとの間の通信は、全て MessagePack RPC を使います。

[MessagePack](http://msgpack.org) および [MessagePack RPC](https://github.com/msgpack-rpc/msgpack-rpc) についての詳しい説明については、当該ページを御覧下さい。仕様については、[MessagePack specification](https://github.com/msgpack/msgpack/blob/master/spec.md) にあります。

デバイスゲートウェイに提供するAPIのデータの型については、以下のものを使用します。

1. Integer represents an integer
2. Nil represents nil
3. Boolean represents true or false
4. Float represents a floating point number
5. String UTF-8 string
6. Array represents a sequence of objects

<p class="tip">
    Everysenseが提供したRest APIの通信メソッドは<label class="label">POST</label>のみとなります。
</p>

通信プロトコル: `https`

ホスト: `api.every-sense.com`

ポート: `8001`

Url: `https://api.every-sense.com:8001`

<br>

## API返り値
Everysense APIの返り値は `code` `data` `reason` `message` `trace` 四つの部分に組み立てられます。
`code`が負数の場合、`data`の代わりに `reason` `message` `trace` が返り値に参入します。

コード(code)
* 0 : 成功
* -1 : リソース不存在
* -2 : 認証失敗
* -10 : パラメーター誤用または漏らし 
* -20 : システムエラー
<br>

## Example
自分のアカウントでユーザーUUIDを取得する場合
<label class="label">POST</label>`https://api.every-sense.com:8001/auth_user`

リクエストボディー:
```
[
    "someone", // ログインID
    "password123" // パスワード
]
```

返り値:

<label class="label success">成功</label>
```
{
    "code":0,
    "uuid":"1ec1075c-c7d1-47ee-a601-6cd5e6170d5e"
}
```

<label class="label danger">失敗</label>
```
{
    "code":-2,
    "reason":"invalid user or password"
}
```

<br>

## Rest API クライアント・パッケージ

#### Ruby 2.x

パッケージリンク先: [_Open-ES_](https://github.com/every-sense/open-es)

インストール:
``` bash
git clone https://github.com/every-sense/open-es.git
```

使い方:
``` ruby
require 'EverySense'

client = EverySense::JsonClient.new
client.auth_user("someone", "password123")
```

返り値:
```
{'code': 0, 'uuid': '1ec1075c-c7d1-47ee-a601-6cd5e6170d5e'}
```
<br>

#### Python 3.x

パッケージリンク先: [_everyclient_](https://github.com/daiyanze/everyclient)

インストール:
``` bash
pip install everyclient

or

pip3 install everyclient
```

使い方:
``` python
from everyclient import EveryClient

EveryClient().auth_user(["someone","password123"])
```

返り値:
```
{'code': 0, 'uuid': '1ec1075c-c7d1-47ee-a601-6cd5e6170d5e'}
```
<br>
<br>
