# ffmpeg的安装

使用`homebrew`安装,简单高效.

```bash
homebrew install ffmpeg
```

> 其中包含了ffplay,无需再次安装

homebrew如果下载慢(或者是换了源也很慢),请参考:

[Homebrew国内如何自动安装(国内地址)](https://zhuanlan.zhihu.com/p/111014448)

# 采集音频

```bash
ffmpeg -f avfoundation -i :0 audio.wav
-f: 使用 avfoundation 采集数据 mac是使用avfoundation; window使用dshow; Linux 使用alsa
-i: url (input) 指定从哪采集数据 可以是现有资源 也可以是声卡输入 比如 0:0 左边指的是视频,右边指的是音频 数字代表的是设备序号
```

如果你的设备有多个声卡和录影设备,你需要找到你想用的设备序号.

```b
ffmpeg -f avfoundation -list_devices true -i ""
[AVFoundation indev @ 0x7f958b52a380] AVFoundation video devices:
[AVFoundation indev @ 0x7f958b52a380] [0] FaceTime高清摄像头（内建）
[AVFoundation indev @ 0x7f958b52a380] [1] Capture screen 0
[AVFoundation indev @ 0x7f958b52a380] AVFoundation audio devices:
[AVFoundation indev @ 0x7f958b52a380] [0] Steinberg UR22mkII
[AVFoundation indev @ 0x7f958b52a380] [1] Soundflower (64ch)
[AVFoundation indev @ 0x7f958b52a380] [2] MacBook Pro麦克风
[AVFoundation indev @ 0x7f958b52a380] [3] Speaker Audio Recorder
[AVFoundation indev @ 0x7f958b52a380] [4] Soundflower (2ch)
```

比如我想使用内建麦克风录音,那么就是

```bas
ffmpeg -f avfoundation -i :2 audio.wav
```

结束就直接按q结束,或者使用`control+c`

播放音频可以自己用播放器打开或者使用`ffplay`

```bas
ffplay audio.wav
```

参考文章:

- [ffmpeg Documentation](https://ffmpeg.org/documentation.html)

## wav简介

wav是包装头+PCM数据,PCM是原始声音数据