# Open Policy Agent

OPAと呼ぶ。

オープンソースな権限周りの実装で、コンテナにもサイドカーにも、ライブラリでの組み込みにもできる。GO言語で開発されている。

# 実行環境

## 公式コンテナ

公式コンテナを実行する。

```scala
docker run --rm -d --name opa -p 8181:8181 openpolicyagent/opa run --server
```

### 一時使用

```json
docker run -it --rm openpolicyagent/opa run
```

## プライグランド

[The Rego Playground](https://play.openpolicyagent.org/p/ikesWCFIH8)

# API

## Dataを登録

参照するデータを登録する。データのグループ名をURIに指定してPUTする。

データはJSON。

### サンプルデータ

subordinates.json

```json
{
  "alice": ["bob"],
  "bob": [],
  "charlie": ["david"],
  "david": []
}
```

### データ登録API

```scala
curl -X PUT -H 'Content-Type:application/json' --data-binary @subordinates.json \
    localhost:8181/v1/data/subordinates
```

## Policyを登録

Regoで記述したPolicyを登録する。

### サンプルプログラム

httpapi_authz.rego

```scala
package httpapi.authz

# 社員情報をインポート
import data.subordinates as subord

default allow = false

# 社員は自身の給与を参照できる
allow {
  some username
  input.method == "GET"
  input.path = ["finance", "salary", username]
  input.user == username
}

# マネージャーは部下の給与も参照できる
allow {
  some username
  input.method == "GET"
  input.path = ["finance", "salary", username]
  subord[input.user][_] == username
}
```

### ポリシー登録

プログラム中の `package` 名と登録先のURIの最後の箇所は一致している必要がある。ドットで繋がれている場合はアンダースコアに置き換える。

```json
curl -X PUT -H 'Content-Type: text/plain' --data-binary @httpapi_authz.rego \
    localhost:8181/v1/policies/httpapi_authz
```

## ポリシー確認

プログラム中の `package` 名がURIに入ってくる。

`/data/{パッケージ名。ドットはスラッシュに変換}`

戻り値は、プログラム中に宣言したオブジェクト名とその結果が戻る。サンプルの場合、以下のよう。

```json
{"result":{"allow":false}}
```

URIでオブジェクトを指定することも可能。その場合結果のみ返る。

`/data/{パッケージ名。ドットはスラッシュに変換}/allow`

```json
{"result":false}
```

### 自分自身のデータを取得するエンドポイントにアクセス可能か確認する。

アクセス可能が返ってくる。

```json
curl -s -X POST -H 'Content-Type:application/json' \
    localhost:8181/v1/data/httpapi/authz/allow \
    -d @- <<EOS
{
  "input": {
    "method": "GET",
    "path": ["finance", "salary", "alice"],
    "user": "alice"
  }
}
EOS
```

### 許可されている他者のデータを取得するエンドポイントにアクセス可能か？

アクセス可能が返って来る。

```json
curl -s -X POST -H 'Content-Type:application/json' \
    localhost:8181/v1/data/httpapi/authz/allow \
    -d @- <<EOS
{
  "input": {
    "method": "GET",
    "path": ["finance", "salary", "bob"],
    "user": "alice"
  }
}
EOS
```

### 許可されていない他者のデータを取得するエンドポイントにアクセス可能か？

アクセス不可が返って来る。

```json

```

## 公式

[REST API](https://www.openpolicyagent.org/docs/latest/rest-api/)

# Rego (Policy Language)

## 公式

[Policy Language](https://www.openpolicyagent.org/docs/latest/policy-language/)

## 事始め

公式コンテナを使って、API経由でPolicyとDocumentを登録して、許諾をさせてみる

[注目のOpen Policy Agent、その概要とKubernetesでの活用事例](https://thinkit.co.jp/article/17511)

公式コンテナを使って、インタラクティブに動かす

[Open Policy Agentを始めてみよう - kenfdev's blog](https://kenfdev.hateblo.jp/entry/2019/03/19/092927)

Policy as a Codeについて語っている。Regoの公式サンプルへのリンクがついている

[Policy as Code を実現する Open Policy Agent に憧れて。ポリシーコードでAPI仕様をLintする | フューチャー技術ブログ](https://future-architect.github.io/articles/20200930/)