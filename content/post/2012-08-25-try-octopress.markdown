---
comments: true
date: 2012-08-25T00:00:00Z
title: Octopressを試す
url: /2012/08/25/try-octopress/
---

はてなダイアリーも広告が入るようになってしまったので、これを機会にGithub+Octopressも試してみる。  
TumblrとかLokkaとか、WordPressに出戻りも考慮したけどどれもしっくりこないので期待。

## 参考にしたページとか
[Big Sky :: githubとjekyllとoctopressで作る簡単でモダンなブログ](http://mattn.kaoriya.net/software/lang/ruby/20111017205717.htm "Big Sky :: githubとjekyllとoctopressで作る簡単でモダンなブログ")  
[GithubとOctopressでモダンな技術系ブログを作ってみる - Glide Note - グライドノート](http://blog.glidenote.com/blog/2011/11/07/install-octopress-on-github/ "GithubとOctopressでモダンな技術系ブログを作ってみる - Glide Note - グライドノート")  
[octopress でブログを書いてみる - ハタさんのブログ(出張版)](http://nowelium.github.com/blog/2012/03/26/octopress-diff.html "octopress でブログを書いてみる - ハタさんのブログ(出張版)")  
[Octopress用Tag Cloudプラグインをリリースします - T.I.D.](http://tokkonopapa.github.com/blog/2012/01/04/octopress-plugin-for-categories-cloud/ "Octopress用Tag Cloudプラグインをリリースします - T.I.D.")  

## それ以外にやったこと
[Octopressの記事管理用プラグイン、Octoeditor.vimを作った - Glide Note - グライドノート](http://blog.glidenote.com/blog/2012/04/02/octoeditor.vim/ "Octopressの記事管理用プラグイン、Octoeditor.vimを作った - Glide Note - グライドノート")を導入したり
``` vim
nnoremap <Leader>of :Unite file_rec:<C-r>=expand(g:octopress_path."/source/_posts/")<CR><CR>
```
Unite.vimで記事一覧開くのマッピングしたり

``` sh
pip install pygments
vim _config.yml
  pygments: true
```
シンタックスハイライト用にPygments入れたり

## 雑感
 やっぱ普通にエディタで書けるの便利。  
vimperator使ってるとテキストエリアで外部エディタ使えるけど、
なんかの拍子に吹っ飛んでっちゃったことがあってそれ以来あんまり使わなくなってたので。

配布されてるテーマ少ないので慣れないCSSとか画像とか触るはめになってつらい。  
`rake install['theme']`するとそのたびにファイルが上書きされるみたいで色々試すとなんかグチャグチャになってしまったりとか。  

## TODO
適当に書き足したCSSが悲しい事になってるのどうにかして欲しい。  
Favicon欲しい。
