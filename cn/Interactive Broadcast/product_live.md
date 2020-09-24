
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Mon Sep 14 2020 10:27:44 GMT+0800 (CST)
---
# 产品概述

Agora 视频互动直播（Live Interactive Video Streaming）可以实现一对多，多对多的音视频互动直播。

Agora 视频互动直播不同于视频通话。视频通话不区分主播和观众，所有用户都可以发言并看见彼此；视频直播的用户分为主播和观众，只有主播可以自由发言，且被其他用户看见。详见[通信和直播场景有什么区别](https://docs.agora.io/cn/faq/profile_difference)。

常见的 CDN 直播是一个主播和多个观众，是单向的。而 Agora 互动直播还能多个主播之间，观众与主播之间连麦，就像在小剧场里观众可以上台表演一样。适用于娱乐直播如狼人杀、教育直播如小班课、电商直播中的导购问答等强互动场景。同时，也适用对图像质量要求高的一对一视频聊天。

## 功能和场景

Agora 视频互动直播提供丰富的功能，你可以根据自己的场景需求灵活组合。

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| 主要功能             | 功能描述                                                     | 典型适用场景                                                 |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 观众连麦           | 观众与主播连麦聊天，观众围观。                               | <li>大型直播时，主播邀请观众互动 <li>狼人杀、剧本杀          |
| 跨直播间连麦         | 多个主播跨直播间，连麦互动，观众围观。                       | PK 连麦                                                      |
| 伴奏混音             | 将本地或在线的音频和用户声音，同时发送并播放给频道内其他用户 | <li>在线合唱 <li>针对幼儿的音乐互动课堂                      |
| 基础美颜          | 支持基础的美颜功能，包括设置美白、磨皮、祛痘、红润效果。 | 娱乐直播美颜      |
	| 屏幕共享      | 把屏幕内容同步展示给频道内的其他用户，支持指定共享某个屏幕或窗口，同时支持指定共享区域。      | <li>互动课堂<li>游戏主播展示游戏实战                         |
| 修改音视频原始数据   | 可支持变声，支持获取媒体引擎的原始语音或视频数据，对原始数据进行处理 | <li>语音聊天室变声<li>娱乐直播美颜                           |
| 在线媒体流输入       | 可以将媒体流作为一个发送端接入正在进行的直播房间。通过将正在播放的音视频添加到直播中，主播和观众可以在一起收听/观看媒体流的同时，实时互动。 可以对输入源的视频属性进行设置。 | <li>主播和观众一起看电影 <li>主播和观众一起看比赛            |
| 自定义视频源和渲染器 | 支持自定义的视频源和渲染器，可以不使用系统摄像头，使用自己构建的摄像头视频源，屏幕共享视频源，或者文件视频源等，可以更灵活地处理视频，比如添加美颜效果、滤镜等。 | <li>需要使用自定义的美颜库或者前处理库<li>开发者 App 中已经有自己的图像视频模块<li>开发者希望使用非摄像头的视频源，比如录屏数据<li>有些系统独占的视频采集设备，为了避免与其他业务冲突，需要灵活的设备管理策略。 |
| 推流到 CDN           | 将频道内的音视频内容通过 CDN 推送到其他 RTMP 服务器： <li>能够随时启动或停止推流 <li>能够在不间断推流的同时增减推流地址 <li>能够调整合图布局 | <li>在朋友圈、微博等推广直播内容<li>频道人数超限时，让更多人观看直播 |

	
更多的玩法，点击查看示例代码：

- [PK 连麦](https://github.com/AgoraIO/ARD-Agora-Online-PK/blob/master/README.zh.md)
- [直播答题](https://github.com/AgoraIO/HQ)
- [一起 KTV](https://github.com/AgoraIO/Agora-Online-KTV/blob/master/README.zh.md)
- [在线语音聊天室](https://github.com/AgoraIO-Usecase/Chatroom)
- [娃娃机](https://github.com/AgoraIO/Wawaji)
- [剧本杀](https://github.com/AgoraIO-Usecase/Murder-Mystery-Game)

## 关键特性

| 特性                      | Agora 视频互动直播指标                                           |
| ------------------------- | ------------------------------------------------------------ |
| SDK 包体积                | 4.61 ～ 10.41 MB                                              |
| 多主播互动                | 17 人                                                        |
| 最多观众人数              | 100 万                                                       |
| 跨频道主播连麦            | 支持                                                         |
| 视频属性                  | <li>SDK 采集支持 1080p 分辨率，60 fps 帧率 <li>自采集支持 4K |
| 音频属性                  | <li>音频采样率：16 kHz - 48 kHz <li>支持单、双声道           |
| 音频抗丢包率              | 上下行抗丢包率 70%                                           |

### 平台兼容

互动直播支持 iOS、Android、Windows、macOS、Electron、Unity、Web、小程序，并支持平台间互通，具体的兼容性要求见下表。

| 平台       | 支持版本                                                     |
| ---------- | ------------------------------------------------------------ |
	| Android    | <p>4.1+</p><p>Android SDK 支持如下 ABI：</p><ul><li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64                                                         |
| iOS        | 8.0+                                                         |
	| Windows    | <p>Windows 7+</p><p>Windows SDK 支持如下架构：<p><ul><li>x86<li>x64                                                      |
| macOS      | 10.0+                                                        |
| Unity      | <p>2017+</p><p>Unity SDK 支持如下平台：<p><ul><li>Android (armeabi-v7a、arm64-v8a、x86)<li>iOS<li>Windows (x86、x86_64)<li>macOS                                                         |
| 微信小程序 | 支持                                                         |
| Web        | <li>Chrome 58+ <li>Chrome 49（仅 Windows XP）<li>Firefox 56+ <li>Safari 11+ <li>Opera 45+ <li>QQ 10+ <li>360 安全浏览器 9.1+ |

<div class="alert note">Web 平台的支持情况还与设备型号及系统版本等有关，详见 <a href="https://docs.agora.io/cn/faq/browser_support">Agora Web SDK 支持哪些浏览器？</a></div>
	
## 相关链接

[Agora RTC SDK 最多支持多少人同时在线？](https://docs.agora.io/cn/faq/capacity)
