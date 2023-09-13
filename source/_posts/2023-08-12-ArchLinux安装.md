---
title: ArchLinux安装
description: 本文介绍使用archinstall脚本安装ArchLinux桌面环境，以及安装完成之后的配置，但不包含使用archinstall脚本的配置过程
mathjax: true
tags:
  - Linux
  - ArchLinux
  - 开发环境
categories:
  - 工具文档
abbrlink: 2013454d
date: 2023-08-12 19:01:34
---

# ArchLinux安装

## 关键词：ArchLinux安装

> 使用官方提供的安装脚本：archinstall，快速安装ArchLinux，并对系统进行日常使用的必要配置

## 1. 镜像制作

### 1.1 镜像下载

[Arch Linux - Downloads](https://archlinux.org/download/)

### 1.2 镜像制作工具下载

[Index of /downloads](http://rufus.ie/downloads/)

### 1.3 使用Rufus制作启动U盘

#### 1.3.1 选择镜像文件

#### 1.3.2 改分区类型为GPT

<img src="https://blog.love.sc.cn/image/202308121142724.png?x-image-process=style/style-bucket" alt="image.png" />
图1:改分区类型为GPT

#### 1.3.3 dd模式写入镜像

![image.png](https://blog.love.sc.cn/image/202308121141468.png?x-image-process=style/style-bucket)
图2:dd模式写入镜像

## 2. BIOS设置

### 2.1 进入BIOS设置

### 2.2 禁用安全模式

### 2.3 修改启动顺序，将U盘启动顺序放在第一个

## 3. 使用archinstall安装系统

//TODO

## 4. 基本设置

### 4.1 安装中文字体

```bash
pacman -S ttf-hannom noto-fonts noto-fonts-extra noto-fonts-emoji noto-fonts-cjk adobe-source-code-pro-fonts adobe-source-sans-fonts adobe-source-serif-fonts adobe-source-han-sans-cn-fonts adobe-source-han-sans-hk-fonts adobe-source-han-sans-tw-fonts adobe-source-han-serif-cn-fonts wqy-zenhei wqy-microhei
```

### 4.2 开启sshd服务，远程连接Linux

```bash
systemctl start sshd

systemctl enable sshd
```

### 4.3 连接过变更密码导致报错解决方法：进入  C:\Users\用户名\.ssh，删除其中文件

### 4.4 ssh连接root用户

> 也可以通过ssh连接到普通用户，提权到root用户，不用改配置文件

配置文件

```bash
vim	/etc/ssh/sshd_config
```

将PermitRootLogin no 改为 PermitRootLogin yes

### 4.5 systemctl enable --now sshd    (设置sshd开机自启，可选)

## 5. 显卡驱动

### 5.1 安装显卡驱动

```bash
pacman -S xf86-video-intel
                                            (Intel核心显卡驱动，用Intel核显就装，否则不用装)
pacman -S mesa nvidia(-lts) nvidia-settings nvidia-dkms nvidia-utils nvidia-prime
                                            (nvidia显卡驱动，用nvidia显卡就装，否则不用装)
pacman -S xf86-video-amdgpu
                                              (AMD显卡驱动，用amd显卡的就装)
```

### 5.2 例

我的笔记本，AMD核显以及3060显卡，所以安装后两个。
nvidia-dkms 与 nvidia-lts 不兼容，如果装lts驱动的话无需安装dkms 。

```bash
pacman -S mesa nvidia-lts nvidia-settings  nvidia-utils nvidia-prime xf86-video-amdgpu
```

## 6. 添加存储库

### 6.1 ArchLinuxCN

```bash
vim /etc/pacman.conf
#添加以下内容
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

这是中科大的源，你也可以选择清华、阿里等

更新GPG密钥

```bash
pacman -Sy archlinuxcn-keyring
```

### 6.2 Mutilab

在安装的时候已经添加了，配置文件中有，找到，然后放开注释即可

## 7. 必要软件

### 7.1 安装git

```bash
pacman -S git
```

### 7.2 安装 AUR工具(yay)

```bash
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
# 容易编译失败，多试几次
```

### 7.3 输入法

#### 7.3.1. 安装 fcitx5

```bash
sudo pacman -S fcitx5-im fcitx5-qt fcitx5-gtk fcitx5-chinese-addons fcitx5-pinyin-zhwiki
```

#### 7.3.2. 配置输入法环境

```bash
sudo vim /etc/environment
#加入以下内容：

GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus

#重启后即可生效
```

