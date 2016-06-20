# AWS料金チートシート

あるある構成でアプリケーションを構築した場合の料金シュミレート。

※あくまで参考までに。

- [静的ウェブサイト[S3+CloudFront+Route53]](#静的ウェブサイト[S3+CloudFront+Route53])

## 静的ウェブサイト[S3+CloudFront+Route53]

**想定**

`5,000ページビュー/日`くらいの静的ウェブサイト

| サービス                  | 料金($/月) |
|:--------------------------|:-------------|
| Amazon S3 Service（日本） | 0.02         |
| Amazon Route 53 サービス  | 0.80         |
| Amazon CloudFront Service | 13.00        |
| 合計                      | 13.82        |

---

**シュミレート詳細**

[見積もり結果](http://calculator.s3.amazonaws.com/index.html?lng=ja_JP#r=NRT&key=calc-980C2AB9-378A-4559-A5F5-06E66A850BA4)

- S3オブジェクト1オブジェクトあたり: `99.3KB`<sup>*1</sup>
- CloudFront転送量(GB): `150,000ページビュー` x `728.4KB` = `104GB`<sup>*1</sup>
- S3PUTリクエスト数: `100オブジェクト` x `デプロイ4回` = `400リクエスト`<sup>*2</sup>
- S3GETリクエスト数: `5オブジェクト` x `30回` = `150リクエスト`<sup>*3</sup>
- Route53クエリ数(回): `150,000ページビュー` x `5オブジェクト` = `750,000回`<sup>*1</sup>
- S3保有オブジェクト容量(GB): `100オブジェクト` x `99.3KB` = `0.009GB`<sup>*1</sup>

<sup>*1</sup>_転送量やオブジェクト数の値は、[Qiita](http://qiita.com/)の適当なページを幾つか[サンプリングし平均化して算出](https://docs.google.com/spreadsheets/d/1IryHwstCkwgHpm9iMjaUd-qVm2MN3BObLsN8FD80wJA/edit?usp=sharing)_

<sup>*2</sup>_S3に保存されているオブジェクトの数を100オブジェクト、月間デプロイ回数を4回としている_

<sup>*3</sup>_CloudFrontのキャッシュ有効期限を仮に1日と想定(S3オリジンへのアクセス回数は1日に1回)_
