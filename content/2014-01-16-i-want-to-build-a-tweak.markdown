---
comments: true
date: 2014-01-16T00:00:00Z
published: true
title: iOSのTweak作りたい
url: /2014/01/16/i-want-to-build-a-tweak/
---

## ゴール
連絡先の住所をタッチするとGoogle Mapsで開くTweakを作る

## 参考にしたところ
[ひとりぶろぐ » iOS 7の中身に興味津々！Jailbreakなしでファイルシステムを覗いてみたりしよう](http://hitoriblog.com/?p=19242)  
[dotfiles/install\_theos.sh at master · r-plus/dotfiles](https://github.com/r-plus/dotfiles/blob/master/install_theos.sh)  
[seekpoint: theos](http://seekpoint.blogspot.jp/search/label/theos)


## つまずいたところ
``` diff deb.mk
# homebrewで入れたcoreutilsを優先してるとエラー
- 	$(ECHO_NOTHING)echo "Installed-Size: $(shell du $(_THEOS_PLATFORM_DU_EXCLUDE) DEBIAN -ks "$(THEOS_STAGING_DIR)" | cut -f 1)" >> "$@"$(ECHO_END)
+ 	$(ECHO_NOTHING)echo "Installed-Size: $(shell /usr/bin/du $(_THEOS_PLATFORM_DU_EXCLUDE) DEBIAN -ks "$(THEOS_STAGING_DIR)" | cut -f 1)" >> "$@"$(ECHO_END)

# homebrewで入れたdpkgを使ってるとエラー
- 	$(ECHO_NOTHING)COPYFILE_DISABLE=1 $(FAKEROOT) -r dpkg-deb -b "$(THEOS_STAGING_DIR)" "$(_THEOS_DEB_PACKAGE_FILENAME)" $(STDERR_NULL_REDIRECT)$(ECHO_END)
+ 	$(ECHO_NOTHING)COPYFILE_DISABLE=1 $(FAKEROOT) -r dpkg-deb -Zgzip -b "$(THEOS_STAGING_DIR)" "$(_THEOS_DEB_PACKAGE_FILENAME)" $(STDERR_NULL_REDIRECT)$(ECHO_END)
```
``` diff Makefile
# デフォルトのMakefileだとiPhone 5sに未対応のバイナリができる
+ ARCHS = armv7 arm64
```

## だいたいの流れ
1. iFunBoxでMobilePhone.appをコピーしてきて`class-dump -H MobilePhone.app -o headers`した
2. `ag -i 'open' headers`した結果PhoneApplication.hにopenURLがあったのでこれにUIAlertViewを差し込んでみる
3. urlとか住所とか別アプリに飛ぶようなのをタッチした時にアラート出るのでURLのスキームがmapsのやつだけGoogle Mapsに投げれば良いんじゃないの
4. Browser Changerをいじくりまわして設定しなおしてみた結果、地図だけデフォルト変更できちゃったので解決
