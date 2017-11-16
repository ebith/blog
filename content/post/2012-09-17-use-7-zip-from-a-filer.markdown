---
comments: true
date: 2012-09-17T00:00:00Z
published: true
title: ファイラから7-zip(7zG.exe)使うようにすると捗る
url: /2012/09/17/use-7-zip-from-a-filer/
---

Windowsではずっと統合アーカイバプロジェクトのDLL群とそれらを使うアーカイバにお世話になってきたけど、  
[7-Zip](http://sevenzip.sourceforge.jp/ "7-Zip")のほうが速いので7-Zip付属の7zG.exe, 7z.exeとB2E(Bridge To Executables)に移行することにした。  
B2Eの本家は[Noah](http://www.kmonos.net/lib/noah.ja.html "Noah - kMonos.NET")だけど、[LhaForge](http://claybird.sakura.ne.jp/garage/lhaforge/ "泥巣 : 作品庫 - LhaForge")の方がコマンドラインオプションが充実していて使いやすいのでそっちを使う。

{% gist 3737689 %}
zip, 7z圧縮用と展開用のスクリプト。

```
  C:\usr\app\lfarchive\LhaForge.exe /b2e /e                   #展開
  C:\usr\app\lfarchive\LhaForge.exe /b2e /c:zip /method:Store #圧縮(Zip Store)
  C:\usr\app\lfarchive\LhaForge.exe /b2e /c:7z /method:LZMA2  #圧縮(7z LZMA2)
```
LhaForgeをファイラから呼ぶ場合こんな感じ。
