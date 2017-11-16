---
comments: true
date: 2012-09-08T00:00:00Z
published: true
title: 新PCにWindows 7入れた
url: /2012/09/08/install-windows-7/
---

Windows 7のインストールは3回目ぐらい、XPの頃と比べると本当に楽になった。  
OSのインストール後の流れをメモっておく。

## ドライバのインストール
- マザーボード系
    - チップセット
    - LAN
    - USB3
- GPU
- サウンドカード

## 設定
- サービスの停止
    - Superfetch
    - Windows Search
- SNP: 無効([ITアーキテクトの「やってはいけない」 - ［Windows 7編］ネットワーク設定を標準で使ってはいけない：ITpro](http://itpro.nikkeibp.co.jp/article/COLUMN/20100824/351391/ "ITアーキテクトの「やってはいけない」 - ［Windows 7編］ネットワーク設定を標準で使ってはいけない：ITpro"))
- システムの復元: 無効
- ページングファイル: 最小
- ハイバネーション: オフ
- バックアップ: 週1に設定

## ソフトのインストール
- [COMODO Internet Security](http://www.comodo.com/home/internet-security/free-internet-security.php "Free Internet Security, Download Internet Security Software Suite - Comodo")
    - インストールに関しては2chのテンプレが詳しい([2ch検索: [comodo internet security]](http://find.2ch.net/?STR=comodo+internet+security "2ch検索: [comodo internet security]"))
    - 日本語化([NipsuのBlog](http://torichuu.blog69.fc2.com/ "NipsuのBlog"))
- [Dropbox](https://www.dropbox.com/ "Dropbox - 生活をシンプルに")
- [FRAPS](http://www.fraps.com/ "FRAPS show fps, record video game movies, screen capture software")
- [Firefox](http://www.mozilla.jp/firefox/ "次世代ブラウザ Firefox — 高速・安全・カスタマイズ自在な無料ブラウザ")
- [Google 日本語入力](http://www.google.co.jp/ime/ "Google 日本語入力 - ダウンロード")
- [Growl for Windows](http://www.growlforwindows.com/gfw/ "Growl for Windows")
- [iTunes](http://www.apple.com/jp/itunes/download/ "アップル - iTunes - iTunesを今すぐダウンロード")
    - [iPhone 構成ユーティリティ](http://www.apple.com/jp/support/iphone/enterprise/ "Apple - サポート - iPhone - エンタープライズ")
    - C:\Users\ebith\AppData\Roaming\Apple Computer\MobileSyncとC:\Users\ebith\Music\iTunesをデータドライブに移動してシンボリックリンクをはっておく
- [Link Shell Extension](http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html "Link Shell Extension")
- [Skype](http://www.skype.com/intl/ja/home "オンラインで無料のSkypeインスタント通話と固定電話への格安通話 - Skype")
- [Steam](http://store.steampowered.com/ "Steam へようこそ")
- [TrueCrypt](http://www.truecrypt.org/ "TrueCrypt - Free Open-Source On-The-Fly Disk Encryption Software for Windows 7/Vista/XP, Mac OS X and Linux")
- [VirtualBox](https://www.virtualbox.org/ "Oracle VM VirtualBox")
    - [VBoxHeadlessTray](http://www.toptensoftware.com/VBoxHeadlessTray/ "Topten Software")
- [のどか](http://www.appletkan.com/nodoka.htm "汎用キーバインディング変更ソフト「のどか」")

その他のファイルはバックアップからコピーして戻す。  
ソフト自体はポータブルなのにインストーラー版しか提供しないソフトが多くてこういう時すごく面倒くさいよね。  

