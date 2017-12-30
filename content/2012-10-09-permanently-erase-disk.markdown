---
comments: true
date: 2012-10-09T00:00:00Z
published: true
title: HDDのデータを念入りに消去する
url: /2012/10/09/permanently-erase-disk/
---

``` sh
sfdisk -l                   # 消したいディスクを確認する
shred -v -n 3 -z /dev/hoge  # ランダムデータを3回書き込んだのちゼロフィル
```
大抵のディストリで可能らしいけど[SystemRescueCd](http://www.sysresccd.org/SystemRescueCd_Homepage "SystemRescueCd")がシンプルで扱いやすい。
