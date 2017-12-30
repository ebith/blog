---
comments: true
date: 2012-09-17T00:00:00Z
published: true
title: VPSのバックアップをDropboxに取る
url: /2012/09/17/server-backup-to-dropbox/
---

簡易的なバックアップで良いので取っておきたかったので、  
tarで固めてlzoで圧縮してDropboxに送るバックアップスクリプト書いた。  
[BASH Dropbox Uploader](http://www.andreafabrizi.it/?dropbox_uploader "BASH Dropbox Uploader")が必要。

{% gist 3735146 %}

DropboxのAPIからのアップロードが1ファイル150MBまでなので  
ファイル分割とかできるといいかもしれないけど今んとこ必要ないので付いてない。
