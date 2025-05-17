# Home Assistant的自动化

一旦你的设备全部设置好以后，是时候锦上添花了，

我们将创建两个自动化，一个是在日落时候点亮灯光，第二个是在工作日前晚上的设定时间调暗灯光的。

## 在日落前点亮灯光

### 前提条件

本次教程假设你已经完成以下内容

- 你已经下载了HA
- 你已经完成了登陆步骤
- 你已经按照步骤添加了集成
- 你的灯泡（一般是智能灯泡）已经被集成进入了HA
  - 如果你还没有灯泡并且不确定买哪种，试试 [Philips Hue](https://www.home-assistant.io/integrations/hue/), [nanoleaf](https://www.home-assistant.io/integrations/nanoleaf/),，或者其他支持WLED功能的产品

 ### 在日落前点亮灯光

1. 前往设置->自动化与场景并且点击创建自动化。![The automation editor.](https://www.home-assistant.io/images/getting-started/automation-editor.png)

   - 然后，点击创建新自动化，随之会出现空自动化页面

     ![The start of a new automation.](https://www.home-assistant.io/images/getting-started/new-automation.png)

2.创建自动化的创建触发自动化的的条件(触发器,是指定义用于触发自动化的平台值或条件的集合。)

- 在这种情况下，我们想要使用日落事件去触发我们的自动化

- 点击添加触发器，输入Sun，然后选择Sun选项

  

3.选择日落 

- 我们想要在日落事件前触发自动化,所以让我们在偏移时间前添加-00:30

4.一旦我们定义触发器后,我们就应该设置接下来该发生什么

- 点击添加行为

5.输入灯光并点击开灯

- 对于这个自动化,我们准备点亮客厅的所有灯光,所以让我们来点击区域
- 当然要先把灯泡设备划分到一个对应区域
- 如果你想要学习更多关于区域中设备分组的知识，参考[区域文档](https://www.home-assistant.io/docs/organizing/areas/)。

![A new automation with the action set up to turn on the lights in the living room.](https://www.home-assistant.io/images/getting-started/action.png)

6.要保存自动化，请选择 “保存”。为自动化命名，添加描述，然后再次保存。

- 当输入名称时候记得要具体，因为你会有许多自动化。举个例子，在日落时候开启客厅的灯。
- 现在，只要等到日落前的三十分钟时候，就可以看到自动化的魔力了。
- 或者按照接下来步骤立即测试你的自动化功能。

## 工作日前夜调暗灯光

1.前往设置->自动化与场景并且点击创建自动化。![The automation editor.](https://www.home-assistant.io/images/getting-started/automation-editor.png)

- 然后点击创建新自动化，随后出现一个空自动化页面

  ![The start of a new automation.](https://www.home-assistant.io/images/getting-started/new-automation.png)
  

2.假如我们想在21：45调暗灯光，这代表着我们需要一个依据时间而触发的自动化

  - 选择添加自动化->时间与位置-时间
  - 选择固定时间并输入时间。![A new automation with a fixed time trigger filled in.](https://www.home-assistant.io/images/getting-started/automation_trigger_fixed_time.png)

3.我们只想在明天是工作日的情况下才这么做。所以要添加条件

- 点击添加条件->实体->状态

- 在 实体下输入 “工作” 并选择您的工作日传感器。

- 在 “状态” 下，点击on（这个选项说明第二天是工作日）。

4.接下来，确保关灯的前灯是亮着的。

-  为了完成这个目的，需要使用如果-就行为，选择添加动作->创建组件->如果-就
-  你现在创建了一个条件执行的组件，从下列实体中选择你的灯泡
-  在如果.*选项下，选择添加条件->实体->状态
-  在状态选项下选择on




5. 现在我们要定义当条件为真时 (当灯亮着时) 执行的动作

   - 在就执行选项下, 点击添加动作->开灯

   - 在实体选项想下选择你的灯。

   - 定义灯光设置，如亮度、温度或颜色。可用设置取决于灯光。

     ![Screenshot showing the then section of an if-then action](https://www.home-assistant.io/images/getting-started/automation_if-then-action_then.png)6.要保存自动化，请选择 “保存”。为自动化命名 (例如，在工作日前的晚上调暗客厅桌子的光线), 添加一个描述，然后再次保存。

     7.测试你的自动化

     - 转到[**开发人员工具**>**模板选项**](https://my.home-assistant.io/redirect/developer_template)卡。

     - [按照](https://www.home-assistant.io/docs/configuration/templating/#processing-incoming-data)段末尾的描述创建模板所需的所有变量（源）。

     - 复制您的模板代码并将其粘贴到模板编辑器中变量之后。

     - 如果有必要，请更改源的值并检查模板是否按您希望的方式工作并且不会产生任何错误。

       

   ​       8.如果完成本入门指南后，您有兴趣阅读有关自动化的更多信息，我们推荐以下页面：

   
   
   - [触发器](https://www.home-assistant.io/docs/automation/trigger/)
   
     - [条件](https://www.home-assistant.io/docs/automation/condition/)
     - [动作](https://www.home-assistant.io/docs/automation/action/)