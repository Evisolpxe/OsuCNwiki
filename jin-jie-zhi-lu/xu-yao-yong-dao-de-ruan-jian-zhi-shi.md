# 需要用到的软件知识

## 1.数位板驱动

### 1.1 Wacom官方驱动

下载地址：[http://support.wacom.com.cn/download/drivers.aspx?ctype=mode](http://support.wacom.com.cn/download/drivers.aspx?ctype=mode)

在2017年以前，Bamboo系列（CTL460/470/471）的驱动和Intuos系列（CTL480/490、PTH系列数位板）的驱动是分开的，但是现在新版的驱动已经支持两个产品线的数位板。

如果你需要在网吧安装，必须使用2015年v6.3.15-2\(Intuos系列\)或v.5.3.5-3\(Bamboo系列\)或更早版本的驱动，连续安装两次来绕过重新启动才能使用的限制。

下载完成后，首先设置为笔模式，然后找到【使用Windows Ink】选项，去掉其复选框，最后根据个人喜好调整映射。没有标准答案，每个人的映射都有差别，高Rank玩家的映射也不一定适合你。

### 1.2 TabletDriver第三方驱动

下载地址：[https://github.com/hawku/TabletDriver/releases](https://github.com/hawku/TabletDriver/releases)

2018年3月由外国友人开发的第三方驱动，持续维护至今，除了Wacom数位板外还支持XP Pen（一个日本的品牌）、绘王、高漫等其他品牌的数位板。

相对于Wacom官方驱动，这款驱动对玩家开放了延迟设置，并提供了一种低延迟的去抖方案，玩家不再需要逐个测试各个版本驱动的延迟。

**默认设置会带来最低的延迟，同时也会导致较强的光标抖动，在悬空/Azuki打图时对移动的干扰较大，建议调节Filter标签页中的第二个降噪Filter！**

下载解压后，需要先运行目录下的install\_vmulti\_driver.bat来安装驱动，然后使用TabletDriverGUI启动用户界面。

界面介绍：

![](https://up.ppy.sh/files/qq20190417151951.png)

![](https://up.ppy.sh/files/qq20190417152006.png)

![](https://up.ppy.sh/files/qq20190417152009.png)

## 2.降低输入延迟

### 2.1 游戏帧数

很难以置信也难以理解的是，osu! 的输入和渲染挂钩，也就是游戏渲染帧数也同样是读取输入的帧数。

因此**千万不要在osu!中开启帧数限制**，除非你计算机的散热性能真的非常有限，需要使用帧数限制降低功耗！

由于大部分键盘的输入在1khz，因此也推荐提升游戏帧数到1000以上，具体帧数与硬件的关联请查看硬件篇。

### 2.2 数位板

#### 2.1.1 Wacom官方驱动

Wacom在近几年的官方驱动中加入了移动平滑，会带来数十毫秒的输入延迟，在高BPM锐角移动时尤其明显。

**Windows Ink功能会带来点击卡顿和延迟，严重影响体验，请务必在玩osu!时关闭！**

使用2013-2014年的驱动（6.3.10w2和5.3.3-2）可以在驱动里关闭Windows Ink的同时略微降低延迟（更早的驱动需要手动在操作系统内卸载组件），可以在[英文官网](https://www.wacom.com/en/support/product-support/drivers)下载。

#### 2.1.2 TabletDriver第三方驱动

将延迟设置直接开放给玩家，见上方介绍。

### 2.3 键盘/鼠标

刷新率直接影响输入延迟，请尽可能挑选1khz刷新率的设备。

### 2.4 Windows设置

* 关闭Xbox dvr\(win10\)
* 关闭全屏优化模式\(1709及以上\)
* 音频格式请设置为16bit 44.1khz\(高了要重算 带来延迟\)

### 2.5 NVIDIA显卡驱动

（AMD用户不需要设置，没什么用而且实际手感比N卡好）

Phyx处理器：CPU

3D设置：

针对osu!关闭以下选项：

* 三重缓冲
* 各向异性过滤
* 垂直同步
* 平滑处理-模式
* 平滑处理-灰度纠正
* 着色缓存器
* 纹理过滤-三线性优化
* 纹理过滤-各向异性采样优化
* 线程优化

修改以下设置：

* CUDA-GPUS设为无
* 最大预渲染帧数设为【1】
* 纹理过滤-负LOD偏移设为【锁定】
* 纹理过滤-质量设为【高性能】

### 2.6 其他osu!设置

* Don't change dim level during breaks：关闭 ，虽然很好用但是会增加延迟
* 全屏模式：开，在同时使用两个不同刷新率的显示器的情况下，低刷新率显示器上有画面改变时会拖慢高刷新率显示器的刷新率，但是全屏应用不受影响。

### 2.4 DWM

**警告：挂起DWM进程属于硬核玩法，虽然能降低输入延迟，但是也会几乎使操作系统无法使用，请三思而行！**

请仔细阅读[https://github.com/Phorofor/DWM.ForceSwitch项目的readme文件，务必先了解后果再进行操作！](https://github.com/Phorofor/DWM.ForceSwitch项目的readme文件，务必先了解后果再进行操作！)

## 3.直播

### 3.1 推流和录像：OBS

下载地址：[http://obsproject.com/download](http://obsproject.com/download)

#### 3.1.1 基本使用

一个**场景**可以理解为一种直播内容，由一些**来源**和它们的布局构成。

例如，直播比赛需要使用显示器捕获源来获取直播端，并且只有直播端一个来源，占满所有屏幕；

而直播打图除了有osu!（游戏源），还有歌曲信息（文本源）和摄像头（视频捕获设备），这些**来源**有固定的布局。此时直播比赛和直播打图就可以设置为两个场景。

直播打图时，推荐自己制作一个Banner，给画面中除了游戏和摄像头的黑边部分放一些图片，改善观众的体验。

#### 3.1.2 一些设置

**编码优先级**

OBS提供了从Fast到Slow的优先级，Slow会占用更多系统资源，推荐先设置为Very Fast再逐步调优。

**码率**

码率就是你推流使用的最高上传速度，但是它会受物理带宽的限制，码率高于物理带宽时会出现大量的丢帧，在观众眼中就是转圈卡顿。但是较低的码率设置会导致在视频流每帧颜色变化较大时，尝试压缩视频体积，导致大量的马赛克。

**分辨率和帧率**

高分辨率需要很高的码率，高帧数则需要更慢的编码优先级。

当然，1080p@60FPS能给观众带来清晰又顺滑的体验，当你的网络环境不允许时，尽可能先降低分辨率再降低帧率。

提示：虽然你的Banner可能要容纳1080p的游戏和320p的摄像头，进而远大于1080p，但是最终输出还是推荐进行缩放，以避免在投稿时被二次压制，同时也可以提高码率。

**最佳实践**

首先测试你的上传带宽（注意KBps和kbps的不同），将码率设置为比带宽较小的值，进行直播测试。如果右下角提示丢帧，进一步减小码率。

根据最终的码率结果在下方三种方案中挑选一种，如果不丢帧的最高码率低于2500，建议放弃直播。

**1080p 60fps**

视频分辨率: 1920\*1080

码率: 4500 - 6000 kbps

帧数: 60 or 50 fps

关键帧间隔: 2 seconds

AVC \(h.264\) Profile: Main/High

AVC \(h.264\) Level: 4.2

**720p 60fps**

视频分辨率: 1280\*720

码率: 3500 to 5000 kbps

帧数: 60 or 50 fps

关键帧间隔: 2 seconds

AVC \(h.264\) Profile: Main/High

AVC \(h.264\) Level: 4.1

**720p 30fps**

视频分辨率: 1280\*720

码率: 2500 to 4000 kbps

帧数: 30 or 25 fps

关键帧间隔: 2 seconds

AVC \(h.264\) Profile: Main/High

AVC \(h.264\) Level: 3.1

### 3.2 直播比赛

近几年国内比赛增多（详见比赛页面），玩家的电脑配置也逐渐提升，因此参与比赛直播的人也越来越多。

#### 3.2.1 前置条件和直播端准备

首先你必须是osu! supporter，否则无法启动直播端。

复制一个新的osu客户端，建议不带Skins、Songs、Replays三个文件夹．然后打开新的客户端，登陆你的用户（需要是supporter），选择记住用户名和密码，版本选“正式版”，并把皮肤改成default．

退出后，在osu文件夹里新建一个空文件“tournament.cfg”，运行客户端，此时就应该会进入直播端，此时游戏会自动填写tournament.cfg。

#### 3.2.2 赛前准备：

编辑你的tournament.cfg，添加/修改如下一行：

```text
acronym = 比赛名，由裁判提供
```

接下来根据比赛裁判要求，设置直播端的皮肤和音效，并且将本场比赛使用的谱面安装到直播端。

比赛开始前，由裁判创建房间，如果房间前缀和你的配置一样，应该能在左下角看到房间。

**首先联系本场裁判给你直播权限**（否则无法看到房间内聊天，并且需要重启直播端并重新添加权限才能显示），接下来点击房间名即可开始围观场上所有选手。

需要注意的是同一个比赛多场赛事同时进行的情况，需要仔细根据比赛队伍区分场次。

#### 3.2.3 直播端使用

右下角的“注意事项”指的是上方标题，热手的时候填上“Warm up”；

“当前最高”指的是BO数，比如BO9就填9，联系比赛裁判获取；

底下的toggle可以切换顶上显示标题还是显示比分，一般热手完时使用；

panic是重载，在比赛时有人没围观上之类的事故时使用，重新围观所有选手；

顶端星星是双方得分，一般会根据房间情况自动增加，重赛时需要手动操作。

#### 3.2.4 直播设置

由于直播端由多个osu!窗口和直播端管理窗口构成，无法和普通游戏时一样使用游戏源捕获（可以逐个捕获但是要手动排序，偶尔会出现小客户端消失时更麻烦），需要直接使用**显示器捕获**。

视频输出的分辨率应修改为1280\*720，直播端下方的设置界面可以无需播出。

### 3.3 实时显示歌曲信息：Sync

#### 3.3.1 主程序和默认插件下载

[https://github.com/OsuSync/Sync/releases](https://github.com/OsuSync/Sync/releases)

主程序最新版本号为2.18.2，内置插件有：

BanManager —— 弹幕拉黑

BeatmapSuggest —— 谱面推荐

DefaultPlugin —— 默认插件，处理直播事件（礼物等）

NowPlaying —— 传统的osu!信息获取插件，通过接受osu!推送到MSN的信息+处理osu文件信息来获取当前歌曲。

PPQuery —— 向自己的IRC账号发送消息查询PP。（其实是转发给Tillerino

RecentlyUserQuery —— 用户名/ID转化

#### 3.3.2 插件安装

**为了让它拥有实时PP、谱面信息显示，需要安装以下插件。**

安装步骤：将插件release地址里最新的.zip文件下载，覆盖到Sync目录后，**启动一次Sync，并视情况在config.ini里修改配置**。

**实时PP显示：**

[https://github.com/OsuSync/RealTimePPDisplayer/releases](https://github.com/OsuSync/RealTimePPDisplayer/releases)

将当前PP实时输出到内存映射/窗口

配置项说明：

OutputMethods 插件输出模式。WPF为独立窗口，MMF为内存映射文件，text为输出到硬盘文本，可以用逗号分隔，同时输出多个目标

UseText 单独控制是否输出文本（？）

TextOutputPath 文本路径

DisplayHitObject 是否展示点击统计（300/100/50/X的数量）

PPFontSize PP字体

PPFontColor PP字体颜色

HitObjectFontSize 点击统计字体

HitObjectFontColor 点击统计字体颜色

BackgroundColor 背景颜色，默认绿色，方便OBS采用色键功能抠图

WindowHeight WPF窗口宽高 WindowWidth

SmoothTime 渐变时间

FPS ……

Topmost WPF模式窗口是否置顶

WindowTextShadow 字体阴影

DebugMode 调试输出

RoundDigits PP小数位

PPFormat PP格式，默认为${rtpp}pp ，括号内支持三维/最大PP 具体参见github.com/OsuSync/RealTimePPDisplayer/wiki/How-to-customize-my-output-content%3F

HitCountFormat 点击统计格式，默认为${n100}x100 ${n50}x50 ${nmiss}xMiss，括号内支持内容参见上文

IgnoreTouchScreenDecrease 是否无视触屏Mod

RankingSendPerformanceToChat 对Rank图，输出PP到IRC

**屙屎信息源：**

[https://https://github.com/OsuSync/OsuRTDataProvider-Release/releases](https://https://github.com/OsuSync/OsuRTDataProvider-Release/releases)

读取osu!内存的插件，因此只有release而没有源码。

配置项说明：

ListenInterval 读取频率，默认即可。过小会导致资源浪费

EnableTourneyMode 对比赛端的支持

TeamSize 比赛端每队人数

ForceOsuSongsDirectory 强制osu!文件夹

GameMode 如果自动识别模式失败，需要手动指定

DisableProcessNotFoundInformation 是否隐藏“找不到osu!.exe信息”，支持True/False

EnableModsChangedAtListening 是否尝试在听歌时跟踪Mod变化

以下两个插件有中文文档，参考code页下的说明即可。

**谱面信息输出插件：**

[https://github.com/OsuSync/OsuLiveStatusPanel/releases](https://github.com/OsuSync/OsuLiveStatusPanel/releases)

需要注意的是配置里AllowUsedMemoryReader改为1，而AllowUsedNowPlaying改成0，否则听歌时没有输出。

**歌词插件：**

[https://github.com/OsuSync/LyricDisplayerPlugin/releases](https://github.com/OsuSync/LyricDisplayerPlugin/releases)

## 4.音频延迟

### 4.1 介绍

* 当前osu!版本使用DirectSound作为音频输出的API。DirectSound的缓冲区长度由Windows控制，会导致音效和音频一起晚40ms播放，可以认为不可修改。
* 由于osu的帧采样特性，音效也是按帧播放的（上一帧点击，下一帧播放音效），帧率过低的情况会导致音效延迟并且错位。

  **影响**

* 由于音频的错位，我们需要在节拍响起之前的40ms按键（这里暂时忽视帧数导致的输入延迟），对于听力敏感的玩家来说会影响游戏体验。
* 点击音效本来应该是提供点击回馈的一部分，它反馈迟钝将影响到下一个物件的点击,40ms音效延迟的概念是一次正常点击时手指完全抬起来音效才会响，有些人很难在音效响起之前就把手指抬起来，**而这对更高bpm的复杂节奏产生一定影响**。

当然上述影响固然是因人而异的，有些人这个症状比较轻微。

### 4.2 解决办法

令人难过的是，osu并不支持任何低延迟音效组件\(coreaudio/wasapi/asio\)，而coreaudio和asio更是可以配合专业声卡达到5ms以内的音频延迟，这将对游戏体验有极大的提升。

这个问题很久以前就提出来过，ppy对要求他加上asio的态度比较搞笑（他认为从键盘到音效响起的录音并不能说明问题，然而他又不自己给出测试方法，而且他也认为asio没有用）。即使到了lazer也不会用上能支持asio/wasapi的osu，请大家尽可能死心。

虽然我们不能真的拿枪怼他脸上逼着他做asio输出，但是对音效延迟忍无可忍的朋友可以尝试以下内容。

* linux下用wine运行osu，调整wine的DirectSound Buffer，最低可致13ms左右，远低于windows的40ms。有完善的方案，但是这样的音频输出质量极差，基本不能听。
* **关闭音效，全局-40ms，听按键盘的声音打图。**
* 推荐使用real.exe，可以稍微降低一点DirectSound延迟到30ms。差距比较小但是仍然有用。

~~目前国内\#1 iMey 便是Linux玩家。~~

想体验低延迟但又不想倒腾的朋友，可以试试steam上McOsu的wasapi组件，本文不展开阐述。

**real.exe 使用说明：**

项目地址: [https://github.com/miniant-git/REAL](https://github.com/miniant-git/REAL)

1. **备选项（安装HD Audio）**右键点击此电脑 -&gt; 选择“管理” -&gt; 选择左侧“设备管理器” -&gt; 展开“音频输入和输出” -&gt; 右键点击你的音频输出设备，选择“更新驱动程序和软件” -&gt; “浏览计算机以查找...” -&gt; “从计算机的设备驱动程序列表中选取...” -&gt; “通用软件设备” -&gt; 保存
2. 将音频输出设为默认，开启real.exe即可。

{% hint style="warning" %}
### 注意事项

1. osu!无论如何也不会通过asio接口输出的，而asio声卡跑DirectSound的延迟比板载声卡更高，一块Focusrite 2i4声卡甚至跑出了85ms的延迟。
2. 不听音效也不能提高音频格式（位数和采样），osu最佳运行是16bit 44100hz，高于此格式均需要重采样，由于游戏的每帧采样特性，音频重采样会造成帧延迟，体现于光标变拖
3. 不使用的音频硬件请禁用，例如显卡的音频输出。
{% endhint %}

> 本篇贡献者: EmertxE, Mother Ship

