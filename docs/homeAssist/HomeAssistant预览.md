# Home Assistant教程   

## 简介
对于智能家居来说，现在的市场还是比较匮乏，目前接入较好的就是小米，常见的智能家居管家例如HomeKit，却只支持ios系统，对于安卓系统，Home Assistant(下称HA)作为目前优秀的开源智能家居平台，由全球bug修复者和 DIY 爱好者社区提供动力，提供了多种预留接口，支持自定义开发，AI的蓬勃发展，让智能家居不再局限于过去的科技水平，如今的品牌壁垒，。

效果预览

#### 高预算
对于高预算的使用者来说，可以在[<u>官网</u>](https://support.nabucasa.com/hc/en-us/categories/24638797677853)（**$99.00**）购买HA盒子或者购物平台购买集成的HA盒子(200r-1100r)，对于效果预览阶段，七天无理由退货时间应该够了。
#### **低预算(自行diy制作)**
1. 必需:虚拟机，网线(3r-5r/米)，有线网卡(30r，仅在电脑未内置无线网卡时所需)，科学上网（不解释）

2. 可选:蓝牙适配器(30r，建议)...

### 操作流程
以下仅以window下virtualbox（下称vbox）举例.其余系统操作方法可在[官网流程](https://www.home-assistant.io/installation/)查看

步骤

1. ### [百度网盘下载](https://pan.baidu.com/s/1MpQKqQ-_BxXGvPSzw73PeQ?pwd=mf7k)

   ​	vbox与官方虚拟硬盘文件压缩包,解压压缩包(也可以自行下载官网镜像)

2. ### 创建虚拟机

```
	2.1 点击新建
```

<img src="resource\pic\image-20250513190658727.png" alt="image-20250513190658727" style="zoom:67%;"/>

​	

```
	2.2 选择虚拟机类型。给虚拟机取个名字，选择类型为Linux，subtype为Oracle Linux，版本为Oracle Linux（64-bit），虚拟光盘一定留空。
```

<img src="resource\pic\image-20250513210807310.png" alt="image-20250513210807310" style="zoom:67%;" />

		2.3 在硬件选项下，内存大小为2G，处理器数量2g，一定要开启EFi（否则无法启动操作系统）

<img src="resource\pic\image-20250513205516273.png" alt="image-20250513205516273" style="zoom:67%;" />

​		

```
	2.4配置虚拟硬盘文件
```

<img src="resource\pic\image-20250513205921648.png" alt="image-20250513205921648" style="zoom:67%;" />

```
	2.5 再选择注册
```

<img src="resource\pic\image-20250513210102349.png" alt="image-20250513210102349" style="zoom:67%;" />

​	

```
	2.6 选择第一步解压好的文件，点击选择后点击完成
```

<img src="resource\pic\image-20250513210643711.png" alt="image-20250513210643711" style="zoom:67%;" />

​				

```
	2.7 右键进入设置
```

<img src="resource\pic\image-20250513191339906.png" alt="image-20250513191339906" style="zoom:67%;" />

​		

```
	2.8 内存至少为2G，处理器选两个（先点击到expert模式，basic与expert模式切换时候，配置信息会丢失）
```

<img src="resource\pic\image-20250513192614538.png" alt="image-20250513192614538" style="zoom:67%;" />

<img src="resource\pic\image-20250513211106965.png" alt="image-20250513211106965" style="zoom:67%;" />

		2.9 配置音量控制器

<img src="resource\pic\image-20250513192859362.png" alt="image-20250513192859362" style="zoom:67%;" />

```
	2.10 配置网卡（此处很重要，且较为复杂）
```

​									<img src="resource\pic\image-20250513193045576.png" alt="image-20250513193045576" style="zoom:67%;" />		

​		

```
连接方式必须选择桥接，且网卡必须为有线网卡，选择接入网线（这样最稳定）

```

常见问题，判断网卡类型

| 名称包含                            | 说明                            |
| ----------------------------------- | ------------------------------- |
| `Wi-Fi` 或 `Wireless`               | 明确是无线网卡                  |
| `Ethernet` / `GbE` / `LAN` / `PCIe` | 明确是有线网卡                  |
| `TAP-Windows Adapter`               | 虚拟网卡（常用于 VPN）          |
| `Remote NDIS`                       | 手机 USB 网络共享产生的虚拟网卡 |



```
	2.11 主界面选择启动
```

<img src="resource\pic\image-20250513212625336.png" alt="image-20250513212625336" style="zoom:67%;" />

```
	2.12 查看虚拟机运行情况
```

<img src="resource\pic\image-20250513213620193.png" alt="image-20250513213620193" style="zoom:67%;" />

```
	2.13 查看前端页面
```

​		进入图中所示网址 如图本机IPV4 为192.168.2.122（<u>以自身情况为准</u>，用自己安装的虚拟机IP地址）拼接下方的两个端口（8123 与 4357），本例为192.168.2.122:8123 与192.168.2.122:4357，进入浏览器分别输入192.168.2.122:8123 与192.168.2.122:4357

<img  src="resource\pic\df11e8ea-a6ad-4964-a9a9-16aa24421333.png" alt="image-20250513213620193" style="zoom:67%;">

<img src="resource\pic\65fdf2d5-c259-4098-a271-37149968384e.png" alt="65fdf2d5-c259-4098-a271-37149968384e" style="zoom:67%;" >

​	下载需要等一会，界面显示20分钟，我等了2个小时

常见问题

- 虚拟机正常显示ip，但浏览器访问不到主页

   - 查看ip:端口是否正确
   - 查看windows防火墙

- 虚拟机无法启动

  - 查看efi是否开启，硬盘文件正确解压与选择
  
- 浏览器一直卡在准备页面
  - 查看详细信息可发现无法访问github，需要科学上网
  
    
