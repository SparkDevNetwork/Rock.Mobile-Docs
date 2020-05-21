# Video Player

_Inherits from_ [_Xamarin.Forms.ContentView_](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.contentview)

While there are block commands that will allow you to start playback of an audio or video file in full-screen mode, usually you want that video embedded on the page. For example, on your message library pages you might want to display the sermon title, then the video in-line, and then the description of that sermon. This view will allow you to have a video embedded into a page. The video itself can be set to auto-play \(use sparingly\) or to wait for the user to instruct it to play.

If a video is not set to AutoPlay then a thumbnail will be displayed. If you specifiy a `ThumbnailSource` for the view, then that image will be used as the thumbnail. Otherwise, the view will attempt to generate a thumbnail by inspecting the stream and using the `ThumbnailPosition` property value as the offset into the video to capture a thumbnail. Due to limitations in both iOS and Android, auto-thumbnail generation only works on MP4/M4V files. It will not work on HLS streams \(usually having an `m3u8` extension\).

**Properties**

| Property | Type | Description |
| :--- | :--- | :--- |
| Source | string | The URL to be loaded into the video player. Supports MP4 and HLS. |
| ThumbnailSource | string | The URL to be loaded as the thumbnail. Overrides any automatic thumbnail generation. |
| PlayButtonSource | string | The URL to be loaded as the play button if AutoPlay is `false`. This image can be any size but we recommend 128x128. It will be displayed inside a 64x64 box, but will maintain aspect ratio. _Defaults to **resource://Rock.Mobile.Resources.PlayButton.png**._ |
| AutoPlay | bool | If `true` then the video will start playing as soon as it has loaded into the player. _Defaults to `false`._ |
| InitalAspectRatio | string | The initial aspect ratio to use until the video has loaded and we know the actual aspect ratio. Can be specified as either a `width:height` ratio \(example `16:9`\) or as a decimal number \(example `1.77777`\). _Defaults to **16:9**._ |
| ThumbnailPosition | double? | The position into the video to use to generate a thumbnail automatically if no ThumbnailSource is specified. Value is specified in seconds. _Defaults to **2**_ |

**Example**

```text
<Rock:VideoPlayer Source="https://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"
                  AutoPlay="false" />
```

![](../../.gitbook/assets/videoplayer-1.png)



