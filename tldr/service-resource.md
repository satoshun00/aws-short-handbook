# サービス/リソース


サービスは[サービス一覧](http://aws.amazon.com/jp/about-aws/global-infrastructure/regional-product-services/)に示される、AWSの製品名を指す。

AWSのサービスはリソースを作成することで機能する。

例:

```
Auto Scaling # 'オートスケーリング'というサービス
  - AutoScalingGroup    # 'AutoScalingGroup'リソース
  - LaunchConfiguration # 'LaunchConfiguration'リソース
  - LifecycleHook       # 以下略
  - ScalingPolicy
  - ScheduledAction
```

AWSのサービスは多いが、他のサービスやリソースを単にラップして使いやすくしたようなサービスもある。(ElasticBeanstalk, ECS, EMR)

AWSのサービスを使うには、リソースから理解すると良い。

リソースは以下のツールで操作できる。

- [AWS CLI](http://docs.aws.amazon.com/cli/latest/index.html)
- [各プログラミング言語向けに用意されたSDK](https://aws.amazon.com/jp/tools/)
- 各サービスに用意されたWebAPI
- [AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/Welcome.html)
- その他、サードパーティ製のライブラリ

---

**`AWS CLI`, `SDK`, `WebAPI`**からリソースを操作する場合、単純な`Create, Read, Update, Delete`のインターフェースで操作できる。

また、**`CloudFormation`**<sup>*</sup>からリソースを操作する場合、リソースとオプションをjsonフォーマットで記載してリソースを作成する。

リソースに指定可能なオプションはこれら全てでインターフェースが共通なため、操作に必要な(あるいは任意の)設定値が何かを網羅的に理解できる。

[参考]Auto Scaling サービス > `AutoScalingGroup` リソース を作成する:

- [CLI](http://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-auto-scaling-group.html)
- [WebAPI](http://docs.aws.amazon.com/ja_jp/AutoScaling/latest/APIReference/API_CreateAutoScalingGroup.html)
- [JavaScript SDK](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/AutoScaling.html#createAutoScalingGroup-property)
- [AWS::AutoScaling::AutoScalingGroup](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-as-group.html)

<sup>*</sup>_AWSのリソース構成をjsonで管理するAWSのサービスの1つ_

---

[`AWS マネジメントコンソール`](https://aws.amazon.com/jp/console/)でブラウザーからもリソースの操作ができる。

しかし、ブラウザーからの操作は簡潔なUIゆえに実際に自分がどのリソースにどんな操作を行ったかが全くわからないので正直おすすめしない。

## まとめ

- AWSでなにかをはじめるにはリソースを作成する
- リソースを理解するのが大事
- リソースは`AWS CLI`, `SDK`, `WebAPI`, `CloudFormation`のいずれかで操作すると良い

## Tips; `ARN`

リソースのうち一部のリソースに長い識別子が付く。

See: [Amazon リソースネーム（ARN）と AWS サービスの名前空間](https://docs.aws.amazon.com/ja_jp/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-autoscaling)

## ドキュメント(公式)
- [AWS CLI](http://docs.aws.amazon.com/cli/latest/index.html)
- [アマゾン ウェブ サービスのツール](https://aws.amazon.com/jp/tools/)
- [AWS SDK for JavaScript](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/index.html)
- [AWS CloudFormation とは](http://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [Amazon リソースネーム（ARN）と AWS サービスの名前空間](https://docs.aws.amazon.com/ja_jp/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-autoscaling)
