# HomeAssist基础概念与术语



## 集成

集成是一系列软件，它使得HomeAssistant（下称HA）去连接平台与其他软件。举个例子，飞利浦下的Hue产品通过使用飞利浦集成，可以与HA去与硬件控制器HueBridge通信。HA管理下任何可以使用Hue Bridge这个集成的设，将会以设备的形式出现在HA界面。（应该和小米差不多）

一些集成卡片会以小图标形式展示信息。

- 例如这个云图标 ![Cloud icon](https://www.home-assistant.io/images/getting-started/cloud-icon.png) 说明这个集成依赖第三方云服务。

- 这个文件 ![Configuration file icon](https://www.home-assistant.io/images/getting-started/config-file_icon.png) 说明这个集成没有通过UI设置。你可能是在`configuration.yaml`下设置了它，或者这个集成被其他集成依赖。如果需要配置，也可以在 `configuration.yaml`文件下配置。

- 这个自定义图标 ![Custom icon](https://www.home-assistant.io/images/getting-started/custom-icon.png) 说明这不是个官方集成 而是自己制作的。它可以来自其它来源,举个例子，它可能来自HA社区商店。

完整列表参见[集成文档](https://www.home-assistant.io/integrations?brands=featured)



## 实体

​	实体是HA中存储信息的数据块。可以代表HA中的传感器信息，行为信息或功能信息，用于监控物理属性或控制其他实体（所以改变实体中的数据可以控制相关设备）。其通常是设备或服务的一部分。实体具有状态。状态是什么举个例子,灯的亮度是0-100,大于0,亮度为1-100灯都处于开启的状态,0是关闭状态。



## 设备

​	HA中的设备概念不止是一个或几个实物，而是一个逻辑分组。一个设备可以代表一个有着多个传感器的物理实物，传感器作为与设备相关的实体显示。例如运动传感器可能被视作一个设备，它可以提供运动检测，温度，光照水平作为实体。实体具有状态，例如当检测到运动时检测到状态，以及当没有运动时清除状态。



## 区域

​	HA中的区域概念是一个逻辑概念，为实体与设备分组来匹配真实世界（你家）的物理位置。举个例子，客厅分组下的设备与实体总在你家的客厅处。区域使得你可以对目标位置下的整组目标设备进行调用。举个例子，关闭客厅的所有灯光，区域的概念不仅可以包含你的客厅，卧室，等，还可以划分为楼层。不同区域可以自动划分为不同卡片。



## 自动化

​    一系列可重复的行为可以被设置为自动运行。自动化由三个关键部分构成：

1. 触发器：启动自动化的事件，举个例子
2. 条件：在执行操作之前必须满足的可选的前提条件。例如，如果有人在家。
3. 动作 ：与设备互动，如开灯。

要了解自动化的基础知识，请参阅[自动化基础页面](https://www.home-assistant.io/docs/automation/basics/)，或者尝试自己[创建](https://www.home-assistant.io/getting-started/automation)一个自动化。