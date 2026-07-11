# Eclipseへのインポート手順

1. インポートで `既存Gradleプロジェクト` を選択
2. Cloneしたフォルダを選択
3. 次へ次へで完了
4. インポートしたプロジェクトを選択して右クリック、コマンドプロンプト
5. `gradlew cleanEclipse eclipse` を実行
    1. ここでEclipseプロジェクトとして必要な設定を作成している
    2. これをしないと、Eclipseから実行できない
6. `initializeProject.ps1` を実行
    1. プロジェクトをエクスプローラで開く
    2. `initializeProject.ps1` を右クリック
    3. Power Shellで実行を選択
    4. これは、gradle.propertiesファイルを作成し、DB接続情報を書き込む。ローカル環境での実行には必要
7. プロジェクトを右クリックし、実行を選択、Spring Applicationを選択
8. localhost:8080をブラウザで指定