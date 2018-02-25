
### 效果图：
![img](https://i.loli.net/2018/01/20/5a632d8925eaf.png)
### 主题-图标-gnome-shell
打开teaks
![img](https://i.loli.net/2018/01/20/5a632f596353c.png)
主题：arc
图标：numix
shell：arc
![img](https://i.loli.net/2018/01/20/5a632e4f99be7.png)
要使用shell，需要在扩展中勾选user theme
![img](https://i.loli.net/2018/01/20/5a632ec1c1fa6.png)
### 安装chrome-gnome-shell
chrome-gnome-shell是一个通过chrome浏览器来管理gnome扩展的东东，当然使用它需要给chrome安装一个插件，在[这里](https://extensions.gnome.org/local/)点击链接安装即可：
![img](https://i.loli.net/2018/01/20/5a6331597f9e7.png)
由于源里的chrome-gnome-shell依赖python2，所以直接从源码安转：
```
git clone git://git.gnome.org/chrome-gnome-shell
cmake -DPython_ADDITIONAL_VERSIONS=3 -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_EXTENSION=OFF
sudo make install
```
安装完成后，打开[这个链接](https://extensions.gnome.org/local/)，可以看到已经安装的扩展，如果要安装新的扩展，也非常简单，在要安装的扩展页面点击右上角的开关即可：
![img](https://i.loli.net/2018/01/20/5a6330b0a32f4.png)
比较常用的扩展有：  
netspeed：显示网速  
dash to dosk：移动左侧菜单栏  