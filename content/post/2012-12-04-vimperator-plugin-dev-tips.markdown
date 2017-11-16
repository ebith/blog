---
comments: true
date: 2012-12-04T00:00:00Z
published: true
title: Vimperatorプラグインを書いてみたい人のためのtips
url: /2012/12/04/vimperator-plugin-dev-tips/
---

[Vimperator Advent Calendar 2012](http://atnd.org/events/34070 "Vimperator Advent Calendar 2012 : ATND") 4日目のebithです。  
今日はVimperatorプラグインを書いてみたい、もしくは書こうとは思ったけど良くわからなかった人向けの情報などを紹介します。  
だいたいは僕がプラグインを作り始めた時に困ったりハマったりしたことですが、お役に立てれば幸いです。

## そもそもどう書けば良いの
[サンプル用のプラグイン](https://github.com/vimpr/vimperator-plugins/blob/master/function-template.js "vimperator-plugins/function-template.js at master · vimpr/vimperator-plugins")にプラグインには欠かせないコマンドやマッピングなどの定義例が書いてあります。  
また、[vimpr/vimperator-plugins](https://github.com/vimpr/vimperator-plugins "vimpr/vimperator-plugins")には200近いプラグインがあるので参考になるはずです。

## プラグイン作るときに便利なやつ

### ライブラリ
Vimperator組込みの`util`や[\_libly.js](https://github.com/vimpr/vimperator-plugins/blob/master/_libly.js "vimperator-plugins/_libly.js at master · vimpr/vimperator-plugins")があります。

### コマンド
`:js`や`:echo`を使えば手軽にJavaScriptを実行できます。例) `:echo util`  
`:source`がプラグインの再読み込みに使えます。

### プラグイン
[auto\_source.js](https://github.com/vimpr/vimperator-plugins/blob/master/auto_source.js "vimperator-plugins/auto_source.js at master · vimpr/vimperator-plugins") - 指定ファイルが変更されると自動的に:sourceします。  
[echopy.js](https://github.com/vimpr/vimperator-plugins/blob/master/echopy.js "vimperator-plugins/echopy.js at master · vimpr/vimperator-plugins" ) - echoしつつクリップボードにコピーします。
<!--- [sitesearch](https://github.com/vimpr/vimperator-rc/blob/b6a3ef4de8a7befebbdbad56c1762423bbe1e7f7/anekos/.vimperatorrc.js#L595-L612 "vimperator-rc/anekos/.vimperatorrc.js at b6a3ef4de8a7befebbdbad56c1762423bbe1e7f7 · vimpr/vimperator-rc")
 Googleのサイト内検索を開く-->

### プリントデバッグ
`:js!`で開くエラーコンソールに、liberator.log()で出力できます。  
ログに残るタイプのechoであるliberator.echomsg()やliberator.echoerr()も便利です。`:messages`であとからでも読めます。

## 既存の動作を変更するプラグインを書きたい
\_libly.jsの$U.aroundを使います。anekosさんの記事が詳しいです。  
[Vimperator 本体のメソッドを書き換えるときの指針](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20100120/1263929847 "Vimperator 本体のメソッドを書き換えるときの指針 - Death to false Web browser! - vimperatorグループ")、
[vimperator で、既存のコマンドにオレオレ動作を差しこむ](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20120811/1344688021 "vimperator で、既存のコマンドにオレオレ動作を差しこむ - Death to false Web browser! - vimperatorグループ")

## ググってもググっても情報出てこない
プラグインに限らずVimperator全体に言えることですが、Googleなどで検索してもまとまった日本語の情報はイマイチ出てきません。  
[vimpr/vimperator-plugins](https://github.com/vimpr/vimperator-plugins "vimpr/vimperator-plugins")をgrepしたりしてなんとかしましょう。  
JavaScript自体に関しては[Mozilla Developer Network](https://developer.mozilla.org/ "Mozilla Developer Network")が心強いです。

## 最後に
やってみると案外なんとかなるもので、単なるソフトウェアオタクでも少しはプラグイン書けるようになりました。  
多くの人がプラグインを書けばそれだけVimperatorが便利になってみんなが幸せなので是非ともチャレンジしてください。
