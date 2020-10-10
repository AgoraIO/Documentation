
---
title: 播放音频文件
description: How to play audio effect files and enable audio mixing
platform: iOS,macOS
updatedAt: Sat Oct 10 2020 04:36:42 GMT+0800 (CST)
---
# 播放音频文件
## 功能描述

在通话或直播过程中，除了用户自己说话的声音，有时候需要播放自定义的声音或者音乐文件并且让频道内的其他人也听到，比如需要给游戏添加音效，或者需要播放背景音乐等，Agora 提供以下两组方法可以满足播放音效和音乐文件的需求。

开始前请确保已在你的项目中实现基本的实时音视频功能。详见快速开始文档：
- iOS: [实现音视频通话](../../cn/Video/start_call_ios.md)/[实现互动直播](../../cn/Video/start_live_ios.md)
- macOS: [实现音视频通话](../../cn/Video/start_call_mac.md)/[实现互动直播](../../cn/Video/start_live_mac.md)

## 示例项目

我们在 GitHub 上提供已实现播放混音和音效文件功能的开源示例项目。你可以下载体验并参考源代码。

- iOS: [AudioMixing](https://github.com/AgoraIO/API-Examples/blob/master/iOS/APIExample/Examples/Advanced/AudioMixing/AudioMixing.swift)
- macOS: [AudioMixing](https://github.com/AgoraIO/API-Examples/blob/master/macOS/APIExample/Examples/Advanced/AudioMixing/AudioMixing.swift)

## 播放音效文件

音效通常指持续很短的音频。播放音效文件方法主要用来播放短小的氛围音，比如鼓掌、游戏子弹撞击声音等，可以多个音效叠加播放，且音效文件可以预加载以提高性能。
音效由音频文件路径指定，soundId 为自行设定的音效 ID，需保证唯一性。SDK 并不强制如何定义 sound id，保证每个音效有唯一的识别即可。一般的做法有自增 id，使用音效文件名的 hashCode 等。

### 实现方法

参考如下步骤，在你的项目中实现播放音效文件：

1. 在加入频道前调用 `preloadEffect` 方法预加载音效文件，可以多次调用该方法加载多个音效文件。
2. 加入频道后调用 `playEffect` 方法播放音效文件，可以多次调用该方法同时播放多个音效。我们建议最多同时播放三个音效文件。

在开始播放音效后你还可以调用其他音效相关的方法实现更多功能，包括暂停播放音效、设置音效音量、释放预加载的音效等。

```swift
// Swift
// 预加载音效（推荐），需注意音效文件的大小，并在加入频道前完成加载
// 仅支持 mp3，aac，m4a，3gp，wav 格式
// 开发者可能需要额外记录 id 与文件路径的关联关系，用来播放和停止音效
let soundId = 1
let filePath = "your filepath"

// 可以加载多个音效
agoraKit.preloadEffect(soundId, filePath: filePath)

// 播放音效
let soundId = 1                 // 要播放的音效 id 
let filePath = "your filepath"  // 播放文件的路径
let loopCount = 1               // 播放次数，-1 代表无限循环
let pitch = 1                   // 音效的音调
let pan = 1                     // 音效的空间位置，0表示正前方
let gain = 100                    // 音量，取值 0 ~ 100， 100 代表原始音量
let publish = true              // 是否令远端也能听到音效的声音
agoraKit.playEffect(Int32(soundId), filePath: filePath, loopCount: Int32(loopCount), pitch: pitch, pan: pan, gain: gain, publish: publish)

// 暂停所有音效播放
agoraKit.pauseAllEffects()

// 获取音效的音量，范围为 0 ~ 100
let volume = agoraKit.getEffectsVolume()

// 保证所有音效文件的播放音量在原始音量的 80% 以上
volume = volume < 80 ? 80 : volume
agoraKit.setEffectsVolume(volume)

// 设置指定音效文件的播放音量为原始音量的 50%
agoraKit.setVolumeOfEffect(soundId:"1", 50)

// 继续播放暂停的音效
agoraKit.resumeAllEffects()

// 停止所有音效
agoraKit.stopAllEffects()
```

```objective-c
// Objective-C
// 预加载音效（推荐），需注意音效文件的大小，并在加入频道前完成加载
// 仅支持 mp3，aac，m4a，3gp，wav 格式
// 开发者可能需要额外记录 id 与文件路径的关联关系，用来播放和停止音效
int soundId = 1;
NSString *filePath = "your filepath";

// 可以加载多个音效
[agoraKit preloadEffect: soundId filePath: filePath];

// 播放音效
int soundId = 1;
NSString *filePath = "your filepath";
int loopCount = 1;
double pitch = 1;
double pan = 1;
double gain = 100;
BOOL publish = true;

[agoraKit playEffect: soundId filePath: filePath loopCount: loopCount pitch: pitch pan: pan gain: gain publish: publish];

// 暂停所有音效播放
[agoraKit pauseAllEffects];

// 获取音效的音量，范围为 0 ~ 100
int volume = [agoraKit getEffectsVolume];

// 保证所有音效文件的播放音量在原始音量的 80% 以上
volume = volume < 80 ? 80 : volume;
[agoraKit setEffectsVolume: volume];

// 设置指定音效文件的播放音量为原始音量的 50%
[agoraKit setVolumeOfEffect: soundId:@"1" volume:50];

// 继续播放暂停的音效
[agoraKit resumeAllEffects];

// 停止所有音效
[agoraKit stopAllEffects];
```

### API 参考

- [`playEffect`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:)
- [`preloadEffect`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/preloadEffect:filePath:)
- [`pauseAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAllEffects)
- [`getEffectsVolume`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getEffectsVolume)
- [`setEffectsVolume`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [`setVolumeOfEffect`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVolumeOfEffect:withVolume:)
- [`resumeAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAllEffects)
- [`stopAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAllEffects)

### 开发注意事项

- 预加载不是一个必须的步骤，一般来说为了提高性能或者需要反复播放某个特定的音效的时候，我们建议使用预加载。但如果音效文件比较大，不建议预加载。
- 以上方法都有返回值，返回值小于 0 表示方法调用失败。

## 音乐混音

混音是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。混音方法主要用来播放比较长的背景音，比如直播的时候播放的音乐，同时只可以有一个文件播放。如果在混音播放第一个文件的过程中播放第二个文件，会自动停止第一个文件的播放。
Agora 混音功能支持如下设置：

- 混音或替换： 混音指的是音乐文件的音频流跟麦克风采集的音频流进行混音（叠加）并编码发送给对方；替换指的是麦克风采集的音频被音乐文件的音频流替换掉，对方只能听见音乐播放。
- 循环：可以设置是否循环播放混音文件，以及循环次数。
- 调节音量：可以同时或分别调节音乐文件在本地和远端的播放音量。
- 调节音调：可以分别调节本地人声的音调和音乐文件的音调。

### 实现方法

```swift
// Swift
// loopback 为 true 只有本地可以听到混音或替换后的音频流; 为 false 本地和对方都可以听到混音或替换后的音频流
// replace 为 true 只推送设置的本地音频文件或者线上音频文件; 不传输麦克风收录的音频, 为 false 音频文件内容将会和麦克风采集的音频流进行混音
// cycle 为 -1 代表永久循环；其它 >0 的整数表示预设混音播放的循环次数
let filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3"
let loopback = false
let replace = false 
let cycle = 1 
  
// 开始播放混音
agoraKit.startAudioMixing(filePath, loopback: loopback, replace: replace, cycle: cycle)

// 设置混音音量。将本地及远端用户听到的音乐文件音量设置为原始音量的 50%。
agoraKit.adjustAudioMixingVolume(50)
```

```objective-c
// Objective-C
// loopback 为 YES 只有本地可以听到混音或替换后的音频流; 为 NO 本地和对方都可以听到混音或替换后的音频流
// replace 为 true 只推送设置的本地音频文件或者线上音频文件; 不传输麦克风收录的音频, 为 false 音频文件内容将会和麦克风采集的音频流进行混音
// cycle 为 -1 代表永久循环；其它 >0 的整数表示预设混音播放的循环次数
NSString *filePath = @"http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";
BOOL loopback = NO;
BOOL replace = NO;
NSInteger cycle = 1;

// 开始播放混音
[agoraKit startAudioMixing: filePath loopback: loopback replace: replace cycle: cycle];

// 设置混音音量。将本地及远端用户听到的音乐文件音量设置为原始音量的 50%
[agoraKit adjustAudioMixingVolume: 50];
```

你也可以通过 `adjustAudioMixingPlayoutVolume` 方法和 `adjustAudioMixingPublishVolume` 方法分别调节本地用户和远端用户听到的混音音量。其中 `volume` 的取值范围为 [0, 100]。

```swift
// Swift
// 将远端用户听到的音乐文件音量设置为原始音量的 50%
agoraKit.adjustAudioMixingPublishVolume(50)
// 将本地用户听到的音乐文件音量设置为原始音量的 50%
agoraKit.adjustAudioMixingPlayoutVolume(50)
```

```objective-c
// Objective-C
// 将远端用户听到的音乐文件音量设置为原始音量的 50%
[agoraKit adjustAudioMixingPublishVolume: 50];
// 将本地用户听到的音乐文件音量设置为原始音量的 50%
[agoraKit adjustAudioMixingPlayoutVolume: 50];
```

<div class="alert note">adjustAudioMixingPlayoutVolume 方法和 adjustAudioMixingPublishVolume 方法要在加入频道后调用。</div>

### API 参考

- [`startAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioMixing:loopback:replace:cycle:)
- [`stopAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioMixing)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`setLocalVoicePitch`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoicePitch:)
- [`setAudioMixingPitch`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPitch:)
- [`pauseAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAudioMixing)
- [`resumeAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAudioMixing)
- [`getAudioMixingDuration`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingDuration)
- [`getAudioMixingCurrentPosition`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingCurrentPosition)
- [`setAudioMixingPosition`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioMixingPosition:)

### 开发注意事项

- 以上方法都有返回值，返回值小于 0 表示方法调用失败。
- 在频道内调用混音方法，否则会有潜在问题。
