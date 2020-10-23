
---
title: Agora Cloud Recording RESTful API
description: Cloud recording restful api reference
platform: All Platforms
updatedAt: Thu Oct 22 2020 11:35:53 GMT+0800 (CST)
---
# Agora Cloud Recording RESTful API
This article contains detailed help for the Cloud Recording RESTful APIs.

> You can also visit our interactive API documentation [Cloud Recording RESTful API](https://docs.agora.io/en/cloud-recording/restfulapi/). It contains detailed help for each Cloud Recording RESTful API and its parameters, and provides the **Try it out** function which allows you to send RESTful API requests and receive responses directly on the web page.

![](https://web-cdn.agora.io/docs-files/1585032370986)


## <a name="auth"></a>Authentication

The RESTful APIs only support HTTPS. Before using the Agora RESTful API, you need to pass the [basic HTTP authentication](https://docs.agora.io/en/faq/restful_authentication).

## Data format

All requests are sent to the host: `api.agora.io`.

- Request: See the examples in the following APIs.
- Response: The response content is in JSON format.

<div class="alert warning">All the request URLs and request bodies are case sensitive.</div>

## Call sequence

Use the RESTful APIs in the following steps:

1. Call the [`acquire`](#acquire) method to request a resource ID for cloud recording.
2. Call the  [`start`](#start) method within five minutes after getting the resource ID to start the recording.
3. Call the [`stop`](#stop) method to stop the recording.

During the recording, you can call the [`query`](#query) method to check the recording status.

See [Sample code](../../en/cloud-recording/cloud_recording_rest.md) for an example of using the RESTful APIs.


## <a name="acquire"></a>Gets the recording resource

Before starting a cloud recording, you need to call this method to get a resource ID.

> One resource ID can only be used for one recording session.

- Method:POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/acquire

> The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.

If this method call succeeds, you get a resource ID (`resourceId`) from the HTTP response body. The resource ID is valid for five minutes, so you need to [start recording](#start) with this resource ID within five minutes.

### Parameters

The following parameter is required in the URL.

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| `appid`   | String | The [App ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#appid) used in the channel to be recorded. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | The name of the channel to be recorded.                      |
| `uid`           | String | A string that contains the UID of the recording client, for example `"527841"`. The UID needs to meet the following requirements: <li>It is a 32-bit unsigned integer within the range between 1 and (2<sup>32</sup>-1).</li><li><b><em>It is unique and does not clash with any existing UID in the channel. </em></b></li><li>It should not be a string. Ensure that all UIDs in the channel are integers.</li> |
| `clientRequest` | JSON   | A client request including the `resourceExpiredHour` field. `resourceExpiredHour` is an integer, which sets the time limit (in hours) for the Cloud Recording RESTful API calls. It must be between `1` and `720`, the default value being `72`. The time limit starts from when you successfully start a cloud recording and get `sid`(the recording ID ). When you exceed the time limit, you can no longer call `query`, `updateLayout`, and `stop`. |

### An HTTP request example of `acquire`

- The request URL is `https://api.agora.io/v1/apps/<yourappid>/cloud_recording/acquire`.
- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:

```json
{
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
      "resourceExpiredHour":  24
   }
}
```

### A response example of `acquire`

```json
"Code": 200,
"Body":
{
 "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.

## <a name="start"></a>Starts cloud recording

After getting a resource ID, call `start` to begin cloud recording. The Cloud Recording service does the following after you call `start`:

1. [Subscribes](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#sub) to the audio and video streams in the channel according to `recordingConfig`.
2. Processes the subscribed media streams, such as generating recorded files in the specified file format, taking video screenshots, or uploading recorded files to an extension service.
3. Uploads the recorded files or screenshots to your third-party cloud storage according to `storageConfig`.

The HTTP method and endpoint of `start`:

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/mode/\<mode\>/start

> The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The App ID used in the channel to be recorded.               |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `mode`       | String | The recording mode. Supports individual mode (`individual`) and composite mode (`mix`). Composite mode is the default mode. In individual mode, Agora Cloud Recording records the audio and video as separate files for each UID in a channel. In composite mode, Agora Cloud Recording generates a single mixed audio and video file for all UIDs in a channel. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | The name of the channel to be recorded.                      |
| `uid`           | String | A string containing the UID of the recording client. Must be the same `uid` used in the [`acquire`](#acquire) method. |
| `clientRequest` | JSON   | A specific client request that requires the following parameters: <li>`token`: (Optional) String. The [dynamic key](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#token) used for the channel to record. Ensure that you set this parameter if App Certificate is enabled for your application. See [Set up Authentication](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms).</li><li>[`recordingConfig`](#recordingConfig): JSON. Configurations for subscribing to media streams.</li><li>[`recordingFileConfig`](#recordingFileConfig): (Optional) JSON. Configurations for the recorded files.</li><li>[`snapshotConfig`](#snapshotConfig): (Optional) JSON. Video screenshot configurations.</li><li>[`storageConfig`](#storageConfig): JSON. Third-party cloud storage configurations.</li><li>[`extensionServiceConfig`](#extensionServiceConfig): (Optional) JSON. Configurations for third-party extension services. Currently supports ApsaraVideo for VoD only.</li> |

#### Requirements for parameters in ` clientRequest`:

- You must configure the stream subscription.
- You can choose to configure either recorded files or video screenshots at one time.
- Each extension service has specific compatibility requirements. See the table below for details:

| Parameter                                     | Configuration                                | Compatibility                                                | Recording mode          |
| :-------------------------------------------- | :------------------------------------------- | :----------------------------------------------------------- | :---------------------- |
| [`recordingConfig`](#recordingConfig)         | Stream subscription                          | Compatible with all other settings                           | Individual or composite |
| [`recordingFileConfig`](#recordingFileConfig) | Recorded files                               | Cannot be set with `snapshotConfig` simultaneously           | Individual or composite |
| [`snapshotConfig`](#snapshotConfig)           | Video screenshots                            | Cannot be set with `recordingFileConfig` or `aliyun_vod_service` simultaneously | Individual              |
| [`extensionServiceConfig`](#extensionServiceConfig)                      | ApsaraVideo for VoD （`aliyun_vod_service`） | Cannot be set with `snapshotConfig` or `storageConfig` simultaneously | Composite               |
| [`storageConfig`](#storageConfig)             | Third-party cloud storage                    | Cannot be set with `aliyun_vod_service` simultaneously       | Individual or composite |


#### <a name="recordingConfig"></a>Recording configuration

`recordingConfig` is a JSON object for configuring how the Cloud Recording service subscribes to the media streams in the channel. `recordingConfig` has the following fields:

- `channelType`: Number. The channel profile. Agora Cloud Recording must use the same channel profile as the Agora RTC Native/Web SDK. Otherwise, issues may occur.
  - `0`: (Default) Communication profile. 
  - `1`: Live broadcast profile.
- `streamTypes`: (Optional) Number. The type of the media stream to subscribe to:
  - `0`: Subscribes to audio streams only.
  - `1`: Subscribes to video streams only.
  - `2`: (Default) Subscribes to both audio and video streams.
- `decryptionMode`: (Optional) Number. The decryption mode that is the same as the encryption mode set by the Agora Native/Web SDK. When the channel is encrypted, Agora Cloud Recording uses `decryptionMode` to enable the built-in decryption function. 
  - `0`: (Default) None.
  - `1`: AES-128, XTS mode.
  - `2`: AES-128, ECB mode.
  - `3`: AES-256, XTS mode.
- `secret`: (Optional) String. The decryption password when decryption mode is enabled. 
- `audioProfile`: (Optional) Number. The profile of the output audio stream, including the sample rate, bitrate, encoding mode, and the number of channels. You cannot set this parameter in individual recording mode.
  - `0`: (Default) Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 48 Kbps.
  - `1`: Sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.
  - `2`: Sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.
- `videoStreamType`: (Optional) Number. The type of the video stream to subscribe to.
  - `0`: (Default) Subscribes to the high-quality stream.
  - `1`: Subscribes to the low-quality stream.
- `maxIdleTime`: (Optional) Number. Agora Cloud Recording automatically stops recording and leaves the channel when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. The value range is from 5 to 2<sup>32</sup>-1. Once the recording stops, the recording service generates new recorded files if you call `start` for the second time.

<div class="alert note"><ul><li>In a communication channel, the recording service does not recognize a channel as an idle channel, so long as the channel has users, regardless of whether they send stream or not.</li><li>If a live broadcast channel has an audience without a host for a set time (<code>maxIdleTime</code>), the recording service automatically stops and leaves the channel.</li></div>

- `transcodingConfig`: (Optional) JSON. The video transcoding configuration. You cannot set this parameter in individual recording mode. If you set this parameter, ensure that you set `width`, `height`, `fps`, and `bitrate`. Refer to [Set the Video Profile](../../en/cloud-recording/recording_video_profile.md) for detailed information about setting `transcodingConfig`.
  - `width`: (Mandatory) Number. The width of the mixed video (pixels). The default value is 360. `width` should not exceed 1920, and `width`*`height` should not exceed 1920 * 1080; otherwise, an error occurs.
  - `height`: (Mandatory) Number. The height of the mixed video (pixels). The default value is 640. `height` should not exceed 1920, and `width`*`height`  should not exceed 1920 * 1080; otherwise, an error occurs.
  - `fps`: (Mandatory) Number. The video frame rate (fps). The default value is 15.
  - `bitrate`: (Mandatory) Number. The video bitrate (Kbps). The default value is 500.
  - `maxResolutionUid`: (Optional) String. When `mixedVideoLayout` is set as `2` (vertical layout), you can specify the UID of the large video window by this parameter.
  - `backgroundColor`: (Optional) String. The background color of the canvas (the display window or screen) in RGB hex value. The string starts with a "#". The default value is `"#000000"`, the black color.
  - `mixedVideoLayout`: (Optional) Number. The video mixing layout. 0, 1, and 2 are the [predefined layouts](../../en/cloud-recording/cloud_recording_layout.md). If you set this parameter as 3, you need to set the layout by the `layoutConfig` parameter.
    - `0`: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
    - `1`: Best fit layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
    - `2`: Vertical layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.
    - `3`: Customized layout. Set the `layoutConfig` parameter to customize the layout.
  - `layoutConfig`: (Optional) JSONArray. An array of the configuration of each user's region. Supports 17 users at most. Each user's region configuration is a JSON object with the following parameters:
    - `uid`: (Optional) String. The string contains the UID of the user displaying the video in the region. If this parameter is not specified, the configurations apply in the order of the users joining the channel.
    - `x_axis`: (Mandatory) Float. The relative horizontal position of the top-left corner of the region. The value is between 0.0 (leftmost) and 1.0 (rightmost), and is accurate to six decimal places.
    - `y_axis`: (Mandatory) Float. The relative vertical position of the top-left corner of the region. The value is between 0.0 (top) and 1.0 (bottom), and is accurate to six decimal places.
    - `width`: (Mandatory) Float. The relative width of the region. The value is between 0.0 and 1.0, and is accurate to six decimal places.
    - `height`: (Mandatory) Float. The relative height of the region. The value is between 0.0 and 1.0, and is accurate to six decimal places.
    - `alpha`: (Optional) Float. The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque), and is accurate to six decimal places. The default value is 1.0.
    - `render_mode`: (Optional) Number. The video display mode:
      - `0`: (Default) Cropped mode. Uniformly scales the video until it fills the visible boundaries (cropped). One dimension of the video may have clipped contents.
      - `1`: Fit mode. Uniformly scales the video until one of its dimension fits the boundary (zoomed to fit). Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

- `subscribeAudioUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose audio you want to subscribe. The length of the array should not exceed 32 UIDs. Agora recommends that you do not set the array as empty. Once you set the parameter, do not set `streamTypes` in `recordingConfig` as `1`. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
	
- `unSubscribeAudioUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose audio you do not want to subscribe. Once you set this parameter, the recording service subscribes to the audio of all UIDs except the specified ones. The length of the array should not exceed 32 UIDs. Agora recommends that you do not set the array as empty. Once you set the parameter, do not set `streamTypes` in `recordingConfig` as `1`. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
	
- `subscribeVideoUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose video you want to subscribe. The length of the array should not exceed 32 UIDs. Agora recommends that you do not set the array as empty. Once you set the parameter, do not set `streamTypes` in `recordingConfig` as `0`. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
	
- `unSubscribeVideoUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose video you do not want to subscribe. Once you set this parameter, the recording service subscribes to the video of all UIDs except the specified ones. The length of the array should not exceed 32 UIDs. Agora recommends that you do not set the array as empty. Once you set the parameter, do not set `streamTypes` in `recordingConfig` as `0`. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
	
<div class="alert note">
If you set up a subscription list for audio, but not for video, then Agora Cloud Recording will not subscribe to any video streams. <br>If you set up a subscription list for video, but not for audio, then Agora Cloud Recording will not subscribe to any audio streams.</div>

- `subscribeUidGroup`: (Optional) Number. The estimated maximum number of subscribed users. **You must set this parameter in individual mode.** For example, if `subscribeVideoUids` is `["100","101","102"]` and `subscribeAudioUids` is `["101","102","103"]`, the number of subscribed users is 4.

  - `0`: 1 to 2 UIDs
  - `1`: 3 to 7 UIDs
  - `2`: 8 to 12 UIDs
  - `3`: 13 to 17 UIDs

#### <a name="recordingFileConfig"></a>Configurations for the recorded files

`recordingFileConfig` is a JSON Object for configuring recorded files. You cannot set both `recordingFileConfig` and `snapshotConfig` at the same time, otherwise an error will occur. `recordingFileConfig `includes the following fields:

- `avFileType`: (Optional) JSONArray. The format of the recorded files. `avFileType` can only take `["hls"]`, setting the recorded files to M3U8 and TS formats.

#### <a name="snapshotConfig"></a>Snapshot configuration

`snapshotConfig` is a JSON object for configuring how Cloud Recording takes screenshots. Pay attention to the following parameters, as incorrect settings result in errors or failure to capture screenshots.

- In the URL of your request, `mode` must be `individual`.
- You cannot set both `recordingFileConfig` and `snapshotConfig` at the same time.
- `streamTypes` must be `1` or `2`.
- If you have set `subscribeAudioUid`, you must also set `subscribeVideoUids`.

`snapshotConfig` includes the following fields:

- `captureInterval`: (Optional) Integer. The time interval (in seconds) between two successive screenshots. `captureInterval` should be within the range [5, 3600]. The default value is `10`.
- `fileType`: JSONArray. File format of the screenshots. `fileType` can only take `["jpg"]`, setting screenshots to the JPG format.

#### <a name="storageConfig"></a>Cloud storage configuration

`storageConfig` is a JSON object that configures the third-party cloud storage with the following fields.

- `vendor`: Number. The third-party cloud storage vendor.    

  - `0`: [Qiniu Cloud](https://www.qiniu.com/en/products/kodo)
  - `1`: [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls)
  - `2`: [Alibaba Cloud](https://www.alibabacloud.com/product/oss)
  - `3`: [Tencent Cloud](https://intl.cloud.tencent.com/product/cos)
  - `4`: [Kingsoft Cloud](https://en.ksyun.com/post/product/KS3.html)

- `region`: Number. The regional information specified by the third-party cloud storage:
  When the third-party cloud storage is [Qiniu Cloud](https://www.qiniu.com/en/products/kodo) (`vendor` = 0):

  - `0`: East China
  - `1`: North China 
  - `2`: South China
  - `3`: North America 
  - `4`: Southeast Asia 	

  When the third-party cloud storage is [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls) (`vendor` = 1):

  - `0`: US_EAST_1 
  - `1`: US_EAST_2 
  - `2`: US_WEST_1 
  - `3`: US_WEST_2 
  - `4`: EU_WEST_1 
  - `5`: EU_WEST_2 
  - `6`: EU_WEST_3 
  - `7`: EU_CENTRAL_1 
  - `8`: AP_SOUTHEAST_1 
  - `9`: AP_SOUTHEAST_2 
  - `10`: AP_NORTHEAST_1 
  - `11`: AP_NORTHEAST_2 
  - `12`: SA_EAST_1 
  - `13`: CA_CENTRAL_1 
  - `14`: AP_SOUTH_1 
  - `15`: CN_NORTH_1 
  - `17`: US_GOV_WEST_1 

  When the third-party cloud storage is [Alibaba Cloud](https://www.alibabacloud.com/product/oss) (`vendor` = 2):

  - `0`: CN_Hangzhou 
  - `1`: CN_Shanghai 
  - `2`: CN_Qingdao 
  - `3`: CN_Beijing
  - `4`: CN_Zhangjiakou 
  - `5`: CN_Huhehaote 
  - `6`: CN_Shenzhen 
  - `7`: CN_Hongkong 
  - `8`: US_West_1 
  - `9`: US_East_1 
  - `10`: AP_Southeast_1 
  - `11`: AP_Southeast_2 
  - `12`: AP_Southeast_3 
  - `13`: AP_Southeast_5 
  - `14`: AP_Northeast_1 
  - `15`: AP_South_1 
  - `16`: EU_Central_1 
  - `17`: EU_West_1 
  - `18`: EU_East_1

  When the third-party cloud storage is [Tencent Cloud](https://intl.cloud.tencent.com/product/cos) (`vendor` = 3):

    - `0`：AP_Beijing_1
    - `1`：AP_Beijing
    - `2`：AP_Shanghai
    - `3`：AP_Guangzhou
    - `4`：AP_Chengdu 
    - `5`：AP_Chongqing 
    - `6`：AP_Shenzhen_FSI 
    - `7`：AP_Shanghai_FSI 
    - `8`：AP_Beijing_FSI 
    - `9`：AP_Hongkong 
    - `10`：AP_Singapore 
    - `11`：AP_Mumbai 
    - `12`：AP_Seoul 
    - `13`：AP_Bangkok 
    - `14`：AP_Tokyo 
    - `15`：NA_Siliconvalley 
    - `16`：NA_Ashburn 
    - `17`：NA_Toronto 
    - `18`：EU_Frankfurt 
    - `19`：EU_Moscow
	
  When the third-party cloud storage is [Kingsoft Cloud](https://en.ksyun.com/post/product/KS3.html) (`vendor` = 4):

    - `0`：CN_Hangzhou
    - `1`：CN_Shanghai
    - `2`：CN_Qingdao
    - `3`：CN_Beijing
    - `4`：CN_Guangzhou
    - `5`：CN_Hongkong
    - `6`：JR_Beijing
    - `7`：JR_Shanghai
    - `8`：NA_Russia_1
    - `9`：NA_Singapore_1

- `bucket`: String. The bucket of the third-party cloud storage.

- `accessKey`: String. The access key of the third-party cloud storage. Agora suggests that you use a write-only access key.

- `secretKey`: String. The secret key of the third-party cloud storage.

- `fileNamePrefix`: (Optional) JSONArray. An array of strings. Sets the path of the recorded files in the third-party cloud storage. For example, if `fileNamePrefix` = `["directory1","directory2"]`, Agora Cloud Recording will add the prefix "`directory1/directory2/`" before the name of the recorded file, that is, `directory1/directory2/xxx.m3u8`. The prefix's length, including the slashes, should not exceed 128 characters. The string itself should not contain symbols such as slash, underscore, or parenthesis. The supported characters are as follows:
  - The 26 lowercase English letters: a to z
  - The 26 uppercase English letters: A to Z
  - The 10 numbers: 0 to 9

#### <a name="extensionServiceConfig">Extension service configuration</a>

`extensionServiceConfig` is a JSON Object for extension service configurations. Extension services are a collection of applications that process the media streams generated from the Agora RTC SDK. One example extension service is VoD service.

`extensionServiceConfig` has the following fields:

- `errorHandlePolicy`: (Optional) String. Error handling policy. You can only set it to the default value, `"error_abort"`, which means that once an error occurs to an extension service, all other non-extension services, such as stream subscription, also stop.

- `extensionServices`: JSONArray. An array of the configuration for each extension service. `extensionServices` includes the following fields:

  - `serviceName`: String. The name of the extension service. To use ApsaraVideo for VoD, set it as `aliyun_vod_service`. 

    <div class="alert note">To use ApsaraVideo for VoD:<ul><li>You must choose composite recording mode.</li><li>Do not configure <code>snapshotConfig</code> or <code>storageConfig</code>.</li></ul></div>

  - `errorHandlePolicy`: (Optional) String. Error handling policy. You can only set it to the default value, `"error_abort"`, which means that if an error occurs to the current extension service, other extension services also stop.

  - `serviceParam`: JSON. Configurations for an extension service. To use ApsaraVideo for VoD, configure as follows: 

    - `accessKey`: String. `AccessKeyId` in the AccessKey of Alibaba Cloud. For details, see [Alibaba Cloud's documentation](https://www.alibabacloud.com/help/doc-detail/53045.html).
    - `secretKey`: String. `AccessKeySecret` in the AccessKey of Alibaba Cloud. For details, see [Alibaba Cloud's documentation](https://www.alibabacloud.com/help/doc-detail/53045.html).
    - `regionId`: String. The service region. For details, see [Alibaba Cloud's documentation](https://www.alibabacloud.com/help/doc-detail/43248.html).
    - `apiData`: JSON. Configurations for ApsaraVideo for VoD.
      - `videoData`: JSON. Video configurations.  See [Alibaba Cloud's documentation](https://www.alibabacloud.com/help/doc-detail/55407.html) for details about the following parameters.
        - `title`: String. The title of the video.
        - `description`: (Optional) String. The description of the video.  
        - `coverUrl`: (Optional) String. The URL of the custom video thumbnail.
        - `cateId`: (Optional) String. The ID of the video category.
        - `tags`: (Optional) String. The tags of the video. 
        - `templateGroupId`: (Optional) String. The ID of the transcoding template group.
        - `userData`: (Optional) String. The custom configurations.
        - `storageLocation`: (Optional) String. The bucket.
        - `workflowId`: (Optional) String. The ID of the workflow. 

### An HTTP request example of `start`

- The request URL is：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/mode/<mode>/start
```

- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is basic HTTP authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body

#### Record

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "channelType": 0, 
            "transcodingConfig": {
                "height": 640, 
                "width": 360,
                "bitrate": 500, 
                "fps": 15, 
                "mixedVideoLayout": 1,
                "backgroundColor": "#FF0000"
                        },
            "subscribeVideoUids": ["123","456"], 
            "subscribeAudioUids": ["123","456"],
            "subscribeUidGroup": 0
        }, 
        "recordingFileConfig": {
            "avFileType": ["hls"]
        },
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2,
            "fileNamePrefix": ["directory1","directory2"]
        }
    }
}
```
####	VoD service
```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "channelType": 0,
            "subscribeUidGroup": 0
       },
        "extensionServiceConfig": {
          "extensionServices": [{
            "serviceName": "aliyun_vod_service",
            "serviceParam": {
              "secretKey": "xxxxxx",
              "accessKey": "xxxxxx",
              "regionId": "cn-shanghai",
              "apiData": {
                "videoData": {
                  "title": "My Video",
                  "description": "This is my first video",
                  "coverUrl": "http://xxxxx"
               }
             }
           }
         }]
       }
   }
}
```

#### Capture snapshots

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "token": "<token if any>",
        "recordingConfig": {
            "maxIdleTime": 30,
            "streamTypes": 2,
            "channelType": 0, 
            "subscribeUidGroup": 0
        }, 
        "snapshotConfig": {
            "captureInterval": 5,
            "fileType": ["jpg"]
        },
        "storageConfig": {
            "accessKey": "xxxxxxf",
            "region": 3,
            "bucket": "xxxxx",
            "secretKey": "xxxxx",
            "vendor": 2,
            "fileNamePrefix": ["directory1","directory2"]
        }
    }
}
```

### A response example of `start`

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.

## <a name="updateUID"></a>Updates the subscription lists

During a cloud recording, you can call this method to update the subscription lists multiple times. This method call overrides the existing subscription configuration.



- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/update



> The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.



### Parameters



The following parameters are required in the URL:



| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The App ID in the channel to be recorded.                    |
| `resourceid` | String | The resource ID requested by [`acquire`](#acquire). |
| `sid`        | String | The recording ID created by [`start`](#start). |
| `mode`       | String | The recording mode. Supports individual mode (`individual`) and composite mode (`mix`). Composite mode is the default. |



The following parameters are required in the request body:



| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | The name of the channel to be recorded.                      |
| `uid`           | String | A string that contains the UID of the recording client. Must be the same UID used in the [`acquire`](#acquire) method. |
| `clientRequest` | JSON   | A specific client request which includes the `streamSubscribe` parameter. Use `streamSubscribe` to update the subscription list. |



`streamSubscribe` requires the following parameters:

- audioUidList: (Optional) JSON. The audio subscription list. If you set this parameter, do not set `streamTypes` in `recordingConfig` as `1` (video only).
  - `subscribeAudioUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose audio you want to subscribe. The length of the array should not exceed 32 UIDs, and the array should not be empty. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
  - `unsubscribeAudioUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose audio you do not want to subscribe. Once you set this parameter, the recording service subscribes to the audio of all UIDs except the specified ones. The length of the array should not exceed 32 UIDs, and the array should not be empty. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
- videoUidList: (Optional) JSON. The video subscription list. If you set this parameter, do not set `streamTypes` in `recordingConfig` as `0` (audio only).
  - `subscribeVideoUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose video you want to subscribe. The length of the array should not exceed 32 UIDs, and the array should not be empty. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.
  - `unsubscribeVideoUids`: (Optional) JSONArray. An array of the user IDs (UIDs) of the users whose video you do not want to subscribe. Once you set this parameter, the recording service subscribes to the video of all UIDs except the specified ones. The length of the array should not exceed 32 UIDs, and the array should not be empty. See [Set up subscription lists](https://docs.agora.io/en/cloud-recording/cloud_recording_subscription) for details.



### A request example of `update`



- The request URL is:



```
https://api.agora.io/v1/apps/<appid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/update
```

- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authorization. See [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:



```
{
 "uid": "527841",
 "cname": "httpClient463224",
 "clientRequest": {
  "streamSubscribe": {
   "audioUidList": {
    "subscribeAudioUids": ["#allstream#"]
   },
   "videoUidList": {
    "unSubscribeVideoUids": ["444", "555", "666"]
   }
  }
 }
}                                                         
```

### **A response example of** `update`



```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```



- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording.
- `sid`: String. The recording ID. The unique identification of the current recording.

## <a name="update"></a>Updates the video mixing layout

During a recording, you can call this method to update the video mixing layout multiple times.

This method call overrides the existing layout configurations. For example, if you set the `backgroundColor` parameter as `"#FF0000"` (red) when starting a recording and call this method to update the layout without setting the `backgroundColor` parameter, the background color changes back to black (the default value).

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/updateLayout

> The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The App ID used in the channel to be recorded.               |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Supports composite mode (`mix`) only. |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | The name of the channel to be recorded.                      |
| `uid`           | String | A string that contains the UID of the recording client. Must be the same `uid` used in the [`acquire`](#acquire) method. |
| `clientRequest` | JSON   | A specific client request. See the following list for details. |

`clientRequest` requires the following parameters:

- `maxResolutionUid`: (Optional) String. When the `layoutType` parameter is set as `2` (vertical layout), you can specify the UID of the large video window by this parameter.
- `mixedVideoLayout`: (Optional) Number. The video mixing layout. 0, 1, and 2 are the [predefined layouts](../../en/cloud-recording/cloud_recording_layout.md). If you set this parameter as 3, you need to set the layout by the `layoutConfig` parameter.
  - `0`: (Default) Floating layout. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
  - `1`: Best fit layout. This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
  - `2`: Vertical layout. One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.
  - `3`: Customized layout. Set the `layoutConfig` parameter to customize the layout.
- `backgroundColor`: (Optional) String. The background color of the canvas (the display window or screen) in RGB hex value. The string starts with a "#". The default value is `"#000000"`, the black color.
- `layoutConfig`: (Optional) JSONArray. An array of the configuration of each user's region. Supports 17 users at most. Each user's region configuration is a JSON object with the following parameters:
  - `uid`: (Optional) String. The string contains the UID of the user displaying the video in the region. If this parameter is not specified, the configurations apply in the order of the users joining the channel.
  - `x_axis`: (Mandatory) Float. The relative horizontal position of the top-left corner of the region. The value is between 0.0 (leftmost) and 1.0 (rightmost). `x_axis` can also be an integer 0 or 1.
  - `y_axis`: (Mandatory) Float. The relative vertical position of the top-left corner of the region. The value is between 0.0 (top) and 1.0 (bottom). `y_axis` can also be an integer 0 or 1.
  - `width`: (Mandatory) Float. The relative width of the region. The value is between 0.0 and 1.0. `width` can also be an integer 0 or 1.
  - `height`: (Mandatory) Float. The relative height of the region. The value is between 0.0 and 1.0. `height` can also be an integer 0 or 1.
  - `alpha`: (Optional) Float. The transparency of the image. The value is between 0.0 (transparent) and 1.0 (opaque). The default value is 1.0.
  - `render_mode`: (Optional) Number. The video display mode:
    - `0`: (Default) Cropped mode. Uniformly scales the video until it fills the visible boundaries (cropped). One dimension of the video may have clipped contents.
    - `1`: Fit mode. Uniformly scales the video until one of its dimension fits the boundary (zoomed to fit). Areas that are not filled due to the disparity in the aspect ratio will be filled with black.

### A request example of `updateLayout `

- The request URL is:

```
https://api.agora.io/v1/apps/<appid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/updateLayout
```

- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authorization, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.
- The request body:

```json
{
    "uid": "527841",
    "cname": "httpClient463224",
    "clientRequest": {
        "mixedVideoLayout": 3,
        "backgroundColor": "#FF0000",
        "layoutConfig": [
        {
             "uid": "1",
             "x_axis": 0.1,
             "y_axis": 0.1,
             "width": 0.1,
             "height": 0.1,
             "alpha": 1.0,
             "render_mode": 1
         },
        {
             "uid": "2",
             "x_axis": 0.2,
             "y_axis": 0.2,
             "width": 0.1,
             "height": 0.1,
             "alpha": 1.0,
             "render_mode": 1
         }
         ]
     }
}
```

### A response example of `updateLayout`

```json
"Code": 200,
"Body":
{
  "sid": "38f8e3cfdc474cd56fc1ceba380d7e1a", 
  "resourceId": "JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG"
}
```

- `code`: Number. [Status code](#status).
- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.
- `sid`: String. The recording ID. The unique identification of the current recording.

## <a name="query"></a>Queries the recording status

After you start a recording, you can call query to check its status.

<div class="note alert"><code>query</code> works only with an ongoing recording session. If you call <code>query</code> after a recording ends or after it starts with error, you get a 404 (Not Found) error.</div>

- Method: GET
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/query

> The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The App ID used in the channel to be recorded.               |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Supports individual mode (`individual`) and composite mode (`mix`). |

### An HTTP request example of `query`

- The request URL is：

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/query
```

- `Content-type` is `application/json;charset=utf-8`.
- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

### A response example of `query`
#### Recording or taking screenshots
```json
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileListMode": "json",
    "fileList": [
   {
      "filename": "xxx.m3u8",
      "trackType": "audio_and_video",
      "uid": "123",
      "mixedAllUser": true,
      "isPlayable": true,
      "sliceStartTime": 1562724971626
   },    
   {
      "filename": "xxx.m3u8",
      "trackType": "audio_and_video",
      "uid": "456",
      "mixedAllUser": true,
      "isPlayable": true,
      "sliceStartTime": 1562724971626
   }
   ],
    "status": "5",
    "sliceStartTime": 1562724971626
   }       
}
```
#### VoD
```json
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "extensionServiceState":[
     {
        "payload":{
          "state": "inProgress",
          "videoInfo":[
           {
              "fileName":"38f8e3cfdc474cd56fc1ceba380d7e1a_httpClient463224.m3u8",
              "videoId":"3af7751d658a4381bbed893381708c92"
           }
         ]
       },
        "serviceName": "aliyun_vod_service"
     }
   ],
    "subServiceStatus":{
      "recordingService": "serviceInProgress"
   }
 }
}
```
- `code`: Number. [Status code](#status).

- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.

- `sid`: String. The recording ID. The unique identification of the current recording.

- `serverResponse`: JSON. The details about the recording status. 
  <div class="alert note">The child elements in <code>serverResponse</code> vary according to your configurations in <code>start</code>. For example, if you configure <code>snapshotConfig</code> or <code>extensionServiceConfig</code> in <code>start</code>, then <code>query</code> does not return <code>fileListMode</code>.</div>
	
  - `fileListMode`: String. The data type of `fileList`.
    - `"string"`: `fileList` is a string. In composite mode, `fileListMode` is `"string"`.
    - `"json"`: `fileList` is a JSONArray. In individual mode, `fileListMode` is `"json"`.
  - `fileList`: When `fileListMode` is `"string"`, `fileList` is a string that represents the filename of the M3U8 file. When `fileListMode` is `"json"`, `fileList` is a JSONArray that contains the details of each recorded file. The `query` method does not return this field if you have set `snapshotConfig`.
    - `filename`: String. The name of the M3U8 file.
    - `trackType`: String. The type of the recorded file.
      - `"audio"`: Audio file.
      - `"video"`: Video file (no audio).
      - `"audio_and_video"`: Video file (with audio).
    - `uid`: String. User ID. The user whose audio or video is recorded in the file.
    - `mixedAllUser`: Boolean. Whether the audio and video of all users are combined into a single file.
      - `true`: All users are recorded in a single file.
      - `false`: Each user is recorded separately.
    - `isPlayable`: Boolean. Whether the file can be played online.
      - `true`: The file can be played online.
      - `false`: The file cannot be played online.
    - `sliceStartTime`: Number. The Unix time (ms) when the recording starts.
  - `status`: Number. The status of the cloud service.
    - `0`: The cloud service has not started.
    - `1`: The initialization is complete.
    - `2`: The cloud service is starting.
    - `3`: The cloud service is partially ready.
    - `4`: The cloud service is ready.
    - `5`: The cloud service is in progress.
    - `6`: The cloud service receives the request to stop.
    - `7`: The cloud service stops.
    - `8`: The cloud service exits.
    - `20`: The cloud service exits abnormally.
  - `sliceStartTime`: Number. The time when the recording starts. Unix timestamp (ms).
  - `extensionServiceState`: JSONArray. An array of the detailed status information of each extension service. 
	  - `serviceName`: String. The name of the extension service.  `aliyun_vod_service` means ApsaraVideo for VoD.
	  - `payload`: JSON. The status of the extension service.
	    - `state`: String. The status of uploading media content to the extension service. 
	      - `"inProgress"`: The recorded files are being uploaded to the extension service. 
	      - `"idle"`: No user is sending streams in the channel. Nothing is being uploaded.
	    - `videoInfo`: JSONArray. An array of the M3U8 files and their corresponding video IDs. ApsaraVideo for VoD generates a video ID for each M3U8 file uploaded.
	      - `fileName`: String. The name of the M3U8 file.
	      - `videoId`: String. The corresponding video ID of the M3U8 file.
  - `subServiceStatus`: JSON. The status of the cloud recording submodules.
	  - `recordingService`: String. The status of the video subscription submodule. For details, see [Service status](#service_status). 
	

## <a name="stop"></a>Stops cloud recording

When a recording finishes, call this method to leave the channel and stop recording. To use Agora Cloud Recording again, you need to call the  [`acquire`](#acquire) method for a new resource ID.

- Method: POST
- Endpoint: /v1/apps/\<appid\>/cloud_recording/resourceid/\<resourceid\>/sid/\<sid\>/mode/\<mode\>/stop

> - The request frequency limit is 10 requests per second for each App ID. Contact support@agora.io if you want to raise the limit.
> - Agora Cloud Recording automatically leaves the channel and stops recording when no user is in the channel for more than 30 seconds by default.

### Parameters

The following parameters are required in the URL.

| Parameter    | Type   | Description                                                  |
| :----------- | :----- | :----------------------------------------------------------- |
| `appid`      | String | The App ID used in the channel to be recorded.               |
| `resourceid` | String | The resource ID requested by the [`acquire`](#acquire) method. |
| `sid`        | String | The recording ID created by the [`start`](#start) method.    |
| `mode`       | String | The recording mode. Supports individual mode (`individual`) and composite mode (`mix`). |

The following parameters are required in the request body.

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| `cname`         | String | The name of the channel to be recorded.                      |
| `uid`           | String | A string that contains the UID of the recording client. Must be the same `uid` used in the [`acquire`](#acquire) method. |
| `clientRequest` | JSON   | A specific client request that is empty for this method.     |

### An HTTP request example of `stop`

- The request URL is:

```
https://api.agora.io/v1/apps/<yourappid>/cloud_recording/resourceid/<resourceid>/sid/<sid>/mode/<mode>/stop
```

- `Content-type` is `application/json;charset=utf-8`.

- `Authorization` is the basic authentication, see [RESTful API authentication](https://docs.agora.io/en/faq/restful_authentication) for details.

- The request body:

  ```json
  {
    "cname": "httpClient463224",
    "uid": "527841",
    "clientRequest":{
    }
  }
  ```

### A response example of `stop`

```json
"Code": 200,
"Body":
{
  "resourceId":"JyvK8nXHuV1BE64GDkAaBGEscvtHW7v8BrQoRPCHxmeVxwY22-x-kv4GdPcjZeMzoCBUCOr9q-k6wBWMC7SaAkZ_4nO3JLqYwM1bL1n6wKnnD9EC9waxJboci9KUz2WZ4YJrmcJmA7xWkzs_L3AnNwdtcI1kr_u1cWFmi9BWAWAlNd7S7gfoGuH0tGi6CNaOomvr7-ILjPXdCYwgty1hwT6tbAuaW1eqR0kOYTO0Z1SobpBxu1czSFh1GbzGvTZG",
  "sid":"38f8e3cfdc474cd56fc1ceba380d7e1a",
  "serverResponse":{
    "fileListMode": "json",
    "fileList": [
    {
      "filename": "xxx.m3u8",
      "trackType": "audio_and_video",
      "uid": "123",
      "mixedAllUser": true,
      "isPlayable": true,
      "sliceStartTime": 1562724971626
    },
    {
      "filename": "xxx.m3u8",
      "trackType": "audio_and_video",
      "uid": "456",
      "mixedAllUser": true,
      "isPlayable": true,
      "sliceStartTime": 1562724971626
    }
    ],
    "uploadingStatus": "uploaded"
  }
}
```

- `code`: Number. [Status code](#status).

- `resourceId`: String. The resource ID for cloud recording. The resource ID is valid for five minutes.

- `sid`: String. The recording ID. The unique identification of the current recording.

- `serverResponse`: JSON. The details about the recording status.
  - `fileListMode`: String. The data type of `fileList`. The `query` method does not return this field if you have set `snapshotConfig`.
    - `"string"`: `fileList` is a string. In composite mode, `fileListMode` is `"string"`.
    - `"json"`: `fileList` is a JSONArray. In individual mode, `fileListMode` is `"json"`.
  - `fileList`: When `fileListMode` is `"string"`, `fileList` is a string that represents the filename of the M3U8 file. When `fileListMode` is `"json"`, `fileList` is a JSONArray that contains the details of each recorded file.  The `query` method does not return this field if you have set `snapshotConfig`.
    - `filename`: String. The name of the M3U8 file.
    - `trackType`: String. The type of the recorded file.
      - `"audio"`: Audio file.
      - `"video"`: Video file (no audio).
      - `"audio_and_video"`: Video file (with audio).
    - `uid`: String. User ID. The user whose audio or video is recorded in the file.
    - `mixedAllUser`: Boolean. Whether the audio and video of all users are combined into a single file.
      - `true`: All users are recorded in a single file.
      - `false`: Each user is recorded separately.
    - `isPlayable`: Boolean. Whether the file can be played online.
      - `true`: The file can be played online.
      - `false`: The file cannot be played online.
    - `sliceStartTime`: Number. The Unix time (ms) when the recording starts.     
  - `uploadingStatus`: String. The upload status.
    - `"uploaded"`: All the recorded files are uploaded to the third-party cloud storage.
    - `"backuped"`:  Some of the recorded files fail to upload to the third-party cloud storage and upload to Agora Cloud Backup instead. Agora Cloud Backup automatically uploads these files to your cloud storage. 
    - `"unknown"`: Unknown status.

## <a name="status"></a>Status code

| Code | Description                                                  |
| :--- | :----------------------------------------------------------- |
| 200  | The request is successful.                                   |
| 201  | The request has been fulfilled, resulting in the creation of a new resource. |
| 206  | No user in the channel sent a stream during the recording process, or some of the recorded files are uploaded to the Agora Cloud Backup instead of the third-party cloud storage. |
| 400  | The server cannot process the request due to malformed request syntax, or [the cloud recording service is not enabled](https://docs.agora.io/en/cloud-recording/cloud_recording_rest#enable-cloud-recording). |
| 401  | Unauthorized (incorrect Customer ID/Customer Secret).        |
| 404  | The requested resource could not be found.                   |
| 500  | Internal server error.                                       |
| 504  | The server was acting as a gateway or proxy and did not receive a timely response from the upstream server. |

## <a name="service_status"></a>Service status

| State                     | Description                                                  |
| :------------------------ | :----------------------------------------------------------- |
| "serviceIdle"             | The submodule is idle.                                       |
| "serviceStarted"          | The submodule has started.                                   |
| "serviceReady"            | The submodule is ready.                                      |
| "serviceInProgress"       | The submodule is running.                                    |
| "serviceCompleted"        | All subscribed contents have been uploaded to the extension service. |
| "servicePartialCompleted" | Part of the subscribed contents have been uploaded to the extension service. |
| "serviceValidationFailed" | The validation of the extension service failed. For example, `apiData` is incorrect. |
| "serviceAbnormal"         | The submodule is in an abnormal status.                      |

## Errors

This section lists the common errors you may encounter when using the Agora Cloud Recording RESTful APIs. If you encounter other errors, contact support@agora.io.

- `2`: Invalid parameter. Possible reasons:
	- A parameter's data type is wrong.
	- A parameter is spelt wrong. All the parameters are case sensitive.
	- A parameter value is out of range.
	- A mandatory parameter is missing.
  
- `7`: The recording is already running. Do not repeat the `start` request with the same resource ID.
- `8`: Errors in the HTTP request header fields. Possible reasons:
  - Content-type is wrong. Ensure that the `Content-type` field is `application/json;charset=utf-8`.
  - `cloud_recording` is missing in the request URL.
  - The HTTP method is wrong.
  - The request is not valid JSON
- `49`: Caused by repeated `stop` requests with the same resource ID and recording ID (sid).
- `53`: The recording is already running. This error occurs when you use the same parameters to call `acquire` again and use the new resource ID in the `start` request. To start multiple recording instances, use a different UID for each instance.
- `62`: If you receive this error when calling `acquire`, the cloud recording service is not enabled. See [Enable Cloud Recording](https://docs.agora.io/en/cloud-recording/cloud_recording_rest#enable-cloud-recording) for details.
- `65`: Usually caused by network jitter. Try again with the same resource ID.
- `432`: The parameter in the request is incorrect. Either the parameter is invalid, or the App ID, channel name, or UID does not match the resource ID.
- `433`: The resource ID has expired. You need to start recording within five minutes after getting a resource ID. Call acquire to get a new resource ID.
- `435`: No recorded files created. There is nothing to record because no user is sending any stream in the channel.
- `501`: The recording service is exiting. This error might occur when you call query after stop.
- `1001`: Fails to parse the resource ID. Call acquire to get a new resource ID.
- `1003`: The App ID or recording ID (sid) does not match the resource ID. Ensure that the App ID or recording ID matches the resource ID in each recording session.
- `1013`: The channel name is invalid. The string length must be less than 64 bytes. Supported character scopes are:
  - All lowercase English letters: a to z.        
  - All uppercase English letters: A to Z.
  - All numeric characters: 0 to 9.
  - The space character.
  - Punctuation characters and other symbols, including: "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",".
- `1028`: Invalid parameters in the request body of the `updateLayout` method.
- `"invalid appid"`: The App ID you entered is invalid. If the error persists after you verify the App ID, contact [support@agora.io](mailto:support@agora.io).
- `"no Route matched with those values"`: A possible reason for this message is that you entered an incorrect HTTP method in your request, such as using GET when it should have been POST. Another possible reason is that you sent your request to a wrong URL.
- `"Invalid authentication credentials"`: The following list contains possible reasons for this message. If the error persists after you correct the following issues, contact [support@agora.io](mailto:support@agora.io):
  - The Customer ID or Customer Certificate you entered is incorrect.
  - The cloud recording service is not enabled. See [Enable Cloud Recording](https://docs.agora.io/en/cloud-recording/cloud_recording_rest#enable-cloud-recording) for details.
  - HTTP header contains incorrect information. For example, `Basic` is missing in the `Authorization` field.
  - HTTP header contains incorrectly formatted content. For example, the capitalization is wrong or there is an unnecessary space in the `Content-type` field. The correct value is `application/json;charset=utf-8`.
