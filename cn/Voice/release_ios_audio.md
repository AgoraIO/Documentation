
---
title: 发版说明
description: 
platform: iOS
updatedAt: Wed Nov 04 2020 06:12:33 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora 语音 SDK 的发版说明。

## **简介**

iOS 语音 SDK 支持两种主要场景:

-   语音通话
-   语音直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms) 以及 [音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms) 了解关键特性。

## **3.1.2 版**

该版本于 2020 年 9 月 14 日发布。

**升级必看**

为提升用户体验，防止 app 在 iOS 14.0 版本上触发查看本地网络设备的弹窗提示，该版本默认关闭本地网络连通质量报告功能。[`reportRtcStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportRtcStats:) 中的 [`gatewayRtt`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/gatewayRtt) 参数会失效，请不要使用该参数获取客户端到本地路由器的往返延时。详见 [FAQ](https://docs.agora.io/cn/faq/local_network_privacy)。

如果你不介意弹窗提示，且需启用该功能，请[提交工单](https://agora-ticket.agora.io/)联系声网技术支持。

**问题修复**

该版本修复了推流失败的问题。

## **3.1.1 版**

该版本于 2020 年 8 月 27 日发布。

**升级必看**

该版本修改了区域访问限制的区域码 `AgoraAreaCode`，最新区域码如下：

- `AgoraAreaCodeCN`: 中国大陆。
- `AgoraAreaCodeNA`: 北美区域。
- `AgoraAreaCodeEU`: 欧洲区域。
- `AgoraAreaCodeAS`: 除中国大陆以外的亚洲区域。 
- `AgoraAreaCodeJP`: 日本。
- `AgoraAreaCodeIN`: 印度。
- `AgoraAreaCodeGLOB`:（默认）全球。

如你此前调用 [`sharedEngineWithConfig`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithConfig:delegate:) 方法时指定了访问区域，在由之前版本升级至该版本时，请确保使用正确的区域码。

## **3.1.0 版**
该版本于 2020 年 8 月 11 日发布。

**新增特性**

#### 1. 发布和订阅状态转换回调

该版本新增以下回调方便你了解音频流当前的发布及订阅状态，有助于订阅和发布相关的数据统计：

- `didAudioPublishStateChange`: 音频发布状态发生改变。
- `didAudioSubscribeStateChange`: 音频订阅状态发生改变。

#### 2. 本地首帧发布回调

为提示用户本地音频首帧已发布，该版本新增 `firstLocalAudioFramePublished` 回调。该回调取代 `firstLocalAudioFrame` 回调，我们推荐你不再使用 `firstLocalAudioFrame` 回调。

#### 3. 自定义数据上报

该版本支持自定义数据上报。如需试用，请联系 [sales@agora.io](mailto:sales@agora.io) 开通并商定自定义数据格式。

**改进**

#### 1. 指定访问区域完善

该版本新增以下枚举值，在调用 `sharedEngineWithConfig` 创建 `AgoraRtcEngineKit` 实例时提供更多区域选择。指定访问区域后，集成了 Agora SDK 的 app 会连接指定区域内的 Agora 服务器。

- `AgoraIpAreaCode_JAPAN`: 日本。
- `AgoraIpAreaCode_INDIA`: 印度。

#### 2. 加密

该版本新增 `enableEncryption` 方法，用于开启内置加密，并废弃原加密方法：

- `setEncryptionSecret`
- `setEncryptionMode`

与原加密方法相比，该方法新增对国密 SM4 加密模式的支持，你可以根据需要选择合适的加密模式。

#### 3. 通话中质量透明

该版本进一步扩充了 `AgoraRtcLocalAudioStats` 类和 `AgoraRtcRemoteAudioStats` 类的成员，提供更多音频质量相关数据。

- `AgoraRtcLocalAudioStats` 类新增 `txPacketLossRate`，表示本端到 Agora 边缘服务器的物理音频丢包率 (%)。
- `AgoraRtcRemoteAudioStats` 类中新增 `publishDuration`，表示远端音频流的累计发布时长（毫秒）。

#### 4. 设置音频编码属性

为提升音频性能，该版本对音频编码码率最大值进行如下优化：

| Profile                                 | 3.1.0 版本                                                   | 3.1.0 版本之前                                               |
| :-------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `AgoraAudioProfileDefault`                   | <li>直播场景: 64 Kbps</li><li>通信场景: 18 Kbps</li> | <li>直播场景: 52 Kbps</li><li>通信场景: 18 Kbps</li> |
| `AgoraAudioProfileSpeechStandard`           | 18 Kbps                                                      | 18 Kbps                                                      |
| `AgoraAudioProfileMusicStandard`            | 64 Kbps                                                      | 48 Kbps                                                      |
| `AgoraAudioProfileMusicStandardStereo`     | 80 Kbps                                                      | 56 Kbps                                                      |
| `AgoraAudioProfileMusicHighQuality`        | 96 Kbps                                                      | 128 Kbps                                                     |
| `AgoraAudioProfileMusicHighQualityStereo` | 128 Kbps                                                     | 192 Kbps                                                     |

#### 5. 日志扩容

该版本中，Agora SDK 日志文件的默认个数由 2 个增加至 5 个，单个日志文件的默认大小由 512 KB 扩大至 1024 KB。默认情况下，SDK 会生成 `agorasdk.log`、`agorasdk_1.log`、`agorasdk_2.log`、`agorasdk_3.log`、`agorasdk_4.log 这` 5 个日志文件。最新的日志永远写在 `agorasdk.log` 中。`agorasdk.log `写满后，SDK 会从 1-4 中删除修改时间最早的一个文件，然后将 `agorasdk.log` 重命名为该文件，并建立新的 `agorasdk.log` 写入最新的日志。

#### 6. 其他改进

- 该版本对 iOS 部分机型（使用 Apple A10 及以下芯片）进行性能优化，降低了 CPU 使用率和内存占用。
- 该版本降低了远端用户音频首帧的出声时间。

**问题修复**

该版本修复了如下问题：

- 调用 `setAudioMixingPitch` 方法时，部分参数不生效。
- 断开蓝牙设备时，偶现音频卡顿。
- 特定场景下，收不到 `setMediaMetadataDataSource` 回调。
- 多频道场景中，用户切换频道发言后，退出频道时偶现崩溃。

**API 变更**

#### 新增

- [`didAudioPublishStateChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didAudioPublishStateChange:oldState:newState:elapseSinceLastState:)
- [`didAudioSubscribeStateChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didAudioSubscribeStateChange:withUid:oldState:newState:elapseSinceLastState:)
- [`firstLocalAudioFramePublished`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstLocalAudioFramePublished:)
- [`enableEncryption`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableEncryption:encryptionConfig:)
- [`AgoraRtcLocalAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalAudioStats.html) 类中新增 `txPacketLossRate`
- [`AgoraRtcRemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中新增 `publishDuration`
- [`AgoraScreenCaptureParameters`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html) 类中新增 `windowFocus` 和 `excludeWindowList`
- [`rtmpStreamingEventWithUrl`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingEventWithUrl:eventCode:)
- 警告码: `AgoraWarningCodeAdmCategoryNotPlayAndRecord(1029)` 和 `AgoraWarningCodeApmResidualEcho(1053)`
- 错误码: `AgoraErrorCodeNoServerResources(103)`

#### 废弃

- `setEncryptionSecret`
- `setEncryptionMode`
- `firstLocalAudioFrame`

#### 删除

- 警告码: `AgoraWarningCodeAdmImproperSettings(1053)`

## **3.0.1 版**

该版本于 2020 年 5 月 27 日发布。

**升级必看**

#### iOS 静态库升级动态库

为提升开发体验，该版本将 SDK 由静态库升级为动态库，不再支持静态库。使用动态库可以提高库的安全等级，方便 app 上传至 App Store，且避免与第三方库产生不兼容等问题。

如果你由之前版本的静态库升级为当前版本，需要重新集成，详见[升级指南](../../cn/Voice/migration_apple.md)。

<div class="alert note">iOS 13.3.1 版本对动态库的支持有已知问题，已在 iOS 13.4 版本修复。</div>

**新增特性**

#### 1. 调整音乐文件音调

为方便调整混音时音乐文件的播放音调，该版本新增 `setAudioMixingPitch` 方法。通过设置该方法的 `pitch` 参数，你可以升高或降低音乐文件的音调。该方法仅对音乐文件音调有效，对本地人声不生效。

#### 2. 变声与混响

为提高用户的音频体验，该版本在 `setLocalVoiceChanger` 和 `setLocalVoiceReverbPreset` 中分别新增以下枚举值：

- 新增了以 `AgoraAudioVoiceBeauty` 为前缀和以 `AgoraAudioGeneralBeautyVoice` 为前缀的枚举值，分别实现美音或语聊美声功能。
- 新增了以 `AgoraAudioReverbPresetFx` 为前缀的枚举值和 `AgoraAudioReverbPresetVirtualStereo`，分别实现增强版混响效果和虚拟立体声效果。

你可以查看进阶功能[变声与混响](../../cn/Voice/voice_changer_apple.md)了解使用方法和注意事项。

#### 3. 远端音频数据后处理多频道支持 (C++)

在多频道场景下，为方便后处理各频道的远端音视频数据，该版本在 `IAudioFrameObserver` 类中新增 [`isMultipleChannelFrameWanted`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a4b6bdf2a975588cd49c2da2b6eff5956) 和 [`onPlaybackAudioFrameBeforeMixingEx`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ab0cf02ba307e91086df04cda4355905b)。

成功注册音频观测器后，如果你将 `isMultipleChannelFrameWanted` 的返回值设为 `true`，就可以通过上述回调获取多个频道对应的音频数据。在多频道场景下，我们建议你将返回值设为 `true`。

**改进**

- 该版本优化了通话时的音频效果。频道中多个用户同时讲话时，不会减弱任何一方的音频效果。
- 该版本降低了对设备整体 CPU 的占用。

**问题修复**

- 修复了 `didRemoteAudioStateChanged` 回调不准、音频无声、混音、声音卡顿等问题。
- 修复了通话无法正常结束、`didClientRoleChanged` 回调多次等问题。

**API 变更**

该版本新增以下 API：

-  [`setAudioMixingPitch`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPitch:)
- [`AgoraAudioVoiceChanger`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraAudioVoiceChanger.html) 枚举中新增以 `AgoraAudioVoiceBeauty` 为前缀和以 `AgoraAudioGeneralBeautyVoice` 为前缀的枚举值
- [`AgoraAudioReverbPreset`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraAudioReverbPreset.html) 枚举中新增以 `AgoraAudioReverbPresetFx` 为前缀的枚举值，以及 `AgoraAudioReverbPresetVirtualStereo` 枚举值
- [`AgoraRtcRemoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 中新增 `totalActiveTime`

## **3.0.0.2 版**

该版本于 2020 年 4 月 22 日发布。

**新增特性**

#### 设置区域访问限制

该版本新增 `sharedEngineWithConfig` 方法，支持在创建 `AgoraRtcEngineKit` 实例时指定服务器的访问区域。该功能为高级设置，适用于有访问安全限制的场景。目前支持的区域有中国大陆、北美、欧洲、亚洲（中国大陆除外）和全球（默认）。

指定访问区域后，集成了 Agora SDK 的 app 在指定区域使用时，正常情况下会连接指定区域的 Agora 服务器；在非指定区域使用时，会从所在区域和指定区域的服务器地址中，择优选择服务器建立连接。

**问题修复**

该版本修复了无法连接蓝牙耳机的问题。

**API 变更**

#### 新增

[`sharedEngineWithConfig`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithConfig:delegate:)

## **3.0.0 版**
该版本于 2020 年 3 月 4 日发布。并于 2020 年 3 月 24 日修复了偶现的音频无声、混音、`didClientRoleChanged` 回调多次、崩溃等问题。

在该版本对通信场景采用了全新的系统架构，并升级了通信和直播场景下的 last mile 网络策略。在带宽不足时，新的网络策略能充分利用上下行有限带宽提升有效码率，从而增强弱网对抗能力，极大提升了弱网情况下通信和直播场景的终端用户体验。

由于通信场景采用了新的系统架构，为保证新老版本通信用户的互通兼容，我们使用了回退机制。如果频道内有老版本通信用户加入，则当前版本 (3.0.0) 的终端用户会回退成老版本通信。一旦回退，频道内所有用户都无法享受新版本带来的体验提升。因此我们强烈推荐同步升级所有终端用户到当前版本。

同时，我们对本地服务端录制进行了升级发布。为确保享受全新架构和网络策略优化带来的好处，使用本地服务端录制的客户，请务必同步升级本地服务端录制 SDK 至 3.0.0 版本。

新增特性、改进与问题修复详见下文。

**升级必看**
#### 1. 静态库更名与新增动态库

为与其他平台保持一致，该版本将 SDK 的库名由 AgoraRtcEngineKit 变更为 AgoraRtcKit。如果你由老版本的 SDK 升级至该版本，请务必重新导入类。详细步骤见《快速开始》中的[导入类](https://docs.agora.io/cn/Voice/start_call_ios?platform=iOS#a-nameimportclassa2-导入类)章节。

同时，为提升开发体验，该版本新增动态库支持。你可以在静态库和动态库之间任选一个进行集成，其中动态库的包名为 Agora_Native_SDK_for_iOS_v3_0_0_VOICE_Dynamic。

使用动态库可以提高库的安全等级，方便 app 上传至 App Store，且避免与第三方库产生不兼容等问题。如果选择动态库，则需要重新进行集成并导入类。该步骤大约需要 5 分钟。详见《快速开始》中的[集成 SDK](https://docs.agora.io/cn/Voice/start_call_ios?platform=iOS#集成-sdk) 和[导入类](https://docs.agora.io/cn/Voice/start_call_ios?platform=iOS#a-nameimportclassa2-导入类)章节。

<div class="alert info">下表展示分别使用动态库和静态库生成 ipa 文件过程中各文件体积的差异：

<table>
    <tr>
        <td width="12%"><b>库类型</b></td>
        <td width="12%"><b>ipa 体积 (M)</b></td>
        <td width="15%"><b>解压后体积 (M)</b></td>
        <td width="19%"><b>Frameworks 文件夹体积 (M)</b></td>
        <td width="17%"><b>二进制文件体积 (M)</b></td>
        <td width="25%"><b>Frameworks 文件夹 + 二进制文件总体积 (M)</b></td>
    </tr>
    <tr>
        <td>动态库</td>
        <td>27</td>
        <td>56.6</td>
        <td>44</td>
        <td>2.4</td>
        <td>46.4</td>
    </tr>
    <tr>
        <td>静态库</td>
        <td>26.5</td>
        <td>55.3</td>
        <td>30.1</td>
        <td>15.1</td>
        <td>45.2</td>
    </tr>
</table>
	使用动态库集成时，SDK 不再存放于二进制文件中，而是作为一个独立的库存放在 Frameworks 文件夹中。与使用静态库集成相比，二进制文件体积减少了 12.7 M，Frameworks 文件夹体积增加了 13.9 M。
</div>

**新增特性**

#### 1. 多频道管理

为方便用户在同一时间加入多个频道，该版本新增了 [`AgoraRtcChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcChannel.html) 和 [`AgoraRtcChannelDelegate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) 类。通过创建多个 `AgoraRtcChannel` 对象，用户可以加入各 `AgoraRtcChannel` 对象对应的频道中，实现多频道功能。

加入多个频道后，用户可以同时接收多个频道的流，但只能同时在一个频道内发流。该功能适用于用户需要同时接收多个频道的流，或频繁切换频道发流的场景。详细的集成步骤和注意事项，请参考[加入多频道](../../cn/Voice/multiple_channel_apple.md)。

#### 2. 调节本地播放的指定远端用户音量

该版本新增 [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustUserPlaybackSignalVolume:volume:) 方法，用以调节本地用户听到的指定远端用户的音量。通话或直播过程中，你可以多次调用该方法，来调节多个远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。

#### 3. 媒体播放器组件

为丰富直播玩法，Agora 发布了媒体播放器组件，支持主播在直播过程中，播放本地或在线媒体资源，并同步分享给频道内所有用户。详情请参考[媒体播放器组件发版说明](https://docs.agora.io/cn/Interactive%20Broadcast/mediaplayer_release_ios?platform=iOS)。

**改进**

#### 1. 音频编码属性

为满足更高音质需求，该版本调整了直播场景下 `AgoraAudioProfileDefault(0)` 对应的音频编码属性，详见下表：

| SDK 版本   | `AgoraAudioProfileDefault(0)`                                  |
| :--------- | :---------------------------------------------------------- |
| 3.0.0      | 48 KHz 采样率，音乐编码，单声道，编码码率最大值为 52 Kbps。 |
| 3.0.0 之前 | 32 KHz 采样率，音乐编码，单声道，编码码率最大值为 52 Kbps。 |

#### 2. 质量透明

为方便开发者获取更多通话统计信息，该版本在 [`AgoraChannelStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) 类中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 成员，方便更好地监控通话的质量和通话过程中的内存变动。

#### 3. 其他提升

该版本自动开启直播场景下 Native SDK 与 Web SDK 的互通，并废弃原有的 `enableWebSdkInteroperability` 方法。

**问题修复**

- 修复了混音、音频录制、音频编码、回声等音频问题。
- 修复了特定场景下偶现的 app 崩溃、日志文件、推流不稳定等问题。

**API 变更**

#### 行为变更

该版本在调用 [`enableLocalAudio (NO)`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLocalAudio:) 后，不会引起通话音量切换为媒体音量。

#### 新增

- [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) 结构体新增 `channelId` 成员
- [`createRtcChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/createRtcChannel:)
- [`AgoraRtcChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcChannel.html) 类
- [`AgoraRtcChannelDelegate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) 类
- [`AgoraChannelStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) 类中新增 `gatewayRtt`、`memoryAppUsageRatio`、`memoryTotalUsageRatio` 和 `memoryAppUsageInKbytes` 成员

#### 废弃

- `enableWebSdkInteroperability`
- `didAudioMuted`、`firstRemoteAudioFrameDecodedOfUid` 和 `firstRemoteAudioFrameOfUid`，使用 [`remoteAudioStateChangedOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) 取代
- `streamPublishedWithUrl` 和 `streamUnpublishedWithUrl`，使用 [`rtmpStreamingChangedToState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) 取代

## **2.9.1 版**
该版本于 2019 年 9 月 19 日发布。新增特性与修复问题列表详见下文。

**新增特性**

#### 人声检测

为判断本地用户是否说话，该版本在启用说话者音量提示 [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) 方法中新增 bool 型的 `report_vad` 参数。启用该参数后，你会在 [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:) 回调报告的 [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) 结构体中获取本地用户的人声状态。

**改进**

#### 设置客户端录音采样率

为方便用户设置客户端录音的采样率，该版本废弃了原有的 `startAudioRecording` 方法，并使用新的同名方法进行取代。新的方法下，录音采样率可设为 16、32、44.1 或 48 kHz。原方法仅支持固定的 32 kHz 采样率，该版本继续保留原方法但我们不推荐使用。

**问题修复**

#### 音频

- 偶现音频卡顿。
- 通话被第三方应用打断后，用户再回到频道时，音频异常。
- 进入频道后偶现回声。

#### 其他

- 修复了用户调用 [`joinChannelByUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:) 接口，在未成功加入频道前，进行切换网络操作，导致此后无法正常正常收到 [`didUpdatedUserInfo`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:) 回调。
- 旁路推流串流。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增

- [`startAudioRecording`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:)
- [`enableAudioVolumeIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:)，新增 `report_vad` 参数
- [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) 类，新增 `vad` 成员

#### 废弃

- `startAudioRecording`

## **2.9.0 版**
该版本于 2019 年 8 月 16 日发布。新增特性与修复问题列表详见下文。

**升级必看**

#### 1. RTMP 推流

该版本起，Agora 删除如下接口：

- `configPublisher`

如果你的 App 使用上述接口实现 RTMP 推流功能，请确保将 Native SDK 升级至最新版本，并改用如下接口实现推流：

- [`setLiveTranscoding`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)
- [`addPublishStreamUrl`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
- [`removePublishStreamUrl`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
- [`rtmpStreamingChangedToState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)

新的推流实现方法，详见[推流到 RTMP](../../cn/Voice/cdn_streaming_apple.md)。

#### 2. 关闭/开启本地音频采集

为提高通信场景下，本地用户关闭麦克风后听到的音质，该版本在 `enableLocalAudio`(true) 后，将系统音量修改为媒体音量。调用 `enableLocalAudio`(false) 后，系统音量自动切换为通话音量。

**新增特性**

#### 1. 快速切换频道

为方便直播频道中的观众用户快速切换到其他频道，该版本新增 [`switchChannelByToken`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 方法。和先调 [`leaveChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:)，再调 [`joinChannelByToken`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) 相比，该方法能实现更快的频道切换。调用 [`switchChannelByToken`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 方法切换到其他直播频道后，本地会先收到离开原频道的回调 [`didLeaveChannelWithStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLeaveChannelWithStats:)，再收到成功加入新频道的回调 [`didJoinChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinChannel:withUid:elapsed:)。

#### 2. 跨频道媒体流转发

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)

在跨频道媒体流转发过程中，SDK 会通过 [`channelMediaRelayStateDidChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:) 和 [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:) 回调报告媒体流转发的状态和事件。

该场景的实现方法、API 调用时序、示例代码及开发注意事项，请参考 [跨直播间连麦](../../cn/Voice/media_relay_apple.md) 指南。

#### 3. 本地及远端音频状态

为方便用户了解本地及远端的音频状态，该版本新增 [`localAudioStateChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) 和 [`remoteAudioStateChangedOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) 回调。新的回调下，本地及远端音频有如下状态：

- 本地音频：Stopped(0)、Recording(1)、Encoding(2) 和 Failed(3)。状态为 Failed(3) 时，你可以通过 `error` 参数中返回的错误码定位及排查问题。
- 远端音频：Stopped(0)、Starting(1)、Decoding(2)、Frozen(3) 和 Failed(4)。你可以在 `reason` 参数中了解引起远端音频状态发生改变的原因。

#### 4. 本地音频数据

为方便更好地了解通话质量，获取更多质量相关数据，该版本新增 [`localAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:) 回调，通过 `numChannels`、`sentSampleRate`、`sentBitrate` 参数报告本地音频统计信息。

#### 5. 远端音频帧拉取

为提升音频播放体验，该版本新增如下接口，支持使用拉取的方式获取远端音频数据。App 可以对拉取到的原始音频数据进行处理后再渲染，获取想要的音频效果。
- [`enableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

该方法和 `onPlaybackAudioFrame` 回调相比，区别在于：

- `onPlaybackAudioFrame`：SDK 每 10 毫秒通过回调将音频数据传输给 App。如果 App 处理延时，可能会导致音频播放抖动。
- `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte`：App 主动拉取音频数据。通过设置音频数据，SDK 可以调整缓存，帮助 App 处理延时，从而有效避免音频播放抖动。

**改进**

#### 1. 通话中质量透明

该版本进一步扩充了 [`AgoraChannelStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) 类的成员：
- `AgoraChannelStats` 类：累计发送音频字节数及累计接收音频字节数


#### 2. 其他改进

- 优化了 GameStreaming 场景下的音频质量。
- 优化了通信场景下用户关闭麦克风后听到的音质。

**问题修复**


#### 音频

- 修复了与 Web 互通时听声辨位过程中出现的声音失真的问题。
- 修复了使用语音 SDK 过程中出现的崩溃问题。
- 修复了特殊场景下直播观众设置 Audio Session 为 Playback 后出现的音频卡顿问题。
- 修复了听声辨位场景下，Web 端加入频道后，听不到声音的问题。

#### 其他

- 修复了偶现的旁路推流串流的问题。
- 修复了调用 [`leaveChannel`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) 后崩溃的问题。

**API 变更**

为提升用户体验，Agora SDK 在该版本中对 API 进行了如下变动：

#### 新增
- [`enableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)
- [`localAudioStateChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:)
- [`remoteAudioStateChangedOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:)
- [`localAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:)
- [`switchChannelByToken`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 
- [`startChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)
- [`AgoraChannelStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) 类新增 `txAudioBytes`和 `rxAudioBytes` 成员

#### 废弃

- [`didMicrophoneEnabled`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didMicrophoneEnabled:)，请改用 [`localAudioStateChange`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) 回调的 AgoraAudioLocalStateStopped(0) 或 AgoraAudioLocalStateRecording(1)。
- [`audioTransportStatsOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)，请改用 [`remoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) 回调。

#### 删除

- `configPublisher`

## **2.8.0 版**

该版本于 2019 年 7 月 8 日发布。新增特性详见下文。

**新增特性**

#### 1. 全平台支持 String 型的用户名

很多 App 使用 String 类型的用户名。为降低开发成本，Agora 新增支持 String 型的 User account，方便用户通过如下接口直接使用 App 账号加入 Agora 频道：

- [registerLocalUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

对于其他接口，Agora 沿用 Int 型的 UID。Agora Engine 会维护 UID 和 User account 映射表，你可以随时通过 String user account 获取 UID，或者通过 UID 获取 String user account，无需自己维护映射表。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

**Note**：

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。目前支持 String 型 User account 的 SDK 如下：

	- Native SDK：v2.8.0 及之后版本
	- Web SDK：v2.5.0 及之后版本

 如果你的频道内有不支持 String 型 User account 的用户，则只能使用 Int 型的 User ID。
- 如果你使用该版本的 Native SDK 将用户名升级至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。如果使用 String 型 User account 加入频道，请确保使用该 User account 或其对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。

#### 2. 音频卡顿回调

为监控通话过程中的音频传输质量，方便开发者客观体验通信质量，该版本在远端音频统计数据 [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中新增 `totalFrozenTime` 和 `frozenRate` 成员，报告远端用户在加入频道后发生音视频的卡顿时长及卡顿率。

同时，该版本在 [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中还新增 `numChannels`、`receivedSampleRate` 和 `receivedBitrate` 成员。

**改进**

为方便开发者统计掉线率，该版本在 [connectionChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调的 `AgoraConnectionChangedReason` 参数中添加 `AgoraConnectionChangedKeepAliveTimeout(14)` 成员，表示 SDK 与服务器连接保活超时，引起 SDK 连接状态发生改变。


**API 变更**

为提升用户体验，Agora 在 v2.8.0 版本中对 API 进行了如下变动：

#### 新增

- [registerLocalUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- [AgoraRtcRemoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) 类中新增 `numChannels`，`receivedSampleRate`，`receivedBitrate`，`totalFrozenTime` 和 `frozenRate` 成员

#### 废弃

- [AgoraLiveTranscoding](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) 类中的 `lowLatency` 成员

## **2.4.1 版**

该版本于 2019 年 6 月 12 日发布。新增特性、功能改进与修复问题列表详见下文。

**升级必看**

如下内容涉及 SDK 的行为变更。如果你是由之前版本的 SDK 升级至该版本，升级前请务必阅读。

#### RTMP 推流

为提高推流服务的易用性，该版本对推流接口的参数设置进行了如下限制：

| 类**/**接口                 | 参数限制                                                     |
| --------------------------- | ------------------------------------------------------------ |
| [AgoraLiveTranscoding](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) 类 | <li>[videoFrameRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoFramerate)：设置转码推流的帧率，单位为 fps，取值范围为 [0, 30]，默认值为 15。如果设值超过 30，Agora 服务端会自动调整为 30<li>[videoBitrate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoBitrate)：设置转码推流的码率，单位为 Kbps，默认值为 400。用户可以根据 [Video Profile 参考表](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate)中的码率值进行设置。如果设置的码率超出合理范围，服务端会在合理区间内对码率值进行自适应<li>[videoCodecProfile](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoCodecProfile)：设置转码推流的视频编码规格，可设为 **BASELINE**、**MAIN** 或 **HIGH**。若设为其他值，服务端会改为默认值 **HIGH **<li>[size](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/size)：设置转码推流的视频分辨率。size 的最小值不低于 16 x 16</li> |
| [AgoraImage](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraImage.html) 类           | `url`：字符长度不得超过 **1024** 字节                        |
| [addPublishStreamUrl](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)     | `url`：字符长度不得超过 **1024** 字节                        |
| [removePublishStreamUrl](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)  | `url`：字符长度不得超过 **1024** 字节                        |

同时，该版本在 `LiveTranscoding` 类中新增 [audioCodecProfile](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) 参数，支持设置音频编码的规格。默认规格为 LC-AAC。

此外，该版本还对 [streamPublishedWithUrl](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) 方法的 `errorCode` 参数新增了五个错误码，方便快速定位与排查问题。

**新增特性**

#### 1、推流状态回调

为方便推流用户了解当前的推流状态，并在推流出错时了解原因排查问题，该版本新增 [rtmpStreamingChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) 回调。该回调下，推流有 `Idle`、`Connecting`、`Running`、`Recovering` 和 `Failure` 五种状态。当推流状态为 FAILURE 时，用户可以参考该回调 `errCode` 参数返回的错误码进行问题排查。原有的 `streamPublishedWithUrl` 和 `streamUnpublishedWithUrl` 回调仍可以使用，但我们不再推荐。

#### 2、网络连接失败原因梳理

为方便开发者更好地排查网络连接相关故障，该版本梳理并整合了网络连接相关的错误码，在原有 [connectionChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调的 `reason` 参数中新增八个导致网络连接失败的原因。新增后，只要网络连接发生错误，SDK 都会返回该回调。同时该版本废弃了原有的警告码 `AgoraWarningCodeLookupChannelRejected(105)` 和错误码 `AgoraErrorCodeTokenExpired(109)`、`AgoraErrorCodeInvalidToken(110)`。

#### 3、本地网络连接类型回调

为方便用户了解本地网络的连接类型，该版本新增 [networkTypeChangedToType](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:) 回调。该回调下，本地网络连接有 `Unknown`、`Disconnected`、`Lan` `Wifi`、`2G`、`3G`、`4G` 七种类型。当网络连接短暂中断时，该回调能帮助开发者辨别引起中断的原因是网络切换还是网络条件不好。

#### 4、获取播放伴奏音量

为方便开发者获取伴奏的播放音量，排查音量相关问题，该版本新增 [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) 和 [getAudioMixingPublishVolume](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) 方法，用以分别获取音乐文件在本地和远端的播放音量。

#### 5、精确回调远端音频首帧解码

为更精准地获取远端用户的出声时间，该版本新增 [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:) 回调，用以向 App 层报告 SDK 已完成远端音频首帧解码。在远端用户加入频道后首次发送音频，或远端用户 15 秒不发音频后再次发送时，该回调均会被触发。该回调与 `firstRemoteAudioFrameOfUid` 的区别在于，`firstRemoteAudioFrameOfUid` 在收到首个音频包时触发，先于 `firstRemoteAudioDecodedOfUid`。

**改进**

#### 1、在线音效叠加

为提高用户体验，构造丰富有趣的实时音视频场景，该版本新增支持同时播放多个**在线**音效文件。开发者可以通过多次调用 [playEffect](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:publish:) 方法，传入不同的在线音效文件的 URL，实现音效叠加。

#### 2、质量透明

- 该版本在通话相关的统计信息 [AgoraChannelStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html) 类中，新增 [txPacketLossRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) 和  [rxPacketLossRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) 参数，分别返回本地客户端到服务器和服务器到本地客户端的丢包率。
- 该版本对 AgoraLocalVideoStats 和 AgoraRemoteVideoStats 类作了如下变动，方便用户更精准地获取本地和远端视频流的统计信息：
  - [AgoraRtcLocalVideoStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html)：新增 [encoderOutputFrameRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) 参数
  - [AgoraRtcRemoteVideoStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html)：新增 [decoderOutputFrameRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) 参数，并将原有的 receivedFrameRate 参数更名为 [rendererOutputFrameRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate)

#### 3、其他改进

- 优化了[AgoraAudioScenario](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraAudioScenario.html) 为 `GameStreaming` 时的音质效果
- 优化了部分场景下的语音延时
- SDK 包大小降低约 0.5 M
- 默认启用音频质量通知回调。开发者无需调用 enableAudioQualityIndication 方法，也可以收到 [remoteAudioStats](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) 回调
- 提升了推流服务的稳定性

**问题修复**

#### 音频

- 修复了音频被 Siri 打断且无法恢复的问题

#### 其他

- 修复了用户退出频道后仍然收到 `networkQuality` 回调的问题
- 修复了偶现的崩溃问题，提升了系统稳定性

**API 变更**

为提升用户体验，Agora 在 v2.4.1 版本中对 API 进行了如下变动：

#### 全平台 C++ 接口行为一致

从该版本起，Native SDK 保证了各平台 C++ 接口的行为一致性，方便用户通过统一的 C++ 接口，在各平台保持业务逻辑一致。同时在 C++ 头文件的 `IRtcEngine` 类中实现了 `RtcEngineParameters` 类下的所有方法。各接口的适用范围及使用注意事项详见 [Agora C++ API Reference for All Platforms 首页](https://docs.agora.io/cn/Voice/API%20Reference/cpp/index.html)。

#### 新增

- [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPlayoutVolume)
- [getAudioMixingPublishVolume](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPublishVolume)
- [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:)
- [networkTypeChangedToType](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:)
- [rtmpStreamingChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)
- `AgoraLiveTranscoding` 类新增参数 [audioCodecProfile](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile)
- `AgoraChannelStats` 类新增参数 [txPacketLossRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) 和 [rxPacketLossRate](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate)

#### 废弃

- `enableAudioQualityIndication`
- 警告码 `AgoraWarningCodeLookupChannelRejected(105)`，请改用 [connectionChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调中的 AgoraConnectionChangedRejectedByServer(10)
- 错误码 `AgoraErrorCodeTokenExpired(109)`，请改用 [connectionChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调中的 AgoraConnectionChangedTokenExpired(9)
- 错误码 `AgoraErrorCodeInvalidToken(110)`，请改用 [connectionChangedToState](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调中的 AgoraConnectionChangedInvalidToken(8)

## 2.4.0 版及之前

**2.4.0 版**

该版本于 2019 年 4 月 1 日发布。新增特性、功能改进与修复问题列表详见下文。

#### 升级必看

- Agora Voice SDK for iOS 在 2.4.0 版本新增 `CoreML.framework` 库依赖。请确保在集成时添加该库，详见[集成 SDK](https://docs.agora.io/cn/Voice/start_call_ios?platform=iOS#%E9%9B%86%E6%88%90-sdk)。
- 如果你希望通过 CocoaPods 自动导入库，请确保在运行 `pod install` 前，先运行 `pop update` 更新本地 CocoaPods 库。如果你希望通过指定 SDK 版本号获取最新版，请在 Podfile 中将版本号指定为 `'AgoraRtcEngnine_iOS', '2.4.0.1'`。

#### 新增特性

##### 1. 变声和混响

在语音聊天室场景中添加变声和混响效果，能有效增强社交的趣味性。该版本在原有音效设置接口的基础上，新增 [`setLocalVoiceChanger`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:) 和 [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:) 方法，开发者无需手动设置音效参数，直接选择想要的本地语音变声或混响效果。详情请参考[变声与混响](../../cn/Voice/voice_changer_apple.md)。

##### 2. 听声辨位

该版本新增 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) 和 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 方法，支持本地用户听声辨位。用户需要在加入频道前调用 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) 开启远端用户的语音立体声，然后在 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 中设置远端用户声音出现的位置，通过左右耳听到的声音差异，对远端用户的声音产生方位感。在多人在线游戏场景，如射击游戏中，该功能可以增加游戏角色的方位感，模拟真实场景。

##### 3. 通话前 Last-mile 网络探测

在通话前进行 Last-mile 网络探测，可以有效帮助本地用户判断和预测上行网络质量是否良好。该版本新增通话前 Last-mile 网络探测接口 [`startLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)、[`stopLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest) 及 [`lastmileProbeResult`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)，向用户反馈开始通话前上下行网络的带宽、丢包、网络抖动和往返时延数据。

##### 4. 音乐文件播放状态

该版本为播放音乐文件新增回调 [`localAudioMixingStateDidChanged`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)，方便用户获知音乐文件的播放状态（成功/失败），以及播放出错的原因。同时新增一个警告码 701，当播放音乐文件时，本地音乐文件不存在、文件格式不支持或无法访问在线音乐文件 URL 时，均会触发该警告码。

##### 5. 设置日志文件大小

Agora SDK 有 2 个日志文件，每个文件默认大小为 512 KB。为解决该大小无法满足部分用户需求的问题，该版本新增接口 [`setLogFileSize`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)，用于设置 SDK 输出的日志文件大小。

##### 6. 云代理服务

支持使用云代理服务，方便部署企业防火墙的用户正常使用 Agora 的服务，详见[使用云代理服务](../../cn/Voice/cloudproxy_native.md)。

#### 功能改进

##### 1. 质量测试与透明

- 该版本使用新的 [`startEchoTestWithInterval`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:) 接口取代原有的 [`startEchoTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTest:)，新增参数 `intervalInSeconds`，用于设置返回测试结果的时间间隔。
- 该版本在本地视频流统计信息 [`AgoraRtcLocalVideoStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html) 类中新增 [`sentTargetBitrate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetBitrate)，[`sentTargetFrameRate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetFrameRate)，[`qualityAdaptIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/qualityAdaptIndication) 三个参数，分别反映目标码率、目标帧率与和上次返回的本地视频流统计信息相比，本地视频质量的自适应情况。

##### 2. 核心质量改进

- 降低了音频延时
- 提升了视频质量和稳定性
- 缩短了远端视频的出图时间

#### 问题修复

##### 音频相关

- 修复了调用 `enableLocalAudio` 接口导致的蓝牙断开的问题
- 新增支持中文字符音乐
- 修正 `pushExternalAudioFrameSampleBuffer` 成功的返回值为 YES
- 修复了高音声音变弱的问题
- 修复了偶现的声音快放问题

##### 其他

- 统一了 Android 和 iOS 平台上 SDK 判断远端用户掉线的时间
- 修复了转码推流场景下，SEI 信息与媒体流不同步的问题

#### API 整理

为提升用户体验，Agora 在 v2.4.0 版本中对 API 进行了如下变动：

##### 新增

- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`enableSoundPositionIndication`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:)
- [`setRemoteVoicePosition`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 
- [`startLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`startEchoTestWithInterval`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`setLogFileSize`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)
- [`localAudioMixingStateDidChanged`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)
- [`lastmileProbeResult`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

##### 废弃

- `startEchoTest`


**2.3.3 版**

该版本于 2019 年 1 月 24 日发布。修复问题详见下文。

#### 问题修复

修复了 `networkQuality` 回调不准确的问题。

**2.3.2 版**
该版本于 2019 年 1 月 16 日发布。新增特性与修复问题详见下文。

#### 升级必看

2.3.2 除了下文提到的功能和改进外，整体提升直播场景下视频弱网下抗丢包能力，提高流畅度，降低卡顿率。升级前，请了解版本兼容性:

- Native SDK 版本号须大于等于 1.11 版本
- Web SDK 版本号须大于等于 2.1 版本

#### 新增功能

##### 1. 控制音乐文件的播放音量

为方便用户控制混音音乐文件在本地及远端的播放音量，该版本在已有 [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) 的基础上新增 [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) 和 [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) 接口，用于分别控制混音音乐文件在本地和远端的播放音量。

添加新的方法后，原有的 [adjustPlaybackSignalVolume](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) 由控制人声和音乐的音量改为仅控制人声的音量。因此，如果要静音本地播放，需同时设置 `adjustPlaybackSignalVolume(0)` 和 `adjustAudioMixingPlayoutVolume(0)`。

该版本梳理了用户在音频采集到播放过程中可能会需要调整音量的场景，及各场景对应的 API，供用户参考使用。详见官网文档[调整通话音量](../../cn/Voice/volume_ios.md)。

#### 改进

##### 1. 提供更透明的质量数据统计

为提升质量透明的用户体验，该版本废弃了原有的 `audioQualityOfUid` 回调，并新增 `remoteAudioStats` 回调进行取代。和原来的接口相比，新接口使用更为综合的算法，通过引入音频丢帧率、端到端的音频延迟、接收端网络抖动的缓冲延迟等参数，使回调结果更贴近用户感受。同时，该版本优化了 `networkQuality` 的算法，对上下行网络质量采用不同的计算方法，使评分更精准。

- [`remoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)：通话中远端音频流的统计信息回调。用于替换	`audioQualityOfUid`
- [`networkQuality`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:)：通话中每个用户的网络上下行 Last mile 质量报告回调。

Agora SDK 计划在下一个版本对如下 API 进行进一步改进：

- [`lastmileQuality`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)：通话前网络上下行 Last mile 质量报告回调

该版本对数据统计相关回调进行了统一梳理，相关回调及算法详见[通话前检测网络质量](../../cn/Voice/lastmile_quality_ios.md)。

##### 2. 改进获取 SDK 网络连接状态的生成策略

为提升 SDK 使用数据统计的准确性和合理性，该版本新增如下接口，用以获取 SDK 的网络连接状态，以及连接状态发生改变的原因。

- [`getConnectionState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)：获取 SDK 的网络连接状态
- [`connectionChangedToState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)：SDK 的网络连接状态已改变回调

该版本废弃了原有的 [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) 和 [`rtcEngineConnectionDidBanned`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) 回调。

在新的接口下，SDK 共有 5 种连接状态：未连接、正在连接、已连接、正在重新建立连接和连接失败。当连接状态发生改变时，都会触发 `connectionChangedToState` 回调。当条件满足时，原有的 `rtcEngineConnectionDidInterrupted` 和 `rtcEngineConnectionDidBanned` 回调也会触发，但 Agora 不再推荐使用。

##### 3. 优化打分反馈机制

为方便用户（开发者）收集最终用户（应用程序使用者）对使用应用进行通话或直播的反馈，该版本将 [`rate`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) 接口中的打分范围缩小为 1 - 5，减少最终用户的打分干扰。Agora 建议在应用程序中集成该接口，方便应用程序收集用户反馈。

##### 4. 音乐场景的音质优化

该版本针对高音质需求场景，如音乐教学等进行了音质改进。通过调用 [`setAudioProfile`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:)，将 [`AgoraAudioProfile`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraAudioProfile.html) 设置为 `MusicHighQuality(4)`，[`AgoraAudioScenario`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Constants/AgoraAudioScenario.html) 设置为 `GameStreaming(3)` 实现，在有效消除回声、降低噪音的同时，不损害音乐的音质。

##### 5. 其他改进

- 提升了推流稳定性
- 优化了 API 的调用线程
- 优化了 SDK 在 iOS 中低端设备上的性能
- 增加了重新检测耳机插入和蓝牙设备连接的代码
- 降低了音频延时


#### 问题修复

##### 音频相关：

- 修复了设备在连接蓝牙的状态下，退出频道后，音频不走蓝牙的问题
- 修复了调用 `startAudioMixing` 播放音乐文件时出现的崩溃问题
- 修复了麦克风在禁用的状态下，设备插上耳机后，禁用失效的问题
- 修复了外放条件下，上下麦、系统电话打断、Siri 中断、进退频道等多种场景下，出现的无法调节外放音量的问题
- 修复了应用从后台切回前台时，出现的出声音慢的问题


#### API 整理

为提升用户体验，Agora 在 v2.3.2 版本中对 API 进行了如下变动：

##### 新增

- [`getConnectionState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`remoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)

##### 废弃

- [`audioQualityOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

**2.3.1 版**

该版本于 2018 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

#####  关闭/重新开启本地语音功能

应用程序在加入频道时，语音功能是默认打开的。为满足用户只接收而不发送音频流的需求，该版本新增 `enableLocalAudio` 接口，方便应用程序在进入频道后关闭或重新开启本地语音功能。关闭本地语音功能后，应用程序会收到 `didMicrophoneEnabled` 回调，并停止采集本地音频流。该方法不影响接收和播放远端音频流。

该功能与 `muteLocalAudioStream` 的区别在于前者不采集不发送，而后者是采集但不发送。

#### 改进

- 优化了 iOS 低端设备在纯音频通信场景下的 CPU 消耗

#### 问题修复

- 修复了某些 iOS 设备上偶现的崩溃问题
- 修复了直播场景下，观众端因统计有误出现的延迟的问题

**2.3.0 版**

该版本于 2018 年 8 月 31 日发布。新增特性与修复问题列表详见下文。

Agora SDK 在 v2.3.0 版本中，全面提升了视频功能的稳定性及可用性，保证了更加可靠的实时通信。同时音视频质量也得到进一步提高。 视频方面，通过优化编码性能，增强了弱网对抗能力，减少卡顿时间，提升视频流畅度；音频方面，采用深度学习算法，改进了通话中的音频质量。

#### 升级必看

-   该版本新增了一个系统库依赖：`Accelerate.framework`。该系统库可以进行大规模的数学计算和图像计算，并针对高性能进行了优化。
-   为更好地提升用户体验，Agora SDK 在 v2.1.0 版本中对动态秘钥进行了升级。如果你当前使用的 SDK 是 v2.1.0 之前的版本，并希望升级到 v2.1.0 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。


#### 新增功能

本次发版新增如下功能：

##### 1. 提前 30 秒提醒 Token 即将过期

由于 Token 具有一定的时效，在通话过程中如果 Token 即将失效，SDK 会提前 30 秒触发回调 `tokenPrivilegeWillExpire`，提醒应用程序更新 Token。当收到该回调时，用户需要重新在服务端生成新的 Token，然后调用 `renewToken` 将新生成的 Token 传给 SDK。

##### 2. 按用户返回音频上下行码率、帧率、丢包率及延迟

为方便统计每个用户的音视频上下行码率、帧率及丢包率，该版本新增 `audioTransportStatsOfUid` 回调。 通话或直播过程中，当用户收到远端用户发送的音视频数据包后，会周期性地发生该回调上报，频率约为 2 秒 1 次。 回调中包含用户的 UID、音/视频接收码率、丢包率、以及延迟时间（毫秒）。 并在统计频道内通话相关数据的 `Rtcstats` 类中增加 `lastmileDelay` 参数，返回客户端到 vos 服务器的延迟。

##### 3. 设置 SDK 对 Audio Session 的管理限制

在默认情况下，SDK 和 App 对 Audio Session 都有控制权，但某些场景下，App 会希望限制 Agora SDK 对 Audio Session 的控制权限， 而使用其他应用或第三方组件对 Audio Session 进行操控。为满足该需求，本版本新增 `setAudioSessionOperationRestriction` 接口。 用户可以选择相应的 Restriction，来实现 SDK 不同程度的管理限制。该方法可动态使用，在加入频道前，或频道中均能调用。


#### 改进功能

-   优化了一对一音视频的质量，在降低延时、防止卡顿方面提升明显。优化效果重点覆盖东南亚、南美、非洲和中东等地区
-   直播场景下，改善了音频编码器的效率，保证通话质量的同时节省用户流量
-   采用深度学习算法，改进了通话及直播中的音频质量


#### 问题修复

-   修复了某些 iOS 设备上 App 偶发的闪退的问题
-   修复了特定场景下某些 iOS 设备上推流后出现的崩溃问题
-   修复了某些 iOS 设备上偶现的崩溃的问题
-   修复了某些 iOS 设备上频繁暂停、恢复播放所有音效时出现的崩溃的问题
-   修复了多人连麦场景下，主播频繁进出频道时，偶现的主播内存异常增长的问题
-   修复了特定场景下偶现的主播加入频道、下麦再上麦后，远端用户听不到主播声音的问题
-   修复了特定场景下某些 iOS 设备上偶现的接听系统电话再回到频道后，听不到声音的问题
-   修复了特定场景下偶现的直播观众无法调节频道内通话音量的问题
-   修复了频繁进出频道时，某些 iOS 设备上出现的崩溃的问题
-   修复了通信场景下偶现的其他端看不到 iOS 端视频画面的问题
-   修复了 2 人连麦过程中，一端播放背景音乐时将自己静音或关闭音频后，另一端闪退的问题
-   修复了特定场景下，iOS 端和 Web 端互通时，Web 频繁进出频道后，iOS 设备上出现的崩溃的问题
-   修复了特定场景下，预加载音效时某些设备上偶现的崩溃问题
-   修复了偶现的 iOS 与 macOS 设备 无法进入频道互通的问题
-   修复了直播场景下，使用第三方应用播放音乐时，某些 iOS 设备上出现的退出频道时崩溃的问题
-   修复了特定场景下，某些 iOS 设备上出现的退出频道时崩溃的问题
-   修复了直播场景下，某些 iOS 设备上出现的拉流过程中，其他用户拉流也能成功的问题
-   修复了部分设备上偶现的使用声卡时回声的问题
-   修复了特定场景下，某些 iOS 设备上无法调节音量的问题


#### API 整理

为提升用户体验，Agora 在 v2.3.0 版本中对 API 进行了梳理，并针对部分接口进行了如下处理：

为避免在直播转码推流中添加多个相同 User，v2.3.0 版本中新增如下接口：

-   `addUser`
-   `removeUser`


以下接口与录制相关，在 v2.3.0 版本后不再支持。Agora 提供专门的 Recording SDK 用于更好的录制服务，详见 [Agora Recording SDK 发版说明](../../cn/Product%20Overview/release_recording.md)。

-   `startRecordingService`
-   `stopRecordingService`
-   `refreshRecordingServiceStatus`
-   `didRefreshRecordingServiceStatus`

以下接口长期处于弃用状态，现进行删除，v2.3.0 版本后不再支持：

-   `setSpeakerphoneVolume`


**2.2.3 版**

该版本于 2018 年 7 月 5 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

为更好地提升用户体验，Agora SDK 在 v2.1.0 版本中对动态秘钥进行了升级。如果你当前使用的 SDK 是 v2.1.0 之前的版本，并希望升级到 v2.1.0 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

#### 问题修复

-   修复了特定场景下偶发的线上统计崩溃的问题。
-   修复了直播时特定场景下偶发的崩溃的问题。
-   修复了多人直播连麦时，SDK 内存增长的问题。
-   复了偶发的无法正常反馈频道内谁在说话以及说话者音量的问题。
-   修复了直播时偶发的观众听到主播声忽大忽小的问题。


**2.2.2 版**

该版本于 2018 年 6 月 21 日发布。修复问题列表详见下文。

#### 问题修复

- 修复了特定场景下偶发的线上统计崩溃的问题
- 修复了 iOS 设备无法同时使用媒体和信令服务的问题
- 修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题

**2.2.1 版**

该版本于 2018 年 5 月 30 日发布。新增特性与修复问题列表详见下文。

#### 问题修复

-   修复了部分设备上偶现的 Crash 问题。
-   修复了部分设备上内存泄漏的问题。
-   修复了部分设备上播放网络伴奏时某些 App 闪退的问题。


**2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

本次发版新增如下功能：

##### 1. 音效混响进频道

播放音效 `playEffect` 接口新增了一个 `publish` 参数，用于在播放音效时，远端用户可以听到本地播放的音效。

> 如果你的 SDK 是由之前版本升级到 v2.2 版本，请务必关注该接口功能的变动。

##### 2. 服务端部署代理服务器

通过部署 Agora 提供的代理服务器安装包，设有企业防火墙的用户可以设置代理服务器，使用 Agora 的服务。详见 [企业部署代理服务器](../../cn/Voice/cloudproxy_native.md) 中的描述。


#### 改进功能

本次发版改进如下功能：

##### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication`接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

##### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

##### 3. 进入频道前网络条件监测

为方便用户在进频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

##### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。用户可以通过设置 `setAudioProfile` 中的 Scenario：AgoraAudioScenarioGameStreaming = 3 来实现高保真的音乐传输。

##### 5. 支持 Bitcode

新增支持 Bitcode 功能。支持 Bitcode 的 SDK 包大小约为普通包的 2.5 倍；使用 Bitcode 开发的 App 在上传 App Store 后，App Store 会对其进行优化及瘦身，瘦身程度视 App 的代码量而定，代码量越大，瘦身程度越高。

#### 问题修复

-   修复了某些 iOS 设备导致频道内其他端的回音问题。


**2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

#### 升级必看

该版本的 SDK 修改了 `setVideoProfile` 方法在直播场景下的码率值，修改后的码率值与 2.0 版本一致。

#### 问题修复

-   修复了 SDK 没有设置 Delegate 时，偶尔收不到 Block 回调的问题。

-   修复了 SDK 的外链符号里，有 NSAssertionHandler 的问题。

-   修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。

-   修复了直播场景下，调用 `enableWebSdkInteroperability `接口后，iOS 端偶尔看不到 Win 10 系统 Web 端视频画面的问题。

-   修复了通信或直播过程中偶现 Crash 的问题。


**2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。


#### 问题修复

-   修复了之前版本 SDK 在 iOS 11 平台上崩溃的问题。

-   修复了之前版本 SDK 在 dtx+aac 模式下会视频卡顿的问题。


**2.1.1 版**

该版本于 2018 年 3 月 16 日发布。

请正在或已集成 2.1 SDK 的客户尽快升级更新！ 本次发版修复了一个的系统风险，请尽快升级以免对服务造成影响。

**2.1.0 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

#### 新增功能

本次发版新增如下功能：

##### 1. 开黑

新增了一个游戏开黑场景，用于节省流量和去除杂音，通过调用 API setAudioProfile 实现。

##### 2. 音效均衡和音效混响

在直播场景下，主播如果需要通过内置的麦克风美化和定制自己的语音输入，可以通过调用 API `setLocalVoiceEqualization` 和 `setLocalVoiceReverb` 轻易地设置音效均衡和混响来实现所需要的效果。

##### 3. 在线频道信息查询

新增 Restful API 查询用户在频道中的状态信息，查询指定频道内的分角色用户列表，查询厂商频道列表，查询用户是否为连麦用户等。详见:

-   通话场景, 详见 [控制台 RESTful API](../../cn/API%20Reference/dashboard_restful_communication.md)

-   互动直播场景, 详见 [控制台 RESTful API](../../cn/API%20Reference/dashboard_restful_live.md)


#### 改进

本次发版改进如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>改进</td>
<td>描述</td>
</tr>
<tr><td>卡顿</td>
<td>改善了观众模式下的卡顿现象</td>
</tr>
<tr><td>卡顿</td>
<td>改善了特定设备导致的卡顿现象</td>
</tr>
<tr><td>鉴权模型优化</td>
<td>旧鉴权模型一个密钥只能对应一个服务权限(例如加入频道)，而新的一个 Token 包含了所有的服务权限(例如加入频道，连麦，推流等)</td>
</tr>
</tbody>
</table>



#### 问题修复

-   修复了自采集方案退出频道后 app 录不到声音的问题;

-   修复了偶现的崩溃;

-   修复了偶现的关闭麦克风无法听到声音的问题;


**2.0.2 版**

该版本于 2017 年 12 月 15 日发布。新增特性与修复问题列表详见下文。

#### 问题修复

修复了 ffmpeg 符号冲突问题;


**2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**


-   伴奏和音效回调更新如下:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>废弃，已经被 <code>rtcEngineLocalAudioMixingDidFinish</code> 替代</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>新增，当本地结束音效播放时触发该回调</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
<td>新增，当远端开始伴奏播放时触发该回调</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>新增，当远端结束音效播放时触发该回调</td>
</tr>
</tbody>
</table>


-   通信和直播场景下支持音频自采集功能，新增以下 API:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
<td>开启外部音频采集</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>关闭外部音频采集</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>推送外部音频帧</td>
</tr>
</tbody>
</table>


-   通信和直播场景下支持服务端踢人功能。如有需要，请联系 [sales@agora.io](mailto:sales@agora.io) 开通该功能。


#### **问题修复**

修复了音频路由和蓝牙相关的若干问题。

**1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `setAudioProfile` 设置音频参数和应用场景。

-   新增 API `setLocalVoicePitch` 提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume` 提供调节耳返音量功能。


#### **改进**

-   优化了在高码率下的音频体验。

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像\(均值为 938 ms, 网络状态良好时可达 734 ms\)。

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。



#### **问题修复**

修复了部分 iOS 机器上偶现的崩溃。


**1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### **问题修复**

-   解决了 iOS 11 在 iPhone 7\(及以上版本\) 手机外放下无法调节音量的问题。

-   优化了特定场景下出现的回声问题。


**1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `didClientRoleChanged` 用于提醒直播场景下主播、观众上下麦的回调。

-   新增单独关闭语音播放的功能。

-   新增功能支持服务端推流失败回调。

-   为 SDK 新增了 module map, 意味着 Swift 项目以后不需要添加桥接文件才能使用。


#### **改进**

-   可以在客户端设置推流的 profile


#### **修复问题**

修复了部分机型上偶现的崩溃。

**1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   在 API 方法 `setEncryptionMode` 里新增了加密模式 `aes-128-ecb`。
-   在 API 方法 `startAudioRecording`里新增了参数 `quality` 用于设置录音音质。
-   新增了一系列 API 管理音效。
-   直播场景下， 新增了 API 方法 `injectStream`在当前频道内插入一条 RTMP 流。该功能目前为 beta 版。


#### **修复问题**

修复了部分机型上偶现的崩溃问题。


