---
comments: true
date: 2013-06-02T00:00:00Z
published: true
title: BitTorrent SyncをVPSにセットアップする
url: /2013/06/02/bittorrent-sync/
---

## ダウンロードと起動
``` sh
wget http://btsync.s3-website-us-east-1.amazonaws.com/btsync_x64.tar.gz
tar xvfz btsync_x64.tar.gz
mv btsync ~/bin/
rehash
btsync
sudo ufw allow xxxxxx
```

## 外からWebUIにアクセスできるようにする
``` nginx
server {
  listen 443;
  server_name hoge.feelmy.net;

  ssl on;
  ssl_certificate server.crt;
  ssl_certificate_key server.key;
  ssl_verify_client on;
  ssl_client_certificate cacert.pem;
  if ($ssl_client_s_dn !~ "CN=ebith") {
    return 403;
  }

  location / {
    proxy_redirect off;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:8888;
  }
}
```

## 使ってみた感じ
- P2Pなので速度出る
- フォルダ単位の共有で、追加や削除が簡単
- ワンタイム、リードオンリーなどの共有オプション有り
- .Syncignoreで除外ファイル設定可能

起動しっぱなしのPCとかリソース余らせてるサーバがある場合は良い選択っぽい。
