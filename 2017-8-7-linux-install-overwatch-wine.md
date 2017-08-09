## linux下安装overwatch(wine)
操作系统：archlinux

### 1.安装wine
```
sudo pacman -S wine
```
### 2.下载wine-overwatch
进入[](https://lutris.net/files/runners/)
下载wine-overwatch-2.13-x86_64.tar.gz
解压wine-overwatch-2.13-x86_64.tar.gz
```
cd wine-overwatch-2.13-x86_64/bin
./winecfg
```
然后会出现一个窗口：  
在窗口中，选择“Windows Version”为"Windows XP"  
然后点击“Apply”  
然后再选择“Staging”  
选中前两个选项  
然后点击“Apply”，“ok”
### 3.下载Overwatch-Setup.exe
进入[](https://us.battle.net/account/download/index.xml?show=bnetapp)
找到Oerwatch，点击windows版下载也只有（windows版）
将Overwatch-Setup.exe放入"wine-overwatch-2.13-x86_64/bin/"文件夹下
```
./wine Overwatch-Setup.exe
```
弹出窗口，选择“cotinue”->“cotinue”->"ok"
然后登录帐号->"close"
然后安装overwatch，发现无法安装
关闭
### 3.更改设置
```
./winecfg
```
选择“Windows Version”为"Windows7"->"apply"->"ok"
```
./wine /home/dy/.wine/drive_c/Program Files (x86)/Blizzard App/Battle.net.exe
```
点击安装。
等待安装结束后，关闭窗口。
然后：
```
./winecfg
```
选择“Windows Version”为"Windows XP"。
再：
```
./wine /home/dy/.wine/drive_c/Program Files (x86)/Blizzard App/Battle.net.exe
```
