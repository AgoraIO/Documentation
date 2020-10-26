
---
title: Adjust the Volume
description: How to adjust volume on iOS
platform: iOS
updatedAt: Mon Oct 26 2020 04:45:01 GMT+0800 (CST)
---
# Adjust the Volume
## Introduction

The Agora RTC SDK enables you to manage the volume of the recorded audio or of the audio playback according to your actual scenario. For example, to mute a remote user in a one-to-one call, you can set the audio playback volume as 0.

This article provides the APIs and additional information relating to audio recording, audio mixing, and audio playback volume settings.

![](https://web-cdn.agora.io/docs-files/1578885967798)

## Sample project

We provide an open-source [JoinChannelAudio](https://github.com/AgoraIO/API-Examples/blob/master/iOS/APIExample/Examples/Basic/JoinChannelAudio/JoinChannelAudio.swift) sample project that implements adjusting the recording, playback, and ear-monitoring volume on GitHub. You can download the sample project and view the source code.

## Implementation
Before adjusting the audio volume, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Audio%20Broadcast/start_call_ios.md) or [Start Live Interactive Streaming](../../en/Audio%20Broadcast/start_live_ios.md).

### Adjust the recording volume

**Recording** is the process of sampling audio by a recording device and transmitting the recorded signal to the sender. To adjust the recording volume, you can **set the volume of the recorded signal**.

![](https://web-cdn.agora.io/docs-files/1578020909500)

Call `adjustRecordingSignalVolume` to set the volume of the recorded signal.
The `volume` parameter represents the audio level of the recorded signal, ranging between 0 and 400:
- 0: Mute.
- 100: (Default) The original volume.
- 400: Four times the original volume (amplifying the audio signals by four times).

#### Sample code

```swift
// Swift
// Sets the volume of the recorded signal as 50% of the original volume.
agoraKit.adjustRecordingSignalVolume(50)
```

```objective-c
// Objective-C
// Sets the volume of the recording signal as 50% of the original volume.
[agoraKit adjustRecordingSignalVolume: 50];
```

#### API reference

- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustRecordingSignalVolume:)

### Adjust the playback volume

**Playback** is the process of playing the received audio signal on the local playback device. To adjust the playback volume, you can **set the volume of the audio signal**.

![](https://web-cdn.agora.io/docs-files/1578887639765)

You can use `adjustPlaybackSignalVolume` or `adjustUserPlaybackSignalVolume` to set the volume of the audio signal.
- `adjustPlaybackSignalVolume`：
  - Universally sets the playback audio level of all remote users after audio mixing.
  - The `volume` parameter represents the playback audio level, ranging between 0 and 400. 
- `adjustUserPlaybackSignalVolume`：
  - Adjusts the playback audio level of a specified remote user after audio mixing. Call this method as many times as necessary to adjust the playback volume of different remote users, or to repeatedly adjust the playback volume of the same remote user.
  - The `volume` parameter represents the playback audio level, ranging between 0 and 100. 

<div class="alert note"><li>As of v2.3.2, to mute the local audio playback, you must call both adjustPlaybackSignalVolume and adjustAudioMixingPlayoutVolume, and set the volume parameter as 0.<li>Call adjustUserPlaybackSignalVolume after joining a channel.</li></div>

#### Sample code

```swift
// Swift
// Sets the volume of the local playback of all remote users as 50% of the original volume.
agoraKit.adjustPlaybackSignalVolume(50)
// Sets the volume of the local playback of a specified remote user as 50% of the original volume.
agoraKit.adjustUserPlaybackSignalVolume(uid, volume: 50)
```

```objective-c
// Objective-C
// Sets the volume of the local playback of all remote users as 50% of the original volume.
[agoraKit adjustPlaybackSignalVolume: 50];
// Sets the volume of the local playback of a specified remote user as 50% of the original volume.
[agoraKit adjustUserPlaybackSignalVolume: uid, volume: 50];
```

#### API reference

- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustPlaybackSignalVolume:)
- [`adjustUserPlaybackSignalVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustUserPlaybackSignalVolume:volume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)

### Adjust the in-ear monitor volume

In audio recording, mixing, and playback, you can call `setInEarMonitoringVolume` to adjust the volume of the in-ear monitor.

![](https://web-cdn.agora.io/docs-files/1578887697061)

The `volume` parameter represents the volume of the in-ear monitoring, ranging between 0 and 100.
- 0: Mute.
- 100: (Default) The original volume.

<div class="alert note">Call enableInEarMonitoring(true) before calling this method.</div>

#### Sample code

```swift
// Swift
// Enables in-ear monitoring.
agoraKit.enableInearMonitoring(true)
// Sets the in-ear monitor volume as 50% of the original volume.
agoraKit.setInEarMonitoringVolume(50)
```

```objective-c
// Objective-C
// Enables in-ear monitoring.
[agoraKit enableInEarMonitoring(true)];
// Sets the in-ear monitor volume as 50% of the original volume.
[agoraKit setInEarMonitoringVolume: 50];
```

#### API reference

- [`enableInEarMonitoring`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableInEarMonitoring:)
- [`setInEarMonitoringVolume`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setInEarMonitoringVolume:)

### Get the information of the loudest speaker (callback)

When recording, mixing, or playing audio, you can use the following methods to get the data of the loudest speaker in the channel.

-  Reports users with the highest peak volumes. The `reportAudioVolumeIndicationOfSpeakers` callback reports the user IDs the corresponding volumes of the currently loudest speakers in the channel.

  <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>

Sample code

```swift
// Swift
// Gets the the user IDs of the users with the highest peak volume, the corresponding volumes, as well as whether the local user is speaking.
// @param speakers is an array containing the user IDs and volumes of the local and the remote users. The volume parameter ranges between 0 and 255.
// @param totalVolume refers to the total volume after audio mixing, ranging between 0 and 255.
func rtcEngine(_ engine: AgoraRtcEngineKit, reportAudioVolumeIndicationOfSpeakers speakers:
[AgoraRtcAudioVolumeInfo], totalVolume: Int) {
}
```

```objective-c
// Objective-C
// Gets the the user IDs of the users with the highest peak volume, the corresponding volumes, as well as whether the local user is speaking.
// @param speakers is an array containing the user IDs and volumes of the local and the remote users. The volume parameter ranges between 0 and 255.
// @param totalVolume refers to the total volume after audio mixing, ranging between 0 and 255.
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine reportAudioVolumeIndicationOfSpeakers:(NSArray<AgoraRtcAudioVolumeInfo*> *_Nonnull)speakers totalVolume:(NSInteger)totalVolume {
}
```

- Reports the user with the highest average volume. The `activeSpeaker` callback reports the user ID with the highest average volume during a certain period of time.

  <div class="alert note">You must call enableAudioVolumeIndication to be able to receive this callback.</div>

Sample code

```swift
// Swift
// Gets the user ID of the user with the highest average volume during a certain period of time. A uid of 0 indicates the local user.
func rtcEngine(_ engine: AgoraRtcEngineKit, activeSpeaker speakerUid: UInt) {
}
```

```objective-c
// Objective-C
// Gets the user ID of the user with the highest average volume during a certain period of time. A uid of 0 indicates the local user.
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine activeSpeaker:(NSUInteger)speakerUid {
}
```

#### API reference

- [`reportAudioVolumeIndicationOfSpeakers`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:reportAudioVolumeIndicationOfSpeakers:totalVolume:4)
- [`activeSpeaker`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:activeSpeaker:)

## Considerations

Setting the audio level too high may cause audio distortion on some devices.

## Reference

When adjusting the audio volume, you can also refer to the following articles:

- [How to solve the problem of low volume?](https://docs.agora.io/en/faq/audio_low)
