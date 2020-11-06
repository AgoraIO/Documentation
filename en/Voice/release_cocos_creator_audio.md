
---
title: Release Notes
description: 
platform: Cocos Creator
updatedAt: Fri Nov 06 2020 14:37:07 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Cocos Creator SDK.

## Introduction

The Agora Cocos Creator SDK, developed upon the Agora Android SDK and the Agora iOS SDK, supports all functions for the corresponding versions of these SDKs. The Agora Cocos Creator SDK typically supports the following scenarios:

- Voice Call
- Live Interactive Audio Streaming

For the key features included in each scenario, see [Agora Voice Call Overview](../../en/Voice/product_voice.md) and [Agora Live Interactive Audio Streaming Overview](../../en/Voice/product_live_audio.md).

## v3.1.2

v3.1.2 was released on October 30, 2020.

**Main features**

#### 1. Mobile platform support

This release supports iOS and Android platforms.

#### 2. Interoperability with the Agora Web SDK

This release enables interoperability with the Agora Web SDK by default.

#### 3. Encryption

This release supports encrypting audio streams. The following table shows the encryption libraries for the Android and iOS platforms. If you do not intend to use this function, you can remove the encryption libraries to decrease the SDK size.

| Platform | Encryption libraries             |
| :------- | :------------------------------- |
| Android  | `libagora-crypto.so`             |
| iOS      | `AgoraRtcCryptoLoader.framework` |

#### 4. Cloud proxy

This release supports the cloud proxy service. See [Use Cloud Proxy](../../en/Voice/cloudproxy_native.md) for details.

**Reference**

See the following documentation to quickly integrate the SDK and implement real-time voice communication in your project.

- [Start a Voice Call](../../en/Voice/start_call_audio_cocos_creator.md) or [Start Live Interactive Audio Streaming](../../en/Voice/start_live_audio_cocos_creator.md)
- [API Reference](https://docs.agora.io/en/Voice/API%20Reference/cocos_creator_voice/index.html)

Agora also provides an open-source [Voice-Call-for-Mobile-Gaming](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/master/Basic-Voice-Call-for-Gaming/Hello-CocosCreator-Voice-Agora) on GitHub.
