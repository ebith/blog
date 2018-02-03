---
title: "Raspberry Pi Zero Wのセットアップ"
date: 2018-01-28T01:36:03+09:00
draft: false
---

## SDカードの準備
1. [Download Raspbian for Raspberry Pi](https://www.raspberrypi.org/downloads/raspbian/)から落としたRaspbian Liteを[Etcher](https://etcher.io/)で焼く。  
2. `/boot/ssh`を作る。
3. `/boot/wpa_supplicant.conf`にWifi設定を書き込む。
4. `/boot/config.txt`を変更する。

### /boot/wpa\_supplicant.conf
```
country=JP
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
  ssid="foo"
  psk=bar
}
```

### /boot/config.txt
使わないオーディオをオフにし、GPUメモリは最小、トラブル対策にシリアル通信をオン。  
シリアル通信はOSの起動で転ける様な時でも原因を確認できる。
```
#dtparam=audio=on
gpu_mem=16
enable_uart=1
```

## Raspbian Stretch Liteのセットアップ
`ssh pi@raspberrypi.local`で繋がる。デフォルトのパスワードは`raspberry`  
接続できたら母艦から`ssh-copy-id pi@raspberrypi.local`する。  
`raspi-config`ではホスト名とパーティションの拡大とロケール、タイムゾーンあたりを確認。  
適当なタイミングで`ssh-keygen -t ed25519`してGitHubに登録しておく。

### 初期設定
```sh
passwd
sudo apt update
sudo apt upgrade
sudo raspi-config
echo "NTP=ntp.nict.jp" | sudo tee -a /etc/systemd/timesyncd.conf
echo -e "Port 75826\nPasswordAuthentication no\nPermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
```

### いつもの環境をつくる
母艦からscpで常用ツールのARMv6バイナリを送りつけておく。peco, ghq, exaとか。  
chshのあと新たなログインでは反映されず再起動が必要なのが謎。  
vimの設定ファイルはVPSにリポジトリがあるのでそっちから取って来る。公開鍵の登録とか`~/.ssh/config`の編集が必要。
```sh
sudo apt install git zsh vim tmux gawk
sudo update-alternatives --config editor
ghq get ebith/dotfiles
ln -s ~/.ghq/github.com/ebith/dotfiles/.config ~/.config
ln -s ~/.ghq/github.com/ebith/dotfiles/.gitconfig ~/.gitconfig
ln -s ~/.ghq/github.com/ebith/dotfiles/.zshenv ~/.zshenv
ln -s ~/.ghq/github.com/ebith/dotfiles/.zshrc ~/.zshrc
ln -s ~/.ghq/github.com/ebith/dotfiles/.tmux.conf ~/.tmux.conf
curl -sL --proto-redir -all,https https://raw.githubusercontent.com/zplug/installer/master/installer.zsh| zsh
chsh -s /bin/zsh
sudo systemctl reboot
zplug install
exec $SHELL -l
rm .bash_history .bash_logout .bashrc .profile
mkdir -p ~/.vimlocal/{backup,swap,undo}
git clone vps:bare/vimfiles.git .vim
```

