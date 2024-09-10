# 慧湖不通



> 文星宿舍宽带网络跨平台解决方案
>
> Related: 慧湖通 文星 HuiHuTongTools



## 1. UPDATE 信息更新

**2024/9/6:** 物业通知本日12-13:00因施工原因临时断网，疑似实际进行检测策略更新，更新后用于监测的证书指纹如下所示，之前应该也是锐捷的，但我没有保存
```
CN(Common Name): ruijie
0(Organization): Ruijie
OU(Organizational Unit): SEC
L(Locality): Fuzhou
C(Country): CN
ST(State or Province): Fujian
Sha2_Fpr: F6 24 41 E7 A3 06 E7 A9 28 A0 FB 5E 52 87 71 DE E5 A3 B8 78 84 3D 8E 65 0A 79 12 F2 FA 44 A1 BE
Sha1_Fpr: 67 13 61 FF D2 B3 72 3B 0E 5D BD DC 46 46 B2 46CF EB CD 5B
NotBefore: 2018/1/19
NotAfter: 2045/6/6
```




**2024/9/5:** 一站式:物业与相关营业厅从未将文星网络"升级"一事告知学校，学校对此不知情，**鼓励进行12315举报**。此为官方非正式的回复

**2024/9/3:** 营业厅运维说至少有两个检测方案，其中一个已经确定是锐捷，另一个高度疑似Drcom；另有深度包检测功能，但没提及是否已经开启；后续有能力随时换检测&封禁手段，但未知真伪。

**2024/9/3:** 确认文星为试点区域，检测手段后续会同步至文缘，时间未知。

**2024/9/3:** 已确认文星不同区域的检测手段和策略不同，可能与设置以及负载有关。

**2024/9/2:** 文星已经全量上线密码登录，如登陆时仍只能扫码，请重启光猫。

## 广告 :point_down: :point_down: :point_down:

本人Surf项目正参与学院评选，希望大家能够帮忙投票支持，谢谢!

> 支持方式: 点击链接https://core.xjtlu.edu.cn/mod/questionnaire/view.php?id=8245, 选择编号**2024-0320**的项目，点击确认即可，约15秒即可完成，thanks
>![POSTER_FINAL_0828_RGB2CMYK_ONLINE_PUBLISH_VERSION](https://github.com/user-attachments/assets/beb0c600-6dd9-414f-8a07-650ae8a2ca6d)


## 2. FAQ 常见问题

**Q1. 能在文缘/文萃/文荟使用吗**

文缘检测方式极为简单，不需要使用这里面的内容，除非后续更新到了和文星一样的检测方式。



**Q2. 需要单独购买硬件吗？**

不用也不建议，本项目的目标就是在不额外购买设备/宽带的前提下正常使用网络。在所有的开发完成后，只需要一个普通路由器即可。

但有其他的需求(如pt，私人dns，代理等) 建议使用软路由。



**Q3. 是否收费**

开源，免费，但不保证更新时间，影响到叔叔赚钱会暂时隐藏仓库和下载链接



> **冷知识: 2023年底部分营业厅就对周边学校网络"升级"一事进行了可行性分析，最终认为XJTLU的同学最多也只会在开学的时候投诉, 部分有能力的同学在发现有解决方案后，要么把开源程序挂在网上卖，要么自己偷偷用；原本有意见的同学买完用着也懒的计较了。等事情一平息就顺手把剩下几个宿舍区也给"升级"了，再美美多赚3倍钱。**
>
> **以上内容为原文转述，旁边两学校则是营业厅怕学生意见大不好处理，不得不放弃了"升级"计划。 今年和职业技术学院一起升级"，这样下去过几年要不如专科了**
>
> <u>**如果你对本项目感兴趣或者有相关能力，欢迎加入，也希望各位不要收费，否则只会让他们变本加厉**</u>



## 3. 平台适配计划

11月底前、考试周以及毕业后不会保证开发进度

| Platform                    | 自动登录     | 设备数量解锁       | 路由器&子网解锁    |
| --------------------------- | ------------ | ------------------ | ------------------ |
| OpenWRT                     | 开发中       | :white_check_mark: | :white_check_mark: |
| Windows/Linux<br/>-VM方案   | 开发中       | 后续适配           | 后续适配           |
| Windows/Linux<br/>-原生方案 | 后续适配     | 后续适配           | 后续适配           |
| Android&Mac                 | 暂无适配计划 | 暂无适配计划       | 暂无适配计划       |



## 4. 特殊硬件适配

| Name                                                 |                      |                   |
| ---------------------------------------------------- | -------------------- | ----------------- |
| NewWifi3<br>Unlook by Breed@RT-N56UB1-newifi3D2-512M | 已做固件与EEPROM适配 | 已测试(2024/8/31) |
| 待定                                                 |                      |                   |



## 5. 检测与限制原理

推测由锐捷提供鉴权与拦截服务，至少还有一个系统参与了流量特征检查。以下是已知的检测类型



- ### MAC地址检测


​	MAC地址检测用于限制同时连接设备的数量，一个账号下最多允许三台在线设备。

​	使用网线连接时，每个适配器均参与连接设备MAC地址计算。

​	使用路由器时，光猫收到的以太网帧相关地址字段均为路由器的MAC地址，对外仅显示单个设备连接



- ### UA检测


​	2024/9/2日后确认全量部署了UA检测，UA检测不仅能用于限制同时连接设备的数量，还能探测是否使用了路由器。

​	同一个设备发出的UA大致相同，若侦测到的UA类型数量大于MAC地址数量，则说明子网中存在路由器(或热点)，因此进行阻断。

​	**出口处设置代理统一修改UA与MAC即可**



- ### TTL检测


​	TTL检测也用于定位路由器，数据报的TTL字段在发送后经过每个路由器时会减1，当收到的TTL!=设定值则重定向到blacklist.php进行	阻断

​	宿舍使用路由器(非AP模式)后，packet的TTL字段在离开本机网络适配器后会-1，在经过路由器后再次-1，在光猫端检测时小于设定	值，因此拦截。

​	一些代理与游戏加速器软件会在部分系统中会创建虚拟适配器，经过改软件的流量在系统内部已经对packet的TTL字段进行了修改,因	此在正常连接的情况下也会被阻断。

​	**在流量出口处将所有packet的TTL设为128(或较大的数)即可规避检测**。注意不要设置TTL不变，因为内部网络可能有多个子网，例如	使用加速器的情况下仍会被检测。



- ### IP检测


​	IP检测用于防止使用路由器，且部署在前端。

​	进行密码或扫码认证时，web端将首先检查本机的IP是否位于本楼层所属DHCP池中，若不在，则弹窗提示并退出。

​	已知目前只有桌面端部署了该检测，移动页面模式下不含该代码。由于检测路径位于浏览器中。**可简单的通过开发者模式删除该流	程或使用自动登录来绕过。**



- ### 特征检测


​	非常传统的检测方式

​	在"升级"前文星宿舍区已至少进行了1年的p2p流量特征检测，已知封禁策略与宿舍的物理位置和运营商都有关(疑似部分楼层不限	速，可能是忘了设置)，**建议对该类流量进行代理或做转发**



- ### 名单检测


​	非常传统的检测方式

​	若目标IP地址在黑名单中，直接阻断，暂不清楚是否有白名单机制



- ### DNS检测


​	在"升级"前文星部分楼层已至少进行了1年DNS劫持，不过不明显。"升级"后似乎未启用该检测



- ### 深度包检测


​	(2024/9/4)高度怀疑已经或即将启用深度包检测。深度包检测主要阻断代理流量。

​	深度包检测的开销较大，按照惯例，目标ip归属地是重要检测依据，为提升检测效率，该系统可能直接放行目的IP为内地的流量(因为	代理的终点一半都在海外)

​	**可以多设置一层中转代理来解决**



## 6. 自动登录

慧湖通使用微信二维码或密码进行鉴权，为经过修改的锐捷认证系统，认证服务器为10.10.16.101:8080/



- #### 定时重放登录


Mentohust([HustLion/mentohust: 接续HustMoon开发的Mentohust，继续更新 (github.com)](https://github.com/HustLion/mentohust))是专门用于锐捷系统的自动登录工具，但文星宿舍区对认证算法进行了一定的修改。后续计划在此基础上进行修改开发，移植到不同的平台。



- #### 自动扫码登录


作为备份方案，若频繁更新认证算法则考虑使用自动扫码来登录。

当需要上网的设备获取到上级设备发来的登录页面时，使用局域网将内容数据转发至内网的手机即可。手机后台使用推送服务将解码后的二维码(或直接发送链接)发送至你的微信。能够实现(已经配置过的)设备(在后续连接时)你的微信会自动收到认证用的二维码，然后长按手动扫码。安卓端使用的方案是Termux+ServerChan([Server酱·Turbo版 | 一个请求通过API将消息推送到个人微信、企业微信、手机客户端和钉钉群、飞书群 (ftqq.com)](https://sct.ftqq.com/))



## 7. 设备数量解锁

以下是未修改内容，之后有空再更新

## 8. 路由器&子网解锁

以下是未修改内容，之后有空再更新

## 9. 组网方案

以下是未修改内容，之后有空再更新

### ~~PLAN A - 使用现有设备 - Route模式~~


~~![image-20240831205021408](https://github.com/ZTX1836255060/HuiHuBuTong/blob/main/assets/image-20240831205021408.png)~~

> ~~无需新设备，当需要对每台设备进行修改，路由器重启后需要重新认证~~

- ~~路由器保持路由模式，启用DHCP服务器等功能，光猫网线连接WAN口~~
- ~~路由器设置内将MAC地址修改为任意已经过认证的设备~~
- ~~修改每台需要联网的设备TTL的配置，~~
  - ~~Windows在HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters处修改，其他平台类似~~
- ~~核心原理是packet抵达路由器时，其TTL字段在出路由器-1后刚好合法，以太网帧又被替换为已认证地址(路由器MAC)，此时HHT只能检测到单台设备。~~





### ~~PLAN B - 使用现有设备 - 设备路由模式 - EASY MODE~~

~~![](https://github.com/ZTX1836255060/HuiHuBuTong/blob/main/assets/image-20240831211032773.png)~~

> ~~无需新设备，不使用虚拟机，最多仅需要修改一台电脑的TTL设置，但该电脑需要保持开启~~

- ~~一台经过认证的电脑进行连接光猫，并进行网络共享~~
  - ~~使用windows自带的无线网络共享: 设置->网络->移动热点~~
  - ~~或使用有线(有多余接口的情况下)连接开启AP模式的路由器，其余设备无线连接路由器，下游设备无需认证。~~





### ~~PLAN C - 使用现有/专有设备 - 设备路由模式~~

~~![image-20240831213854395](https://github.com/ZTX1836255060/HuiHuBuTong/blob/main/assets/image-20240831213854395.png)~~

> ~~需要支持运行Container/VM的电脑作为路由器~~
>
> ~~或~~
>
> ~~使用现有软路由~~
>
> ~~需要有相关经验~~

- ~~软路由进行认证，TTL修改，MAC替换~~
- ~~AP提供无线网络~~
- ~~根据设备类型按下表进行配置~~

| ~~设备~~                | ~~方案~~                                                     | ~~文件~~              |
| ----------------------- | ------------------------------------------------------------ | --------------------- |
| ~~软路由~~              | ~~无直接适配，建议使用OpenWRT自行编译<br>或者使用虚拟机运行Ikuai镜像<br>现有OpenWRT设备也可根据原理部分自行修改设置~~ | ~~HHBT_OPENWRT.ZIP~~  |
| ~~Windows10/11或Linux~~ | ~~VM方案,底层为配置好的Ikuai虚拟机<br>1.使用VMWare导入虚拟机镜像<br>2.设置适配器关联~~ | ~~HHBT_VM_IKUAI.ZIP~~ |

~~Ikuai虚拟机是在之前项目的镜像上修改的，里面可能设置了自动重启，记得关掉~~





### ~~PLAN D - 使用专用设备~~

~~![image-20240831213521544](https://github.com/ZTX1836255060/HuiHuBuTong/blob/main/assets/image-20240831213521544.png)~~

> ~~需要能够安装OpenWRT/相关系统的可刷机路由器~~

- ~~已完成对RT-N56UB1-newifi3D2-512M的完整适配，如果你有该设备可直接导入备份的固件，无需任何其他操作~~
  - ~~账号为root，密码为huihubutong~~
  - ~~建议使用Breed进行固件更新~~
  - ~~MAC地址在Breed内修改(使用EEPROM-方案为公版，勿使用其他版本)~~
  - ~~文件为HHBT-NEWIFI3D2-BREED.ZIP~~
- ~~部分华硕系设备理论上可以使用OpenWRT，但未经过测试。~~
  - ~~如果你有时间参与适配，建议开源上传并分享至相关论坛~~



## 10. 其他功能

- ### 光猫破解


获取超管密码，解锁SSH等权限

**疑似已有成熟方案，此处不对该内容进行介绍，也不建议在宿舍区内使用，如需要请自行搜索。**

注: 可能使用了TR069进行在线配置下发，不要删除该连接 



- ### 单线多拨


绕过运行商带宽限制，请使用cat5e以上的网线

**此处不对该内容进行介绍，也不建议在宿舍区内使用。**



- ### 链路聚合


链路聚合可以进行局域网内的流量调度，吞吐量<=所有宽带带宽的总和

**需要单个宿舍内开通多个相同运行商的宽带&你室友的支持**

推荐使用connectify dispatch进行OS级别的聚合，无需对现有网络环境进行改造。



- ### 慧湖通门禁卡加密原理&主密钥&算法&复制方式


以下是未修改内容，之后有空再更新



## 11. 文件校验

| ~~Name~~                     | ~~DESC~~                            | ~~MD5~~                              |
| ---------------------------- | ----------------------------------- | ------------------------------------ |
| ~~HHBT-NEWIFI3D2-BREED.ZIP~~ | ~~固件与Breed控制台~~               | ~~4B40242143375209729C14430221A53B~~ |
| ~~HHBT_VM_IKUAI.ZIP~~        | ~~ikuai OS,用于VMWare的虚拟机文件~~ | ~~80E0B9840AAC597B234141D48492912E~~ |
| ~~HHBT_OPENWRT.ZIP~~         | ~~dump文件~~                        | ~~FFA75B3B1DCB63E272456399ED336C79~~ |

~~下载链接见Download link.txt~~ 

2024/9/2:下载链接暂时下线，后续根据情况更新。



## 12. 相关项目

以下是未修改内容，之后有空再更新
https://github.com/ELT17604/FuckHuiHuTong
todo: authdesc + project desc

## 13. 声明

本项目提到的所有内容与XJTLU&/慧湖通&/淘宝/纸条上的付费解锁无关

本项目不包含单线多拨或VPN等可能存在安全隐患的代码

不用于盈利，未来也不会计划盈利，其余见开源协议
