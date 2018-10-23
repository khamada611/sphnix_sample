 sphnix_sample
=====

Sorry. This page is Japanese only.
sphnix_sample\source\contents1\images\sample.png is licensed by Pronama LLC.
https://kei.pronama.jp/guideline-others/

## これなに？

Sphnixで浜田が主に使いそうな部分をサンプルでまとめたものです。自分用って奴でごめんなさい。以下の項目が作ってありますので、近い事したい方の参考になるかもです。

* もの凄く初歩的なreSTが（しかもにわかなので色々酷いと思います）
* 画像
* reSTで表を作る
* CVSから表を作る
* シーケンス図
* フローチャート
* Read the Docsの利用
* 通常だと横枠固定なのですが、それは取り払っています。

## ざっくり動かすまで

### Sphnix関係のインストール

* pythonは既にあるものとします
* Windows/Python2.x(32bit) で確認しました（CRLF改行なのはそのためです…）
* 元々使い込んでたPythonでやってるので不足があるかもですが、以下の感じでSphnix関係をインストールしました

```
> python -m pip install --upgrade pip
> pip install sphinx
> pip install sphinxcontrib-seqdiag
> pip install sphinxcontrib-blockdiag
> pip install sphinx_rtd_themetest
```
### 本サンプルのビルドなど

ま、普通にやるだけなんですが…

* 注意が必要な事として source/conf.py のseqdiag_fontpathとblockdiag_fontpath にフォントのパスを入れる必要がありますが、Windowsの感覚で入れてますので、Linuxとか他の場合はよしなに願います。
* sphnix_sampleに移動して、以下でHTMLが出来ます。
```
>make html
```
* sphnix_sample\build\htmlのindex.htmlをダブルクリックすれば文章が出てきます。

## 主なライセンス

* sphnix_sample\source\contents1\images\sample.pngは、プロ生ちゃん利用ガイドライン（下記URL）に基づく利用が必要ですのでご注意ください。
https://kei.pronama.jp/guideline-others/

* 後のは著作権を主張するような物はありません。お好きにどうぞ。ただ当然ですが使用した際の損害は誰も請け負ってくれません。そこだけ注意で。
