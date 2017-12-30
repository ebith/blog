---
comments: true
date: 2014-01-02T00:00:00Z
published: true
title: iPhone 5s jailbreak memo
url: /2014/01/02/iphone-5s-jailbreak-memo/
---

## 参考にしたページ
- [[iOS] 『iOS 7.0～7.0.4』を完全脱獄する方法！『evasi0n 7』 iPhone 5sやiPad Airにも対応 | Tools 4 Hack](http://tools4hack.santalab.me/how-to-ios7-untethered-jailbreak-evasi0n7.html)
- [[iOS 7 脱獄] iPhone 5s（A7デバイス）で使用出来る 脱獄アプリ まとめ [随時更新] | Tools 4 Hack](http://tools4hack.santalab.me/ios7-a7-jbapp-tweak-arm64-compatibility-list.html)

## なんかやったこと
- サーバ落ち過ぎでうざいのでCydiaからUltrasn0wのリポジトリ消した
- mobileとrootのパスワード変更した
- /etc/hosts 差し替えて広告消した
- CCTogglesで追加されるSpringboardの隠し設定からParallax Effect消した
- /etc/ssh/sshd\_config を編集して公開鍵認証のみにした
- /etc/sudoers を編集してmobileでsudoできるようにした

## インストールしたパッケージとか
{% gist 8369853 %}
