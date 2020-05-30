# step-functions-local-test
Step FunctionsのELTバッチをローカルでテストする仕組みの実装

# これなに
Step Functionsのローカル実行テストをしたい

## 行う処理
簡単なETLの仕組み

- Extractor
    - どこぞのサーバからデータをCSVで引っこ抜く
- Loader
    - 中間テーブルにCSVの中身を突っ込む
- Transformer
    - 中間テーブルのデータを成型して本番DBにUPSERTする

その他、前後のSlack通知などをLambdaで補う

## バッチの種類
- Fargate
    - 重めのバッチはECS Fargateで動くことを想定する
    - 言語はScala?
- Lambda
    - 軽い処理はLambdaで動くことを想定する
    - 言語はRuby?
