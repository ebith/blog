---
comments: true
date: 2012-12-22T00:00:00Z
published: true
title: Vimperatorを便利に使うためのtips
url: /2012/12/22/useful-tips-for-vimperator/
---

[Vimperator Advent Calendar 2012](http://atnd.org/events/34070 "Vimperator Advent Calendar 2012 : ATND") 22日目担当のebithです。  
今日はgistで公開されているプラグインや各ユーザのrcにある便利な設定など、[vimpr](http://vimpr.github.com/ "Welcome to vimpr!")のプラグインリポジトリ外のものを紹介します。

## QB.js ([https://gist.github.com/1766649](https://gist.github.com/1766649 "QB.js 2012/02/08 現在使っているバージョン"))
コマンドラインへメッセージを表示するechoをGrowlへの通知に変更するプラグインです。  
[Growl/GNTP](https://addons.mozilla.org/ja/firefox/addon/growlgntp/ "Growl/GNTP :: Add-ons for Firefox")のアドオンが必要になりますがWindows/Linux/Macなど各種OSで使用できます。  
webサービスを利用するためのプラグインはコマンドの正否にechoが使われることが多いので、そういったプラグインを入れている人にはオススメです。

## コマンドラインを <Esc> でもあなたのこと、忘れない!! ([https://gist.github.com/2944628](https://gist.github.com/2944628 "コマンドラインを <Esc> でもあなたのこと、忘れない!!"))
途中でESCを押して消してしまった入力を履歴に残します。  
入力途中にググりたくなっても安心です。

## lazy command
コマンドを遅延実行します。  
プラグインが読み込まれないと実行できないコマンドを.vimperatorrcに書けるようになります。
```
# lazyコマンドを定義
command! -nargs=+ lazy autocmd VimperatorEnter .* <args>

# URL補完の設定。はてなブックマーク拡張のHとadd-url-completer.twのTに遅延実行が必要
lazy set complete=slHT

# loginManager.js用。各種Webサービスに自動的にログインする
lazy :login hatena
lazy :login livedoor
lazy :login nicovideo
```

## autocmd 駆動時のエコーをやめる ([https://gist.github.com/1028562](https://gist.github.com/1028562 "autocmd 駆動時のエコーをやめる"))
autocmdが静かになります。  
autocmdは自動処理のためのもので`:help :autocmd`に詳しく書かれています。
``` vim 便利なautocmd一例
" Gmail/LDR/はてブでは新規タブをバックグラウンドで開く
autocmd LocationChange '^(?!https?://(mail\.google\.com/(mail|a)/|reader\.livedoor\.com/reader/|b\.hatena\.ne\.jp/(?!(entry|articles|guide))))' :set! browser.tabs.loadDivertedInBackground=false
autocmd LocationChange '^https?://(mail\.google\.com/(mail|a)/|reader\.livedoor\.com/reader/|b\.hatena\.ne\.jp/(?!(entry|articles|guide)))' :set! browser.tabs.loadDivertedInBackground=true
```

## サイト内検索
Googleを使ってサイト内検索するコマンドです。  
オリジナルより少し変更を加えていて、ディレクトリ名が必要な@wikiやはてなダイアリーに対応しています。
``` javascript
nnoremap ,/ :sitesearch<Space>

javascript <<EOM
commands.addUserCommand(
  ['sitesearch'],
  'Search in this site',
  function (args) {
    let url = window.content.location.hostname;
    [
      /d\.hatena\.ne\.jp/,
      /www\d+\.atwiki\.jp/,
    ].forEach(function(r){
      url += r.test(url) ? '/' + window.content.location.pathname.split('/')[1] : '';
    });
    liberator.open(
      'http://www.google.com/search?q=' +
        encodeURIComponent(args.literalArg) +
        '+site%3A' +
        url,
      liberator.NEW_TAB
    );
  },
  {
    completer: function (context) completion.url(context, 'S'),
    literal: 0,
  },
  true
);
EOM
```

## vimpMonkey [http://d.hatena.ne.jp/wlt/20110105/1294204461](http://d.hatena.ne.jp/wlt/20110105/1294204461 "VimperatorでGreasemonkeyみたいな事をする - wltの日記")
VimperatorでGreasemonkeyみたいな事をするためのユーティリティ関数です。  
例では@wikiのページタイトルの前後を入れ替えています。ツリー型タブユーザにオススメです。
``` javascript vimpMonkey一例
javascript <<EOM
// @wikiのページタイトル - を区切りに前後入れ替え
vimpMonkey('http://www\\d+\\.atwiki\\.jp', function(){
  content.document.title = content.document.title.replace(/(.*) - (.*)/, '$2 - $1');
});

// VimperatorでGreasemonkeyみたいな事をする - wltの日記 - http://d.hatena.ne.jp/wlt/20110105/1294204461
function vimpMonkey(urlRegexPattern, func) {
  var cmd = eval('(function(args) {' +
    'var content = tabs.getTab(args.tab - 1).linkedBrowser.contentWindow;' +
    'var unsafeContent = content.wrappedJSObject;' +
    func.toSource() + '();' +
  '})');
  autocommands.add('PageLoad', urlRegexPattern, cmd);
}
EOM
```
