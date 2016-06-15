# 料金

課金の主要素は以下。

1. コンピューティング: インスタンスの起動時間に対する課金
2. ストレージ: 保存されているデータ容量に対する課金
3. データ転送: 通信量に対する課金

コンピューティングがともなうと高くつきやすい(EC2, ELB, RDSなど)。

データ転送はインバウンドが無料。基本的にはアウトバウンドに課金。

[SIMPLE MONTHLY CALCULATOR](http://calculator.s3.amazonaws.com/index.html?lng=ja_JP)という公式の料金計算サービスがあるが、
サービス知識が必要になるため初見で料金イメージをつかむには使いにくい。

See [付録: AWS料金チートシート](../appendix/pricing-cheatsheet.md)

## ドキュメント(公式)
- [AWS クラウド料金の理念](https://aws.amazon.com/jp/pricing/)
