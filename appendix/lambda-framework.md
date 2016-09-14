# Lambdaのフレームワーク

[Lambda](../services/lambda.md)のコードを管理するためのフレームワークで代表的なものをピックアップする。

Lambdaに関する説明は[サービス/Lambda](../services/lambda.md)を参照のこと。

## [apex](http://apex.run/)

Github: [apex/apex](https://github.com/apex/apex) <iframe src="https://ghbtns.com/github-btn.html?user=apex&repo=apex&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

Node.jsのWebアプリケーションフレームワーク[express](https://expressjs.com/)の作者でも有名な[TJ Holowaychuk](https://github.com/tj)が作成したLambdaフレームワーク。

### 特徴

- Golangで書かれたフレームワーク
- Lambda関数のデプロイ、実行を管理できる
- Lambda関数はNode.js、PythonもしくはJavaしかサポートしていないが、Golangで書いたLambda関数をデプロイ出来る
  (Golangで書かれたコードをバイナリにビルドし、そのバイナリを実行するNode.jsのコードを自動生成することで実現)
- Lambda関数のデプロイに特化しており、イベントソースマッピングの作成は基本的にサポートしていないが、
  [Terraform](https://www.terraform.io/)<sup>*</sup>の実行がラップされているのでそれを使って自前で管理することが出来る
- デプロイライフサイクル中のHookで任意のシェルスクリプトを実行できるので、ビルド手順のカスタマイズが出来る
- `alias`パラメーターでstage/prodのような環境分岐が出来る

<sup>*</sup>_[Terraform](https://www.terraform.io/)はAWSの環境構築をコード化出来るサードパーティ製のアプリケーション_

### ストラクチャ

```
project.json
functions
├── bar
│   ├── function.json
│   └── index.js
└── foo
    ├── function.json
    └── index.js
```

**functions/foo/function.json (Lambda関数の設定ファイル)**

```
{
  "description": "Node.js example function",
  "runtime": "nodejs",
  "memory": 128,
  "timeout": 5,
  "role": "arn:aws:iam::293503197324:role/lambda"
}
```

### コマンド

※詳しい使い方は[ドキュメント](https://github.com/apex/apex)を読んで下さい

```
# デプロイ
apex deploy [関数名]

# 関数の実行
apex invoke [関数名] < event-sample.json
```

---

## [serverless](https://serverless.com/)

Github: [serverless/serverless](https://github.com/serverless/serverless) <iframe src="https://ghbtns.com/github-btn.html?user=serverless&repo=serverless&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

[Austen Collins](https://aws.amazon.com/jp/heroes/usa/austen-collins/)<sup>*</sup>が作ったサーバーレスアーキテクチャのシステムを構築するためのフレームワーク。

Austenは[Amazon re:invent 2015のLambdaセッション](https://aws.amazon.com/jp/blogs/compute/aws-lambda-sessions-at-reinvent-2015/)でこのフレームワークを発表し、AWS Community Hero<sup>*</sup>にも選ばれている。

<sup>*</sup>_Austen Collins: このフレームワークをプロダクトに、Serverless, Incとして起業した模様_

<sup>*</sup>_AWS Community Hero: AWSコミュニティに貢献した人に送られる名誉ある称号らしい_

### 特徴

- Node.jsで書かれたフレームワーク
- Lambda関数のデプロイ、実行を管理できる
- イベントソースマッピングの作成もサポート。簡単な設定ファイルでイベントソースとLambda関数の紐付けを行ってくれる
- Lambda関数を取り巻く環境構築を全てラップしており、内部的にはCloudFormationテンプレートを作成してスタックを作成する
- デプロイライフサイクル中に実行されるプラグインを作成できるので、ビルド手順のカスタマイズが出来る
- `stage`パラメーターでstage/prodのような環境分岐が出来る<sup>*</sup>

<sup>*</sup>_環境分岐: stageごとにCloudFormationスタックが作成される。そのため、Lambda関数のalias機能やAPI Gatewayのステージ機能はサポートしていない_

### ストラクチャ

```
bar.js
foo.js
serverless.env.yml
serverless.yml
```

**serverless.yml (環境の設定ファイル)**

```
service: example

provider:
  name: aws
  runtime: nodejs4.3

functions:
  barFunction:
    handler: bar.hello

events:
  - schedule: rate(10 minutes)
```

### コマンド

※詳しい使い方は[ドキュメント](https://github.com/serverless/serverless)を読んで下さい

```
# 環境+関数のデプロイ
serverless deploy

# 関数のデプロイ
serverless deploy function --function [関数名]

# 関数の実行
serverless invoke --function [関数名] --path event-sample.json
```

## 比較表

| 項目 | apex | serverless |
|:------------ |:-------------:|:-----:|
| Lambda関数のデプロイ | ○ | ○ |
| イベントソースマッピング | ○ | △ (Terraform) |
| Golangサポート | × | ○ |
| デプロイ方法 | CloudFormation | AWS SDK (Golang) |
| カスタムビルド | ○ (Plugin) | ○ (Hookシェルスクリプト) |
| 環境分岐 | ○ (`stage`パラメーター) | ○ (`alias`パラメーター) |
| Lambda関数内の環境変数 | ×<sup>*</sup> | ○ (`env`パラメーター) |
| ライセンス | MIT | MIT |

<sup>*</sup>Lambda関数内の環境変数: Node.jsであればwebpackが使えるので、[`DefinePlugin`](https://webpack.github.io/docs/list-of-plugins.html#defineplugin)等で実現可能
