### 1.在内存卡上刷入lede
```
└─▪ sudo dd if=lede-17.01.4-brcm2708-bcm2710-rpi-3-ext4-sdcard.img of=/dev/sdb
记录了581632+0 的读入
记录了581632+0 的写出
297795584 bytes (298 MB, 284 MiB) copied, 27.6607 s, 10.8 MB/s
```

### 2.修改/etc/network
```
config interface 'lan'
        option type 'bridge'
        #option ifname 'eth0'
        option proto 'static'
        option ipaddr '192.168.1.1'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option gateway '192.168.1.254'
        option dns '114.114.114.114'

config interface 'wan'
        option proto 'dhcp'
        option ifname 'eth0'
```

### 3.修改/etc/wireless
```
config wifi-device 'radio0'
        option type 'mac80211'
        option channel '11'
        option hwmode '11g'
        option path 'platform/soc/3f300000.mmc/mmc_host/mmc1/mmc1:0001/mmc1:0001:1'
        option htmode 'HT20'
        option disabled '0'

config wifi-iface 'default_radio0'
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'LEDEDY'
        option encryption 'psk2'
        option key 'mypasswd'   #大于等于8位
```
## 安装ss

### 4.显示架构
```
root@LEDE:~# opkg print-architecture | awk '{print $2}'
all
noarch
arm_cortex-a53_neon-vfpv4
```
我的架构是arm_cortex-a53_neon-vfpv4

### 5.添加下面两行到/etc/opkg/customfeeds.conf
```
src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/base/arm_cortex-a53_neon-vfpv4
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/luci
```

```
opkg update
opkg install ChinaDNS 
opkg install luci-app-chinadns 
opkg install dns-forwarder 
opkg install luci-app-dns-forwarder 
opkg install shadowsocks-libev 
opkg install luci-app-shadowsocks 
opkg install simple-obfs 
opkg install ShadowVPN 
opkg install luci- APP-shadowvpn
```

```
wget http://openwrt-dist.sourceforge.net/auto_install.sh
chmod +x auto_install.sh
./auto_install.sh
```