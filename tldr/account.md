# アカウント

アカウントは一般的に以下のヒエラルキーで構成される。

```
rootアカウント
  - IAMユーザー
  - IAMユーザー
```

企業のアカウント管理体制によっては以下の構成になることもある。

```
一括請求アカウント
  - rootアカウント
    - IAMユーザー
    - IAMユーザー
  - rootアカウント
    - IAMユーザー
```

**`rootアカウント`**

- 一般的にこのアカウントの単位でAWSへの支払いを行う
- すべてのリソースに対するフルアクセス権限を持つ
- ログイン画面はIAMユーザーと異なる
- このアカウントの単位でアカウントIDが発行される
- アカウントIDは`IMAユーザー`がAWSログイン画面で入力するやつ
- アカウントIDは`ARN`に含まれる

**`IAMユーザー`**

- AWSアカウントの下に作業用ユーザーとして作成する
- 一般的に作業ユーザーが1人であっても、rootアカウントは作業に使用せず、作業用のIAMユーザーを作成するのがよい
- IAMのグループ/ポリシー/ロール機能を使ってアクセス権限をコントロールできる

**`一括請求アカウント`**

- rootアカウントへの請求を1つのアカウントに集約するためのアカウント

## ドキュメント(公式)
- [IAM のベストプラクティス](http://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/best-practices.html)
- [AWS アカウント ID](https://docs.aws.amazon.com/ja_jp/general/latest/gr/acct-identifiers.html)
- [一括請求を使用した複数アカウントの料金の支払い](https://docs.aws.amazon.com/ja_jp/awsaccountbilling/latest/aboutv2/consolidated-billing.html)
