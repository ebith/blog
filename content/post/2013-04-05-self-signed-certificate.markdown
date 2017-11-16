---
comments: true
date: 2013-04-05T00:00:00Z
published: true
title: オレオレ認証局をたててオレオレ証明書を発行する
url: /2013/04/05/self-signed-certificate/
---

## 概要
- CA.(sh|pl)を使わず、openssl.cnfの変更も最小限で済ます
- オレオレ認証局の証明書をブラウザに登録して警告が出ないようにする
- ワイルドカードなサーバー証明書を作る
- クライアント証明書も作る

## 下準備
``` sh
sudo cp /etc/ssl/openssl.cnf /etc/ssl/openssl.cnf.orig
vi /etc/ssl/openssl.cnf
  -dir            = ./demoCA
  +dir            = .

  +[ feelmy_net ]
  +subjectAltName = DNS:feelmy.net,DNS:*.feelmy.net
```

## ルート証明書を作る
``` sh
mkdir ca
cd ca
touch index.txt
echo 01 > serial
mkdir newcerts private
openssl req -new -newkey rsa:2048 -keyout private/cakey.pem -out careq.pem
  Country Name (2 letter code) [AU]:JP
  State or Province Name (full name) [Some-State]:Nara
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:Oiraya Factory
  Common Name (e.g. server FQDN or YOUR name) []:CA
chmod 400 private/cakey.pem

openssl ca -in careq.pem -out cacert.pem -selfsign -days 3650 -extensions v3_ca -batch
rm careq.pem
chmod 444 cacert.pem newcerts/01.pem
```

## サーバー証明書を作る
``` sh
openssl req -new -newkey rsa:2048 -nodes -keyout cert.key -out cert.csr
  Country Name (2 letter code) [AU]:JP
  State or Province Name (full name) [Some-State]:Nara
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:Oiraya Factory
  Common Name (e.g. server FQDN or YOUR name) []:*.feelmy.net
openssl ca -in cert.csr -out cert.crt -days 3650 -extensions feelmy_net -batch
rm cert.csr
chmod 400 cert.key
chmod 444 cert.crt
```

## クライアント証明書を作る
``` sh
openssl req -new -newkey rsa:2048 -nodes -keyout client.key -out client.csr
  Country Name (2 letter code) [AU]:JP
  State or Province Name (full name) [Some-State]:Nara
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:Oiraya Factory
  Common Name (e.g. server FQDN or YOUR name) []:ebith
openssl ca -in client.csr -out client.crt -days 3650 -extensions feelmy_net -batch
openssl pkcs12 -export -in client.crt -inkey client.key -out client.p12
rm client.csr client.crt client.key
chmod 400 client.p12
```

## 雑感とか
今思うとディレクトリは名前の変更にとどめたほうがファイルがごちゃっとならなくて良かったっぽい。  
普通にCommon Nameを\*.feelmy.netにして証明書を作るとfeelmy.netで通用しない。  
それを突破するにはSubject Alternative Nameを使うしかないっぽいけどSANを使うとopenssl.cnfの編集を避けられないようでつらい。  

作った証明書類はこんな感じにリネームしといた。
``` sh
mv cacert.pem oiraya-ca.pem
mv cert.key feelmy.net.key
mv cert.crt feelmy.net.crt
mv client.p12 feelmy.net.p12
```

## 参考にしたとこ
- [OpenSSLでオレオレ認証局を立ち上げる | www.tietew.jp](http://www.tietew.jp/2007/01/30/how-to-setup-ca-using-openssl "OpenSSLでオレオレ認証局を立ち上げる | www.tietew.jp")
- [クライアント証明書の発行](http://www.daily-labo.com/ygg16_3.html "クライアント証明書の発行")
