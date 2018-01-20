---
title: "NintendoSwitchをPCから操作する"
date: 2018-01-21T08:30:59+09:00
draft: false
---
{{% twitter tweetid="954858876028907521" %}}
ゼノブレイド2のゴルトムント飛行甲板で自動採集している様子。

既に[Splatoon2で自動ドット打ち](https://github.com/shinyquagsire23/Switch-Fightstick)や[ゼルダの雪玉ボウル自動化](https://github.com/bertrandom/snowball-thrower)があり、これらの元である[progmem/Switch-Fightstick](https://github.com/progmem/Switch-Fightstick)をシリアル通信経由で操作するようにしたやつ。  
Aボタン連打の様な単純なことをしたいだけなら雪玉ボウルのJoystick.cを書き換えるのが手っ取り早いしそれで[用は済んでる](https://twitter.com/ebith/status/952892746305495040)んだけどせっかくなので作った。  
[ebith/Switch-Fightstick: Automate operation for Nintendo Switch](https://github.com/ebith/Switch-Fightstick)


## ハードウェア
![ハードウェアの例](https://pbs.twimg.com/media/DT1wGHQVAAYdYAs.jpg:small)

- 適当なマイコンボード ([Pro Micro ATmega32U4 ATMEGA32U4 AU 5V/16MHz](https://www.aliexpress.com/item/Free-Shipping-New-Pro-Micro-for-arduino-ATmega32U4-5V-16MHz-Module-with-2-row-pin-header/2040881593.html))
  - 素人がはんだ付けするには怖い位置にatmega32u4が乗ってる。Arduino Microのブートローダーが書き込まれていた
- USBシリアル変換アダプタ ([超小型ＵＳＢシリアル変換モジュール](http://akizukidenshi.com/catalog/g/gM-08461/))
- USB 2.0 A <-> Micro-B ケーブル * 2

あたりが最低限必要。ブレッドボードの代わりに[スルホール用テストワイヤ](http://akizukidenshi.com/catalog/g/gC-09831/)という手もある。リセットはピンセットとか。

マイコンボードを新しく買うならPro Micro系統(ATMega32U4搭載ボード)が安くて良い。  
最近発売されたAdafruitの[Itsy Bitsy 32u4](https://learn.adafruit.com/introducting-itsy-bitsy-32u4?view=all)が$10でリセットボタン有りピン省略無し強固そうなUSB端子で良さそうだけど入手難易度が高い。  
その他バリエーションや詳細に関しては[(Arduino) Pro Micro](https://ht-deko.com/arduino/promicro.html)が詳しい。  
[A-Star 32U4](https://www.switch-science.com/catalog/list/?keyword=A-Star+32U4)なんかもATMega32U4が載ってる。  
こだわりが無ければAliExpressで売ってるやつが安いが初期不良や納期にリスクがある。

## ソフトウェア
ポッ拳コントローラ(WiiU)に偽装することでスイッチを操作する。  
Macでやったがavr-dudeとavr-gccがあればなんでも良いはず。

### Pro MicroをPCに繋いでソフトウェアを書き込む
```sh
brew install avr-dude osx-cross/avr/avr-gcc
git clone --recursive https://github.com/ebith/Switch-Fightstick.git
cd Switch-Fightstick
make
# ここでPro Microをリセット
avrdude -pm32u4 -cavr109 -D -P$(ls /dev/tty.usbmodem*) -b57600 -Uflash:w:Joystick.hex
```

## 動かす
### A連打を試す
```sh
pip install pyserial
./example/rapid-fire-a-button.py /dev/tty.usbserial*
```

もちろん`screen /dev/tty.usbserial*`などから手入力することも可能で後述のコマンドを入力して改行(エンター)すると動く。

### コマンド
- `Button (Y|B|A|X|L|R|ZL|ZR|SELECT|START|LCLICK|RCLICK|HOME|CAPTURE|RELEASE)`
- `(LX|LY|RX|RY) (MIN|MAX|CENTER)`
- `HAT (TOP|TOP_RIGHT|RIGHT|BOTTOM_RIGHT|BOTTOM|BOTTOM_LEFT|LEFT|TOP_LEFT|CENTER)`
- 例
  - `Button ZR`
  - `RY MIN`
  - `HAT BOTTOM`

動作の確認には[HTML5 Gamepad Tester](http://html5gamepad.com/)が便利。

## 次回
Raspberry Pi Zero WをUSBデバイスにして同じことをしたいなと思って下調べしているところ。  
Zero WHが発表されて買いやすくなりそうだし、価格もそこそこだし、できることがめっちゃ増えるし楽しそう。
