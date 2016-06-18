# vagrant-pixiv-isucon2016

## Overview

Pixivさんの[社内ISUCON2016](https://github.com/catatsuy/private-isu)とほぼ同じ環境を構築するためのVagrantfileです。

## Usage

- vagrant実行環境を用意する
- このリポジトリ内のVagrantfileを手元に用意する
  - 必要に応じてVagrantfileを編集する
- Vagrantfileがあるディレクトリで`vagrant up`を実行する
  - ベンチマーク用サーバ(bench)と参加者用サーバ(image)が起動
- Ansibleによるプロビジョニングが完了したら`vagrant ssh`を実行する
```
vagrant ssh bench
vagrant ssh image
```
- ベンチマークを実行する
```
/opt/go/bin/benchmarker -t http://(imageのIPアドレス)/ -u /opt/go/src/github.com/catatsuy/private-isu/benchmarker/userdata
```
## 動作確認

macOS + VirtualBox 5.0.20 + Vagrant 1.8.1で動作確認済です。
VMWare Desktopやlxcでも動作するかもしれませんが未確認です。

## 本来の設定と異なるところ

- 本来のベンチマークサーバはc4.xlarge(vCPU 4, メモリ7.5GB)ですが、メモリーの割り当ては1GBに設定しています

## FAQ

### virtualboxで以下のようなエラーメッセージが表示される

> The provider 'virtualbox' that was requested to back the machine
> 'default' is reporting that it isn't usable on this system. The
> reason is shown below:
> 
> Vagrant has detected that you have a version of VirtualBox installed
> that is not supported. Please install one of the supported versions
> listed below to use Vagrant:
> 
> 4.0, 4.1, 4.2, 4.3

Vagrantのバージョンが古い可能性があります。最新のVagrantを使用してください。

### vagrant upを実行するとvboxsfのエラーが表示される

> Failed to mount folders in Linux guest. This is usually because
> the "vboxsf" file system is not available. Please verify that
> the guest additions are properly installed in the guest and
> can work properly. The command attempted was:
> 
> mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3` vagrant /vagrant
> mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant
> 
> The error output from the last command was:
> 
> /sbin/mount.vboxsf: mounting failed with the error: No such device

[これと同じ現象](http://qiita.com/hapicky/items/a7f9d56588f96d005fad)と思われます。気にせず`vagrant provision`を実行してください。

### vagrant upを実行するとAnsibleのエラーが表示される

何らかの理由によりprovisionに失敗したものと思われます。`vagrant provision`を実行してください。

### プログラムの動かし方がわからない

以下をご確認ください。

- [ISUCON6出題チームが社内ISUCONを開催！AMIも公開！！ - pixiv inside](http://inside.pixiv.net/entry/2016/05/18/115206)
- [catatsuy/private-isu](https://github.com/catatsuy/private-isu)
- [社内ISUCON 当日マニュアル](https://github.com/catatsuy/private-isu/blob/master/manual.md)

### ブラウザで動作確認ができない

Vagrantfileのネットワーク設定がデフォルトのままなので適当に変更してください。
よくわからない場合は`# config.vm.network "private_network", ip: "192.168.33.10"`のコメントを外してブラウザから192.168.33.10にアクセスしてみてください。
