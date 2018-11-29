---
comments: true
date: 2012-10-01T00:00:00Z
published: true
title: ラズベリーパイを手に入れた！
url: /2012/10/01/i-got-raspberry-pi/
---

<a href="http://www.flickr.com/photos/ebith/8038651368/" title="Raspberry Pi 1/2 by Ebith, on Flickr"><img src="https://farm9.staticflickr.com/8462/8038651368_d716208375_n.jpg" width="320" height="239" alt="Raspberry Pi 1/2"></a><a href="http://www.flickr.com/photos/ebith/8038648103/" title="Raspberry Pi 2/2 by Ebith, on Flickr"><img src="https://farm9.staticflickr.com/8316/8038648103_f163c4d92a_n.jpg" width="320" height="239" alt="Raspberry Pi 2/2"></a>  
6月末にRSに注文したものの、届く気配もなかったのでModMyPiに頼んでみると1週間で到着。  
残念ながらRev1だったけど、Arch Linuxを入れてHeadless運用でしばらく遊ぶことにする。

最初の接続までは[約3,300円で買えるLinuxパソコンRaspberry PiをMacで使う | ひとりぶろぐ](http://hitoriblog.com/?p=9733 "約3,300円で買えるLinuxパソコンRaspberry PiをMacで使う | ひとりぶろぐ")を参考にした。  
[RasPiWrite](https://github.com/exaviorn/RasPiWrite "exaviorn/RasPiWrite")はエラーを吐くので適当なfork版を使う。

``` sh
nmap -sP 192.168.0.0/24
ssh root@x.x.x.x
```
alarmpiのホスト名だと繋げなかったのでIP探してsshで接続。

``` sh
pacman -Syu     # Arch Linuxのアップデート
passwd          # rootのパスワード変更
adduser         # 普段使い用のユーザー追加(sudo用にwheelグループにも所属させておく)

# sudo導入
pacman -S sudo
visudo
  %wheel ALL=(ALL) ALL
  Defaults timestamp_timeout=30

# 固定IPにする(systemd/Services - ArchWiki - https://wiki.archlinux.org/index.php/Systemd/Services#Static_Ethernet_network)
vi /etc/conf.d/network
  interface=eth0
  address=192.168.0.32
  netmask=24
  broadcast=192.168.0.255
  gateway=192.168.0.1
vi /etc/systemd/system/network.service
  [Unit]
  Description=Network Connectivity
  Wants=network.target
  Before=network.target
  [Service]
  Type=oneshot
  RemainAfterExit=yes
  EnvironmentFile=/etc/conf.d/network
  ExecStart=/sbin/ip link set dev ${interface} up
  ExecStart=/sbin/ip addr add ${address}/${netmask} broadcast ${broadcast} dev ${interface}
  ExecStart=/sbin/ip route add default via ${gateway}
  ExecStop=/sbin/ip addr flush dev ${interface}
  ExecStop=/sbin/ip link set dev ${interface} down
  [Install]
  WantedBy=multi-user.target
vi /etc/resolve.conf
  nameserver 143.90.130.165
  nameserver 143.90.130.39
systemctl disable dhcpcd@eth0.service
systemctl enable network.service

# ssh, sshd周り
vi /etc/ssh/sshd_config
  PermitRootLogin no
  PubkeyAuthentication yes
  PasswordAuthentication no
  Port xxxxx
systemctl restart sshd.service
su ebith
ssh-keygen  # ローカルにコピーしたり~/.ssh/config設定したりしとく
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

# DDNS
vi ~/.crontab
  curl 'http://dynamic.name-services.com/interface.asp?Command=SetDNSHost&HostName=home&Zone=feelmy.net&DomainPassword=xxxxx'
crontab ~/.crontab

# ロケール
sudo vi /etc/locale.gen
  ja_JP.UTF-8 UTF-8
sudo locale-gen
sudo vi /etc/locale.conf
  LANG=ja_JP.UTF-8

# 環境設定
sudo pacman -S git rsync vim zsh
git clone git@github.com:ebith/dotfiles.git
cd dotfiles
git sm init
git sm update
cd
./dotfiles/create_link.sh
chsh -s /bin/zsh
```

[うちのbotがエアコンの操作方法を覚えた - ぬいぐるみライフ(仮)](http://d.hatena.ne.jp/mickey24/20120911/air_conditioner_with_arduino "うちのbotがエアコンの操作方法を覚えた - ぬいぐるみライフ(仮)")とかすごく楽しそうなのでUSBの赤外線リモコン繋いだら簡単に似たようなことできるのかなとか思ったけど、調度良さそうなのが見つからないしそう簡単な話ではなかった。
