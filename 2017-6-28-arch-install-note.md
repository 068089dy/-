# Archlinux安装小记

## 1.联网
(1).列出网口
```
# ip link
```
(2).连接wifi
```
# wifi-menu
```
## 2.挂载分区
(1).建立并挂载系统分区
```
# mkfs.ext4 /dev/sda6
# mount /dev/sda6 /mnt
```
(2).建立并挂载efi分区
```
# mkfs.vfat /dev/sda2
# mkdir /mnt/boot
# mount /dev/sda2 /mnt/boot
```
(3).建立并挂载交换分区
```
# mkswap /dev/sda3
# swapon /dev/sda3
```
## 3.安装基本系统
(1).改源
```
# nano /etc/pacman.d/mirrorlist
# Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
# pacman -Syy
```
(2).安装基本系统
```
# pacstrap /mnt base base-devel
```
额外安装（几个连接无线网络需要的软件包）
```
# pacstrap /mnt iw dialog wpa_supplicant wpa_actiond
```
(3).生成 fstab
```
# genfstab -U /mnt >> /mnt/etc/fstab
```
## 4.进入系统
```
# arch-chroot /mnt /bin/bash
```
## 5.时区语言设置

(1).设置时区(这一步执行失败，不影响系统安装)
```
# ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
(2).设置时间标准 为 UTC，并调整 时间漂移:
```
# hwclock --systohc --utc
```
(3).指定需要的本地化类型
```
# nano /etc/locale.gen
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_TW.UTF-8 UTF-8
```
生成 locale 讯息
```
# locale-gen
```
(4).将系统 locale 设置为en_US.UTF-8
```
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```
(5).设置主机名
```
# echo myhostname > /etc/hostname
```
(6).设置 root 的密码
```
# passwd
```
## 6.安装引导
```
# pacman -S efibootmgr dosfstools
# pacman -S grub os-prober  
```
UEFI 用户这么做：
```
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck
```

## 7.安装后相关配置
安装中文字体
```
# pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji
```
激活需要的服务

(1).
```
# systemctl enable gdm
```
(2).网络管理
```
# systemctl enable NetworkManager
```
## 8.编码设置
查看编码
```
# locale
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_MESSAGES to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
LANG=en_US.UTF-8
LC_CTYPE=en_US.UTF-8
LC_NUMERIC=C
LC_TIME=C
LC_COLLATE="en_US.UTF-8"
LC_MONETARY=C
LC_MESSAGES="en_US.UTF-8"
LC_PAPER=C
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT=C
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
```
改变编码
```
# nano .bashrc   (或者.bash_profile)
添加
export LC_CTYPE=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# LC_CTYPE=en_US.UTF-8
# LC_ALL=en_US.UTF-8
```
