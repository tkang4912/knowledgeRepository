# 动作

动作是自动化中执行的内容，自动化动作部分和脚本语法一样，可以通过动作或事件与其他动作交互。

对于动作来说，可以通过指定实体id（entity_id）来执行对应的动作。例如，指定亮度。

你可以通过动作来激活场景，这种方式可以让你的设备按照你想的方式运行。

```yml
automation:
  # 在日落后，把厨房与客厅的灯光颜色设置为红色，亮度150
  triggers:
    - trigger: sun
      event: sunset
  actions:
    - action: light.turn_on
      target:
        entity_id:
          - light.kitchen
          - light.living_room
      data:
        brightness: 150
        rgb_color: [255, 0, 0]

automation 2:
  # 日落前三十分钟 发信息提醒看日落
  triggers:
    - trigger: sun
      event: sunset
      offset: -00:30
  variables:
    notification_action: notify.paulus_iphone
  actions:
    # Actions are scripts so can also be a list of actions
    - action: "{{ notification_action }}"
      data:
        message: "Beautiful sunset!"
    - delay: 0:35
    - action: notify.notify
      data:
        message: "Oh wow you really missed something great."
```

自动化内容中可以包含自动化条件。在一个动作中可以包含多个动作与自动化条件，它们将按照您属性的顺序进行处理。如果条件的结果为假，动作将停止，因此在该条件之后的任何动作都不会执行。

# 脚本动作

很多集成都会在自动化事件发生时候执行各种各样的动作.自动化执行中的动作是最常执行的动作.但动作也可以通过脚本、仪表盘或者语音命令(Amazon Echo等)来执行。执行动作的动作的配置选项在所有集成之间都是相同的，统一在本页中进行描述。

本页中的示例只是自动化集成配置方法中的一种，对于其它集成也可以用其它方法。

```
 提示

点击开发者工具下的动作(Actions)标签页可以查找可用的动作。（很显然这些动作是预制的在HA中的,如果需要自定义的可能需要写代码...）
```

## 基础知识

在实体 group.living_room 上执行 homeassistant.turn_on 动作。这将打开 group.living_room 的所有智能家电成员。你也可以使用 entity_id:all, 它将打开所有可能的实体。

```yaml
action: homeassistant.turn_on
target:
  entity_id: group.living_room
```

## 目标区域和设备

除了指定实体，你也可以指定区域与设备。甚至你可以混合着多个使用，这功能可以通过target这个关键字完成。

target至少包含以下内容中的一个：area_id、device_id、entity_id。这些内容中的每一个都可以是一个列表。area_id、device_id、entity_id的值需要小写。

以下示例使用一个动作就可以打开客厅**区域**的所有灯、两个额外的灯**设备**和两个额外的灯**实体**：

```yaml
action: light.turn_on
target:
  area_id: living_room
  device_id:
    - ff22a1889a6149c5ab6327a8236ae704
    - 52c050ca1a744e238ad94d170651f96b
  entity_id:
    - light.hallway
    - light.landing
```

## 向动作传输数据

您还可以在目标实体旁边指定其他参数。如下示例，light.turn_on 动作指定了亮度，且颜色为红色。

```yaml
action: light.turn_on
target:
  entity_id: group.living_room
data:
  brightness: 120
  rgb_color: [255, 0, 0]
```

可以在每个集成的文档页面上找到动作参数的完整列表，方式与 light.turn_on 动作相同。

## 使用模板来确定到底执行哪个动作

使用模板可以支持动态选择要执行的动作。如下示例，根据传感器的温度是否大于15%，判断是否开灯。

```yaml
action: >
  {% if states('sensor.temperature') | float > 15 %}
    switch.turn_on
  {% else %}
    switch.turn_off
  {% endif %}
entity_id: switch.ac
```

## 使用动作开发者工具

你可以在动作开发者工具在测试给动作传递数据，举个例子，例如，您可以尝试打开或关闭 “群组”(请参阅[群组](https://www.home-assistant.io/integrations/group/)以了解更多信息)

为了开启或者关闭群组请传递以下信息

```
Domain: homeassistant
Action: turn_on
Action data: { "entity_id": "group.kitchen" }
```

使用模板表达式决定属性

## 模板表达式可以给动作传递数据。

```yaml
action: thermostat.set_temperature
target:
  entity_id: >
    {% if is_state('device_tracker.paulus', 'home') %}
      thermostat.upstairs
    {% else %}
      thermostat.downstairs
    {% endif %}
data:
  temperature: "{{ 22 - distance(states.device_tracker.paulus) }}"
```

##  使用模板表达式处理返回值

某些动作可以在自动化使用结束后返回数据。这个数据叫做返回值（ action response data），动作返回值通常用于动态或者较大的数据，并不适合于实体状态。相关示例有下周即将到来的日历事件和详细的驾驶指示。

模板表达式也可用于处理响应数据。该操作可以指定返回值变量response_variable。您可以为 response_variable 定义任何名称。此示例执行一个操作，并将响应存储在名为日程（agenda）的变量中。

```yaml
action: calendar.get_events
target:
  entity_id: calendar.school
data:
  duration:
    hours: 24
response_variable: agenda
```

```
关键点

在动作中数据域是否可以使用取决于该动作使用的通知类型。
```
举个例子
```yaml
action: notify.gmail_com
data:
  target: "gduser1@workspacesamples.dev"
  title: "Daily agenda for {{ now().date() }}"
  message: >-
    Your agenda for today:
    <p>
    {% for event in agenda['calendar.school'].events %}
    {{ event.start}}: {{ event.summary }}<br>
    {% endfor %}
    </p>
```

## HA内置的动作

有四个 homeassistant 操作不绑定到任何单个域，它们是：

Homeassistant.turn_on—— 打开一个实体 (支持被打开), 例如自动化、开关等。

Homeassistant.turn_off—— 关闭一个实体 (支持被关闭), 例如自动化、开关等。

Homeassistant.toggle - 关闭已打开的实体，或打开已关闭的实体 (支持打开和关闭)

Homeassistant.update_entity—— 请求更新实体，而不是等待下一个计划的更新，例如 Google 旅行时间传感器、模板传感器或灯

完整的操作细节和示例可以在 homeassistant 集成页面上找到。



