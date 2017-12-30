---
comments: true
date: 2012-09-07T00:00:00Z
published: true
title: Gyazoのアップロード先をTumblrにするやつ(Gyamblr) API v2版 for OS X
url: /2012/09/07/gyamblr-for-osx(tumblr-api-v2)/
---

気づいたらAPI v1の提供が終了したとかでアップロードできなくなっていたのでv2版書きました。

## 使い方
scriptをGyazoのものと差し替えれば動きますが、tumblifeのgemが必要だったりOAuthの例に漏れず各種トークンが必要だったりするのでよしなにしてください。  
トークン類は[Tumblr](http://www.tumblr.com/oauth/apps "Tumblr")でアプリを登録して、oauth.rbを使うと良いと思います。  

{% gist 3657917 %}
