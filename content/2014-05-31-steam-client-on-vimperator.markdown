---
comments: true
date: 2014-05-31T00:00:00Z
published: true
title: VimperatorプラグインなSteamクライアント作った
url: /2014/05/31/steam-client-on-vimperator/
---

![:steam play dark](http://i.gyazo.com/2c0610f54dd67fe0ea2b567f0f4ed2ae.png)  
こんな感じ

![:steam play -sort playtime! dark](http://i.gyazo.com/eca7b1576b65dba9457efd06a6465e10.png)  
ソートもできる

### 使い方
- .vimperatorrcにlet g:steam\_id = "7656119XXXXXXXXXX"という感じでSteamIDを設定しておく
    - SteamIDはプロフィールページで:echo content.wrappedJSObject.g\_rgProfileData.steamidを実行するとわかる
- プラグイン読み込む [https://gist.github.com/ebith/352db6741eb4df8231ee](https://gist.github.com/ebith/352db6741eb4df8231ee)
- あとはsteamコマンドを使うだけ
    - 文字入力で絞り込んでTabで選択の後Enterで起動する
