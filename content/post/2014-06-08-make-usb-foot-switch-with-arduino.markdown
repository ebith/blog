---
comments: true
date: 2014-06-08T00:00:00Z
published: false
title: Arduino Leonardoを使ってUSBフットスイッチを自作する
url: /2014/06/08/make-usb-foot-switch-with-arduino/
---

## 準備
[vim-plugins - コマンドライン+VimでArduinoをはじめよう - Qiita](http://qiita.com/izumin5210/items/5d6f8cb0c3d130ac96e9)

ino がbrew install coreutilsした環境に対応してなかったので`/usr/local/opt/python/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/ino/commands/upload.py`を書き換えて対応
```diff
- ret = subprocess.call([self.e['stty'], file_switch, port, 'hupcl'])
+ ret = subprocess.call(['/bin/stty', file_switch, port, 'hupcl'])

```

ino.iniを使うとbuildやuploadでオプション指定する必要なくなるので更に便利
```sh
echo 'board-model = leonardo' > ino.ini
```

たまに`Couldn’t find a Leonardo on the selected port. Check that you have the correct port selected. If it is correct, try pressing the board's reset button after initiating the upload.`とかなるのは何でもいいのでArduino.appから書き込むと直る

## フットスイッチのために

