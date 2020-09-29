
---
title: Release Notes
description: 
platform: iOS
updatedAt: Sun Sep 27 2020 09:59:01 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Video SDK for iOS.

## Overview

The Video SDK supports the following scenarios:

-   Voice or video call
-   Live interactive audio or video streaming

For the key features included in each scenario, see [Agora Voice Call Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms), [Agora Video Call Overview](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms), [Agora Live Interactive Audio Streaming Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms) and [Agora Live Interactive Video Streaming Overview](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms).

## v3.1.2

v3.1.2 was released on September 14, 2020.

**Compatibility changes**

To improve user experience, this version disables the local network connection quality report by default, to prevent the prompt to find local network devices from popping up when an end user launches the app on an iOS 14.0 device. The [`gatewayRtt`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/gatewayRtt) parameter in the [`reportRtcStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportRtcStats:) callback is disabled by default. Do not use `gatewayRtt` to obtain the round-trip delay between the client and the local router. See the [FAQ](https://docs.agora.io/en/faq/local_network_privacy) for details.

If you do not mind the prompt, and want to enable the `gatewayRtt` parameter, please contact Agora technical support via [support@agora.io](mailto:support@agora.io).

**Issues fixed**

This release fixed the following issues:

- When you use MediaIO to switch to a different video source, the screen gets frozen.
- When a remote user leaves the channel, the view of the remote user becomes black.
- The [`firstLocalVideoFrameWithSize`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstLocalVideoFrameWithSize:elapsed:) and [`firstRemoteVideoFrameOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteVideoFrameOfUid:size:elapsed:) callbacks are not triggered at the right time.

## v3.1.1

v3.1.1 was released on August 27, 2020.

**Compatibility changes**

This release changes the `AgoraAreaCode` for regional connection. The latest area codes are as follows:

- `AgoraAreaCodeCN`: Mainland China.
- `AgoraAreaCodeNA`: North America.
- `AgoraAreaCodeEU`: Europe.
- `AgoraAreaCodeAS`: Asia, excluding Mainland China.
- `AgoraAreaCodeJP`: Japan.
- `AgoraAreaCodeIN`: India.
- `AgoraAreaCodeGLOB`: (Default) Global.

If you have specified a region for connection when calling [`sharedEngineWithConfig`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithConfig:delegate:), ensure that you use the latest area code when migrating from an earlier SDK version.

## v3.1.0

v3.1.0 was released on August 11, 2020.

**New features**

#### 1. Publishing and subscription states

This release adds the following callbacks to report the current publishing and subscribing states:

- `didAudioPublishStateChange`: Reports the change of the audio publishing state.
- `didVideoPublishStateChange`: Reports the change of the video publishing state.
- `didAudioSubscribeStateChange`: Reports the change of the audio subscribing state.
- `didVideoSubscribeStateChange`: Reports the change of the video subscribing state.

#### 2. First local frame published callback

This release adds the `firstLocalAudioFramePublished` and `firstLocalVideoFramePublished` callbacks to report that the first audio or video frame is published. The `firstLocalAudioFrame` callback is deprecated from v3.1.0.

#### 3. Custom data report

This release adds the `sendCustomReportMessage` method for reporting customized messages. To try out this function, contact [support@agora.io](mailto:support@agora.io) and discuss the format of customized messages with us.

**Improvement**

#### 1. Regional connection

This release adds the following regions for regional connection. After you specify the region for connection, your app that integrates the Agora SDK connects to the Agora servers within that region.

- `AgoraIpAreaCode_JAPAN`: Japan.
- `AgoraIpAreaCode_INDIA`: India.

#### 2. Advanced screen sharing

This release adds the following callbacks to the `AgoraVideoSourceProtocol` protocol for screen sharing:
- `captureType`: Get the capture type of the custom video source.
- `contentHint`: Get the content hint of the custom video source

#### 3. CDN live streaming

This release adds the `rtmpStreamingEventWithUrl` callback to report events during CDN live streaming, such as failure to add a background image or watermark image.

#### 4. Encryption

This release adds the `enableEncryption` method for enabling built-in encryption, and deprecates the following methods:

- `setEncryptionSecret`
- `setEncryptionMode`

#### 5. More in-call statistics

This release adds the following attributes to provide more in-call statistics:

- Adds `txPacketLossRate` in `AgoraRtcLocalAudioStats`, which represents the audio packet loss rate (%) from the local client to the Agora edge server before applying anti-packet loss strategies.
- Adds the following attributes in `AgoraRtcLocalVideoStats`: 
  - `txPacketLossRate`: The video packet loss rate (%) from the local client to the Agora edge server before applying anti-packet loss strategies.
  - `captureFrameRate`: The capture frame rate (fps) of the local video.
- Adds `publishDuration` in `AgoraRtcRemoteAudioStats` and `AgoraRtcRemoteVideoStats`, which represents the total publish duration (ms) of the remote media stream.

#### 6. Audio profile

To improve audio performance, this release adjusts the maximum audio bitrate of each audio profile as follows:

| Profile                                   | v3.1.0                                                       | Earlier than v3.1.0                                          |
| :---------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `AgoraAudioProfileDefault`                   | <li>For the interactive streaming profile: 64 Kbps</li><li>For the communication profile: 18 Kbps</li> | <li>For the interactive streaming profile: 52 Kbps</li><li>For the communication profile: 18 Kbps</li> |
| `AgoraAudioProfileSpeechStandard`           | 18 Kbps                                                      | 18 Kbps                                                      |
| `AgoraAudioProfileMusicStandard`            | 64 Kbps                                                      | 48 Kbps                                                      |
| `AgoraAudioProfileMusicStandardStereo`     | 80 Kbps                                                      | 56 Kbps                                                      |
| `AgoraAudioProfileMusicHighQuality`        | 96 Kbps                                                      | 128 Kbps                                                     |
| `AgoraAudioProfileMusicHighQualityStereo` | 128 Kbps                                                     | 192 Kbps                                                     |

#### 7. Log files

This release increases the default number of log files that the Agora SDK outputs from 2 to 5, and increases the default size of each log file from 512 KB to 1024 KB. By default, the SDK outputs five log files, `agorasdk.log`, `agorasdk_1.log`, `agorasdk_2.log`, `agorasdk_3.log`, `agorasdk_4.log`. The SDK writes the latest logs in `agorasdk.log`. When `agorasdk.log` is full, the SDK deletes the log file with the earliest modification time among the other four, renames `agorasdk.log` to the name of the deleted log file, and create a new `agorasdk.log` to record the latest logs.

#### 8. Others

- This release improves the performance of some iOS models (using Apple A10 and below chips), reducing CPU utilization and memory footprint.
- This release reduces the playback time of the first remote audio frame.

**Issues fixed**

This release fixed the following issues:

- `setAudioMixingPitch` did not work when setting the `pitch` parameter to certain values.
- Occasional audio freeze when disconnecting from a Bluetooth device.
- The app did not receive the `setMediaMetadataDataSource` callback.
- Occasional crashes when the user left the channel after talking in multiple channels.

**API changes**

#### Added

- [`didAudioPublishStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didAudioPublishStateChange:oldState:newState:elapseSinceLastState:)
- [`didVideoPublishStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoPublishStateChange:oldState:newState:elapseSinceLastState:)
- [`didAudioSubscribeStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didAudioSubscribeStateChange:withUid:oldState:newState:elapseSinceLastState:)
- [`didVideoSubscribeStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoSubscribeStateChange:withUid:oldState:newState:elapseSinceLastState:)
- [`firstLocalAudioFramePublished`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstLocalAudioFramePublished:)
- [`firstLocalVideoFramePublished`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstLocalVideoFramePublished:)
- [`enableEncryption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableEncryption:encryptionConfig:)
- `txPacketLossRate` in [`AgoraRtcLocalAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalAudioStats.html) class
- `txPacketLossRate` and `captureFrameRate` in [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html) class
- `publishDuration` in [`AgoraRtcRemoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) and [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class
- `captureType` and `contentHint` in [`AgoraVideoSourceProtocol`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraVideoSourceProtocol.html) protocol
- [`rtmpStreamingEventWithUrl`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingEventWithUrl:eventCode:)
- Warning code: `AgoraWarningCodeAdmCategoryNotPlayAndRecord(1029)` and `AgoraWarningCodeApmResidualEcho(1053)`
- Error code: `AgoraErrorCodeNoServerResources(103)`

#### Deprecated

- `setEncryptionSecret`
- `setEncryptionMode`
- `firstLocalAudioFrame`

#### Deleted

- Warning code: `AgoraWarningCodeAdmImproperSettings(1053)`

## v3.0.1.1

v3.0.1.1 was released on Jun 18, 2020. This release fixed the following issues:

- Crashes after calling `registerVideoRenderFactory` (deprecated).
- The image enhancement feature does not work.

## v3.0.1

v3.0.1 was released on May 27, 2020.

**Compatibility changes**

#### 1. Dynamic library

This release replaces the static library with a dynamic library for the following reasons:

- Improving overall security.
- Avoiding incompatibility issues with other third-party libraries.
- Making it easier to upload the app to the App Store.

To upgrade the RTC Native SDK, you must re-integrate the dynamic library, `AgoraRtcKit.framework`. This process should take no more than five minutes. See [Migration Guide](../../en/Video/migration_apple.md).

<div class="alert note">Apple supports the dynamic library on iOS 13.4 and later.</div>

#### 2. Frame position for the video observer (C++)

As of this release, to get the video frame from the `onPreEncodeVideoFrame` callback, you must set `POSITION_PRE_ENCODER(1<<2)` in [`getObservedFramePosition`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#ad4c174389264630ffb1b2d24c6030013) as the frame position to observe, as well as implementing the `onPreEncodeVideoFrame` callback.

**New features**

#### 1. Audio mixing pitch

To set the pitch of the local music file during audio mixing, this release adds `setAudioMixingPitch`. You can set the `pitch` parameter to increase or decrease the pitch of the music file. This method sets the pitch of the local music file only. It does not affect the pitch of a human voice.

#### 2. Voice enhancement

To improve the audio quality, this release adds the following enumerate elements in `setLocalVoiceChanger` and `setLocalVoiceReverbPreset`:

- `AgoraAudioVoiceChanger` adds several elements that have the prefixes `AgoraAudioVoiceBeauty` and `AgoraAudioGeneralBeautyVoice`. The `AgoraAudioVoiceBeauty` elements enhance the local voice, and the `AgoraAudioGeneralBeautyVoice` enumerations add gender-based enhancement effects.
- `AgoraAudioReverbPreset` adds the enumeration `AgoraAudioReverbPresetVirtualStereo` and several enumerations that have the prefix `AgoraAudioReverbPresetFx`. The `AgoraAudioReverbPresetVirtualStereo` enumeration implements reverberation in the virtual stereo, and the `AgoraAudioReverbPresetFx` enumerations implement additional enhanced reverberation effects.

See [Set the Voice Changer and Reverberation Effects](../../en/Video/voice_changer_apple.md) for more information.

#### 3. Face detection

This release enables local face detection. After you call `enableFaceDetection` to enable this function, the SDK triggers the `facePositionDidChangeWidth` callback in real time to report the detection results, including the distance between the human face and the device screen. This function can remind users to keep a certain distance from the screen.

#### 4. Fill mode

To improve the user experience of watching videos, this release adds a video display mode `AgoraVideoRenderModeFill(4)`. This mode zooms and stretches the video to fill the display window. You can select this mode when calling the following methods:

- `setupLocalVideo`
- `setupRemoteVideo`
- `setLocalRenderMode`
- `setRemoteRenderMode`

#### 5. Remote video renderer in multiple channels

This release adds `setRemoteVideoRenderer` and `remoteVideoRendererOfUserId` in the `AgoraRtcChannel` class to enable users who join the channel using the `AgoraRtcChannel` object to customize the remote video renderer.

#### 6. Data post-processing in multiple channels (C++)

This release adds support for post-processing remote audio and video data in a multi-channel scenario by adding the following C++ methods:

- The `IAudioFrameObserver` class:  [`isMultipleChannelFrameWanted`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a4b6bdf2a975588cd49c2da2b6eff5956) and [`onPlaybackAudioFrameBeforeMixingEx`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ab0cf02ba307e91086df04cda4355905b).
- The `IVideoFrameObserver` class: [`isMultipleChannelFrameWanted`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#aa6bf2611907a097ec359b83f1e3ba49a) and [`onRenderVideoFrameEx`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#ad325db8ee3a04e667e6db3d1a84f381d).

After successfully registering the audio or video observer, if you set the return value of `isMultipleChannelFrameWanted` as `true`, you can get the corresponding audio or video data from `onPlaybackAudioFrameBeforeMixingEx` or `onRenderVideoFrameEx`. In a multi-channel scenario, Agora recommends setting the return value as `true`.

**Improvements**

#### Frame position (C++)

After successfully registering the video observer, you can observe and get the video frame at each node of video processing. To conserve power consumption, this release enables customizing the frame position for the video observer. Set the return value of the [`getObservedFramePosition`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#ad4c174389264630ffb1b2d24c6030013) callback to set the position to observe:

- The position after capturing the video frame.
- The position before receiving the remote video frame.
- The position before encoding the frame.

#### Others

- Improves in-call audio quality. When multiple users speak at the same time, the SDK does not decrease volume of any speaker.
- Reduces overall CPU usage of the device.

**Fixed issues**

- This release fixed issues with inaccurate report of the `didRemoteAudioStateChanged` callback, no audio, audio mixing and audio freezing.
- This release fixed issues that failure to end a call, and inaccurate report of the `didClientRoleChanged` callback.

**API changes**

This release adds the following APIs:

-  [`setAudioMixingPitch`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPitch:)
- Several elements that have the prefixes `AgoraAudioVoiceBeauty` and `AgoraAudioGeneralBeautyVoice` in the [`AgoraAudioVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraAudioVoiceChanger.html) enumeration
- `AgoraAudioReverbPresetVirtualStereo` and several elements that have the prefixes `AgoraAudioReverbPresetFx` in the [`AgoraAudioReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraAudioReverbPreset.html) enumeration
- [`enableFaceDetection`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableFaceDetection:)
- [`facePositionDidChangeWidth`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:facePositionDidChangeWidth:previewHeight:faces:)
- `AgoraVideoRenderModeFill` in the [`AgoraVideoRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraVideoRenderMode.html) enumeration 
-  [`setRemoteVideoRenderer`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcChannel.html#//api/name/setRemoteVideoRenderer:forUserId:) and [`remoteVideoRendererOfUserId`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcChannel.html#//api/name/remoteVideoRendererOfUserId:) in the `AgoraRtcChannel` class
- `totalActiveTime` in the [`AgoraRtcRemoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class
- `totalActiveTime` in the [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class

## v3.0.0.2

v3.0.0.2 was released on Apr 22, 2020.

**New features**

#### Specify the area of connection

This release adds `sharedEngineWithConfig` for specifying the area of connection when creating an `AgoraRtcEngineKit` instance. This advanced feature applies to scenarios that have regional restrictions. You can choose from areas including Mainland China, North America, Europe, Asia (excluding Mainland China), and global (default).

After specifying the area of connection:

- When the app that integrates the Agora SDK is used within the specified area, it connects to the Agora servers within the specified area under normal circumstances.
- When the app that integrates the Agora SDK is used out of the specified area, it connects to the Agora servers either in the specified area or in the area where the SDK is located.

**Issues fixed**

This release fixed the occasional failure to connect to a Bluetooth headset.

**API changes**

#### Added

[`sharedEngineWithConfig`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithConfig:delegate:)

## v3.0.0

v3.0.0 was released on Mar 4, 2020.

On Mar 24, 2020, we fixed occasional issues relating to no audio, audio mixing, multiple `didClientRoleChanged` callbacks, and SDK crashes.

In this release, Agora improves the user experience under poor network conditions for both the `Communication` and `LiveBroadcasting` profiles through the following measures:

- Adopting a new architecture for the `Communication` profile.
- Upgrading the last-mile network strategy for both the `Communication` and `LiveBroadcasting` profiles,  which enhances the SDK's anti-packet-loss capacity by maximizing the net bitrate when the uplink and downlink bandwidth are insufficient.

To deal with any incompatibility issues caused by the architecture change, Agora uses the fallback mechanism to ensure that users of different versions of the SDKs can communicate with each other: if a user joins the channel from a client using a previous version, all clients using v3.0.0 automatically fall back to the older version. This has the effect that none of the users in the channel can enjoy the improved experience. Therefore we strongly recommend upgrading all your clients to v3.0.0.

We also upgrade the On-premise Recording SDK to v3.0.0. Ensure that you upgrade your On-premise Recording SDK to v3.0.0 so that all users can enjoy the improvements brought by the new architecture and network strategy.

**Compatibility changes**

#### 1. Renaming the static library and adding support for dynamic library

To unify the library names across platforms, this release renames the library from `AgoraRtcEngineKit.framework` to `AgoraRtcKit.framework`. If you upgrade your SDK to v3.0.0, you must re-import the `AgoraRtcKit` class. For details, see [Import the class](https://docs.agora.io/en/Interactive%20Broadcast/start_live_ios?platform=iOS#a-nameimportclassa2-import-the-class) in the Quickstart.

To improve your development experience, this release also adds support for the dynamic library. You can integrate either the static or the dynamic library in your project, and the name of the dynamic library package is Agora_Native_SDK_for_iOS_v3_0_0_FULL_Dynamic. 

Integrating the dynamic library has the following advantages:

- The overall security level is improved.
- Incompatibility issues with other third-party libraries are avoided.
- Uploading the app onto App Store is easier.

If you prefer the dynamic library, you need to re-integrate the SDK and re-import the `AgoraRtcKit` class. This process should take no more than five minutes. See [Integrate the SDK](https://docs.agora.io/en/Interactive%20Broadcast/start_live_ios?platform=iOS#a-nameintegratesdkaintegrate-the-sdk) and [Import the class](https://docs.agora.io/en/Interactive%20Broadcast/start_live_ios?platform=iOS#a-nameimportclassa2-import-the-class) in the Quickstart.

<div class="alert info">The following table shows the difference in the file size when generating ipa files with a dynamic and static library:

<table>
    <tr>
        <td width="8%"><b>Library type</b></td>
        <td width="15%"><b>ipa size (M)</b></td>
        <td width="10%"><b>Decompressed ipa size (M)</b></td>
        <td width="17%"><b>Frameworks folder size (M)</b></td>
        <td width="15%"><b>Binary file size (M)</b></td>
        <td width="25%"><b>Total size of frameworks folder + binary file (M)</b></td>
    </tr>
    <tr>
        <td>Dynamic library</td>
        <td>31.1</td>
        <td>65</td>
        <td>51.47</td>
        <td>2.4</td>
        <td>53.87</td>
    </tr>
    <tr>
        <td>Static library</td>
        <td>30.6</td>
        <td>63.7</td>
        <td>30.1</td>
        <td>22.5</td>
        <td>52.6</td>
    </tr>
</table>

The dynamic library is located in the framework folder as an independent library. Note that the corresponding binary file size does not include the SDK size. Overall, this decreases the binary file size by 20.1 M and increases the framework folder size by 21.37 M.</div>

#### 2. Dual-stream mode not enabled in the Communication profile

As of v3.0.0, the native SDK does not enable the [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode) by default in the `Communication` profile. Call the [`enableDualStreamMode (YES)`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:) method after joining the channel to enable it. In video scenarios with multiple users, we recommend enabling the dual-stream mode.

**New features**

#### 1. Multiple channel management

To enable a user to join an unlimited number of channels at a time, this release adds the [`AgoraRtcChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcChannel.html) and [`AgoraRtcChannelDelegate`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) classes. By creating multiple `AgoraRtcChannel` objects, a user can join the corresponding channels at the same time.

After joining multiple channels, users can receive the audio and video streams of all the channels, but publish one stream to only one channel at a time. This feature applies to scenarios where users need to receive streams from multiple channels, or frequently switch between channels to publish streams. See [Join multiple channels](../../en/Video/multiple_channel_apple.md) for details.

#### 2. Raw video data
Adds the following C++ callbacks to the `IVideoFrameObserver` class to provide raw video data at different video transmission stages, and to accommodate more scenarios.

- `onPreEncodeVideo`: Gets the video data after pre-processing and prior to encoding. This method applies to the scenarios where you need to pre-process the video data.
- `getSmoothRenderingEnabled`: Sets whether to smooth the acquired video frames. The smoothed video frames are more evenly spaced, providing a better rendering experience.

#### 3. Adjusting the playback volume of the specified remote user

Adds [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustUserPlaybackSignalVolume:volume:) for adjusting the playback volume of a specified remote user. You can call this method as many times as necessary in a call or live interactive streaming to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.

#### 4. Agora Mediaplayer Kit

To enrich the playability of the live interactive streaming, Agora releases the Mediaplayer Kit plug-in, which supports the host playing local or online media resources and sharing them with all users in the channel during the live interactive streaming. See [Mediaplayer Kit release notes](https://docs.agora.io/en/Interactive%20Broadcast/mediaplayer_release_ios?platform=iOS) for details.

**Improvements**

#### 1. Audio profiles

To meet the need for higher audio quality, this release adjusts the corresponding audio profile of `AgoraAudioProfileDefault(0)` in the `LiveBroadcasting` profile.

| SDK   | `AgoraAudioProfileDefault(0)`                                  |
| :--------- | :---------------------------------------------------------- |
| v3.0.0      | A sample rate of 48 KHz, music encoding, mono, and a bitrate of up to 52 Kbps. |
| Earlier than v3.0.0 | A sample rate of 32 KHz, music encoding, mono, and a bitrate of up to 52 Kbps. |

#### 2. Mirror mode

The mirror mode determines how the SDK mirrors the video in different stages of transmission. This release supports setting the video mirror mode when encoding and rendering the video:

- Setting a mirror mode for the stream to be encoded: This release adds the `mirrorMode` member to the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) struct struct for setting a mirror effect for the stream to be encoded and transmitted.
- Setting a mirror mode for the streams to be rendered: 
	- We add the `mirrorMode` member to the [`AgoraRtcVideoCanvas`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcVideoCanvas.html) struct. You can use [`setupLocalVideo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupLocalVideo:), to set a mirror effect for the local view, or use [`setupRemoteVideo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupRemoteVideo:) to set a mirror effect for the remote view on the local device.
	- This release also adds the [`setLocalRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setLocalRenderMode:mirrorMode:) and [`setRemoteRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteRenderMode:renderMode:mirrorMode:) methods, both of which take an extra `mirrorMode` parameter. During a call, you can use `setLocalRenderMode` to update the mirror effect of the local view or `setRemoteRenderMode` to update the mirror effect of the remote view on the local device.

#### 3. Quality statistics

Adds the following members in the [`AgoraChannelStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html) class for providing more in-call statistics, making it easier to monitor the call quality and memory usage in real time:

- `gatewayRtt`
- `memoryAppUsageRatio`
- `memoryTotalUsageRatio`
- `memoryAppUsageInKbytes`

#### 4. Others

This release enables interoperability between the Native SDK and the Web SDK by default, and deprecates the `enableWebSdkInteroperability` method.

**Issues fixed**

- Audio issues concerning audio mixing, audio encoding, and echo.
- Video issues concerning the watermark, video aspect ratio, video sharpness, full-screen video, and black outline during the screen share.
- Other issues related to app crashes, log file, and unstable service when pushing streams to the CDN.

**API changes**

#### Behavior change

Calling [`enableLocalAudio (NO)`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLocalAudio:) does not change the in-call volume to media volume.

#### Added

- [`setLocalRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setLocalRenderMode:mirrorMode:)
- [`setRemoteRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteRenderMode:renderMode:mirrorMode:) 
- The `mirrorMode` parameter in the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) struct
- The `mirrorMode` and `channelId` parameter in the [`AgoraRtcVideoCanvas`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcVideoCanvas.html) struct
- The `channelId` parameter in the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) struct
- [`createRtcChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/createRtcChannel:)
- [`AgoraRtcChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcChannel.html) class
- [`AgoraRtcChannelDelegate`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcChannelDelegate.html) class
- The `gatewayRtt`, `memoryAppUsageRatio`, `memoryTotalUsageRatio` and `memoryAppUsageInKbytes` members in the [`AgoraChannelStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html) class

#### Deprecated

- `enableWebSdkInteroperability`
- `setLocalRenderMode`¹. Use the new [`setLocalRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setLocalRenderMode:mirrorMode:) method instead.
- `setRemoteRenderMode`¹. Use the new [`setRemoteRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteRenderMode:renderMode:mirrorMode:) method instead.
- `setLocalVideoMirrorMode`. Use the `mirrorMode` parameter in the [`setupLocalVideo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setupLocalVideo:) and [`setLocalRenderMode`](https://docs.agora.io/en/Video/API%20Reference/oc/v3.0.0/Classes/AgoraRtcEngineKit.html#//api/name/setLocalRenderMode:mirrorMode:) methods instead.
- `firstRemoteVideoFrameOfUid`. Use the [remoteVideoStateChangedOfUid](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback instead.
- `didAudioMuted`, `firstRemoteAudioFrameDecodedOfUid` and `firstRemoteAudioFrameOfUid`. Use the [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callback instead.
- `streamPublishedWithUrl` and `streamUnpublishedWithUrl`. Use the [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) callback instead.

## v2.9.3

v2.9.3 was released on Feb 10, 2020.

This release fixed the following issues:

- The `setRemoteSubscribeFallbackOption method`, which should work in the `LiveBroadcasting` profile only, also works in the `Communication` profile.
- In some one-to-one communication, the downlink media stream falls back to audio-only under poor network conditions.
- In scenarios involving custom video source, the resolution of the received video frame changes as the app switches between the foreground and the background.

## v2.9.1
v2.9.1 is released on Sep 19, 2019.

**New features**

#### 1. Detecting local voice activity

This release adds the `report_vad(bool)` parameter to the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method to enable local voice activity detection. Once it is enabled, you can check the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) struct of the [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:) callback for the voice activity status of the local user.

#### 2. Choosing front and rear cameras

Users can select the front or rear camera before joining a channel. This release adds the `AgoraCameraDirection` member variable to the [`AgoraCameraCapturerConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraCameraCapturerConfiguration.html) class to accomplish this. Choose the front or rear camera by selecting `AgoraCameraDirectionRear(1)` or `AgoraCameraDirectionFront(0)` respectively.

#### 3. Supporting RGBA raw video data

This release supports RGBA raw video data. Use the C++ method [`getVideoFormatPreference`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369) to set the format of the raw video data format.

You can also rotate or mirror the RGBA raw data using the C++ methods [`getRotationApplied`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb) or [`getMirrorApplied`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991) respectively.

**Improvements**

#### 1. Improving the watermark function in live interactive streaming

This release adds a new [`addVideoWatermark`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addVideoWatermark:options:) method with the following settings:

- The `visibleInPreview` member sets whether the watermark is visible in the local preview.
- The `positionInLandscapeMode`/`positionInPortraitMode` member sets the watermark position when the encoding video is in landscape/portrait mode.

This release optimizes the watermark function, reducing the CPU usage by 5% to 20%.

The original `addVideoWatermark` method is deprecated.

#### 2. Supporting more audio sample rates for recording

To enable more audio sample rate options for recording, this release adds a new [`startAudioRecording`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:) method with a `sampleRate` parameter. In the new method, you can set the sample rate as 16, 32, 44.1 or 48 kHz. The original method supports only a fixed sample rate of 32 kHz and is deprecated.

**Issues fixed**

#### Audio

- Audio freezes.
- Abnormal audio when a user rejoins the channel after a third-party app interrupts the call.
- Echoes occur when a user is in a channel.

#### Miscellaneous

- The remote users do not receive the [`didUpdatedUserInfo`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:) callback when the local user switches the network connection before joining the channel and calls the [`joinChannelByUserAccount`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:) method.
- Mixing streams occur in RTMP streaming.

**API changes**

To improve the user experience, we made the following changes in v2.9.1:

#### Added

- [`startAudioRecording`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:sampleRate:quality:)
- [`addVideoWatermark`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addVideoWatermark:options:)
- [`getVideoFormatPreference`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#a440e2a33140c25dfd047d1b8f7239369)
- [`getRotationApplied`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afd5bb439a9951a83f08d8c0a81468dcb)
- [`getMirrorApplied`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_video_frame_observer.html#afc5cce81bf1c008e9335a0423ca45991)
- The `report_vad` parameter in the [`enableAudioVolumeIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableAudioVolumeIndication:smooth:report_vad:) method
- The `vad` member in the [`AgoraRtcAudioVolumeInfo`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcAudioVolumeInfo.html) class
- The `AgoraCameraDirection` member in the [`AgoraCameraCapturerConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraCameraCapturerConfiguration.html) class

#### Deprecated

- `startAudioRecording`
- `addVideoWatermark`

## v2.9.0
v2.9.0 is released on Aug. 16, 2019.

**Compatibility changes**

#### 1. RTMP streaming

In this release, we deleted the following methods:

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`

If your app implements RTMP streaming with the methods above, ensure that you upgrade the SDK to the latest version and use the following methods for the same function:

- [`setLiveTranscoding`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLiveTranscoding:)
- [`addPublishStreamUrl`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)
- [`removePublishStreamUrl`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:)
- [`rtmpStreamingChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)

For how to implement the new methods, see [Push Streams to the CDN](../../en/Video/cdn_streaming_apple.md).

#### 2. Reporting the state of the remote video

This release extends the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with more states of the remote video: Stopped(0), Starting(1), Decoding(2), Frozen(3), and Failed(4). It adds a reason parameter to the callback to indicate why the remote video state changes. The original `remoteVideoStateChangedOfUid` callback is deleted. If you upgrade your Native SDK to the latest version, ensure that you re-implement the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback.

The new callback reports most of the remote video states, and therefore deprecates the following callbacks. You can still use them, but we do not recommend doing so.

- [`didVideoEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoEnabled:byUid:)
- [`didLocalVideoEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalVideoEnabled:byUid:)
- [`firstRemoteVideoDecodedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteVideoDecodedOfUid:size:elapsed:)

<div class="alert note">The triggering timing of the new callback is different from the old one. The new <code>remoteVideoStateChangedOfUid</code> callback is triggered only when the remote video state has changed.</div>

#### 3. Disabling/enabling the local audio

To improve the audio quality in the `Communication` profile, this release sets the system volume to the media volume after you call the `enableLocalAudio`(true) method. Calling `enableLocalAudio`(false) switches the system volume back to the in-call volume.

**New features**

#### 1. Faster switching to another channel

This release adds the [`switchChannelByToken`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method to enable the audience in an interactive streaming channel to quickly switch to another channel. With this method, you can achieve a much faster switch than with the [`leaveChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) and [`joinChannelByToken`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:) methods. After the audience successfully switches to another channel by calling the [`switchChannelByToken`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) method, the SDK triggers the [`didLeaveChannelWithStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLeaveChannelWithStats:) and [`didJoinChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinChannel:withUid:elapsed:) callbacks to indicate that the audience has left the original channel and joined a new one. 

#### 2. Channel media stream relay

This release adds the following methods to relay the media streams of a host from a source channel to a destination channel. This feature applies to scenarios such as online singing contests, where hosts of different interactive streaming channels interact with each other.

- [`startChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)

During the media stream relay, the SDK reports the states and events of the relay with the  [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:) and [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:) callbacks.

For more information on the implementation, API call sequence, sample code, and considerations, see [Co-host Across Channels](../../en/Video/media_relay_apple.md).

#### 3. Reporting the local and remote audio state

This release adds the [`localAudioStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) and [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:) callbacks to report the local and remote audio states. With these callbacks, the SDK reports the following states for the local and remote audio:

- The local audio: Stopped(0), Recording(1), Encoding(2), or Failed(3). When the state is Failed(3), see the `error` parameter for troubleshooting.
- The remote audio: Stopped(0), Starting(1), Decoding(2), Frozen(3), or Failed(4). See the `reason` parameter for why the remote audio state changes.

#### 4. Reporting the local audio statistics

This release adds the [`localAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:) callback to report the statistics of the local audio during a call, including the number of channels, the sending sample rate, and the average sending bitrate of the local audio.

#### 5. Pulling the remote audio data

To improve the experience in audio playback, this release adds the following methods to pull the remote audio data. After getting the audio data, you can process it and play it with the audio effects that you want.

- [`enableExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)

The difference between the `onPlaybackAudioFrame` callback and the `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte` method is as follows:

- `onPlaybackAudioFrame`: The SDK sends the audio data to the app once every 10 ms. Any delay in processing the audio frames may result in an audio delay.
- `pullPlaybackAudioFrameRawData` / `pullPlaybackAudioFrameSampleBufferByLengthInByte`: The app pulls the remote audio data. After setting the audio data parameters, the SDK adjusts the frame buffer and avoids problems caused by jitter in external audio playback.

**Improvements**

#### 1. Reporting more statistics of the in-call quality

This release adds the following statistics in the [`AgoraChannelStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html), [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html), and [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) classes:

- `AgoraChannelStats`: The total number of the sent audio bytes, sent video bytes,  received audio bytes, and received video bytes during a session.
- `AgoraRtcLocalVideoStats`: The encoding bitrate, the width and height of the encoding frame, the number of frames, and the codec type of the local video.
- `AgoraRtcRemoteVideoStats`: The packet loss rate of the remote video.

#### 2. Improving the video quality of live interactive streaming

This release minimizes the video freeze rate under poor network conditions, improves the video sharpness, and optimizes the video smoothness when the packet loss rate is high.

#### 3. Other Improvements

- Improves the audio quality when the audio scenario is set to `GameStreaming`.
- Improves the audio quality after the user disables the microphone in the `Communication` profile.

**Issues fixed**

#### Audio

- When interoperating with a Web app, voice distortion occurs after the native app enables the remote sound position indication.
- Audio freezes when the audience in the `LiveBroadcasting` profile sets the audio session category to playback.
- No audio after a Web app joins the channel when the remote sound position indication is enabled.

#### Video

- Occasional crashes occur when setting the video encoder configuration after joining the channel.
- Crashes occur when repeatedly changing the video resolution after joining the channel.

#### Miscellaneous

- Occasionally mixed streams in RTMP streaming. 
- Crashes occur after calling the [`leaveChannel`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:) method.

**API Changes**

To improve the user experience, we made the following changes in v2.9.0:

#### Added
- [`enableExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableExternalAudioSink:channels:)
- [`disableExternalAudioSink`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/disableExternalAudioSink)
- [`pullPlaybackAudioFrameRawData`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameRawData:lengthInByte:)
- [`pullPlaybackAudioFrameSampleBufferByLengthInByte`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pullPlaybackAudioFrameSampleBufferByLengthInByte:)
- [`localAudioStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:)
- [`remoteAudioStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStateChangedOfUid:state:reason:elapsed:)
- [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:)
- [`localAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStats:)
- [`switchChannelByToken`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/switchChannelByToken:channelId:joinSuccess:) 
- [`startChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startChannelMediaRelay:)
- [`updateChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/updateChannelMediaRelay:)
- [`stopChannelMediaRelay`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopChannelMediaRelay)
- [`channelMediaRelayStateDidChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:channelMediaRelayStateDidChange:error:)
- [`didReceiveChannelMediaRelayEvent`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didReceiveChannelMediaRelayEvent:)
- [`AgoraChannelStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html): `txAudioBytes`, `txVideoBytes`, `rxAudioBytes` and `rxVideoBytes`
- [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html): `encodedBitrate`, `encodedFrameWidth`, `encodedFrameHeight`, `encodedFrameCount` and `codedType`
- [`AgoraRtcRemoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html): `packetLossRate`

#### Deprecated

- [`didMicrophoneEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didMicrophoneEnabled:). Use AgoraAudioLocalStateStopped(0) or AgoraAudioLocalStateRecording(1) in the [`localAudioStateChange`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioStateChange:error:) callback instead.
- [`audioTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback instead.
- [`videoTransportStatsOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:). Use the [`remoteVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStats:) callback instead.
- [`didVideoEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didVideoEnabled:byUid:). Use the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with the following parameters instead:
	- AgoraVideoRemoteStateStopped(0) and AgoraVideoRemoteStateReasonRemoteMuted(5).
	- AgoraVideoRemoteStateDecoding(2) and AgoraVideoRemoteStateReasonRemoteUnmuted(6).
- [`didLocalVideoEnabled`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalVideoEnabled:byUid:). Use the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback with the following parameters instead:
	- AgoraVideoRemoteStateStopped(0) and AgoraVideoRemoteStateReasonRemoteMuted(5).
	- AgoraVideoRemoteStateDecoding(2) and AgoraVideoRemoteStateReasonRemoteUnmuted(6).
- [`firstRemoteVideoDecodedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteVideoDecodedOfUid:size:elapsed:). Use AgoraVideoRemoteStateStarting(1) or AgoraVideoRemoteStateDecoding(2) in the [`remoteVideoStateChangedOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStateChangedOfUid:state:reason:elapsed:) callback instead.

#### Deleted

- `configPublisher`
- `setVideoCompositingLayout`
- `clearVideoCompositingLayout`
- `remoteVideoStateChangedOfUid`

## v2.8.0

v2.8.0 is released on Jul. 8, 2019.

**New features**

#### 1. Supporting string user IDs

Many apps use string user IDs. This release adds the following methods to enable apps to join an Agora channel directly with string user IDs as user accounts:

- [registerLocalUserAccount](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)

For other methods, Agora uses the integer uid parameter. The Agora Engine maintains a mapping table that contains the user ID and string user account, and you can get the corresponding user account or ID by calling the getUserInfoByUid or getUserInfoByUserAccount method.

To ensure smooth communication, use the same parameter type to identify all users within a channel, that is, all users should use either the integer user ID or the string user account to join a channel. 

**Note**:
- Do not mix parameter types within the same channel. The following Agora SDKs support string user accounts:
	- The Native SDK: v2.8.0 and later.
	- The Web SDK: v2.5.0 and later.

 If you use SDKs that do not support string user accounts, only integer user IDs can be used in the channel.
- If you change your user IDs into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.

#### 2. Adding remote audio and video statistics

To monitor the audio and video transmission quality during a call or live interactive streaming, this release adds the `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) and [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) classes, to report the audio and video freeze time and freeze rate of the remote user.

This release also adds the `numChannels`, `receivedSampleRate`, and `receivedBitrate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class.

**Improvements**

This release adds a `AgoraConnectionChangedKeepAliveTimeout(14)` member to the `AgoraConnectionChangedReason` parameter of the [connectionChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback. This member indicates a connection state change caused by the timeout of the connection keep-alive between the SDK and Agora's edge server.

**Issues Fixed**

- Unexpected behaviors when calling the `setRemoteSubscribeFallbackOption` method.
- Occasional deadlocks when calling the `MediaIO` methods.

**API Changes**

To improve your experience, we made the following changes to the APIs:

#### Added

- [registerLocalUserAccount](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/registerLocalUserAccount:appId:)
- [joinChannelByUserAccount](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByUserAccount:token:channelId:joinSuccess:)
- [getUserInfoByUid](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUid:withError:)
- [getUserInfoByUserAccount](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getUserInfoByUserAccount:withError:)
- [didRegisteredLocalUser](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRegisteredLocalUser:withUid:)
- [didUpdatedUserInfo](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didUpdatedUserInfo:withUid:)
- The `numChannels`, `receivedSampleRate`, `receivedBitrate`, `totalFrozenTime`, and `frozenRate` members in the [AgoraRtcRemoteAudioStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteAudioStats.html) class
- The `totalFrozenTime` and `frozenRate` members in the [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class

#### Deprecated

- The `lowLatency` member in the [AgoraLiveTranscoding](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) class


## v2.4.1

V2.4.1 is released on Jun 12th, 2019.

**Compatibility changes**

Ensure that you read the following SDK behavior changes if you migrate from an earlier SDK version.

#### 1. Publishing streams to the RTMP

To improve the usability of the RTMP streaming service, v2.4.1 defines the following parameter limits:

| Class **/** Interface  | Parameter Limit                                              |
| ---------------------- | ------------------------------------------------------------ |
| [AgoraLiveTranscoding](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html)        | <li>[videoFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoFramerate): Frame rate (fps) of the CDN live output video stream. The value range is [0, 30], and the default value is 15. Agora adjusts all values over 30 to 30.<li>[videoBitrate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoBitrate): Bitrate (Kbps) of the RTMP live output video stream. The default value is 400. Set this parameter according to the [Video Bitrate Table](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate). If you set a bitrate beyond the proper range, the SDK automatically adapts it to a value within the range.<li>[videoCodecProfile](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoCodecProfile): The video codec profile. Set it as **BASELINE**, **MAIN**, or **HIGH** (default). If you set this parameter to other values, Agora adjusts it to the default value of **HIGH**.<li>[size](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/size): Pixel of the video. The minimum value of size is **16 x 16**.</li> |
| [AgoraImage](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraImage.html)             | url: The maximum length of this parameter is **1024** bytes. |
| [addPublishStreamUrl](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)     | url: The maximum length of this parameter is **1024** bytes. |
| [removePublishStreamUrl](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) | url: The maximum length of this parameter is **1024** bytes. |

This release also adds the  [audioCodecProfile](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) parameter in the `LiveTranscoding` class to set the audio codec profile type. The default type is LC-AAC, which means the low-complexity audio codec profile.

v2.4.1 also adds five error codes to the error parameter in the [streamPublishedWithUrl](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) method for quick troubleshooting.

#### 2. Renaming the receivedFrameRate parameter i
n the RemoteVideoStats class

v2.4.1 renames the `receivedFrameRate` parameter to [rendererOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) in the [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) class to more accurately describe the statistics of the remote video stream.

**New features**

#### 1. Adding media metadata

In live interactive streaming scenarios, the host can send shopping links, digital coupons, and online quizzes to the audience for more diversified live interactive streaming interactions. v2.4.1 adds the [setMediaMetadataSource](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) and the [setMediaMetadataDelegate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) interface and the [AgoraMediaMetadataDataSource](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) and the [AgoraMediaMetadataDelegate](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html) protocol, allowing the host to add metadata to the output video and to send media attached information.

#### 2. State of the local video

v2.4.1 adds the [localVideoStateChange](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback to indicate the local video state. In this callback, the SDK returns the `Stopped`,` Capturing`, `Encoding`, or `Failed` state. When the state is `Failed`, you can use the error code for troubleshooting. This callback indicates whether or not the interruption is caused by capturing or encoding. This release deprecates the `rtcEngineCameraDidReady` and `rtcEngineVideoDidStop` callbacks.

#### 3. State of the RTMP streaming

v2.4.1 adds the [rtmpStreamingChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) callback to indicate the state of the RTMP streaming and help you troubleshoot issues when exceptions occur. In this callback, the SDK returns the `Idle`, `Connecting`, `Runing`, `Recovering`, or `Failure` state. When the state is `Failure`, you can use the error code for troubleshooting. You can still use the `streamPublishedWithUrl` and `streamUnpublishedWithUrl` callbacks, but we do not recommend using them.

#### 4. More reasons for a network connection state change

In the onConnectionStateChanged callback, v2.4.1 adds error codes to the reason parameter to help you troubleshoot issues when exceptions occur. The SDK returns the [connectionChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback whenever the connection state changes. This release also deprecates `AgoraWarningCodeLookupChannelRejected(105)`, `AgoraErrorCodeTokenExpired(109)`, and `AgoraErrorCodeInvalidToken(110)`.

#### 5. State of the local network type 

v2.4.1 adds the [networkTypeChangedToType](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:) callback to indicate the local network type. In this callback, the SDK returns the `Unknown`, `Disconnected`, `Lan`, `Wifi`, `2G`, `3G`, or `4G` type. When the network connection is interrupted, this callback indicates whether or not the interruption is caused by a network type change or poor network conditions.

#### 6. Getting the audio mixing volume

v2.4.1 adds the  [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [getAudioMixingPublishVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods, which respectively gets the audio mixing volume for local playback and remote playback, to help you troubleshoot audio volume related issues.

#### 7. Reporting when the first remote audio frame is received and decoded

To get the more accurate time of the first audio frame from a specified remote user, v2.4.1 adds the [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:) callback to report to the app that the SDK decodes first remote audio. This callback is triggered in either of the following scenarios:

- The remote user joins the channel and sends the audio stream.
- The remote user stops sending the audio stream and re-sends it after 15 seconds.

The difference between the onFirstRemoteAudioDecoded and `onFirstRemoteAudioFrame` callbacks is that the `onFirstRemoteAudioFram`e callback occurs when the SDK receives the first audio packet. It occurs before the `onFirstRemoteAudioDecoded` callback.


**Improvements**

#### 1. Playing multiple online audio effect files simultaneously

v2.4.1 adds the support for playing multiple online audio effect files simultaneously by allowing you to call the [playEffect](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:publish:) method multiple times with the URLs of the online audio effect files.

#### 2. Reporting more statistics

- v2.4.1 adds the [txPacketLossRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) and  [rxPacketLossRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) parameters in the  [AgoraChannelStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html) class. These parameters return the packet loss rate from the local client to the server and vice versa.

- To provide more accurate statistics of the local and remote video, v2.4.1 makes the following changes to the following classes:
  - [AgoraRtcLocalVideoStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html): Adds the  [encoderOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) parameters
  - [AgoraRtcRemoteVideoStats](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html): Adds the [decoderOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) parameter, and renames the receivedFrameRate parameter to the [rendererOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) parameter

#### 3. Image enhancement

v2.4.1 assigns default values to various parameters in the  [AgoraBeautyOptions](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraBeautyOptions.html) class to improve the image enhancement experience. This release also optimizes the image enhancement algorithm. Test results from Agora Lab suggest that the updated algorithm leads to lower GPU and CPU consumption. The power consumption is optimized by 40% - 50%.

#### 4. Miscellaneous

- Improved the sound quality of the GameStreaming audio scenario.
- Reduced the audio and video latency.
- Reduced the SDK package size by 0.5 M.
- Improved the accuracy of the network quality after users change the video bitrate.
- Enabled the audio quality notification callback by default, that is, enabled the [remoteAudioStats](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) callback without calling the `enableAudioVolumeIndication` method.
- Improved the stability of video services.
- Improved the stability of RTMP streaming.

**Issues fixed**

#### Audio

- The audio stream is interrupted by Siri and does not resume. 

#### Video

- Camera exposure issues. 
- The user cannot see the remote video on the smartwatch. 

#### Miscellaneous

- Users still receive the `onNetworkQuality` callback after leaving the channel.
- Occasional crashes.


**API changes**

To improve your experience, we made the following changes to the APIs:

#### Unified the C++ interface for all platforms

v2.4.1 unifies the behavior of the C++ interfaces across different platforms so that you can apply the same code logic on different platforms. v2.4.1 implements the methods of the `RtcEngineParameters` class in the `IRtcEngine` class. Refer to [Agora C++ API Reference for All Platforms home page](https://docs.agora.io/en/Video/API%20Reference/cpp/index.html) for the applicable platforms and considerations of each interface.

#### Added

- [getAudioMixingPlayoutVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPlayoutVolume)
- [getAudioMixingPublishVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPublishVolume)
- [firstRemoteAudioFrameDecodedOfUid](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:)
- [localVideoStateChange](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:)
- [networkTypeChangedToType](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:)
- [rtmpStreamingChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)
- [setMediaMetadataSource](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) 
- [setMediaMetadataDelegate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) 
-  [AgoraMediaMetadataSource](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) 
- [AgoraMediaMetadataDelegate](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html)
- The [audioCodecProfile](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) parameter in the `LiveTranscoding` class
- The [txPacketLossRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) and [rxPacketLossRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) parameters in the `AgoraChannelStats` class
- The [encoderOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) parameters in the `AgoraRtcLocalVideoStats` class
- The [decoderOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) and [rendererOutputFrameRate](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate) (to replace `receivedRemoteRate`) parameters in the `AgoraRtcRemoteVideoStats` class

#### Deprecated

- `enableAudioQualityIndication`
- `rtcEngineCameraDidReady`. Use  AgoraLocalVideoStreamStateCapturing(1) in the [localVideoStateChange](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.
- `rtcEngineVideoDidStop`. Use AgoraLocalVideoStreamStateStopped(0) in the [localVideoStateChange](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.
- The `AgoraWarningCodeLookupChannelRejected(105)` warning code. Use AgoraConnectionChangedRejectedByServer(10) in the [connectionChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeTokenExpired(109)` error code. Use AgoraConnectionChangedTokenExpired(9) in the [connectionChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeInvalidToken(110)` error code. Use  AgoraConnectionChangedInvalidToken(8) in the [connectionChangedToState](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) callback instead.
- The `AgoraErrorCodeStartCamera(1003)` error code. Use AgoraLocalVideoStreamErrorCaptureFailure(4) in the [localVideoStateChange](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localVideoStateChange:error:) callback instead.


## v2.4.0 and Earlier

**v2.4.0**

v2.4.0 is released on April 1, 2019.

#### **Compatibility changes**

- Agora Video SDK for iOS adds a library dependency on `CoreML.framework` in v2.4.0. Ensure that you add this library when integrating the SDK. For details, see *Integrate the SDK*.
- If you integrate the SDK by using CocoaPods，ensure that you run `pod update` in your Terminal before `pod install`. If you prefer to specify the SDK version to obtain the latest release, ensure that you specify it as `'AgoraRtcEngine_iOS', '2.4.0.1'` in the Podfile.

#### **New Features**

##### 1. Image enhancement

In scenarios such as the video chat, interactive streaming, and online education, basic beautification is a popular feature. v2.4.0 adds the [`setBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:) method, allowing you to adjust the contrast, brightness, smoothness and red saturation of an image. For more information, see [Image Enhancement](../../en/Video/image_enhancement_ios.md).

##### 2. Voice changer and voice reverberation

Adding voice changer and reverberation effects in an audio chat room brings much more fun. v2.4.0 adds the [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:) and [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:) methods, allowing you to change your voice or reverberation by choosing from the preset options. See [Adjust the pitch and tone](../../en/Video/voice_changer_apple.md).

##### 3. Tracking the sound position of a remote user 

v2.4.0 adds the [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) and [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) methods. Call the [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) method before joining a channel to enable stereo panning for the remote users, and then you can call the [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) method to track the position of a remote user.

##### 4. Pre-call last-mile network probe test

Conducting a last-mile probe test before joining the channel helps the local user to evaluate or predict the uplink network conditions. v2.4.0 adds the [`startLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:), [`stopLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest), and [`lastmileProbeResult`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:) APIs, allowing you to get the uplink and downlink last-mile network statistics, including the bandwidth, packet loss, jitter, and round-trip time (RTT).

##### 5.Setting the priority of a remote user's stream

v2.4.0 adds the [`setRemoteUserPriority`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:) method for setting the priority of a remote user's media stream. You can use this method with the [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) method. If the fallback function is enabled for a remote stream, the SDK ensures the high-priority user gets the best possible stream quality.

##### 6. State of an audio mixing file 

v2.4.0 adds the [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:) callback to report any change of the audio-mixing file playback state (playback succeeds or fails) and the corresponding reason. This release also adds the warning code 701, which is triggered if the local audio-mixing file does not exist, or if the SDK does not support the file format or cannot access the music file URL when playing the audio-mixing file.

##### 7. Setting the log file size

The SDK has two log files, each with a default size of 512 KB. In case some customers require more than the default size, v2.4.0 adds the [`setLogFileSize`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:) method for setting the log file size (KB).

#### 8. Cloud proxy

Supports the cloud proxy service. See [Use Cloud Proxy](../../en/Video/cloudproxy_native.md) for details.

#### **Improvements**

##### 1. Accuracy of call quality statistics

- v2.4.0 replaces the [`startEchoTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTest:) method with the [`startEchoTestWithInterval`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:) method. The `intervalInSeconds` parameter of `startEchoTestWithIntervals` allows you to set the interval between when you speak and when the recording plays back.
- v2.4.0 adds three parameters to the [`AgoraRtcLocalVideoStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html) class: [`sentTargetBitrate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetBitrate) for setting the target bitrate of the current encoder, [`sentTargetFrameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetFrameRate) for setting the target frame rate, and [`qualityAdaptIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/qualityAdaptIndication) for reporting the quality of the local video since last count.

##### 2. Video encoder preferences

v2.4.0 provides the following options for setting video encoder preferences:

- Setting preferences under limited bandwidth. v2.4.0 adds two parameters to the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) class: [`minFrameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minFrameRate) and [`degradationPreference`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/degradationPreference). You can use these parameters together to set the minimum video encoder frame rate and the video encoding degradation preference under limited bandwidth. For more information, see [Set the Video Profile](../../en/Video/video_profile_apple.md).
- Setting the camera capture preference. v2.4.0 adds the [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:) method, allowing you to set the camera capture preference. You can choose system performance over video quality or vice versa as needed. For more information, see the [API Reference](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration).

##### 3. Core quality improvements

- Reduces the audio delay.
- Improves the video quality and stability. 
- Shortens the time to render the first remote video frame. 

#### **Issues Fixed**

##### Audio

- Calling the `enableLocalAudio` method disconnects all connected Bluetooth devices.
- The SDK does not support audio mixing URLs with Chinese characters.
- The SDK does not return YES after the `pushExternalAudioFrameSampleBuffer` method call succeeds.
- Volume levels of the high-pitch sound are lowered.
- Sounds are occasionally played fast.

##### Video

- If you skip the `renderMode` setting, the video stretches due to a mismatch with the display.
- Video freezes on some lower-end devices.
- It takes too long to render the first received video frame.

##### Miscellaneous

- The user drop-offline time between Android and iOS is not unified.
- The SEI information does not synchronize with the media stream when publishing transcoded streams to the CDN.

#### **API Changes**

To improve your experience, we made the following changes to the APIs:

##### Added

- [`setBeautyEffectOptions`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:)
- [`setLocalVoiceChanger`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`enableSoundPositionIndication`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:)
- [`setRemoteVoicePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 
- [`startLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`setRemoteUserPriority`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:)
- [`startEchoTestWithInterval`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)
- [`setLogFileSize`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)
- [`localAudioMixingStateDidChanged`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)
- [`lastmileProbeResult`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

##### Deprecated

- `startEchoTest`
- `setVideoQualityParameters`

##### Miscellaneous

v2.4.0 changes the type of the [`frameRate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/frameRate) parameter in the [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) class from `enum` to `int`.

**v2.3.3**
v2.3.3 is released on January 24, 2019. 

#### Issues Fixed

Occasional inaccurate statistics returned in the `networkQuality` callback.

**v2.3.2**

v2.3.2 is released on January 16, 2019. 

#### Compatibility changes

Besides the new features and improvements mentioned below, it is worth noting that v2.3.2:

- Improves the SDK's ability to counter packet loss under unreliable network conditions.
- Improves the communication smoothness.
- Reduces video freezes in the `LiveBroadcasting` profile.

Before upgrading your SDK, ensure that the version is:

- Native SDK v1.11 or later.
- Web SDK v2.1 or later.

#### **New Features**

##### 1. Automatic exposure at a point of interest
v2.3.2 adds the following methods and callback to support camera exposure and improve the captured video quality:

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported): Checks whether the device supports camera exposure.
- [`setCameraExposurePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:): Sets the camera exposure position.
- [`cameraExposureDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:): Occurs when the camera exposure area changes.
You can send the touch point coordinates in the view to the SDK for automatic exposure.

##### 2. Video quality in the live interactive streaming

v2.3.2 adds the [`minBitrate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minBitrate) parameter (minimum encoding bitrate) in the [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) method. The SDK automatically adjusts the encoding bitrate to adapt to the network conditions. Using a value greater than the default value forces the video encoder to output high-quality images but may cause more packet loss and hence sacrifice the smoothness of the video transmission. Agora does not recommend changing this value unless you have special requirements for image quality.

##### 3. Independent audio mixing volume adjustments for local playback and remote publishing

v2.3.2 adds the [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) and [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) methods to complement the [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) method, allowing you to independently adjust the audio mixing volume for local playback and remote publishing.

This release also changes the behavior of the [adjustPlaybackSignalVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:) method to control only the voice volume. Therefore, to mute the local audio playback, call both the `adjustPlaybackSignalVolume(0)` and `adjustAudioMixingVolume(0)` methods.

See [Adjust the Volume](../../en/Video/volume_ios.md) for the scenarios and corresponding APIs.

#### **Improvements**

##### 1. Improves the accuracy of the call quality statistics

v2.3.2 deprecates the `audioQualityOfUid` callback and replaces it with the `remoteAudioStats` callback to improve the accuracy of the call quality statistics. The `remoteAudioStats` callback returns parameters such as the audio frame loss rate, end-to-end audio delay, and jitter buffer delay at the receiver, which are more closely linked to the real user experience. In addition, v2.3.2 optimizes the algorithm of the `networkQuality` callback for the uplink and downlink network qualities.

- [`remoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:): Reports the statistics of the remote audio stream from each user/host. This callback replaces the onAudioQuality callback. 
- [`networkQuality`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:): Reports the last mile network quality of each user in the channel.

Agora plans to improve the following callback in subsequent versions:

- [`lastmileQuality`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:): Reports the last mile network quality of the local user before the user joins a channel.

For the list of API methods related to the call quality statistics and on how and when to use them, see Report [In-call Statistics](../../en/Video/in-call_quality_apple.md).

##### 2. New network connection policy

v2.3.2 adds the following API method and callback to get the current network connection state and reason for a connection state change:

- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState): Gets the connection state of the SDK.
- [`connectionChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:): Occurs when the connection state of the SDK to the server changes.

v2.3.2 deprecates the [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) and [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) callbacks.

In the new API method, the network connection states are "disconnected", "connecting", "connected", "reconnecting", and "failed". The SDK triggers the `connectionChangedToState` callback when the network connection state changes. The SDK also triggers the `rtcEngineConnectionDidInterrupted` and `rtcEngineConnectionDidBanned` callbacks under certain circumstances, but Agora does not recommend using them.

##### 3. Improves the call rating system

v2.3.2 changes the rating parameter in the [`rate`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) method to "1 to 5" to encourage more feedback from end-users on the quality of a call or live interactive streaming. Application developers can use this feedback for future product improvement. Agora strongly recommends integrating this method in your application.

##### 4. Improves the audio quality in music scenarios

v2.3.2 adds high-quality audio in scenarios such as music education. In the [`setAudioProfile`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) method, you can set [`AgoraAudioProfile`](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraAudioProfile.html) as MusicHighQuality(4) and [`AgoraAudioScenario`](https://docs.agora.io/en/Video/API%20Reference/oc/Constants/AgoraAudioScenario.html) as GameStreaming(3) to minimize echo and noise while maintaining the quality of the music.

##### 5. Other improvements

- Minimizes packet loss under unreliable network conditions in the `LiveBroadcasting` profile.
- Accelerates the video quality recovery under network congestion.
- Improves the stability in pushing streams.
- Optimizes the API calling threads.
- Improves the performance of the SDK on mid-end and low-end iOS devices.
- Checks the headset and Bluetooth device connection.
- Reduces the audio delay.

#### **Issues Fixed**

The following issues are fixed in v2.3.2.

##### Audio

- A user joins the live interactive streaming with a Bluetooth headset. The audio is not played through the Bluetooth headset when the user leaves the channel and opens another application.
- Crashes when calling the `startAudioMixing` method to play music files.
- A previously disabled microphone becomes enabled when the device connects to a headset. 
- Cannot adjust the volume of the speaker when users change roles, join and leave channels, or a system phone or Siri interrupts.
- Users do not hear any voice for a while when an application switches back from the background. 

##### Video

- Occasional issues when using an external video source.
- The cursor on the remote side is not in the same position as the local side when sharing the desktop.

#### **API Changes**

To improve your experience, we made the following changes to the APIs:

##### Added

- [`isCameraExposurePositionSupported`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/isCameraExposurePositionSupported)
- [`setCameraExposurePosition`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setCameraExposurePosition:)
- [`getConnectionState`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`CameraExposureDidChangedToRect`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:cameraExposureDidChangedToRect:)
- [`remoteAudioStats`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)

##### Deprecated

- [`audioQualityOfUid`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/en/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

**v2.3.1**

v2.3.1 is released on September 28, 2018. 

#### **New Features**

##### Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio streams after joining a channel, this version adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `didMicrophoneEnabled` callback, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.

The difference between this method and `muteLocalAudioStream` is that `enableLocalAudio` does not capture or send any audio stream, while `muteLocalAudioStream` captures but does not send audio streams.


#### **Improvements**

- Reduces the CPU consumption in the audio communication profile on some low-end iOS devices.


#### **Issues Fixed**

- Occasional crashes on some iOS devices when using customized video source functions.
- Occasional crashes on some iOS devices when using customized remote renderer functions.
- Occasional crashes when switching between the front and rear camera.
- `Communication` profile: If one user disables the video and re-enables it during a one-to-one call, it takes a long time for the other user to see the image.
- `LiveBroadcasting` profile: Occasionally, the app freezes and the user cannot leave the channel after switching between the front and rear cameras on some iOS devices.
- `LiveBroadcasting`: Occasionally, two taps are needed for a successful focus on some iOS devices.
- `LiveBroadcasting`: Delay at the client due to incorrect statistics.
- The timestamp in the captured raw video frames does not refresh with the frame.

**v2.3.0**

v2.3.0 is released on August 31, 2018.

#### **Compatibility changes**

-   To support video rotation and enable better quality for the custom video source, this version deprecates the `setVideoProfile` method and uses the `setVideoEncoderConfiguration` method instead to set the video encoding configurations. You can still use the `setVideoProfile` method but we recommend using the `setVideoEncoderConfiguration` method to set the video profile because:
    -   During the live interactive streaming, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
    -   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.
-   An `Accelerate.framework` library is added to the SDK in v2.3.0, which is capable of large-scale mathematical computations and image calculations, optimized for high performance.
-   The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).


#### **New Features**

##### 1. Fallback options for the live interactive streaming under unreliable network conditions.

The audio and video quality of the live interactive streaming deteriorate under unreliable network conditions. To improve the efficiency of the live interactive streaming, the `setLocalPublishFallbackOption` and `setRemoteSubscribFallbackOption` methods are added. These methods allow the SDK to automatically disable the video stream when the network conditions cannot support both audio and video, and enable the video when the network conditions improve. The SDK triggers the `didLocalPublishFallbackToAudioOnly` or `didRemoteSubscribeFallbackToAudioOnly` callback when the stream falls back to audio-only or when the stream switches back to the video.

##### 2. Notifies the user that the token expires in 30 seconds.

The SDK returns the `tokenPrivilegeWillExpire` callback 30 seconds before a token expires to notify the app to renew it. When this callback is received, you need to generate a new token on your server and call the `renewToken` method to pass the newly-generated token to the SDK.

##### 3. Returns user-specific upstream and downstream statistics, including the bitrate, frame rate, packet loss rate, and time delay

The `audioTransportStatsOfUid` and `videoTransportStatsOfUid` callbacks are added to provide user-specific upstream and downstream statistics, including the bitrate, frame rate, and packet loss rate. During a call or live interactive streaming, the SDK triggers these callbacks once every two seconds after the local user receives audio/video packets from a remote user. The callbacks include the user ID, audio bitrate at the receiver, packet loss rate, and time delay (ms).

##### 4. Sets the SDK’s control over an audio session

The SDK and app both have control over the audio session. However, the app may restrict the SDK’s control over an audio session and allow another app or a third-party component to control it by using the `setAudioSessionOperationRestriction` method. You can implement different levels of control by choosing the corresponding restriction. You can call this method before or after joining the channel.

##### 5. Sets the video encoder configurations

To support scenarios with video rotation and enable better quality for the custom video source, this release deprecates the `setVideoProfile` method and uses the `setVideoEncoderConfiguration` method instead to set the video encoding configurations. You can still use the `setVideoProfile` method but Agora recommends using the `setVideoEncoderConfiguration` method to set the video profile because:

-   During the live interactive streaming, users can set the video orientation mode as adaptive, under which the SDK can transfer rotated video frames without cropping them, thus avoiding the “big headshot” or blurry images at the player.
-   In scenarios involving external video sources, the SDK adjusts the width and height of the output video frames based on the inputting video frames, avoiding unnecessary cropping and thereby rendering more image frames at the player.

The <code>AgoraVideoEncoderConfiguration</code> class provides a set of configurable video parameters, including the dimension, frame rate, bitrate, and orientation. For more information on the API, see `Set the Video Encoder Configuration`.

##### 6. Adds support for background image settings in setLiveTranscoding

The `backgroundImage` parameter is added to the `setLiveTranscoding` method allowing you to set the background image in the combined video of the live interactive streaming.

#### **Improvements**

-   Improves the quality for one-on-one voice/video scenarios with optimized latency and smoothness, especially for areas like Southeast Asia, South America, Africa, and the Middle East.
-   Improves the audio encoder efficiency in the live interactive streaming to reduce user traffic while ensuring the call quality.
-   Improves the audio quality during a call or the live interactive streaming using the deep-learning algorithm.


#### **Issues Fixed**

- The users on the Web client cannot see the video sent from the Native client due to codec bugs.
- Excessive increase in the memory usage when multiple delegated hosts start streaming in the channel.
- Occasional video renderer crashes when the app switches to the background on some iOS devices.
- Occasional app crashes on some iOS devices.
- The remote view does not display on some devices.
- Occasional black screens on some iOS devices.
- Occasional ghost images.
- Occasional green lines at the bottom of the video when a user switches from a low stream to a high stream video in the `Communication` profile.
- Crashes after publishing streams from some iOS devices.
- Occasional crashes on some iOS devices.
- Crashes when a user frequently mutes and resumes all sound effects on some iOS devices.
- Excessive increase in the memory usage for the host when the host frequently joins and leaves a channel that has multiple delegated hosts.
- Occasionally, the remote user cannot hear the host when the host switches between `Audience` and `Broadcaster`.
- Occasionally, the settings applied to the background image in live transcoding do not take effect.
- Occasionally on some devices, the video height and width are swapped in the `Communication` profile.
- Occasionally, the `destroy` method does not respond after a user enables the video and joins a channel.
- Occasionally on some iOS devices, a user fails to hear any sound after returning to the channel from a system phone call.
- Occasionally, the audience cannot adjust the channel volume.
- Occasional crashes when a user frequently joins and leaves the channel.
- Occasional crashes when you frequently set the video encoder profile on some iOS devices.
- Occasionally on iOS devices, a remote user cannot send video in a communication channel.
- Occasional failures to capture video of the delegated host when the hosts and the audience frequently change roles.
- Occasionally on some iOS devices, a web user can subscribe to the low-stream video even when the iOS device does not have the dual-stream mode enabled.
- Occasional crashes when calling the `setCameraFocusPositionInPreview` method on some devices.
- Occasional crashes when one of the two hosts mutes or disables the local audio while playing the background music.
- Occasional crashes on the iOS device when the device interoperates with the Web and when a web user frequently joins and leaves a channel.
- A user cannot join a communication channel after frequently changing the video encoder profiles.
- Occasional crashes on some devices when preloading the sound effects.
- Video resolution inconsistencies between the encoder and decoder in the `LiveBroadcasting` profile.
- Occasional video freezes in the `Communication` or `LiveBroadcasting` profile.
- Occasional crashes when calling the `muteRemoteVideoStream` method after joining the channel.
- Occasionally, some iOS devices still receive the video fallback-specific callbacks even when the video fallback option is not enabled.
- Incorrect video orientation on some iOS devices when setting the video profile of an external video source during the live interactive streaming.
- On iOS, the bitrates cannot reach the target values when manually setting the video profile.
- Occasional inter-operational failures between an iOS and a macOS device.
- Occasional crashes on some iOS devices when a user leaves the interactive streaming channel while playing music using a third-party application.
- Occasional crashes on some iOS devices when leaving the channel.
- On iOS, when a host injects a stream to an interactive streaming channel, other hosts can still inject a second stream to the channel.
- Occasional crashes on some iOS devices when the user frequently turns on and off the camera.
- Occasional video freezes when switching from multiple hosts to a single host.
- Occasional inter-operational failures between SIP devices and the SDK.
- Occasional echo issues when using a specific audio card.
- Occasional video delays on some iOS devices.
- The video window size jumps from small to big on some iOS devices.
- Image blurs on some iOS devices when the camera vibrates.
- Failure to adjust the volume on some iOS devices.


#### **API Changes**

To improve your experience, we made the following changes to the APIs:

To avoid adding too many users with the same uid into the RTMP publishing channel, the following API methods are added in v2.3.0:

-   `addUser`
-   `removeUser`


The following API methods are deleted and no longer supported in v2.3.0. Agora provides the Recording SDK for better recording services. 

-   <code>startRecordingService</code>
-   <code>stopRecordingService</code>
-   <code>refreshRecordingServiceStatus</code>


The following deprecated API methods are deleted and no longer supported from v2.3.0:

-   <code>switchView</code>
-   <code>setSpeakerphoneVolume</code>

**v2.2.3**

v2.2.3 is released on July 5, 2018.

#### **Compatibility changes**

The security keys are improved and updated in v2.1.0. If you are using an Agora SDK version below v2.1.0 and wish to migrate to the latest version, see [Token Migration Guide](../../en/Agora%20Platform/token_migration.md).

#### **Issues Fixed**

- Occasional online statistics crashes.
- Occasional crashes during the live interactive streaming.
- Excessive increase in the memory usage when multiple delegated hosts start streaming in the channel.
- Failure to report the uid and volume of the speaker in a channel.
- Unsteady voice volume of the host in the live interactive streaming.
- Occasional video freeze after a view size change.

**v2.2.2**

v2.2.2 is released on June 21, 2018.

#### **Issues Fixed**

- Fixed occasional online statistics crashes.
- Fixed the issue that the media and the signaling services cannot be accessed at the same time on some iOS devices.
- Fixed the issue of failing to report the uid and volume of the speaker in a channel.
- Fixed the issue of occasional video freeze after a view size change.

**v2.2.1**

v2.2.1 is released on May 30, 2018. 

#### **Issues Fixed**

- Occasional crashes on some iOS devices.
- Occasional memory leak on some iOS devices.
- Occasional app crashes when the app starts audio mixing on some iOS devices.


**v2.2.0**

v2.2.0 is released on May 4, 2018. 


#### **New Features**

##### 1. Play the audio effect in the channel

Adds a <code>publish</code> parameter in the <code>playEffect</code> method for the remote user in the channel to hear the audio effect played locally. 
>  If your SDK is upgraded to v2.2 from a previous version, pay attention to the functional changes of this API.

##### 2. Deploy the proxy at the server

We provide a proxy package for enterprise users with corporate firewalls to deploy before accessing our services. 
##### 3. Get the remote video state

Adds the <code>remoteVideoStateChangedOfUid</code> method to get the state of the remote video stream. 

##### 4. Add watermarks on the interactive video streaming

Adds the watermark function for users to add a PNG file to the local or RTMP streaming as a watermark. Adds the <code>addVideoWatermark</code> and <code>clearVideoWatermarks</code> methods to add and delete watermarks in the live interactive streaming. Adds the <code>watermark</code> parameter in the <code>LiveTranscording</code> interface to add watermarks in RTMP streaming. 

#### **Improvements**

##### 1. Audio volume indication

Improves the <code>enableAudioVolumeIndication</code> method. This method once enabled, sends the audio volume indication of the speaker in its callback at set intervals, regardless of whether anyone is speaking in the channel.

##### 2. Network quality detection during a session

To meet the customers’ need for real-time network quality detection in the channel, the <code>onNetworkQuality</code> method improves its data accuracy. 

##### 3. Last mile network quality detection before joining a channel

To test if the customers’ network condition can support audio or video calls before joining the channel, the <code>onLastmileQuality</code> callback changes its detection base from a fixed bitrate to the bitrate set by the customer in <code>videoProfile</code> to improve data accuracy. When the network condition is unknown, the SDK triggers this callback once every 2 seconds. 

##### 4. Audio quality enhancement

Improves the audio quality in scenarios that involve music playback. To achieve high-fidelity music playback, you can set <code>Scenario：AgoraAudioScenarioGameStreaming = 3</code> in the <code>setAudioProfile</code> method. 

##### 5. Bitcode support

Adds support for Bitcode, which enables App optimization and thinning by the App Store. The package size of the Bitcode SDK is 2.5 times that of the normal one.

#### **Issues Fixed**

- Occasional screen display abnormalities when the iOS device sets its video in the landscape mode.
- Occasional screen display abnormalities when a large number of audience members join as the host in the interactive streaming channel.
- Fixed occasional echo issues caused by some iOS device.


**v2.1.3**

v2.1.3 is released on April 19, 2018.

In v2.1.3, Agora updates the bitrate values of the <code>setVideoProfile</code> method in the `LiveBroadcasting` profile. The bitrate values in v2.1.3 stay consistent with those in v2.0. 

#### **Issues Fixed**

-   Block callbacks are occasionally not received if the delegate is not set.
-   NSAssertionHandler appears in external links to the SDK.
-   Occasional recording failures on some phones when a user leaves a channel and turns on the built-in recording device.
-   `LiveBroadcasting` profile: The iOS client cannot see the video from the web client in Windows 10 even after iOS calls the <code>enableWebSdkInteroperability</code> method.
-   Occasional resolution changes after `UIImagePickerController` is used to enable the system camera and switch back to the `LiveBroadcasting` profile.
-   Occasional crashes during a `Communication` or `LiveBroadcasting` session.


#### **Improvements**

Improves the performance of screen sharing by shortening the time interval between which users switch from screen sharing to the normal `Communication` or `LiveBroadcasting` profile.

**v2.1.2**

v2.1.2 is released on April 2, 2018. 

>  If you upgrade the SDK to v2.1.2 from a previous version, the interactive video streaming quality is better than the communication video quality in the same resolutions, resulting in the live interactive video streaming using more bandwidth. 

#### **New Features**

Extends the <code>setVideoProfile</code> method to enable users to manually set the resolution, frame rate, and bitrate of the video. 

#### **Issues Fixed**

-   Occasional crashes on iOS 11.
-   Video freeze in DTX + AAC mode.


**v2.1.1**

v2.1.1 is released on March 16, 2018. 

Agora has identified a critical issue in SDK v2.1. Upgrade to v2.1.1 if you are using Agora SDK v2.1.

**v2.1.0**

v2.1.0 is released on March 7, 2018. 

#### **New Features**

##### 1. Voice Optimization

Adds a scenario for the game chat room to reduce the bandwidth and cancel the noise with the <code>setAudioProfile</code> method.

##### 2. Enhance the audio effect input from the built-in microphone

In an interactive streaming, the host can enhance the local audio effects from the built-in microphone with the <code>setLocalVoiceEqualization</code> and <code>setLocalVoiceReverb</code> methods by implementing the voice equalization and reverberation effects.

##### 3. Online statistics query

Adds RESTful APIs to check the status of the users in the channel, the channel list of a specific company, and whether the user is an audience or a host. For detailed information, see [RESTful API](../../en/Video/dashboard_restful_communication.md).

##### 4. 17-way video

Adds the support of 17-way video in interactive streaming, see:

-   [Start Live interactive streaming](../../en/Video/start_live_ios.md)
-   [Video Conference of 7+ Users](../../en/Video/multi_user_video_apple.md)


##### 5. Video source customization

Supports the default video-capturing features provided by the camera and the customized video source. 

##### 6. Renderer customization

Supports the default functions provided by the renderers to display the local and remote videos to meet your requirements. We provide a set of interfaces for customized renderers. 

##### 7. Injecting an external video stream

Adds the function of injecting an external video stream to an ongoing live interactive streaming, see [Injecting an External Stream to Live Interactive Streaming](../../en/Video/inject_stream_apple.md).

##### 8. Camera focus change

Adds a <code>cameraFocusDidChanged</code> callback to report to the app when the camera focus area changes.

#### **Improvement**

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Improvement</th>
<th>Description</th>
</tr>
<tr><td>Video Freeze Rate</td>
<td>Reduces the video freeze rate in the audience mode and for specific devices.</td>
</tr>
<tr><td>Authentication</td>
<td>Supports a new authentication mechanism. Each legacy Dynamic Key (Channel Key) corresponds to a single privilege (for example, joining a channel), but each token in the new authentication mechanism includes all privileges (for example, joining a channel, hosting in, and stream-pushing).</td>
</tr>
<tr><td>Billing Optimization</td>
<td>Small video resolutions are charged according to the voice-only mode. For example, 16 &times; 16.</td>
</tr>
<tr><td>API Naming Optimization</td>
<td>Modifies a set of names for the API attributes and enumeration values.</td>
</tr>
</tbody>
</table>



#### **Issues Fixed**

- Occasional crashes.
- Occasional blank screens.
- Occasionally, no voice after turning off the microphone.


**v2.0.2**

v2.0.2 was released on December 15, 2017, and fixes the FFmpeg symbol conflict.


**v2.0**

v2.0 is released on December 6, 2017. 

#### New Features

- Adds the <code>setRemoteVideoStreamType</code> and <code>enableDualStreamMode</code> methods in the `Communication` profile to support dual streams.
- Updates the following callbacks for audio mixing and sound effects:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Name</strong></th>
<th>Description</strong></th>
</tr>
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>Removed. Replaced by <code>rtcEngineLocalAudioMixingDidFinish<c/ode>.</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>Added. Notifies the app when the local audio effect playback stops.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidStart</code></td>
<td>Added. Notifies the app when the remote user starts audio mixing.</td>
</tr>
<tr><td><code>rtcEngineRemoteAudioMixingDidFinish</code></td>
<td>Added. Notifies the app when the remote user stops audio mixing.</td>
</tr>
</tbody>
</table>



- Adds the camera management function in the `Communication` and `LiveBroadcasting` profiles by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th><strong>Name</strong></th>
<th><strong>Description</strong></th>
</tr>
<tr><td><code>isCameraZoomSupported</code></td>
<td>Checks whether the device supports camera zoom.</td>
</tr>
<tr><td><code>isCameraTorchSupported</code></td>
<td>Checks whether the device supports camera flash.</td>
</tr>
<tr><td><code>isCameraFocusPositionInReviewSupported</code></td>
<td>Checks whether the device supports camera manual focus.</td>
</tr>
<tr><td><code>isCameraAutoFocusFaceModeSupported</code></td>
<td>Checks whether the device supports auto-face focus.</td>
</tr>
<tr><td><code>setCameraZoomFactor</code></td>
<td>Sets the camera zoom ratio.</td>
</tr>
<tr><td><code>setCameraFocusPositionInPreview</code></td>
<td>Sets the position for manual focus and activates focusing.</td>
</tr>
<tr><td><code>setCameraTorchOn</code></td>
<td>Sets the device to turn on the camera flash.</td>
</tr>
<tr><td><code>setCameraAutoFocusFaceModeEnabled</code></td>
<td>Sets the device to start auto-face focusing.</td>
</tr>
</tbody>
</table>



- Supports the external audio source in the `Communication` and `LiveBroadcasting` profiles by adding the following API methods:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><th>Name</strong></th>
<th>Description</strong></th>
</tr>
<tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
<td>Enables the external audio source.</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>Disables the external audio source.</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>Pushes the external audio frame.</td>
</tr>
</tbody>
</table>



- Provides a set of RESTful APIs to ban a peer user from the server in the `Communication` and `LiveBroadcasting` profiles. Contact [support@agora.io](mailto:support@agora.io) to enable this function, if required.


#### Issues Fixed

Audio routing and Bluetooth issues.

**v1.14**

v1.14 is released on October 20, 2017. 

#### New Features

-   Adds the <code>setAudioProfile</code> method to set the audio parameters and scenarios.
-   Adds the <code>setLocalVoicePitch</code> method to set the local voice pitch.
-   `LiveBroadcasting`: Adds the <code>setInEarMonitoringVolume</code> method to adjust the volume of the in-ear monitor.


#### Improvements

-   Optimizes the audio at high bitrates.
-   `LiveBroadcasting`: The audience can view the host within one second in a single-stream mode (938 ms on average, and 734 ms under good network conditions).
-   Adds the ability to reduce the bandwidth.
    -   Before v1.14: If you muted the audio of a specific user, the network still sent the stream.
    -   Starting from v1.14: If you mute the audio of a specific user, the network will not send the stream of the user to reduce the bandwidth.
-   Accurate control over the bitrate:
    -   Before v1.14: Inaccurate control over the bitrate often caused huge fluctuations, leading to network congestion and higher rates of packet and frame loss. This affected the accuracy of the bandwidth estimation module, especially when the network was under poor conditions.
    -   Starting from v1.14: Accurate control over the bitrate prevents huge fluctuations avoiding network congestion and shortening the transmission latency.


#### Issues Fixed:

Occasional crashes on iOS devices.

**v1.13.1**

v1.13.1 is released on September 28, 2017. 

-   Fixes the issue of unable to adjust the volume in the speaker mode on iOS 11 with iPhone 7 or later.
-   Optimizes the echo issue under certain circumstances.


**v1.13**

v1.13 is released on September 4, 2017. 

#### New Features

-   Adds the function to dynamically enable and disable acquiring the sound card in the live interactive streaming.
-   Adds the function to disable the audio playback.
-   Adds the module map for the SDK, which means that bridging header files are not necessary for Swift projects.
-   Supports the profile configuration for stream-pushing on the client side.
-   Adds the <code>didClientRoleChanged</code> method to report on a user role switch between the host and the audience in the live interactive streaming.
-   Supports the push-stream failure callback on the server side.


#### Improvements:

The video profile is controllable by the software codec.

#### Issues Fixed:

Occasional crashes.

**v1.12**

v1.12 is released on July 25, 2017.

#### New Features:

-   Adds the <code>injectStream</code> method to inject an RTMP stream into the current channel in live interactive streaming.
-   Adds the <code>aes-128-ecb</code> encryption mode in the <code>setEncryptionMode</code> method.
-   Adds the <code>quality</code> parameter in the <code>startAudioRecording</code> method to set the recording audio quality.
-   Adds a set of APIs to manage the audio effect.


#### Improvements:

In the `Communication` profile, we improve the 320 &times; 180 resolution profile:

-   Keeps the video smooth under poor network and equipment conditions.
-   Enhances the image quality to be better than 180p under good network and equipment conditions.


#### Issues Fixed:

Occasional crashes.
