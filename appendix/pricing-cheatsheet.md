# AWS料金チートシート

あるある構成でアプリケーションを構築した場合の料金シュミレート。

※あくまで参考までに。

- [静的ウェブサイト[S3+CloudFront+Route53]](#静的ウェブサイト[S3+CloudFront+Route53])

## 静的ウェブサイト[S3+CloudFront+Route53]

**前提**

- CloudFrontのオリジンにウェブサイトホスティング設定のS3を配置
- 1ページビューあたりの転送量を`728.4KB`<sup>*</sup>、リクエストオブジェクト数を`5個`とする
- 全てのページが同じ転送量とする
- 全てのページのコンテンツのキャッシュ時間を`1日`とする(`Cahche-Control: max-age=86400`)
- CloudFrontでオブジェクトがキャッシュされているため、S3オリジンへのアクセス回数を月間`30回`(1日1回)とする
- 全部で`100オブジェクト`(html, css, image)で構成されるサイトとし、1オブジェクトあたり`99.3KB`とする
- デプロイにて月間`4回`全オブジェクトの更新を行うものとする
- ページビューのうち、日本70%、アメリカ30%からのアクセスとする
- リージョンは`ap-northeast-1(Tokyo)`とする
- `5,000ページビュー/日`くらいのウェブサイトである

<sup>*</sup>_[Qiita](http://qiita.com/)の適当なページを幾つか[サンプリングし平均化して算出](https://docs.google.com/spreadsheets/d/1IryHwstCkwgHpm9iMjaUd-qVm2MN3BObLsN8FD80wJA/edit?usp=sharing)_

**料金シュミレート(`150,000ページビュー/month`)**

- S3保有オブジェクト容量(GB): `100オブジェクト` * `99.3KB` / 1024 / 1024 = `0.009GB`
- S3PUTリクエスト数: `100オブジェクト` * `デプロイ4回` = `400リクエスト`
- S3GETリクエスト数: `5個` * `30回` = `150リクエスト`
- CloudFront転送量(GB): `150,000ページビュー` * `728.4KB` / 1024 / 1024 = `104GB`
- Route53クエリ数(回): `150,000ページビュー` * `5個` = `750,000回`

[見積もり結果](http://calculator.s3.amazonaws.com/index.html?lng=ja_JP#r=NRT&key=calc-980C2AB9-378A-4559-A5F5-06E66A850BA4)

| サービス                  | 料金(USD) |
|:--------------------------|:----------|
| Amazon S3 Service（日本） | 0.02      |
| Amazon Route 53 サービス  | 0.80      |
| Amazon CloudFront Service | 13.00     |
| 合計                      | 13.82     |

---

- [Qiitaのページサンプリング計算に使ったシート(Google SpreadSheet)](https://docs.google.com/spreadsheets/d/1IryHwstCkwgHpm9iMjaUd-qVm2MN3BObLsN8FD80wJA/edit?usp=sharing)
- [AWSカリキュレーター](http://calculator.s3.amazonaws.com/index.html?lng=ja_JP)
