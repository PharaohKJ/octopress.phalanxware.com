---
layout: post
title: "ついに最後の ST3000DM001 が壊れたので WD30EZRZ/AFP と交換した"
date: 2016-10-24 10:06:59 +0900
comments: true
published: true
categories: 自宅サーバ
---

## 買い替えとサポート

hp のマイクロサーバで 3T x 4 運用している我が家のファイルサーバだが、どうやら 10/14 あたりに1台のHDDが死んでしまっていたらしい(munin のログで確認)。まぁ最後の ST3000DM001 だろうと予想していたらビンゴ。買い替えたのはこれ。

{% amazon medium_image B015FGGWKU %}

届いた商品を開くと、CFD販売の1年保証書がでてきた。「え?商品ページに2.5年保証って書いてあるけど?」と Amazonさん の CS にチャット突撃したら「初期不良は販売店(= Amazonさん)対応ですが、それ以外は保証書の会社に連絡となります」と案内された。

CFD販売のページ曰く、この保証というのは RMA(Return Merchandise Authorization)のことで、ガチのメーカー(= WD)がシリアル番号を元に製造から一定期間は保証してくれるというものらしい。 → [修理はどこに依頼すればよいですか？ | CFD販売株式会社 CFD Sales INC.](http://www.cfd.co.jp/support/faq/20140828-8/)

うーん。このFAQを読むとまた Amazonさんの説明とは食い違っているが、たぶんこの流れだろう

1. 最初の30日だったかな?の「初期不良交換」は販売店にサポートを
2. 初期不良以後、CFD保証期限である1年以内の場合には、販売店にサポートを(たぶんCFDさんにまわされるだろうけど)
3. CFD保証期限を越えた場合、RMA保証でサポートを


## 交換作業

当初は ST3000DM001 x 4 運用だったのが、全部一気に寿命がこず、ゆっくりと死んでいってくれて大変助かった。[自宅サーバのディスク構成と復旧 - Qiita](http://qiita.com/PharaohKJ/items/2741b681cf3bc8c2c5a0) に書いていた内容で対応できた。

前に3台目が壊れたのは10か月前だったらしい。そういうわけで、今のファイルサーバは WD x2 + Hitachi x2 の 12T 構成になった。消費電力の違いをみたいところなんだが、ワットチェッカーが壊れてしまった & サーバの電源抜き差ししたくないので、我慢しておくことにした。

`cp -rp` でバックアップより書き戻した。80M/Sec ぐらいで書き込めていると記録されているが、結局4時間ぐらいかけてコピーしたようだ。

映画『クリード チャンプを継ぐ男』において、主人公の少年がロッキーの作成したメモをスマフォで写真にとり、「これでいいよ」「電話壊れたらどうすんだよ?」「クラウドにある」「クラウド?」っていうシーンがあった。

あれは「主人公が今時の子演出」であり「ロッキーがそういうことには疎いお年寄り」ということを同時に演出したおもしろいシーンだと思うが、結局PC/スマフォユーザーの知見はロッキーぐらいなんじゃないだろうか? 

知識がない人ほど「いかにうまくいったときまで気軽に巻き戻せるか」が大事じゃないか? それよりは1円でも安く使えるほうが大事だ、なんてのが Windows 95 から30年たってもこのザマだというのはいろいろと我々(= コンピュータを使って商売してる人間)の努力や啓蒙が足りてないのかなー? 商品自体は割とでてきてると思うんだが…。値段の問題なのかな。
