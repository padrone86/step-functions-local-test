# Step Functions ローカル実行

## 準備
- awscli
- aws-sam-cli

## 実行
このディレクトリで以下コマンドを実行すると、ローカルでのStep Functions実行が試行できる。

1. Step Functions用のコンテナ (サーバ) を準備する

```sh
$ docker pull amazon/aws-stepfunctions-local:latest
$ docker run --name stepfunctions -p 8083:8083 -d amazon/aws-stepfunctions-local:latest
```

    - (docker-compose化するといいかも)

2. ローカルコンテナに対して `HelloWorld.json` を渡しステートマシンを作成する

```sh
$ aws stepfunctions create-state-machine --endpoint-url http://localhost:8083 --definition file://HelloWorld.json \
  --name "HelloWorld" --role-arn "arn:aws:iam::012345678901:role/DummyRole"
```

こんな感じで実行されて、ステートマシンが作成される

```json
{
    "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
    "creationDate": "2020-05-31T00:28:11.981000+09:00"
}
```

3. 作成したステートマシンを起動する
```sh
$ aws stepfunctions start-execution --endpoint http://localhost:8083 --state-machine-arn arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld --name TestExecution001
```

起動に成功するとこんな感じになる
```json
{
    "executionArn": "arn:aws:states:us-east-1:123456789012:execution:HelloWorld:TestExecution001",
    "startDate": "2020-05-31T00:31:44.091000+09:00"
}
```

4. 実行結果を確認する
```sh
$ aws stepfunctions describe-execution --endpoint http://localhost:8083 --execution-arn arn:aws:states:us-east-1:123456789012:execution:HelloWorld:TestExecution001
```

結果が返却される。 `SUCCEEDED` であれば正常。
```json
{
    "executionArn": "arn:aws:states:us-east-1:123456789012:execution:HelloWorld:TestExecution001",
    "stateMachineArn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
    "name": "TestExecution001",
    "status": "SUCCEEDED",
    "startDate": "2020-05-31T00:31:44.091000+09:00",
    "stopDate": "2020-05-31T00:31:44.122000+09:00",
    "input": "{}",
    "output": "\"Hello World\""
}
