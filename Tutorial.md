# 教程

## 硬件
- **型号**：浙江移动魔百盒 九联 UNT-403G-4U-ZJ  
- **主板**：  
  **注**：UNT-403G 不同的主板短接点和 TTL 接口不一致，请核对一致后再刷机  

目前批次的九联 UNT-403G 主板想通过打开 adb 来免拆刷机是行不通了，因为在 adb 界面会出现厂家的二维码，所以通过 U 盘短接强刷包是目前最好的办法。正常开机情况下的盒子刷机步骤如下：

---

## GK6323 强刷原理
免拆刷机是通过降级后的系统能开 adb，用 adb 把公签的 recovery 替换了原机的 recovery 后，通过写入的另一个 misc 或后期命名为 emmc 的分区，里面是加入了一段启动 recovery 的命令。这样在替换 recovery，写入 misc 后重启盒子就启动 recovery 读取 U 盘 updata.zip 升级包刷机。

**注意！注意！注意！**  
网络上有些刷机包里面是有一个 fastboot.img 文件的, Update 刷机升级后会把这个 fastboot 写入盒子替换掉原来的 fastboot, 如果这个替换的 fastboot 的版本旧, 不兼容新款的主板的话，刷完这个包后等同没了 fastboot 这个启动程序, 会导致盒子不启动而黑屏。  
**解决办法**：是用 hitool 工具把适合的 fastboot 烧一次，或者用兼容性高的强刷包再刷一次就好了。

**短接拆机原理**：  
短接拆机是使芯片读取 emmc timeout，
```plaintext
Bootrom start
Boot Media: emmc (Default Speed)
eMMC: read timeout.
```
从而引发在 USB 中的 fastboot.bin 进行启动，所以只要是 cpu 与闪存的连接传输电阻任意一端与地短路即可触发（来自 [1337565980](https://www.znds.com/forum.php?mod=redirect&goto=findpost&ptid=1249681&pid=65455042&fromuid=3557202)）。在使用短接拆机时，**U 盘非常关键，U 盘非常关键，U 盘非常关键**，在短接刷机时如果机子没反应的话，基本都是盒子无法识别 U 盘导致，更换其他 U 盘即可。

---

## 刷机步骤

### 1. 拆机
- 盒子反面有 3 个螺丝固定，按照下图先卸下螺丝，然后通过撬棒等工具撬开四边，里面都是卡口接口，大力出奇迹。
- ![背面](https://raw.githubusercontent.com/jxjhheric/unt403g-u4-zj/refs/heads/main/PIC/%E8%83%8C%E9%9D%A2.jpg) ![卡口](https://raw.githubusercontent.com/jxjhheric/unt403g-u4-zj/refs/heads/main/PIC/%E5%8D%A1%E5%8F%A3.jpg))
- **版本**：浙江移动魔百盒 型号：九联 UNT-403G-4U-ZJ 的主板如下（不带屏蔽盒）：  
  （此处可插入主板图片）  
- 除此之外网上还有另外 2 种类型，供大家参考：  
  （此处可插入其他类型主板图片）

### 2. 重点
- 寻找主板上的短接点，九联 UNT-403G-4U-ZJ 的主板上的短接点如下：  
  （此处可插入短接点图片）  
- 使用镊子等导电金属按图短接后上电，6 秒-10 秒放开短接，灯亮开始刷机。

---

## 刷机固件
- **强刷包（包含 fastboot.bin）**  
  链接: [https://pan.baidu.com/s/125UFPAR0Tz5C8RyLBkP6nw?pwd=9gjd](https://pan.baidu.com/s/125UFPAR0Tz5C8RyLBkP6nw?pwd=9gjd)  
  提取码: 9gjd  
  **注意**：刷机时一定要确认主板一致，如果刷完黑屏，请更换其他适合主板的含 fastboot.bin 的 update.zip 刷机包。如果刷完出现遥控器关机后无法开机的情况请看下一个包。

- **来自 gunix 的通刷包**  
  Unt403G413G 全国通刷卡刷包（不刷 data 及 vendor)  
  如果刷机完成后插 U 盘读取不到，得刷对应的 vendor 包，  
  链接：[ https://pan.baidu.com/s/15j7ucLKZcupy5qXEzdPo4g?pwd=decg]( https://pan.baidu.com/s/15j7ucLKZcupy5qXEzdPo4g?pwd=decg)  
  提取码：decg  

- **刷机方法 1**  
  利用链接中的 `<一键替换 Recovery.rar>` 替换 recovery，把 Unt403G413G 全国通刷卡刷包（不刷 data 及 vendor) 的 update.zip 放入 U 盘根目录，重启盒子就启动 recovery 读取 U 盘 updata.zip 升级包刷机，
  刷 vendor文件方法：把vendor文件重命名为update.zip，利用adb工具放入sdcard根目录中，然后进入系统--设置--升级系统。
  ![升级](https://raw.githubusercontent.com/jxjhheric/unt403g-u4-zj/refs/heads/main/PIC/%E7%B3%BB%E7%BB%9F%E5%8D%87%E7%BA%A7.jpg)

- **刷机方法 2**  
  不替换 recovery 的话，把 Unt403G413G 全国通刷卡刷包（不刷 data 及 vendor) 的 update.zip 和 b131.bin、bootargs.bin、fastboot.bin、recovery.img 放入 U 盘根目录，通过短接硬刷的方式刷入即可。

---

## TTL 救砖
如果强刷不成功或者变砖，则得上 TTL。  
- **重点**：确认主板上的 TTL 线序 GND，TX，RX，和短接点一样，不同批次的点不一样，九联 UNT-403G-4U-ZJ 的主板上的短接点如下：  
  ![（此处可插入 TTL 短接点图片） ](https://raw.githubusercontent.com/jxjhheric/unt403g-u4-zj/refs/heads/main/PIC/unt403g%E6%B5%99%E6%B1%9F%E7%89%88ttl%E6%8E%A5%E7%BA%BF.jpg) 
- 动手能力差不能焊接的可以使用夹子大法。
  ![jz](https://raw.githubusercontent.com/jxjhheric/unt403g-u4-zj/refs/heads/main/PIC/%E5%A4%B9%E5%AD%90%E5%A4%A7%E6%B3%95ttl.jpg)

### 需要
- USB 转 TTL（ch340，PL2303 等都行，电脑端装好对应驱动，win10 下 PL2303 出现“PL2303HXA 自 2012 已停产，请联系供货商”，只需降级到 2010 年以前的证书（2007、2008、2009 都行）即可）  
- 电脑  
- 网线一根  
- hitool 刷机工具  

### hitool 刷机教程
请参考下面链接中的大佬操作：（刷机前强烈建议备份固件，为后续救砖备用）  
[https://www.cnblogs.com/Sky-seeker/p/17302989.html](https://www.cnblogs.com/Sky-seeker/p/17302989.html)  

- **其中需要一个 Unt403G 的分区表**：[Unt403G 的分区表](https://github.com/jxjhheric/unt403g-u4-zj/edit/main/PIC/UNT403G%E5%88%86%E5%8C%BA%E8%A1%A8.xml)
  **注意点**：选择 Hi3798MV310，使用烧写 eMMC 界面进行烧录和备份


### 替换logo.img
dd if=/mnt/sda1/logo.img of=/dev/block/mmcblk0p7

### 修改遥控器按键
找到system/etc/combinationKeys.xml，编辑。修改添加key、PackageName、ActivityName的值，分别对应遥控器的遥控码、需要启动的apk的包名、启动类名。Key值：如果不清楚，可以通过ButtonMapper.apk中添加自定义按键获取。
![图片](https://b.gx86.cn/zb_users/upload/2025/02/202502051738734538686493.png)

### 挂载system
此固件刷机后如果想修改/system下的文件，会提示仅有只读权限无法操作。
```
mount -o rw,remount /system
mount: '/system' not in /proc/mounts
```
解决方法：
```
ls -al /dev/block/by-name/
```
查看system对应的设备及格式，比如/dev/block/mmcblk0p21 ext4，然后再输入命令挂载
```
mount -t ext4 /dev/block/mmcblk0p21 / > /dev/null 2>&1
```
执行后/system就都有写权限了

### 参考链接
- *[https://www.znds.com/tv-1258035-1-1.html](https://www.znds.com/tv-1258035-1-1.html)
- [https://www.znds.com/forum.php?mod=viewthread&tid=1249681](https://www.znds.com/forum.php?mod=viewthread&tid=1249681)  
- [https://b.gx86.cn/?id=102](https://b.gx86.cn/?id=102)  
- [https://www.znds.com/tv-1242918-1-1.html](https://www.znds.com/tv-1242918-1-1.html)  
