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
- [【FFMPEG】FFMPEG程序捕获Mac设备流媒体](https://blog.csdn.net/arctan90/article/details/50828771)

## wav简介

wav是包装头+PCM数据,PCM是原始声音数据

### 音频三元组

(采样率,采样大小,通道数)

### 为什么要重采样

比如41000/16/2 转成 48000/16/2

- 从设备采集的音频数据和编码器要求的数据不一致
- 扬声器要求的音频数据与要播放的音频数据不一致
- 更方便运算,比如回音消除

### 

# 音频压缩

- 消除冗余信息比如 <20  >10k 的频率
- 无损压缩 7z 需要通过解码

## 音频冗余信息

信号的遮蔽包括**频域遮蔽**和**时域遮蔽**

### 频域遮蔽效应

高声屏蔽低声

![截屏2020-11-02 上午12.47.46](https://pic.4sus2.com/uPic/1604249269316NRdKfe.png)

### 时域遮蔽效应

两人同时说话,低声发生时间越接近高声发生时间,会产生屏蔽

![截屏2020-11-02 上午12.51.33](https://pic.4sus2.com/uPic/1604249495246CQFqzD.png)

## 无损编码

熵编码

- 哈夫曼编码
- 算术编码
- 香农编码

### 常见的音频编码器

POUS,AAC,Ogg,Speex,iLBC,AMR,G.711

> AAC 在直播系统中应用比较广泛
>
> PUUS是较新的编码器
>
> WebRTC默认使用OPUS
>
> 固话一般使用G.711
>
> 评测结果: OPUS>AAC>Ogg

## AAC编码器介绍

AAC目标取代MP3,比MP3保真性好,压缩高

常见AAC LC, AAC HE V2

## AAC 规格描述

**AAC LC** :(Low Complexity)低复杂度规格,码流128K,音质好

**AAC HE**:等于AAC LC+ SBR(Spectral Band Replication) 核心思想是按频谱分保存,低频编码保存主要成分,高频单独放大编码保存音质,码流64K左右

**AAC HE V2**:等于 AAC LC + SBR + PS(Parametric Stereo).核心思想是,双声道中的声音存在某种相似性,只需要存储一个声道的全部信息,然后花很少的字节用参数描述另一个声道的不同.

## 通过ffmpeg命令生成AAC文件

```bash
ffmpeg -i xxx.mp4  输入
			 -vn  表示no video
			 -c:a  libfdk_aac 表示codex,a表示编码器audio 使用libfdk_aac
       -ar 44100 表示音频采样率
       -channels 2 声道数
       -profile:a aac_he_v2 表示对音频设置参数,对音频使用aac_he_v2
       xx.aac 输出
```



# 遇到问题



遇到无法加载编码器 Unknown encoder 'libfdk_aac'

```bash
git clone git://github.com/mstorsjo/fdk-aac
cd fdk-aac
autoreconf -i
./configure
make install
```

在这个过程中需要下载automake 来编译aac相关文件

```bash
brew install automake
```

上面这个方法并没有解决

https://juejin.im/post/6844903986403737607

