---
title: "Raspberry PiのファイルをBackblaze B2にバックアップする"
date: 2018-02-04T03:37:33+09:00
draft: false
---

SDカードはどうやってもそのうち死ぬ運命なのでお手軽にバックアップを取れるようにしておきたい。  
ツールは[rclone](https://rclone.org/)、 ストレージは[Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html)を選んだ。  
Backblaze B2は無料枠が設定されているのがとてもありがたい。  
Bucket毎にLifecycleの設定があるので古いのは消えるようにしておくと手間がなくて良さそう。  
バージョニングもあるしひたすらrclone syncし続ければそれで良いのでとにかく楽。

## rcloneのインストールと初回バックアップ
`--fast-list`を付けていないとTransactions Class CのCapにあっという間に到達するが付けるとメモリを食う。  
クレカを登録してClass CのDaily Capsを$1とかにしておくととりあえずは悩まなくて良くなる。
```sh
wget https://downloads.rclone.org/rclone-v1.39-linux-arm.zip
unzip rclone-v1.39-linux-arm.zip
sudo mv rclone-v1.39-linux-arm/rclone /usr/local/bin/
rclone config
rclone mkdir b2:RaspberryPi1
rclone sync --fast-list --exclude-from=/home/pi/.config/rclone/ignore.home --b2-chunk-size=12M /home/pi b2:RaspberryPi1/home
sudo rclone sync --fast-list --config=/home/pi/.config/rclone/rclone.conf --exclude-from=/home/pi/.config/rclone/ignore.etc --b2-chunk-size=12M /etc b2:RaspberryPi1/etc
```

### --exclude-fromの中身
etcの方はこれで足りてるのか怪しい。
#### home
```
/.ssh/
/external/
/local/
/.cache/
/.local/
node_modules/
```
#### etc
```
/passwd
/passwd-
/shadow
/shadow-
/group
/group-
/gshadow
/gshadow-
```

## systemdを使って毎週バックアップする
### ~/bin/backup.sh
```sh
#!/bin/bash
rclone sync --fast-list --config=/home/pi/.config/rclone/rclone.conf --exclude-from=/home/pi/.config/rclone/ignore.home --b2-chunk-size=12M /home/pi b2:RaspberryPi1/home
rclone sync --fast-list --config=/home/pi/.config/rclone/rclone.conf --exclude-from=/home/pi/.config/rclone/ignore.etc --b2-chunk-size=12M /etc b2:RaspberryPi1/etc
```

### ~/service/backup.service
```
[Unit]
Description=Backup

[Service]
Type=oneshot
ExecStart=/home/pi/bin/backup.sh
```

### ~/service/backup.timer
```
[Unit]
Description=Backup

[Timer]
OnCalendar=weekly
Persistent=true
RandomizedDelaySec=1h
```

```sh
chmod a+x ~/bin/backup.sh
sudo ln -s ~/service/backup.service /etc/systemd/system/
sudo ln -s ~/service/backup.timer /etc/systemd/system/timers.target.wants/
sudo systemctl start backup.timer
sudo systemctl list-timers
```
