# 与HA对话-控制你的系统启动与运行

**该部分将帮助你设置HA的语音助手。**

该助手可以帮助用自然语音而非编程方式控制HA。它建立在开放的声音基础之上，并由我们社区提供技术支持。

最简单的尝试方式就是在我们的配套APP上使用语音助手。请在您的信息中心右上角找到“Assist”图标。

开始使用 Assist 的最简单方法是使用我们推荐的语音助手硬件[Home Assistant Voice Preview Edition](https://www.home-assistant.io/voice-pe/)。

至于 Home Assistant 的其他核心功能，Assist 可以根据您的需求进行个性化和扩展。

- 它支持本地工作和当前最大的大模型。
- 它支持在您的手机上和其他自定义语音设备上运行。

## 开始

当你为HA配置一个语音助手的硬件时，它将使用向导来帮助您配置系统并开始使用语音。

我们推荐的语音助手硬件是[Home Assistant Voice Preview Edition](https://www.home-assistant.io/voice-pe/)。

如果您的硬件不支持我们的向导，请不要担心。以下是两个详细的指南，分别基于您计划如何处理语音（本地语音或使用 Home Assistant Cloud 语音服务）。

- [我计划在本地处理我的声音](https://www.home-assistant.io/voice_control/voice_remote_local_assistant/)
- [我计划使用 Home Assistant Cloud](https://www.home-assistant.io/voice_control/voice_remote_cloud_assistant/)（推荐，因为它最简单）



## 扩展和实验

您的设置启动并运行后，请遵循[最佳实践，查看我们为](https://www.home-assistant.io/voice_control/best_practices)[扩展您的辅助设置](https://www.home-assistant.io/voice_control/expanding_assist)找到的所有功能，并进一步尝试不同的设置，例如[唤醒词](https://www.home-assistant.io/voice_control/about_wake_word/)。您想和绘梨衣对话吗？或者其他人物？如果您想让辅助以有趣的方式响应，您可以创建一个具有[AI个性的](https://www.home-assistant.io/voice_control/assist_create_open_ai_personality/)助手。

您还可以做其他事情来进一步丰富您的设置：

- 语音助手设备可让您在房间中添加辅助功能，并响应唤醒词。[只需 13 美元，即可按照我们的教程创建您自己的语音助手。](https://www.home-assistant.io/voice_control/thirteen-usd-voice-remote/)
- 您可以使用[ESPHome](https://www.esphome.io/components/voice_assistant.html)创建你自己的超棒语音助手，比如[@piitaya](https://github.com/piitaya)用他的 3D 打印 R5 机器人做了什么：
- 如果您想要一个不是一直聆听的语音助手，可以考虑在模拟电话上使用 Assist。它只会在您按喇叭时聆听，并且响应仅供您聆听。请按照我们的教程创建您自己的[模拟电话语音助手](https://www.home-assistant.io/voice_control/worlds-most-private-voice-assistant/)。

## 支持的语言和句子

Assist 旨在支持比其他语音助手更多的语言，但这项工作仍在进行中，我们需要您的帮助。

Assist 已支持多种语言。您可以使用[内置语句](https://www.home-assistant.io/voice_control/builtin_sentences)[来](https://developers.home-assistant.io/docs/voice/intent-recognition/supported-languages)控制实体和区域，也[可以创建自己的触发语句](https://www.home-assistant.io/voice_control/custom_sentences/)。

Assist 没理解你的语句吗？[贡献出来吧](https://www.home-assistant.io/voice_control/contribute-voice)。

*Assist 是在 Home Assistant 2023.2 中引入的。*

## 相关主题

- [Android 上的协助](https://www.home-assistant.io/voice_control/android/)
- [协助苹果](https://www.home-assistant.io/voice_control/apple/)
- [使用 esphome 设备构建一个价值 13 美元的语音遥控器](https://www.home-assistant.io/voice_control/thirteen-usd-voice-remote/)
- [协助的最佳实践](https://www.home-assistant.io/voice_control/best_practices/)

## 相关链接

- [家庭助理云](https://www.nabucasa.com/config/assist/)
- [语音预览版](https://support.nabucasa.com/hc/en-us/categories/24451727188125-Home-Assistant-Voice-Preview-Edition)