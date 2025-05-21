# 自动化触发条件

触发条件是自动化法则的一个可选部分。触发条件可以用于组织自动化操作的执行。在触发器执行后，所有触发条件将进行执行测试。只有在通过所有测试后，自动化才将执行。

触发条件看起来与触发器非常相似，但它们又非常不同触发器会（主动）观察系统中发生的事件，而触发条件只关注触发器触发后的系统的状态（被动）。触发器可以观察到开关被打开，条件只能看到开关当前是开还是关。

自动化的可用条件与脚本语法相同，请参阅[该页面](https://www.home-assistant.io/docs/scripts/conditions/)以获取可用条件的完整列表。

```yml
automation:
  - alias: "Turn on office lights"
    triggers:
      - trigger: state
        entity_id: sensor.office_motion_sensor
        to: "on"
    conditions:
      - or:
        - condition: numeric_state
          entity_id: sun.sun
          attribute: elevation
          below: 4
        - condition: numeric_state
          entity_id: sensor.office_lux_sensor
          below: 10
    actions:
      - action: scene.turn_on
        target:
          entity_id: scene.office_lights
```

自动化中的触发条件，也可以用简单的条件模板，举个例子。

```yml
automation:
  - alias: "Turn on office lights"
    triggers:
      - trigger: state
        entity_id: sensor.office_motion_sensor
        to: "on"
    conditions: "{{ state_attr('sun.sun', 'elevation') < 4 }}"
    actions:
      - action: scene.turn_on
        target:
          entity_id: scene.office_lights
```

