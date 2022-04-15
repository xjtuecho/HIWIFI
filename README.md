# HIWIFI极路由固件仓库

由于众所周知的原因，极路由原厂固件已经停止更新，自带插件也无法使用，本仓库对极路由常用的固件进行汇总。

## 极路由可用固件

- [原厂固件](Factory)：功能少，运行稳定，扩展性差，无线驱动性能好，已经停止更新。
- [Padavan固件](Padavan)：功能多，性能强大，运行稳定，界面流畅，扩展性一般，无线驱动性能好。
- OpenWrt固件:功能多，扩展性好，无线性能弱，使用较复杂，更新频繁。

## Padavan固件说明

Padavan固件使用和原厂固件相同的MTK闭源无线驱动，无线性能好，同时界面比原厂固件流畅，功能也更多，推荐使用。
本仓库发布的Padavan固件均实机测试通过，LAN口、WAN口、LED指示灯、SD卡、USB接口均测试正常。

- 管理IP：192.168.2.1
- 默认用户名：admin，密码：admin
- 默认无线密码：1234567890

HC5861注意事项：

- **HC5861带一个红色千兆口，Padavan固件将千兆口设置为WAN接口**
- HC5861中间的TURBO LED改为了USB显示，USB挂载成功点亮

从其它固件刷Padavan需要在Breed中进行，FLASH布局使用**公版**(0x50000)，刷好Padavan以后升级固件可在Padavan的web页面中进行。
也可以使用ssh中使用mtd_write命令更新固件：

`mtd_write -r write HCxxxx_3.4.39-099_xxxxxxx_v220405.trx Firmware_Stub`

`-r`选项表示更新完重启，`.trx`为要升级的固件。如果更新完Padavan以后有莫名其妙的问题，可以尝试在Padavan重置一下参数。

## 注意事项

极路由型号众多，不同的型号固件通常不能互刷，选择固件时注意CPU型号。
比如：极路由1S分为HC5661和HC5661A；极路由贰分为HC5761和HC5761A。CPU分别是MT7620和MT7628，固件不能通刷。

MT7628为MT7620的降成本版本，去掉了两个RGMII接口，去掉了HNAT，减小了芯片封装尺寸，GPIO从72个减少到47个。

Bootloader推荐使用Breed，在原厂固件中刷入即可，方法请自行搜索。

从原厂固件刷其它固件之前，一定记得备份一个编程器固件，至少要备份eeprom，以备不时之需。
推荐在Breed中备份一份编程器固件，包括了eeprom在内的所有信息。最差的情况下使用编程器写入SPI FLASH即可。

极路由原厂固件的MAC地址存放在FLASH尾部的bdinfo分区中，其它第三方固件MAC地址都存放在eeprom分区中，
第三方固件如Padavan会将FLASH尾部的三个分区擦除，这也是备份原厂固件的重要原因。
第一次由原厂固件刷第三方固件时需要重新将MAC地址写入eeprom分区。以Padavan为例，推荐使用ssh命令写入：

- lan_eeprom_mac：写入LAN的MAC地址
- radio2_eeprom_mac：写入2.4G无线的MAC地址
- radio5_eeprom_mac：写入5G无线的MAC地址
- wan_eeprom_mac：写入LAN的MAC地址

极路由LAN、2.4G、5G的MAC地址是相同的，就是机器背面标签的MAC地址，WAN的地址最后一个字节相对LAN地址+1.
也可以使用Breed写入MAC地址，具体方法请自行搜索。将MAC地址写入eeprom分区以后再刷回原厂固件没有影响。
此外FLASH尾部的bdinfo分区除了MAC地址，还有原厂的一个设备密钥，用于原厂的插件，由于众所周知的原因没什么用了。

## 相关链接

- [Breed](https://breed.hackpascal.net)
- [OpenWrt](https://openwrt.org/)
- [恩山无线论坛](https://www.right.com.cn/forum)
- [不拆机获取ROOT权限](http://www.hiwifi.wtf)