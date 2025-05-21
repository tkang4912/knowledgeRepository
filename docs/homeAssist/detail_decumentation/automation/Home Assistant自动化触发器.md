# 自动化触发器

触发器是启动自动化处理的规则。当任何自动化触发器变为真时（触发器触发），HA将验证触发条件，然后决定是否执行动作

一个自动化可能会被事件，特定的实体状态，特定的时间，或者其它触发条件触发。这些都可以直接指定或者通过模版更灵活地指定。一个自动化中可能有指定多个触发器。

- [Trigger ID](https://www.home-assistant.io/docs/automation/trigger/#trigger-id)
- [Trigger variables](https://www.home-assistant.io/docs/automation/trigger/#trigger-variables)
- [Event trigger](https://www.home-assistant.io/docs/automation/trigger/#event-trigger)
- [Home Assistant trigger](https://www.home-assistant.io/docs/automation/trigger/#home-assistant-trigger)
- [MQTT trigger](https://www.home-assistant.io/docs/automation/trigger/#mqtt-trigger)
- [Numeric state trigger](https://www.home-assistant.io/docs/automation/trigger/#numeric-state-trigger)
- [State trigger](https://www.home-assistant.io/docs/automation/trigger/#state-trigger)
- [Sun trigger](https://www.home-assistant.io/docs/automation/trigger/#sun-trigger)
- [Tag trigger](https://www.home-assistant.io/docs/automation/trigger/#tag-trigger)
- [Template trigger](https://www.home-assistant.io/docs/automation/trigger/#template-trigger)
- [Time trigger](https://www.home-assistant.io/docs/automation/trigger/#time-trigger)
- [Time pattern trigger](https://www.home-assistant.io/docs/automation/trigger/#time-pattern-trigger)
- [Persistent notification trigger](https://www.home-assistant.io/docs/automation/trigger/#persistent-notification-trigger)
- [Webhook trigger](https://www.home-assistant.io/docs/automation/trigger/#webhook-trigger)
- [Zone trigger](https://www.home-assistant.io/docs/automation/trigger/#zone-trigger)
- [Geolocation trigger](https://www.home-assistant.io/docs/automation/trigger/#geolocation-trigger)
- [Device triggers](https://www.home-assistant.io/docs/automation/trigger/#device-triggers)
- [Calendar trigger](https://www.home-assistant.io/docs/automation/trigger/#calendar-trigger)
- [Sentence trigger](https://www.home-assistant.io/docs/automation/trigger/#sentence-trigger)
- [Multiple triggers](https://www.home-assistant.io/docs/automation/trigger/#multiple-triggers)
- [Multiple Entity IDs for the same Trigger](https://www.home-assistant.io/docs/automation/trigger/#multiple-entity-ids-for-the-same-trigger)
- [Disabling a trigger](https://www.home-assistant.io/docs/automation/trigger/#disabling-a-trigger)
- [Merging lists of triggers](https://www.home-assistant.io/docs/automation/trigger/#merging-lists-of-triggers)

## 触发器编号（[Trigger ID](https://www.home-assistant.io/docs/automation/trigger/#trigger-id)）

所有触发器都可以被分配一个编号，如果编号没有被手动分配，触发器的编号将被设置为触发器的下标。 编号知识可以参考 [触发器条件与行为](https://www.home-assistant.io/docs/scripts/conditions/#trigger-condition).触发器编号甚至不需要唯一，多个触发器可以同一个编号，它可以用来将类似的触发器分组，以便在自动化过程中的后续使用 (几个不同类型的触发器应该都能触发某个实体)。

## 触发器变量

对于触发器来说变量有两种不同的类型。但它们都可以像脚本级别的变量一样工作。

第一种方法定义的变量（ my_event: example_event），这种变量必须现在trigger_variables下定义，默认也是trigger_variables前缀（ trigger_variables.my_event），该变量为[受限变量](http://home-assistant.io/docs/configuration/templating/#limited-templates)，例如只在 **自动化加载时计算一次**，不会动态响应变量变化。

第二种方法定义的变量较为简单：允许你在触发器触发时候获取值，可以使用双括号的方式（下称模版方法{{}}）来获取值，例如name: "{{ trigger.event.data.name }}"。

```yaml
automation:
  trigger_variables:
  #第一种
    my_event: example_event
  triggers:
    - trigger: event
      # Able to use `trigger_variables`
      event_type: "{{ my_event }}"
      # These variables are evaluated and set when this trigger is triggered
      variables:
      #第二种
        name: "{{ trigger.event.data.name }}"
```

## 事件触发器

事件触发器会在事件触发时候执行，事件是HA的原始构建块。你可以根据事件是否拥有某些事件名，或者某些特定的事件数据（你代码里面写的），上下文数据（HA自动生成的，例如谁发的事件）来匹配（这样较为灵活）。

事件可以通过集成或 API 触发。类型没有限制。可以在这里找到内置事件的列表。

```yml
automation:
  triggers:
    - trigger: event
      event_type: "MY_CUSTOM_EVENT"
      # optional
      event_data:
        mood: happy
      context:
        user_id:
        # any of these will match
          - "MY_USER_ID"
          - "ANOTHER_USER_ID"
```

也可以同时监听多个事件，这对于不包含或类似数据和上下文的事件很有用。

```yml
automation:
  triggers:
    - trigger: event
      event_type:
        - automation_reloaded
        - scene_reloaded
```

也可以在 event_type、event_data 和上下文（context）选项中使用受限模板。

```yml
automation:
  trigger_variables:
    sub_event: ABC
    node: ac
    value: on
  triggers:
    - trigger: event
      event_type: "{{ 'MY_CUSTOM_EVENT_' ~ sub_event }}"
```

## Home Assistant触发器

在HomeAssist启动或者关闭时触发

```yml
  triggers:
    - trigger: homeassistant
      # Event can also be 'shutdown'
      event: start
```

​	**由关闭事件触发的自动化有 20 秒的运行时间，之后它们将被停止以继续进行关闭。**



## MQTT 触发器

MQTT触发器只在在对应主题上收到指定的信息时候触发。可以选择匹配发送到主题（topic）的载荷（payload：发送的一些数据）。默认的载荷编码是UTF-8。对于图像和其他字节有效载荷，使用“**encoding: ''**禁用解码。

```yml
automation:
  triggers:
    - trigger: mqtt
      topic: "living_room/switch/ac"
      # Optional
      payload: "on"
      encoding: "utf-8"
```

载荷（payload）选项可以与值模板（value_template）联合使用。这种方式会先处理在MQTT指定主题上受到的信息，然后在执行与载荷匹配。下面这个触发器例子，只有在收到主题为`living_room/switch/ac`的数据且该json数据有中state状态为on时候执行。

```yml
automation:
  triggers:
    - trigger: mqtt
      topic: "living_room/switch/ac"
      payload: "on"
      value_template: "{{ value_json.state }}"
```

在主题和载荷选项上可以使用受限模板。

```yml
automation:
  trigger_variables:
    room: "living_room"
    node: "ac"
    value: "on"
  triggers:
    - trigger: mqtt
      topic: "{{ room ~ '/switch/' ~ node}}"
      # Optional
      payload: "{{ 'state:' ~ value }}"
      encoding: "utf-8"
```

## 数值状态触发器

数值状态触发器会在一个实体的值（如果你使用的是`attribute` 属性或者value_template属性那就是属性（attribute）的值），超过给定的阈值。当指定实体的状态发生变化时，尝试将状态解析为一个数字，并在值从上到下或从下到上变化时触发。

```
跨越阈值意味着只有当状态之前不在阈值范围内时，触发器才会被触发。如果你的实体当前状态为 50, 并且你将阈值设置为 75, 那么如果状态变为例如 49 或 72, 触发器就不会被触发，因为阈值从未被跨越。状态必须先变为例如 76, 然后再变为例如 74, 触发器才会被触发。
```

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id: sensor.temperature
      # If given, will trigger when the value of the given attribute for the given entity changes..
      attribute: attribute_name
      # ..or alternatively, will trigger when the value given by this evaluated template changes.
      value_template: "{{ state.attributes.value - 5 }}"
      # At least one of the following required
      above: 17
      below: 25
      # for用于某个状态或触发条件持续成立一段时间后，才触发自动化。
      for:
        hours: 1
        minutes: 10
        seconds: 5
```

**above和below同时设置值时候意味着 numeric_state触发器规定的传感器的返回的温度必须在这两个值之间。在上面的例子中，温度进入 17.1-24.9 范围 (大于 17 且小于 25), 触发器将触发一次。它只会在离开定义的范围并再次进入该范围后再次触发。**

当指定属性（attribute）选项时，触发器将与给定的属性进行比较，而不是实体的状态。

使用 value_template 可以进行动态且更复杂的计算。变量 state是由 entity_id 指定的实体的 state 对象。

实体的状态使用方法可以参考下面

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id: sensor.temperature
      value_template: "{{ state.state | float * 9 / 5 + 32 }}"
      above: 70
```

实体的属性使用方法参考下面

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id: climate.kitchen
      value_template: "{{ state.attributes.current_temperature - state.attributes.temperature_set_point }}"
      above: 3
```

input_number实体、num、sensor、以及zone等包含数值的实体可以使用above与below两个属性。但是只有在触发器指定的实体更新时候才会进行比较。下面例子写的是传感器外的温度大于传感器内部温度就触发，但是该触发器只检测了传感器内的温度，所以如果传感器内的温度不变，传感器外的温度升高后大于传感器内的温度也不会触发。

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id: sensor.outside_temperature
      # Other entity ids can be specified for above and/or below thresholds
      above: sensor.inside_temperature
```

for还可以写成为 `HH:MM:SS`格式, 如下所示：

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id: sensor.temperature
      # At least one of the following required
      above: 17
      below: 25

      # If given, will trigger when condition has been for X time.
      for: "01:10:05"
```

还可以在 for 选项中使用模板。

```yml
automation:
  triggers:
    - trigger: numeric_state
      entity_id:
        - sensor.temperature_1
        - sensor.temperature_2
      above: 80
      for:
        minutes: "{{ states('input_number.high_temp_min')|int }}"
        seconds: "{{ states('input_number.high_temp_sec')|int }}"
  actions:
    - action: persistent_notification.create
      data:
        message: >
          {{ trigger.to_state.name }} too high for {{ trigger.for }}!
```

当实体改变时候，`for`中模板就会重新计算。


for中设置的时间在HA重启或者重新加载自动化的时候会自动重置。如果不想被重置，可以使用将 input_datetime设置为所需时间，然后使用该 [input_datetime](https://www.home-assistant.io/integrations/input_datetime) 作为自动化触发器，在设定的时间执行所需的操作。

## 状态触发器

一般来说，任意给定的实体的值发生改变时候，状态触发器就会触发。触发行为如下：

- 如果只给定了`entity_id`, 触发器会对所有状态变化触发，即使只有一个状态属性发生了变化。
- 如果设置`from`、`to`、`not_from`或 `not_to` 中的一个，触发器会触发任何匹配的状态变化，但如果只是属性发生变化，则不会触发。（状态与属性的区别，比如说灯的亮度属性0-10，亮度0时候的状态是关闭，亮度1-10的状态都是开启）
  - 如果想要状态改变时候触发，不论状态变成什么而不是在设置的属性值改变时触发，可以把`from`,`to`,`no_from`,或者`not_to`其中至少一个设置为null。
- 使用 for 选项无法在 Home Assistant 重启或自动化重新加载后继续运行。
  -  在重新加载与重新启动时候，`for`的等待时候会被重置。
  - 如果你不像触发器这样执行，可以使用自动化将 input_datetime 设置为所需的时间，然后使用该 input_datetime 作为自动化触发器，在设定的时间执行所需的操作。

### 示例

如果 Paulus 或 Anne-Therese 在家一分钟，这个自动化系统就会触发。

```yml
automation:
  triggers:
    - trigger: state
      entity_id:
        - device_tracker.paulus
        - device_tracker.anne_therese
      # Optional
      from: "not_home"
      # Optional
      to: "home"
      # If given, will trigger when the condition has been true for X time; you can also use days and milliseconds.
      for:
        hours: 0
        minutes: 1
        seconds: 0
```

`from`或者`to`可以接收列表值

```yml
automation:
  triggers:
    - trigger: state
      entity_id: vacuum.test
      from:
        - "cleaning"
        - "returning"
      to: "error"
```

如果你想在状态变化时候触发，但不因为属性变化二触发，可以将其设置为 null (这也可以通过将 from、not_from 或 not_to 设置为 null 来实现):

```yml
automation:
  triggers:
    - trigger: state
      entity_id: vacuum.test
      to:
```

### 状态触发器中属性变化使得触发器触发

当指定属性选项时，触发器仅在指定属性发生变化时触发。对其他属性或状态的变化将被忽略。

例如，这个触发器只有在锅炉加热 10 分钟后才会触发：

```yml
automation:
  triggers:
    - trigger: state
      entity_id: climate.living_room
      attribute: hvac_action
      to: "heating"
      for: "00:10:00"
```

每当锅炉的 hvac_action 属性发生变化时，这个触发器就会触发：

```yml
automation:
  triggers:
    - trigger: state
      entity_id: climate.living_room
      attribute: hvac_action
```

### 状态触发器某种状态持续一段时间后触发

你可以使用 `for`使状态触发器仅在状态持续一段时间时才会触发。下面的例子在实体状态变为 “on” 并保持该状态 30 秒时触发：

```yml
automation:
  triggers:
    - trigger: state
      entity_id: light.office
      # Must stay "on" for 30 seconds
      to: "on"
      for: "00:00:30"
```

你可以在`for`的属性中使用模板方法

```yml
automation:
  triggers:
    - trigger: state
      entity_id:
        - device_tracker.paulus
        - device_tracker.anne_therese
      to: "home"
      for:
        minutes: "{{ states('input_number.lock_min')|int }}"
        seconds: "{{ states('input_number.lock_sec')|int }}"
  actions:
    - action: lock.lock
      target:
        entity_id: lock.my_place
```

当实体按指定方式发生变化时，重新执行 `for`中 模板方法。

**使用引号围绕值，以避免 YAML 解析器将值解释为布尔值。**

## 日光触发器

### 日升日落触发器

日升日落触发器当太阳升起或者西沉，即太阳高度到达0度时候触发

可以设置一个可选的时间偏移，使得太阳事件之前或者之后触发。负值就是提前多少时间，正值就是延后多长时间。格式可以使用 hh:mm:ss 格式指定

**由于黄昏的持续时间在一年中有所不同，建议在黄昏或黎明时使用太阳高度触发器来代替日落或日出，并使用时间偏移来触发自动化。**

```yml
automation:
  triggers:
    - trigger: sun
      # 可选的值有sunset或者sunrise
      event: sunset
      # Optional time offset. This example will trigger 45 minutes before sunset.
      offset: "-00:45:00"
```

### 太阳高度触发器（好奇宗教会用到这个吗？）

有时，你可能需要对自动化进行更精细的控制，不仅仅是简单的日落或日出，而是指定太阳的精确高度。随着太阳的上升或者下降，你可以分层次地安排自动化行为。当仅靠 sunset 事件不能够满足条件，例如还不够黑时（如夏天日落后天还很亮），这种方式可以更精准控制， 在这种情况下你可以用太阳高度角在具体时刻触发，比如天真正黑下来时。对于大多数计划在黄昏或黎明时运行的自动化系统，0° 到 - 6° 之间的数值较为合适；在本示例中使用 - 4°:

```yml
automation:
  - alias: "Exterior Lighting on when dark outside"
    triggers:
      - trigger: numeric_state
        entity_id: sun.sun
        attribute: elevation
        # Can be a positive or negative number
        below: -4.0
    actions:
      - action: switch.turn_on
        target:
          entity_id: switch.exterior_lighting
```

如果你想要更加精确，使用这个[太阳高度角计算器](https://gml.noaa.gov/grad/solcalc/)，它将帮助你计算指定时间的太阳角度，你可以根据这个数据进行触发。

虽然说时间的光照情况还与天气，地形等有关，但他们有以下关系。

- 黄昏：0°> 太阳角 >-6°

```
这就是黄昏对普通人的意义：在晴朗的天气条件下，黄昏接近于太阳光照足以让人眼清晰分辨地面物体的极限。足够的照明使得大多数户外活动不再需要人工光源。
```

- 航海黄昏：-6° > 太阳角 > -12°

- 曙暮光：-12° > 太阳角 > -18°

关于这一点，维基百科关于暮光的文章提供了一个非常全面的解释。



## 标签（二维码，NFC）触发器

标签触发器在标签被扫描时会被触发，举个例子。例如，使用HA配套的移动应用程序扫描 NFC 标签。

```yml
automation:
  triggers:
    - trigger: tag
      tag_id: A7-6B-90-5F
```

此外，您还可以通过设置设备编号（device_id）来指定只有在特定设备 / 扫描仪扫描卡时才能触发

```yml
automation:
  triggers:
    - trigger: tag
      tag_id: A7-6B-90-5F
      device_id: 0e19cd3cf2b311ea88f469a7512c307d
```

或在多个可能的设备上触发多个标签：

```yml
automation:
  triggers:
    - trigger: tag
      tag_id:
        - "A7-6B-90-5F"
        - "A7-6B-15-AC"
      device_id:
        - 0e19cd3cf2b311ea88f469a7512c307d
        - d0609cb25f4a13922bb27d8f86e4c821
```

## 模板触发器

模板触发器会在任何实体改变状态时执行模板，且模板执行并且返回值从`false`到`true`(非零数或任何字符串为 true、yes、on、enable)时候触发。

这可以通过模版方法返回布尔值来达到获取返回true或false的目的，举个例子（{{ is_state('device_tracker.paulus', 'home') }}，判断他到家没有），还有可以通过模板方法获得布尔值的方法(下面的示例)。

使用模板触发器，还可以使用 is_state_attr 来评估属性变化 (例如 {{ is_state_attr (‘climate.living_room’,‘away_mode’,‘off’)}})

```yml
automation:
  triggers:
    - trigger: template
      value_template: "{% if is_state('device_tracker.paulus', 'home') %}true{% endif %}"

      # If given, will trigger when template remains true for X time.
      for: "00:01:00"
```

同样的你也可以在 for 选项中使用模板。

```yml
automation:
  triggers:
    - trigger: template
      value_template: "{{ is_state('device_tracker.paulus', 'home') }}"
      for:
        minutes: "{{ states('input_number.minutes')|int(0) }}"
```

不包含实体的模板将每分钟渲染一次。

## 时间触发器

时间触发器会在每天在特定时间或特定日期的特定时间触发。允许的格式有三种：

### 时间字符串

每天都触发的触发器，其时间字符串格式。可以指定为 HH:MM 或 HH:MM:SS。如果未指定秒数，则使用：00。

```yml
automation:
  - triggers:
    - trigger: time
      # 24-hour time format. This trigger will fire at 3:32 PM
      at: "15:32:00"
```

### 输入日期

输入日期时间的实体 ID。

| `has_date` | `has_time` | 描述说明                                 |
| ---------- | ---------- | ---------------------------------------- |
| `true`     | `true`     | 在指定的 **日期和时间** 触发一次。       |
| `true`     | `false`    | 在指定的 **日期的凌晨 00:00** 触发一次。 |
| `false`    | `true`     | **每天** 在指定的时间触发一次。          |

```yml
automation:
  - triggers:
      - trigger: state
        entity_id: binary_sensor.motion
        to: "on"
    actions:
      - action: climate.turn_on
        target:
          entity_id: climate.office
      - action: input_datetime.set_datetime
        target:
          entity_id: input_datetime.turn_off_ac
        data:
          datetime: >
            {{ (now().timestamp() + 2*60*60)
               | timestamp_custom('%Y-%m-%d %H:%M:%S') }}
  - triggers:
      - trigger: time
        at: input_datetime.turn_off_ac
    actions:
      - action: climate.turn_off
        target:
          entity_id: climate.office
```

### 日期时间设备类传感器

具有 “时间戳” 的设备类的传感器的实体 ID。

### 可提前或延后的日期时间设备类传感器

当使用时间戳设备类的传感器提供时间时，可以提供偏移量。这个偏移量将被添加到传感器值 (如果为负，则是提前时间)。

例如，这个触发器在手机闹钟响起前 5 分钟触发。

```yml
automation:
  - triggers:
      - trigger: time
        at:
          entity_id: sensor.phone_next_alarm
          offset: -00:05:00
    actions:
      - action: light.turn_on
        target:
          entity_id: light.bedroom
```

**当使用正偏移量时，触发器可能永远不会触发。这是因为传感器在达到偏移量之前发生了变化。例如，当使用手机闹钟作为触发器时，当闹钟响起时，传感器值会变为新的闹钟时间，这意味着这个触发器也会变为新的时间。**

## 多个时间

可以在列表中提供多个时间。所有格式都可以混合使用。

```yml
automation:
  triggers:
    - trigger: time
      at:
        - input_datetime.leave_for_work
        - "18:30:00"
        - entity_id: sensor.bus_arrival
          offset: "-00:10:00"
```

### 受限模板方法

也可以时间设置上使用受限模板方法。

```yml
blueprint:
  input:
    alarm:
      name: Alarm
      selector:
        text:
    hour:
      name: Hour
      selector:
        number:
          min: 0
          max: 24

  trigger_variables:
    my_alarm: !input alarm
    my_hour: !input hour
  trigger:
    - platform: time
      at:
      - "sensor.{{ my_alarm | slugify }}_time"
      - "{{ my_hour }}:30:00"
```

## 时间模式触发器

使用时间模式触发器，您可以匹配当前时间的小时、分钟或秒是否与特定值匹配。您可以在值前加上 /, 以便在该值可以被该数整除时进行匹配（就是根据设置的小时数或分钟数或者秒数隔多长时间触发一次）。您可以指定 * 来匹配任何值 (当使用 Web 界面时，这是必要的，字段不能保持空白)。

```yml
automation:
  triggers:
    - trigger: time_pattern
      # 将会 每小时的第 5 分钟 触发一次自动化。
      minutes: 5

automation 2:
  triggers:
    - trigger: time_pattern
      # 时间是三点时候每分钟都会触发
      hours: "3"
      minutes: "*"

automation 3:
  triggers:
    - trigger: time_pattern
      # 每五分钟触发
      minutes: "/5"
```

## 持续通知触发器

当添加（added）或删除（removed）与配置的选项匹配的 persistent_notification 时，将触发持续通知触发器。

```yml
automation:
  triggers:
    - trigger: persistent_notification
      update_type:
        - added
        - removed
      notification_id: invalid_config
```

有关事件触发器的更多详细信息，以及可供自动化使用的其他事件数据，请参阅持续通知集成。

## webhook触发器

Webhook 触发器在向 Webhook 终端发出 Web 请求时触发：`/api/webhook/<webhook_id>`。当您在自动化触发器中将 Webhook 端点设置为 `webhook_id` 时，将自动创建该端点。

```yml
automation:
  triggers:
    - trigger: webhook
      webhook_id: "some_hook_id"
      allowed_methods:
        - POST
        - PUT
      local_only: true
```

你可以通过向 http://你的HA的IP:8123/api/webhook/some_hook_id 发送 HTTP POST 请求来运行这个自动化。下面是一个使用 curl 命令行程序的示例，其中包含一个示例表单数据负载：

```bash
curl -X POST -d 'key=value&key2=value2' https://your-home-assistant:8123/api/webhook/some_hook_id
```

Webhook 支持 HTTP POST、PUT、HEAD 和 GET 类型请求；推荐使用 PUT 请求。默认情况下不启用 HTTP GET 和 HEAD 请求，但可以通过将其添加到 allowed_methods 选项中来启用。也可以通过单击 Webhook ID 旁边的设置齿轮菜单按钮，在 UI 中配置请求方法。

默认情况下，只能从与 Home Assistant 位于同一网络的设备或通过 [Nabu Casa Cloud Webhook](https://support.nabucasa.com/hc/en-us/articles/25619382358685-Triggering-an-automation-with-a-webhook-trigger) 访问 Webhook 触发器。 如果想通过互联网直接触发 Webhook，将 local_only 选项设置为 false。也可以通过点击 Webhook ID 旁边的设置设备菜单按钮，在 UI 中配置此选项。

如果您已使用 SSL/TLS 安装了HA，请记得使用 HTTPS URL。

请注意，给定的 Webhook 一次只能用于一个自动化。也就是说，只有一个自动化触发器可以使用特定的 Webhook ID。

### Webhook 数据

负载以form data 或 JSON为有效编码。根据具体情况，其数据将在自动化模板中以 trigger.data 或 trigger.json 的形式提供。URL 查询参数也可以在模板中以 trigger.query 的形式提供。请注意，要使用 JSON 编码的负载，必须将 Content-Type 标头设置为 application/json, 例如：

```bash
curl -X POST -H "Content-Type: application/json" -d '{ "key": "value" }' https://你的HA的ip地址:8123/api/webhook/some_hook_id
```

### Webhook 安全性

Webhook 端点不需要身份验证，除非知道有效的 Webhook ID。Webhook 的安全最佳实践包括：

- 不要使用 Webhook 触发具有破坏性的自动化操作，否则可能会造成安全问题。例如，不要使用 Webhook 来解锁或打开车库门。
- 将 Webhook ID 视为密码：使用很难猜出来的值，而且要保密，不要与你的其它密码相同。
- 不要复制粘贴来自公共来源 (包括蓝图) 的 Webhook ID。始终创建你自己的。
- 如果不需要来自互联网的访问，请为 Webhook 启用 local_only 选项。

## 区域触发器

当实体离开或者进入区域时候，一般来说可以是人或者贵重的东西，区域触发器会触发。要是区域触发器类型的自动化正常工作，你需要设置一个支持GPS坐标的device_trackerpintai。这包括 GPS Logger、OwnTracks 平台和 iCloud 平台。

```yml
automation:
  triggers:
    - trigger: zone
      entity_id: person.paulus
      zone: zone.home
      # Event is either enter or leave
      event: enter # or "leave"
```

## 地理定位触发器

当实体在区域内出现或消失时，会触发地理定位触发器。由地理定位平台创建的实体支持报告 GPS 坐标。由于实体是由这些平台自动生成和删除的，因此通常无法预测实体 ID。相反，这个触发器需要定义一个来源，该源直接链接到其中一个地理定位平台。

```yml
automation:
  triggers:
    - trigger: geo_location
      source: nsw_rural_fire_service_feed
      zone: zone.bushfire_alert_zone
      # Event is either enter or leave
      event: enter # or "leave"
```

## 设备触发器

设备触发器包含一组由集成定义的事件。例如，这包括传感器的状态变化以及来自遥控器的按钮事件。MQTT 设备触发器是通过自动发现设置的。

与状态触发器不同，设备触发器与设备相关联，而不一定与实体相关联。要使用设备触发器，请通过浏览器前端设置自动化。如果你想使用设备触发器来实现不通过浏览器前端管理的自动化，可以从前端的触发器构件中复制 YAML, 并将其粘贴到自动化的触发器列表中。

## 日历触发器

日历触发器在日历事件开始或结束时触发，与使用一次只支持一个事件开始的日历实体状态相比，这种触发器提供了更灵活的自动化功能。

可以提供一个可选的时间偏移，使其在日历事件之前或之后触发一个设定的时间 (例如，事件开始前 5 分钟)。

```yml
automation:
  triggers:
    - trigger: calendar
      # Possible values: start, end
      event: start
      # The calendar entity_id
      entity_id: calendar.light_schedule
      # Optional time offset
      offset: "-00:05:00"
```

有关事件触发器的更多详细信息，以及可供自动化使用的其他事件数据，请参阅[日历](https://www.home-assistant.io/integrations/calendar/)集成。

## 语音触发器

当 Assist 使用默认对话代理与语音助手的句子匹配时，将触发句子触发器。句子触发器仅适用于 Home Assistant Assist。OpenAI 或 Google Generative AI 等外部对话代理不能用于触发自动化。

允许句子使用一些基本的模板语法，如可选词和替代词。例如，[it‘s] party time 将匹配 “party time” 和 “it is party time”。

```yml
automation:
  triggers:
    - trigger: conversation
      command:
        - "[it's ]party time"
        - "happy (new year|birthday)"
```

这个触发器匹配的语音句子将是：

- party time
- it’s party time
- happy new year
- happy birthday

标点符号和大小写被忽略，因此 “It’s PARTY TIME!!!” 也将匹配\

### 相关主题

- [在语音触发器中添加自定义语句](https://www.home-assistant.io/voice_control/custom_sentences/#adding-a-custom-sentence-to-trigger-an-automation)

## 复合型触发器

可以为同一规则指定多个触发器。为此，只需在每个触发器的第一行前加上一个破折号 (-), 并相应地缩进后续行。每当其中一个触发器触发时，自动化规则的处理就会开始。

```yml
  triggers:
    # first trigger
    - trigger: time_pattern
      minutes: 5
      # our second trigger is the sunset
    - trigger: sun
      event: sunset
```

同一触发器中使用多个实体 ID

可以为同一触发器指定多个实体。方法是使用嵌套列表添加多个实体。触发器将被触发并启动，每当触发器对所有列出的实体都为真时，都会处理您的自动化。

```yml
automation:
  triggers:
    - trigger: state
      entity_id:
        - sensor.one
        - sensor.two
        - sensor.three
```

### 禁用触发器

可以禁用自动化中的每个单独触发器，而无需将其删除。为此，可以在触发器中添加 enabled: false。例如：

```yml
# Example script with a disabled trigger
automation:
  triggers:
    # This trigger will not trigger, as it is disabled.
    # This automation does not run when the sun is set.
    - enabled: false
      trigger: sun
      event: sunset

    # This trigger will fire, as it is not disabled.
    - trigger: time
      at: "15:32:00"
```

触发器也可以基于受限模板方法或蓝图输入禁用。这些只会在自动化加载时执行一次。

```yml
blueprint:
  input:
    input_boolean:
      name: Boolean
      selector:
        boolean:
    input_number:
      name: Number
      selector:
        number:
          min: 0
          max: 100

  trigger_variables:
    _enable_number: !input input_number

  triggers:
    - trigger: sun
      event_type: sunrise
      enabled: !input input_boolean
    - trigger: sun
      event_type: sunset
      enabled: "{{ _enable_number < 50 }}"
```

合并触发器列表

**警告**

此功能需要 Home Assistant 版本 2024.10 或更高版本。如果在蓝图中使用此功能，请将蓝图的 min_version 设置为至少此版本。有关更多详细信息，请参阅[蓝图架构文档](https://www.home-assistant.io/docs/blueprint/schema/#min_version)。

在某些高级情况下 (例如，对于具有触发器选择器的蓝图), 可能需要在主触发器列表中插入第二个触发器列表。这可以通过在主触发器列表中添加一个字典来实现，该字典包含唯一的键触发器，而该键的值包含第二个触发器列表。然后，这些触发器将被压缩成一个单一的触发器列表。例如：

```yml
blueprint:
  name: Nested Trigger Blueprint
  domain: automation
  input:
    usertrigger:
      selector:
        trigger:

triggers:
  - trigger: event
    event_type: manual_event
  - triggers: !input usertrigger
```

这个蓝图自动化可以由固定的 manual_event 触发器触发，也可以由触发器选择器中选择的任何触发器触发。这也适用于 wait_for_trigger 操作。



其他：input_datetime的使用方法

```yml
# Example configuration.yaml entry
input_datetime:
  both_date_and_time:
    name: Input with both date and time
    has_date: true
    has_time: true
  only_date:
    name: Input with only date
    has_date: true
    has_time: false
  only_time:
    name: Input with only time
    has_date: false
    has_time: true
```

设置值

```yml
# Sets time to 05:30:00
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    time: "05:30:00"
# Sets time to time from datetime object
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    time: "{{ now().strftime('%H:%M:%S') }}"
# Sets date to 2020-08-24
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    date: "2020-08-24"
# Sets date to date from datetime object
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    date: "{{ now().strftime('%Y-%m-%d') }}"
# Sets date and time to 2020-08-25 05:30:00
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    datetime: "2020-08-25 05:30:00"
# Sets date and time from datetime object
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    datetime: "{{ now().strftime('%Y-%m-%d %H:%M:%S') }}"
# Sets date and/or time from UNIX timestamp
# This can be used whether the input_datetime has just a date,
# or just a time, or has both
- action: input_datetime.set_datetime
  target:
    entity_id: input_datetime.XXX
  data:
    timestamp: "{{ now().timestamp() }}"
```

使用值

```yml
# Example configuration.yaml entry
# Turns on bedroom light at the time specified.
automation:
  triggers:
    - trigger: time
      at: input_datetime.bedroom_alarm_clock_time
  actions:
    - action: light.turn_on
      target:
        entity_id: light.bedroom
```

