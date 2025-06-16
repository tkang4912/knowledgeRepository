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

