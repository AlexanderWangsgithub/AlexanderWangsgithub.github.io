---
layout: post
title: 使用Vagrant作为开发环境
categories: [blog]
tags: [Efficiency]
description: 
---

使用Vagrant作为开发环境
使用Vagrant作为开发环境的好处是
- 一次配置，到处可用，并且跨系统;
- 在你手边只有一个windows电脑时，你依然可以快速配置进行开发工作。
- 整个团队保持

## 下载

- [VirtualBox 下载地址](https://www.virtualbox.org/wiki/Downloads)

- [Vagrant 下载地址](https://www.vagrantup.com/downloads.html)

- [目标操作系统下载地址](http://www.vagrantbox.es/)

  ### 快捷命令

  #### windows

  ```shell
  wget http://download.virtualbox.org/virtualbox/5.1.2/VirtualBox-5.1.2-108956-Win.exe
  wget https://releases.hashicorp.com/vagrant/1.8.5/vagrant_1.8.5.msi
  wget https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box
  ```

  #### mac

  ```shell
  wget http://download.virtualbox.org/virtualbox/5.1.2/VirtualBox-5.1.2-108956-OSX.dmg
  wget https://releases.hashicorp.com/vagrant/1.8.5/vagrant_1.8.5.dmg
  wget https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box
  ```

## 安装

```shell
mkdir -p ~/workspace/vagrant/ubuntu
cd ~/workspace/vagrant/
cp ~/Downloads/ubuntu-14.04-amd64.box ./
vagrant box add "ubuntu" ubuntu-14.04-amd64.box
cd ubuntu
vagrant init "ubuntu"
vagrant up
vagrant ssh
```
## 配置
### 修改配置文件`vi Vagrantfile`
	```
 	config.vm.network "public_network", ip: "172.16.33.199"
 	config.vm.synced_folder "/Users/wanggang/workspace/ubuntu/data", "/home/vagrant"
	```
设置公有网络，可以同局域网直接访问。
设置共享文件夹，便于数据交互。也就是说，开发在主机上，运行在虚拟机上。

### 文件共享问题
参考[How to install VirtualBox Guest Additions for Linux](http://www.tuicool.com/articles/U3U73u)
#### 下载
```
下载对应VBox版本的GuestAdditions。
vagrant@vagrant-ubuntu-trusty:~$ wget http://download.virtualbox.org/virtualbox/5.1.2/VBoxGuestAdditions_5.1.2.iso
```
#### 挂载
```
sudo mount -o loop VBoxGuestAdditions_5.1.2.iso /mnt
mount: block device /home/vagrant/workspace/WGET/VBoxGuestAdditions_5.1.2.iso is write-protected, mounting read-only
```
#### 安装
```
cd /mnt
sudo ./VBoxLinuxAdditions.run 
```
### 重加载配置
`vagrant reload`

## 为什么不用docker？
docker的也可以建立虚拟机开发环境，但是命令方式、文件共享完全不适合开发。