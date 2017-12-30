---
comments: true
date: 2013-01-05T00:00:00Z
published: true
title: さくらのVPS 2G(Ubuntu 12.04)でTerrariaServer(TShock)を動かす
url: /2013/01/05/terraria-server-with-ubuntu-server-12.04/
---

## サーバーのセットアップ
```
sudo apt-get install mono-complete
wget https://github.com/downloads/TShock/TShock/TShock-4.0.zip
unzip TShock-4.0.zip
mv serverplugins ServerPlugins
mono TerrariaServer.exe
  # 初期設定ではボス召喚アイテムが使えないので許可する
  modgroup add guest summonboss
```

## クライアントから管理者登録
```
/auth *******
/user add ebith:******** superadmin
/login ebith ********
/auth-verify
```

## 運用において
`tmux new -s terraria`とかしておくとでサーバーコンソールがどこからでも使えて便利。  
サーバーの基本的な設定は`tshock/config.json`を直接書き換える。[Configuration File Docs](https://tshock.atlassian.net/wiki/display/TSHOCKPLUGINS/Configuration+File+Docs "Configuration File Docs - TShock Documentation - Confluence")を参照。  
`group perm guest`でguestユーザが使えるコマンド一覧が表示される。`modgroup (add|del) guest hoge`で権限操作。[Commands/Permissions list](https://tshock.atlassian.net/wiki/pages/viewpage.action?pageId=3047433 "Commands/Permissions list - TShock Documentation - Confluence")がコマンド一覧。  
サーバーに接続しただけの未登録ユーザはguestに所属する。身内向けならサーバーパスワード有りにして管理者以外は全員guestが楽そう。権限管理的な意味で。
