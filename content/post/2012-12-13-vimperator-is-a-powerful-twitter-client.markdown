---
comments: true
date: 2012-12-13T00:00:00Z
published: true
title: Vimperatorで快適Twitter
url: /2012/12/13/vimperator-is-a-powerful-twitter-client/
---

[Vimperator Advent Calendar 2012](http://atnd.org/events/34070 "Vimperator Advent Calendar 2012 : ATND") 13日目、二週目に突入のebithです。  
今日はVimperatorプラグインの中でお気に入りの一つであるTwittperatorとそのプラグイン群の紹介をしたいと思います。  

## Twittperatorとは
Streamimg APIにいち早く対応したTwitterクライアントであり、Vimperatorのプラグインでもあります。  
ツイートすることやタイムラインの表示の他にRTやFavはもちろん、会話(in\_reply\_to)を辿ったり、Twitterを検索したり、特定のツイート(status)のページや誰かのホームを開いたりと言ったことがコマンドラインから可能です。  
また、プラグインを作ることによって自由度の高い拡張が可能で既に幾つものプラグインが存在しています。

## 使ってみる
[twittperator.js](https://raw.github.com/vimpr/vimperator-plugins/master/twittperator.js)を読み込み、`:tw -getPIN`で得たPINコードを`:tw -setPIN`でセットすることで使えるようになります。  
基本的な使い方は`:tw`コマンドからの補完や`:help twittperator`を参考にしてください。  
また、一例として少しだけ設定を書いておくので必要に応じて.vimperatorrcに書き足すと良いと思います。
``` vim
" Twitterへの接続がSSLのみになったので、Twittperator側でもSSLを使うように
let g:twittperator_use_ssl_connection_for_api_ep = 1

" Streaming APIを使う
let g:twittperator_use_chirp = 1

" 特定の単語を含むツイートを追いかける
let g:twittperator_track_words = "vimp,vimperator,twittperator,ついっぺ,ツイッペ"

" Twitter IDの設定。いくつかのプラグインで必要
let g:twittperator_screen_name = "ebith"

" タイムラインの表示
nnoremap ,wl :tw<CR>

" ツイートする
nnoremap ,ww :tw<Space>

" 返信する
nnoremap ,wr :tw<Space>@

" ツイートに含まれるURLを開く
nnoremap ,w/ :tw!/<Tab>

" 会話を辿る
nnoremap ,wt :tw!thread<Space>
```

## 個性あふれるプラグイン群
Twittperatorには数々の便利なプラグインが提供されています。  
ここでそれらの一部を紹介します。  
[https://github.com/vimpr/vimperator-plugins/tree/master/twittperator](https://github.com/vimpr/vimperator-plugins/tree/master/twittperator "vimperator-plugins/twittperator at master · vimpr/vimperator-plugins · GitHub")からダウンロード可能です。

### add-url-completer
[Twittperator の履歴から URL を取り出して、:tabopen などで補完できるようにする](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20110704/1309706089 "Twittperator の履歴から URL を取り出して、:tabopen などで補完できるようにする - Death to false Web browser! - vimperatorグループ")

### colorful-log-writer
[tail -fするとカラフルなTLが流れてくるファイルを書き出すTwittperatorプラグイン](http://www.flickr.com/photos/ebith/5674571479/ "colorful-log-writer | Flickr - Photo Sharing!")

### eject-alert
[音を鳴らしたりできない環境で、reply に気づけるようにする便利な Twittperator プラグイン](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20120317/1331979169 "音を鳴らしたりできない環境で、reply に気づけるようにする便利な Twittperator プラグイン - Death to false Web browser! - vimperatorグループ")

### pong
[Twitter で超反応するための Twittperator 用プラグイン](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20110330/1301495775 "Twitter で超反応するための Twittperator 用プラグイン - Death to false Web browser! - vimperatorグループ")

### twsidebar
[サイドバーにTL表示をするプラグイン](http://vimperator.g.hatena.ne.jp/nokturnalmortum/20111102/1320245517 "サイドバーにTL表示をするプラグイン - Death to false Web browser! - vimperatorグループ")

### twsidebar-expand-url
[短縮URLを展開するTwittperatorプラグイン(twsidebar向け)](http://d.hatena.ne.jp/ebith/20120808/1344429639 "短縮URLを展開するTwittperatorプラグイン(twsidebar向け) - おいら屋ファクトリー")  
画像のサムネイルを表示したり、ツイートを展開表示する機能も付いています

### urusai-namakubi
[棒読みちゃん](http://chi.usamimi.info/Program/Application/BouyomiChan/ "棒読みちゃん ～ 音声合成で日本語文章を読み上げるツール")にタイムラインを読み上げさせるプラグイン。namakubi.js(Vimperatorプラグイン)も必要になります

## 最後に
普段はtwsidebar+twsidebar-expand-urlでタイムラインを流しながらブラウジングして、目に付くツイートがあればコマンドラインからURLを開いたり会話を辿ったりしています。  
僕の.vimperatorrcのTwittperator関連部分を抜き出したものも用意しておきましたので良ければ参考にしてください。  
[https://gist.github.com/4264015](https://gist.github.com/4264015)
