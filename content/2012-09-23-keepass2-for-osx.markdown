---
comments: true
date: 2012-09-23T00:00:00Z
published: true
title: KeePass2のkdbxをMacでも快適に読めるようにした
url: /2012/09/23/keepass2-for-osx/
---

Macbook触ってる時間が長くなってくるとKeePass2がまともに動かないのが凄く気になってきたし、  
かと言って1Passwordは凄く高いのでどうにかしたくて右往左往してたらkdbxを読めるPerlモジュールを見つけたので頑張った。

PerlのFile::KeePassとIO::Prompter、[mooz/percol](https://github.com/mooz/percol "mooz/percol")が必要。

{% gist 3767494 %}

```sh
./keepass.pl ~/ebith.kdbx
```

みたいな感じで使う。
percolのお陰で絞込み検索が素晴らしい感じなので便利。
