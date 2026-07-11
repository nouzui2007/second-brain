# API Gateway

# 参考サイト（公式ドキュメント）

変数はセットすると、関連する箇所に反映されてコマンドコピーで行けるようになるので楽。

[Quickstart: Deploy an API on API Gateway using the gcloud command-line tool](https://cloud.google.com/api-gateway/docs/quickstart)

# 新規作成

## API作成

プロジェクトに新たにAPIを作成する。

```scala
gcloud api-gateway apis create sample-api --project=journey-builder-stg
```

作成したAPIを確認する。

```scala
gcloud api-gateway apis describe sample-api --project=journey-builder-stg
```

## API Config作成

デプロイ済みのBackendと接続するためにAPI Configを作成する。

Open API形式のファイルが必要。

```scala
gcloud api-gateway api-configs create sample-api-config \
  --api=sample-api --openapi-spec=sample-api.yaml \
  --project=journey-builder-stg --backend-auth-service-account=657759860280-compute@developer.gserviceaccount.com
```

**backend-auth-service-account** サービスアカウントでメールを確認できる。

作成したAPI Configは以下のコマンドで確認する。

```
gcloud api-gateway api-configs describe sample-api-config \
  --api=sample-api --project=journey-builder-stg
```

## Gateway作成

設定を反映する。外部から定義したAPIにアクセスできるようにする。

```
gcloud api-gateway gateways create sample-api-gateway \
  --api=sample-api --api-config=sample-api-config \
  --location=asia-northeast1 --project=journey-builder-stg
```

作成したGatewaの確認は以下のコマンドを使用する。

```
gcloud api-gateway gateways describesample-api-gateway \
  --location=asia-northeast1 --project=journey-builder-stg
```

# 更新

APIの増減、セキュリティ系の変更などがあった場合の流れを説明する。

## API Config作成

API Configは新しいものを作成する。

```
gcloud api-gateway api-configs create sample-api-config2 \
--api=sample-api --openapi-spec=sample-api.yaml \
--project=journey-builder-stg --backend-auth-service-account=657759860280-compute@developer.gserviceaccount.com
```

Gatewayを新しく作成したAPI Config IDで更新する。

```
gcloud api-gateway gateways update　sample-api-gateway \
  --api=sample-api --api-config=sample-api-config2 \
  --location=asia-northeast1 --project=journey-builder-stg
```

## リスト表示

### API Config

```scala
gcloud api-gateway api-configs list --project=journey-builder-stg
```

### Gateway

```scala
gcloud api-gateway gateways list --project=journey-builder-stg
```

## 削除

### Gateway

GatewayはAPI Configを参照しているので、先にGatewayを消す。

`--project` はなくても良い。

```scala
gcloud api-gateway gateways delete sample-api-gateway3 \
  --location=asia-northeast1 --project=journey-builder-stg
```

### API Config

参照しているAPI Configは消せない。

```scala
gcloud api-gateway api-configs delete sample-api-config3 --api=sample-api
```