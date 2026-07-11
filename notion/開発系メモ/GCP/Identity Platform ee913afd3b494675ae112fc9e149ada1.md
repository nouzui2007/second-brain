# Identity Platform

## メールとパスワードでログイン

### ウェブでの実装

[クイックスタート: メールアドレスとパスワードを使用したユーザーのログイン | Identity Platform のドキュメント](https://cloud.google.com/identity-platform/docs/quickstart-email-password?hl=ja)

## トークンの検証

### JWKS

Googleサイトへ問い合わせ

[](https://www.googleapis.com/service_accounts/v1/jwk/securetoken@system.gserviceaccount.com)

### パブリックキーの問い合わせ

ドキュメントではこちらで検証しろと書いてある

[](https://www.googleapis.com/robot/v1/metadata/x509/securetoken@system.gserviceaccount.com)

## 参考サイト

[Identity Platformの使い方 : Y's Software Library](http://ww4.tiki.ne.jp/~yosshie-k/programming/gcp_tips/identity_platform/index.html)

[Email/Password IDプロバイダの設定 : Y's Software Library](http://ww4.tiki.ne.jp/~yosshie-k/programming/gcp_tips/identity_platform/password.html)