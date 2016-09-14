# Lambda

Lambdaは任意のコードをアップロードすることで、Lambdaのサーバー上でそのコードを実行できるサービス。

AWSのサービスをイベント駆動型で構築するにはなくてはならないサービスである。

(e.g. `S3`にファイルが保存されたらLambdaでコードを実行する)

(e.g. `DynamoDB`の××というテーブルに書き込みがあったらLambdaでコードを実行する)

## Lambda関数

Lambdaにアップロードされたコードを`Lambda関数`と呼ぶ。

Lambda関数がサポートするプログラミング言語はNode.js、Python、およびJava。

実行時間は最大300秒まで設定できる。 (それ以上時間のかかる処理は実行できない)

## イベントソース (Lambda関数の実行)

Lambda関数は、`イベントソース`とLambda関数を紐付けることで実行出来る。

イベントを発生させるAWSのサービスを`イベントソース`と呼び、イベントソースとLambda関数を紐付けることを`イベントソースマッピング`と呼ぶ。

Lambdaがサポートする主なイベントソースとユースケースを以下に挙げる。

全てのイベントソースは[サポートされているイベントソース (公式)](http://docs.aws.amazon.com/ja_jp/lambda/latest/dg/invoking-lambda-function.html) を参照。

### S3

S3オブジェクトの作成、削除、更新をトリガーに、指定した関数を実行できる。

例: `画像が保存されたことをトリガーに、その画像のサイズを変換して別名でS3に保存するLambda関数`

### DynamoDB

DynamoDBのテーブルへのレコード追加、削除、更新をトリガーに、指定した関数を実行できる。

例: `DynamoDBテーブルのユーザーレコードが削除されたことをトリガーに、S3にあるユーザーのサムネイルも削除するLambda関数`

### CloudWatch Logs

CloudWatch Logsにログが作成されたことをトリガーに、指定した関数を実行できる。

例: `「Error」という文字列が含まれたエラーログを、Slackのチャンネルに書きこむLambda関数`

### Scheduled Events (CloudWatch Events)

CloudWatch Eventsのイベントスケジュール機能で、定期的に指定した関数を実行できる。

例: `毎日午前10:00(cron(0 10 * * ? *))にGoogleAnalyticsの集計結果をSlackのチャンネルに書きこむLambda関数`

### API Gateway

API Gatewayで作成したREST APIのエンドポイントがコールされたことをトリガーに、指定した関数を実行できる。

例: `API Gatewayで作成した「https://example.com/v1/users」にGETリクエストを送るとLambda関数がコールされ、ユーザー情報を返す`

### オンデマンド

AWSサービスがなくても、AWS SDKやAWS CLI、AWS WebAPIからLambda関数を実行できる。

例:

```
# AWS CLIでLambda関数を実行する
aws lambda invoke --function-name example_function --invocation-type RequestResponse --payload '{"key1":"value1", "key2":"value2", "key3":"value3"}'
```

### その他

その他、全てのイベントソースは[サポートされているイベントソース (公式)](http://docs.aws.amazon.com/ja_jp/lambda/latest/dg/invoking-lambda-function.html) を参照。

## 実行環境

OS: Amazon Linux

メモリ: Lambda関数作成時に指定

CPU: [指定されたメモリに応じて割り当て](https://aws.amazon.com/jp/lambda/faqs/)

- Lambda関数の実行ごとに毎回インスタンスが立ち上がるけではなく、インスタンスは再利用される
- Lambda関数のコードから`/tmp`ディレクトリにファイルの書き込みが出来る
  - `/tmp`ディレクトリにファイルを作成するときはインスタンス再利用を考慮して、Lambda関数の実行に対して一意なファイル名を利用すること

## 料金

リクエスト数(Lambda関数の呼び出し回数)と、Lambda関数の実行時間に対して課金される。

いずれも毎月リセットされる無料利用枠があるため、結構使わない限り課金が発生しない。

**リクエスト数**

- 毎月100万回のリクエストまで無料
- その後`$0.20/100万リクエスト`

**実行時間**

- `GB-秒 = Lambda関数に割り当てたメモリ数 * Lambda関数の実行時間`に対する課金
- 毎月40万GB-秒まで無料

例:

- メモリ割り当てを`128MB`
- Lambda関数の実行時間が、だいたい`3秒`
- 毎日1000回実行される

```
(128(MB) / 1024) * 3000(秒) * 30(日) = 月間11,250GB-秒
```
