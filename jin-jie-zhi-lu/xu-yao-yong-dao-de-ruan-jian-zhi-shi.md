# 需要用到的软件知识

## 1.数位板驱动

### 1.1 Wacom官方驱动

下载地址：http://support.wacom.com.cn/download/drivers.aspx?ctype=mode

在2017年以前，Bamboo系列（CTL460/470/471）的驱动和Intuos系列（CTL480/490、PTH系列数位板）的驱动是分开的，但是现在新版的驱动已经支持两个产品线的数位板。

如果你需要在网吧安装，必须使用2015年v6.3.15-2(Intuos系列)或v.5.3.5-3(Bamboo系列)或更早版本的驱动，连续安装两次来绕过重新启动才能使用的限制。

下载完成后，首先设置为笔模式，然后找到【使用Windows Ink】选项，去掉其复选框，最后根据个人喜好调整映射。没有标准答案，每个人的映射都有差别，高Rank玩家的映射也不一定适合你。

### 1.2 TabletDriver第三方驱动

下载地址：https://github.com/hawku/TabletDriver/releases

2018年3月由外国友人开发的第三方驱动，持续维护至今，除了Wacom数位板外还支持XP Pen（一个日本的品牌）、绘王、高漫等其他品牌的数位板。

相对于Wacom官方驱动，这款驱动对玩家开放了延迟设置，并提供了一种低延迟的去抖方案，玩家不再需要逐个测试各个版本驱动的延迟。

**默认设置会带来最低的延迟，同时也会导致较强的光标抖动，在悬空/Azuki打图时对移动的干扰较大，建议调节Filter标签页中的第二个降噪Filter！**

下载解压后，需要先运行目录下的install_vmulti_driver.bat来安装驱动，然后使用TabletDriverGUI启动用户界面。

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

### 2.2 键盘/鼠标

刷新率直接影响输入延迟，请尽可能挑选1khz刷新率的设备。

### 2.4 DWM

**警告：挂起DWM进程属于硬核玩法，虽然能降低输入延迟，但是也会几乎使操作系统无法使用，请三思而行！**

请仔细阅读https://github.com/Phorofor/DWM.ForceSwitch项目的readme文件，务必先了解后果再进行操作！

## 3.直播

### 3.1 推流和录像：OBS

下载地址：http://obsproject.com/download

#### 3.1.1 基本使用

一个**场景**可以理解为一种直播内容，由一些**来源**和它们的布局构成。

例如，直播比赛需要使用显示器捕获源来获取直播端，并且只有直播端一个来源，占满所有屏幕；

而直播打图除了有osu!（游戏源），还有歌曲信息（文本源）和摄像头（视频捕获设备），这些**来源**有固定的布局。此时直播比赛和直播打图就可以设置为两个场景。

直播打图时，推荐自己制作一个Banner，给画面中除了游戏和摄像头的黑边部分放一些图片，改善观众的体验。

#### 3.1.2 一些设置

##### 编码优先级
OBS提供了从Fast到Slow的优先级，Slow会占用更多系统资源，推荐先设置为Very Fast再逐步调优。

##### 码率
码率就是你推流使用的最高上传速度，但是它会受物理带宽的限制，码率高于物理带宽时会出现大量的丢帧，在观众眼中就是转圈卡顿。但是较低的码率设置会导致在视频流每帧颜色变化较大时，尝试压缩视频体积，导致大量的马赛克。

##### 分辨率和帧率
高分辨率需要很高的码率，高帧数则需要更慢的编码优先级。

当然，1080p@60FPS能给观众带来清晰又顺滑的体验，当你的网络环境不允许时，尽可能先降低分辨率再降低帧率。

提示：虽然你的Banner可能要容纳1080p的游戏和320p的摄像头，进而远大于1080p，但是最终输出还是推荐进行缩放，以避免在投稿时被二次压制，同时也可以提高码率。

##### 最佳实践

首先测试你的上传带宽（注意KBps和kbps的不同），将码率设置为比带宽较小的值，进行直播测试。如果右下角提示丢帧，进一步减小码率。

根据最终的码率结果在下方三种方案中挑选一种，如果不丢帧的最高码率低于2500，建议放弃直播。

##### 1080p 60fps

视频分辨率: 1920*1080

码率: 4500 - 6000 kbps

帧数: 60 or 50 fps

关键帧间隔: 2 seconds

AVC (h.264) Profile: Main/High

AVC (h.264) Level: 4.2


##### 720p 60fps

视频分辨率: 1280*720

码率: 3500 to 5000 kbps

帧数: 60 or 50 fps

关键帧间隔: 2 seconds

AVC (h.264) Profile: Main/High

AVC (h.264) Level: 4.1

##### 720p 30fps

视频分辨率: 1280*720
     
码率: 2500 to 4000 kbps
     
帧数: 30 or 25 fps
     
关键帧间隔: 2 seconds
     
AVC (h.264) Profile: Main/High

AVC (h.264) Level: 3.1

### 3.2 直播比赛

近几年国内比赛增多（详见比赛页面），玩家的电脑配置也逐渐提升，因此参与比赛直播的人也越来越多。

#### 3.2.1 前置条件和直播端准备

首先你必须是osu! supporter，否则无法启动直播端。

复制一个新的osu客户端，建议不带Skins、Songs、Replays三个文件夹．然后打开新的客户端，登陆你的用户（需要是supporter），选择记住用户名和密码，版本选“正式版”，并把皮肤改成default．

退出后，在osu文件夹里新建一个空文件“tournament.cfg”，运行客户端，此时就应该会进入直播端，此时游戏会自动填写tournament.cfg。

#### 3.2.2 赛前准备：

编辑你的tournament.cfg，添加/修改如下一行：

```
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

视频输出的分辨率应修改为1280*720，直播端下方的设置界面可以无需播出。

### 3.3 实时显示歌曲信息：Sync

#### 3.3.1 主程序和默认插件下载

https://github.com/OsuSync/Sync/releases

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

##### 实时PP显示：

https://github.com/OsuSync/RealTimePPDisplayer/releases

将当前PP实时输出到内存映射/窗口

配置项说明：

OutputMethods
插件输出模式。WPF为独立窗口，MMF为内存映射文件，text为输出到硬盘文本，可以用逗号分隔，同时输出多个目标

UseText
单独控制是否输出文本（？）

TextOutputPath
文本路径

DisplayHitObject
是否展示点击统计（300/100/50/X的数量）

PPFontSize
PP字体

PPFontColor
PP字体颜色

HitObjectFontSize
点击统计字体

HitObjectFontColor
点击统计字体颜色

BackgroundColor
背景颜色，默认绿色，方便OBS采用色键功能抠图

WindowHeight
WPF窗口宽高
WindowWidth

SmoothTime
渐变时间

FPS
……

Topmost
WPF模式窗口是否置顶

WindowTextShadow
字体阴影

DebugMode
调试输出

RoundDigits
PP小数位

PPFormat
PP格式，默认为${rtpp}pp ，括号内支持三维/最大PP 具体参见github.com/OsuSync/RealTimePPDisplayer/wiki/How-to-customize-my-output-content%3F

HitCountFormat
点击统计格式，默认为${n100}x100 ${n50}x50 ${nmiss}xMiss，括号内支持内容参见上文

IgnoreTouchScreenDecrease
是否无视触屏Mod

RankingSendPerformanceToChat
对Rank图，输出PP到IRC

##### 屙屎信息源：

https://https://github.com/OsuSync/OsuRTDataProvider-Release/releases

读取osu!内存的插件，因此只有release而没有源码。

配置项说明：

ListenInterval
读取频率，默认即可。过小会导致资源浪费

EnableTourneyMode
对比赛端的支持

TeamSize
比赛端每队人数

ForceOsuSongsDirectory
强制osu!文件夹

GameMode
如果自动识别模式失败，需要手动指定

DisableProcessNotFoundInformation
是否隐藏“找不到osu!.exe信息”，支持True/False

EnableModsChangedAtListening
是否尝试在听歌时跟踪Mod变化



以下两个插件有中文文档，参考code页下的说明即可。


##### 谱面信息输出插件：

https://github.com/OsuSync/OsuLiveStatusPanel/releases

需要注意的是配置里AllowUsedMemoryReader改为1，而AllowUsedNowPlaying改成0，否则听歌时没有输出。

##### 歌词插件：

https://github.com/OsuSync/LyricDisplayerPlugin/releases

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

这个问题很久以前就提出来过，ppy对要求他加上asio的态度比较搞笑（他认为从键盘到音效响起的录音并不能说明问题，然而他又不自己给出测试方法，而且他也认为asio没有卵用）。即使到了lazer也不会用上能支持asio/wasapi的osu，请大家尽可能死心。

虽然我们不能真的拿枪怼他脸上逼着他做asio输出，但是对音效延迟忍无可忍的朋友可以尝试以下内容。

* linux下用wine运行osu，调整wine的DirectSound Buffer，最低可致13ms左右，远低于windows的40ms。有完善的方案，但是这样的音频输出质量极差，基本不能听。
* **关闭音效，全局-40ms，听按键盘的声音打图。**
* 推荐使用real.exe，可以稍微降低一点DirectSound延迟到30ms。差距比较小但是仍然有用。

**real.exe 使用说明：**

右键点击此电脑 -&gt; 选择“管理” -&gt; 选择左侧“设备管理器” -&gt; 展开“音频输入和输出” -&gt; 右键点击你的音频输出设备，选择“更新驱动程序和软件” -&gt; “浏览计算机以查找...” -&gt; “从计算机的设备驱动程序列表中选取...” -&gt; “通用软件设备” -&gt; 保存

开启real.exe即可。

### 注意事项

* osu!无论如何也不会通过asio接口输出的，而asio声卡跑DirectSound的延迟比板载声卡更高，一块Focusrite 2i4声卡甚至跑出了85ms的延迟。
* 不听音效也不能提高音频格式（位数和采样），osu最佳运行是16bit 44100hz，高于此格式均需要重采样，由于游戏的每帧采样特性，音频重采样会造成帧延迟，体现于光标变拖。
* 不使用的音频硬件请禁用，例如显卡的音频输出。

## 5.使用osu!提供的数据进行开发
本篇可能涉及到一些Web开发相关的知识，如果你有兴趣基于osu!的数据构建一款自己的应用，可以参考本篇！

###5.1 osu! API
这是ppy在2013年7月公布的一组API。

文档：https://github.com/ppy/osu-api/wiki

开始使用： 在 https://osu.ppy.sh/p/api 申请一个API KEY，信息随意填写。

限制：每分钟1200次，最高瞬时1400次。

共有的父URL：https://osu.ppy.sh/api/

---
#### 5.1.1谱面信息


/api/get_beatmaps

概览：返回通用的谱面信息。

URL：/api/get_beatmaps

---
参数：

k - api key （必须）

since - 返回在该日期之后ranked的所有谱面。必须是MySQL格式。

s - 指定一个谱面的SetID。(/s/xxxxx)

b - 指定一个谱面的Beatmap ID。（/b/xxxxx）

u - 指定一个用户名/用户数字id。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

m - 模式 (0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania). 默认返回所有模式的谱面。

a - 指定是否包括被转换的谱面（？） (0 = 不包括, 1 = 包括). 只在包含了m参数，并且不为0的情况下有效.被转换的谱面显示它们转换后的难度。默认为0。

h - 谱面哈希值。举个栗子，如果你尝试获取某个rep对应的谱面，而osr文件只包含谱面的哈希值。(例子：a5b99395a42bd55bc5eb1d2411cbdf8b). 默认情况下， 返回的谱面与Hash值无关。

limit - 返回值的数量. 默认值（同样是最大值）是500。

---
返回值：一个包含所有符合指定条件的、ranked谱面的JSON列表。每个难度一个列表。
```json
[{"approved" : "1", // 4 = loved, 3 = qualified, 2 = approved, 1 = ranked, 0 = pending, -1 = WIP, -2 = graveyard 
"approved_date" : "2013-07-02 01:01:12", // ranked日期, 时区为UTC+8 
"last_update" : "2013-07-06 16:51:22", // 最后更新日期，时区同上。 如果谱面被Unranked之后Reranked，该日期可能晚于上面的日期。
"artist" : "Luxion",
"beatmap_id" : "252002", // 每个难度的Beatmap_ID
"beatmapset_id" : "93398", // 所有难度的BeatmapSet_ID
"bpm" : "196",
"creator" : "RikiH_",
"difficultyrating" : "5.59516", // 游戏中和网站上的难度星数
"diff_size" : "4", // CS
"diff_overall" : "6", // OD
"diff_approach" : "7", // AR
"diff_drain" : "6", // HP
"hit_length" : "113", // 第一个note到最后一个note的间隔（不计算breaks）
"source" : "BMS",
"genre_id" : "1", // 0 = any, 1 = unspecified, 2 = video game, 3 = anime, 4 = rock, 5 = pop, 6 = other, 7 = novelty, 9 = hip hop, 10 = electronic (note that there's no 8) 
"language_id" : "5", // 0 = any, 1 = other, 2 = english, 3 = japanese, 4 = chinese, 5 = instrumental, 6 = korean, 7 = french, 8 = german, 9 = swedish, 10 = spanish, 11 = italian
"title" : "High-Priestess", // 歌曲名称
"total_length" : "145", // 第一个note到最后一个note的间隔（计算breaks） 
"version" : "Overkill", // 难度名
"file_md5" : "c8f08438204abfcdd1a748ebfae67421", // 谱面文件（不知道是.osz还是.osu）的MD5哈希值
"mode" : "0", // 模式, 
"tags" : "melodious long", // tags
"favourite_count" : "121", // 被收藏的次数。特别的，该条目采用英式英语，而不是美式英语（favorite_count）
"playcount" : "9001", // 被玩（。）的次数
"passcount" : "1337", // 被pass的次数（玩家没有fail或者retry）
"max_combo" : "2101" // 最大combo数
}, { ... }, ...]
```

#### 5.1.2 玩家信息

/api/get_user

概览：返回用户信息。

URL：/api/get_user

---
参数：

k - api key （必须）

u - 指定一个用户名/用户数字id。

m - 模式(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania).默认值为0。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

event_days - 打出最后成绩的日期（last event date），距离现在的最大天数。取值范围为1-31，默认值为1。
【实际使用时好像没有用。同时指定u参数和该参数，和直接指定u参数没有区别。单指定该参数没有返回。】

---
返回值：
包含用户信息的JSON列表。

```json
[{"user_id" : "1",
"username" : "User name",
"count300" : "1337", 
"count100" : "123", 
"count50" : "69", 
"playcount" : "42", // 以上四条为所有Ranked，Approved，Loved谱面中的计数
"ranked_score" : "666666", // 所有Ranked，Approved，Loved谱面中的最高分计数
"total_score" : "999999998", // 所有Ranked，Approved，Loved谱面中所有成绩的总分。
"pp_rank" : "2442",
"level" : "50.5050",
"pp_raw" : "3113", // 不活跃的玩家PP值是0，以将他们排除出PP榜单
"accuracy" : "98.1234",
"count_rank_ss": "54",
"count_rank_s" : "81", // 获得的SS S A的数目
"count_rank_a" : "862", 
"country" : "DE", // 使用ISO3166-1 alpha-2国家命名规范。 参考http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2/wiki/ISO_3166-1_alpha-2
"pp_country_rank":"1337", // 国内的PP排名
"events" : [{ // 记录这个用户上传的成绩【但是实际应用并没有获取到】
"display_html": "<img src='\/images\/A_small.png'\/>...",//评级对应的图片
"beatmap_id": "222342",
"beatmapset_id": "54851",
"date": "2013-07-07 22:34:04",
"epicfactor": "1" // 这个成绩有多么的“史诗级”，取值范围是1-32
}, { ... }, ...] 
}]
```

#### 5.1.3 按谱面获取成绩

/api/get_scores

概览：获取某张地图前100名的分数信息。

URL：/api/get_scores

---
参数：


k - api key （必须）

b - 指定一个谱面的Beatmap ID。（/b/xxxxx）（必须）

u - 指定一个要返回分数的用户名/用户数字id。

m - 模式 (0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania)，默认为0。

mods -指定一个或者一些mod (具体枚举映射参见后文)

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

limit - 返回值的数量. 默认值是50，最大值是100。

---
返回值：包含选定谱面前100分数信息的JSON列表。

```json
[{"score" : "1234567",
"username" : "User name",
"count300" : "300",
"count100" : "50",
"count50" : "10",
"countmiss" : "1",
"maxcombo" : "321",
"countkatu" : "10",
"countgeki" : "50",
"perfect" : "0", // 只有取得了地图的最大cb时，该值为1 
"enabled_mods" : "76", // 具体参见枚举
"user_id" : "1",
"date" : "2013-06-22 9:11:16",
"rank" : "SH",
"pp" : "1.3019" //4位小数
}, 

{...}, 

...]

```


```
enum Mods{
None = 0,
NoFail = 1,
Easy = 2,
//NoVideo = 4, 
//在2012年以前，关闭背景视频进行游戏是一个Mod，后被删除。
//在2017年下半年有玩家使用触屏打破PP记录后ppy重新启用这个位置，设为Touch Device Mod，对触屏PP大幅度削弱。
Touch Device = 4,
Hidden = 8,
HardRock = 16,
SuddenDeath = 32,
DoubleTime = 64,
Relax = 128,
HalfTime = 256,
Nightcore = 512, // 只和DT搭配使用。例如带NC的成绩，返回576（512+64）
Flashlight = 1024,
Autoplay = 2048,
SpunOut = 4096,
Relax2 = 8192,// Autopilot？
Perfect = 16384,
Key4 = 32768,
Key5 = 65536,
Key6 = 131072,
Key7 = 262144,
Key8 = 524288,
keyMod = Key4 | Key5 | Key6 | Key7 | Key8,
FadeIn = 1048576,
Random = 2097152,
LastMod = 4194304,
FreeModAllowed = NoFail | Easy | Hidden | HardRock | SuddenDeath | Flashlight | FadeIn | Relax | Relax2 | SpunOut | keyMod,
Key9 = 16777216,
Key10 = 33554432,
Key1 = 67108864,
Key3 = 134217728,
Key2 = 268435456
}
```

#### 5.1.4 玩家的BP

/api/get_user_best

获取指定玩家的BP

URL：/api/get_user_best

---
参数：

k - api key （必须）

u - 指定一个要返回分数的用户名/用户数字id。

m - 模式 (0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania)，默认为0。

limit - 返回值的数量. 默认值是10，最大值是100。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

---
返回值：包含了指定用户的BP前10的JSON列表。

```json
[{"beatmap_id" : "222342",
"score" : "1234567",
"username" : "User name",
"maxcombo" : "321",
"count300" : "300",
"count100" : "50",
"count50" : "10",
"countmiss" : "1",
"countkatu" : "10",
"countgeki" : "50",
"perfect" : "0", //只有取得了地图的最大cb时，该值为1 
"enabled_mods" : "76", //具体参见前文枚举
"user_id" : "1",
"date" : "2013-06-22 9:11:16",
"rank" : "SH",
"pp" : "1.3019" //四位小数
},
{ ... },
...]

```
#### 5.1.5 玩家最近的游戏记录


/api/get_user_recent

概览：获取玩家最近（24小时内）的10次游戏记录。

URL：/api/get_user_recent

---
参数：与获取BP一样，只不过limit的最大值是50。

返回值：包含玩家最近10次游戏记录的JSON列表。

字段与BP一致，不再赘述

#### 5.1.6 MP房间信息

/api/get_match

概览：返回一把mp的历史记录。

URL：/api/get_match

---
参数：
k - api key （必须）

mp - 房间id（必须）【也就是官网MP Link的参数】

---
返回值：包括房间信息和玩家成绩的JSON列表。

```json
[{"match":{"match_id" : "1936471",
"name" : "Marcin's game",
"start_time" : "2013-10-06 03:34:54",
"end_time" : null // 并不支持——永远是null },
"games":[{
"game_id" : "45668898",
"start_time" : "2013-10-06 03:36:27",
"end_time" : "2013-10-06 03:40:01",
"beatmap_id" : "181717",
"play_mode" : "0", // standard = 0, taiko = 1, ctb = 2, o!m = 3 
"match_type" : "0", // 并不支持
"scoring_type" : "0", // 胜利条件：score = 0, accuracy = 1, combo = 2, score v2 = 3
"team_type" : "0", // 比赛模式：Head to head = 0, Tag Co-op = 1, Team vs = 2, Tag Team vs = 3 
"mods" : "0", // 房间使用的Mod，参看前文 
"scores" : [{"slot" : "0", // 从0开始，按玩家所在的玩家位排列 
"team" : "0", // 未开启队伍模式则为0, 1为蓝队，2为红队
"user_id" : "722665",
"score" : "3415874",
"maxcombo" : "411",
"rank" : "0", //并不支持
"count50" : "0",
"count100" : "11",
"count300" : "425",
"countmiss" : "1",
"countgeki" : "67",
"countkatu" : "9",
"perfect" : "0", //最大CB
"pass" : "1" // 如果玩家在结尾失败了则为0，pass或者复活了则为1},
{ ... } ...] }, 
{ ... }, ...] 
}]
```

#### 5.1.7 获取回放

/api/get_replay


概览

获取指定玩家在指定谱面的rep。

并发请求限制说明：
该请求对服务器负担较大，每分钟只允许10次查询。同样请注意该请求不是为批处理预设的。

URL：/api/get_replay

---
参数

k - api key （必须）

m - 模式 (0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania)，默认为0。（必须）

b - 指定谱面id（注意！不是BeatmapSet ID，也就是说不是/s/xxxxx而是/b/xxxxx）（必须）

u - 指定玩家。（必须）

---
返回值：一个包含"content"值的JSON列表，该值中含有base-64加密的rep。

```json
{"content":"喵喵喵","encoding":"base64"}
```


请注意，这段二进制数据用base-64解码后，并不是.osr文件的目录。这是LZMA流，在osu-wiki中可以参看定义：

The remaining data contains information about mouse movement and key presses in an wikipedia:LZMA stream (https://osu.ppy.sh/wiki/Osr_(file_format)#Format)

### 5.1.8 osu://协议


osu://mp/<int mpID>/\[<string password>]

加入某个mp房间的连接。前文搞错了一点，这个mpID和前文API用的id不同。如果存在密码，就加上密码参数。

---
osu://edit/<xx:xx:xxx>\[ (x,x,x,x...)]


(x为0-9的整数。) xx:xx:xxx 是某首歌的具体时间点。x,x,x,x,x 没有数量限制 ，是被选中的一些Object，例如滑条，note，转盘等。

通常用于mod，直接在编辑器中选中这些Object，按Ctrl+C复制，再复制到论坛里，就是这样的连接了。

---
osu://chan/#<string ChanName>


加入osu!聊天频道的连接。(例如osu://chan/#italian)

---
osu://dl/<int mapsetID> 


打开某个谱面的osu!Direct下载。

---
osu://spectate/<String username or int userid> 
围观某人。


注意：使用这些连接时，参数并不会包含<>！

举个栗子：[osu://spectate/7679162 点击围观]

将这个链接发送到osu!中任何频道（当然为了社区礼仪，建议发送到私聊或者#announce）再点击，就可以开始围观uid是7679162的人，只要他在线。也可以直接在浏览器访问osu://spectate/7679162这个链接。


###5.2 osu! API v2

这是一些由[kj415j45](https://osu.ppy.sh/users/9367540)在[新官网的web项目中](https://github.com/ppy/osu-web)发掘的API，虽然随着新官网上线，但是并没有公布，功能也非常有限。

相比获取信息，API V2更有用的地方是OAuth，也就是玩家可以授权开发者的网站访问他的数据，而不需要直接输入osu!账号密码的机制。

共有的父URL：https://osu.ppy.sh/api/v2


#### 5.2.1 一次标准的鉴权+访问API流程

OAuth认证中，Client指用户和osu!官网之间的第三方网站。第三方网站可以以用户的身份访问osu!的数据，同时用户可以控制Client能访问的osu!数据内容的范围，而不至于需要向第三方网站提供密码。

如果你的项目不涉及其他玩家对你的授权，而只是你个人使用需要使用API V2返回的某些字段，也仍然需要创建Client。

你需要授权自己的Client访问自己的账户，获得Client访问账户用的access_token，进而以自己账户的身份来访问API V2；还需要编写定时任务，使用refresh_token来刷新自己的access_token。


##### 创建OAuth Client
```http request
POST https://osu.ppy.sh/oauth/clients
X-CSRF-TOKEN: {Your CSRF Token here}
Content-Type: application/json
Cookie: osu_session={Your osu_session in cookies};

{
    "name": "Example client",
    "redirect": "https://example.com/callback"
}
```

其中osu_session和X-CSRF-TOKEN可以在访问新官网后，在Cookie中找到。

只有第一次访问需要带osu_session，后续不需要。（这什么设定）

`redirect`参数指开发者服务器用于接收osu! 回调的接口，请认真填写。

返回：
```json
{
  "user_id": 12345,
  "name": "Example client",
  "secret": "****************************",
  "redirect": "https:\/\/example.com\/callback",
  "personal_access_client": false,
  "password_client": false,
  "revoked": false,
  "updated_at": "YYYY-mm-dd HH:ii:ss",
  "created_at": "YYYY-mm-dd HH:ii:ss",
  "id": 1
}
```
请妥善保存secret和id，接下来的授权过程会用到。


##### 引导用户授权

创建一个链接：

`https://osu.ppy.sh/oauth/authorize?response_type=code&client_id={$client_id}&redirect_uri={$redirect_uri}&state={$state}&scope={$scope}`

其中client_id和redirect_uri与创建Client时完全一致，state会在稍后回调时传回，而scope则可以控制Client要求访问用户内容的范围。

`identify` 将要求所有权限， `friends.read`则只能获取好友。 多个权限使用空格（%20）分割。


##### 接受回调

用户点击后，你预先填写的地址将会收到回调：`{$redirect_uri}?code={$code}&state={$state}`，其中code将会被用来鉴别点击了授权的用户。

```http request
POST https://osu.ppy.sh/oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id={$client_id}
&client_secret={$client_secret}
&redirect_uri={$redirect_uri}
&code={$code}
```
```json
{
    "token_type": "Bearer",
    "expires_in": 86400,
    "access_token": "****",
    "refresh_token": "****"
}
```
至此，你已经可以使用access_token以用户的身份访问osu! API v2了。

#####附带Token
在请求头中添加`Authorization: {$token_type} {$token}`即可。


#### 5.2.2 API V2

完整的列表参见：[ppy/osu-web:/routes/web.php@line//API](https://github.com/ppy/osu-web/blob/master/routes/web.php#L296-L378)

网站开发者用于鉴别用户身份时，常用`/me`接口：

```json
{
    "id": 9367540,
    "username": "kj415j45",
    "join_date": "2016-12-04T03:43:39+00:00",
    "country": {
        "code": "CN",
        "name": "China"
    },
    "avatar_url": "https://a.ppy.sh/9367540?1505577694.jpeg",
    "is_supporter": false,
    "has_supported": true,
    "is_gmt": false,
    "is_qat": false,
    "is_bng": false,
    "is_bot": false,
    "is_active": true,
    "interests": "osu!, code, steam",
    "occupation": "Student",
    "title": null,
    "location": "SWU(RongChang), China",
    "last_visit": "2019-02-10T07:51:00+00:00",
    "twitter": "kj415j45",
    "lastfm": null,
    "skype": "QQ: 919815238",
    "website": "https://kj415j45.space",
    "discord": "kj415j45#3309",
    "playstyle": ["keyboard", "tablet"],
    "playmode": "osu",
    "pm_friends_only": false,
    "post_count": 24,
    "profile_colour": null,
    "profile_order": ["me", "top_ranks", "recent_activity", "historical", "medals", "beatmaps", "kudosu"],
    "cover_url": "https://assets.ppy.sh/user-profile-covers/9367540/02ac0c9670be6d3ad00529245b53b0104384a6887fcf8bfe92ed68ecd2b28312.jpeg",
    "cover": {
        "custom_url": "https://assets.ppy.sh/user-profile-covers/9367540/02ac0c9670be6d3ad00529245b53b0104384a6887fcf8bfe92ed68ecd2b28312.jpeg",
        "url": "https://assets.ppy.sh/user-profile-covers/9367540/02ac0c9670be6d3ad00529245b53b0104384a6887fcf8bfe92ed68ecd2b28312.jpeg",
        "id": null
    },
    "kudosu": {
        "total": 0,
        "available": 0
    },
    "max_blocks": 25,
    "max_friends": 250,
    "account_history": [],
    "active_tournament_banner": [],
    "badges": [
	    {
	        "awarded_at": "2018-04-03T04:29:52+00:00",
	        "description": "Outstanding contribution and organization of the Chinese localisation project for osu!, osu!wiki and osu!lazer",
	        "image_url": "https://assets.ppy.sh/profile-badges/contributor.jpg"
	    }, {...}, ...
    ],
    "favourite_beatmapset_count": [52],
    "follower_count": [164],
    "graveyard_beatmapset_count": [0],
    "loved_beatmapset_count": [0],
    "monthly_playcounts": [
	    {
	        "start_date": "2016-12-01",
	        "count": 269
	    }, {...}, ...
    ],
    "page": {
        "html": "",
        "raw": ""
    },
    "previous_usernames": [],
    "ranked_and_approved_beatmapset_count": [0],
    "replays_watched_counts": [
	    {
	        "start_date": "2017-07-01",
	        "count": 1
	    }, {...}, ...
    ],
    "scores_first_count": [0],
    "statistics": {
        "level": {
            "current": 83,
            "progress": 59
        },
        "pp": 1826.98,
        "pp_rank": 146494,
        "ranked_score": 1647237303,
        "hit_accuracy": 89.6849,
        "play_count": 4768,
        "play_time": 365093,
        "total_score": 3861238114,
        "total_hits": 1140103,
        "maximum_combo": 1089,
        "replays_watched_by_others": 3,
        "is_ranked": true,
        "grade_counts": {
            "ss": 9,
            "ssh": 4,
            "s": 123,
            "sh": 18,
            "a": 196
        },
        "rank": {
            "global": 146494,
            "country": 3752
        },
        "scoreRanks": {
            "XH": 4,
            "SH": 18,
            "X": 9,
            "S": 123,
            "A": 196
        }
    },
    "support_level": 0,
    "unranked_beatmapset_count": [0],
    "user_achievements": [
        {
            "achieved_at": "2019-02-09T05:45:17+00:00",
            "achievement_id": 47
        }, {...}, ...
    ]
}
```





#### 5.2.3 其他认证相关的接口

#####刷新Token
```http request
POST https://osu.ppy.sh/oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&client_id=1
&client_secret=*************
&refresh_token=**********************
```
```json
{
    "token_type": "Bearer",
    "expires_in": 86400,
    "access_token": "****",
    "refresh_token": "****"
}
```

##### 查询Client

```http request
GET https://osu.ppy.sh/oauth/clients
X-CSRF-TOKEN: {Your CSRF Token here}
Content-Type: application/json
```
返回：
```json
[
	{
	  "user_id": 9367540,
	  "name": "Example client",
	  "secret": "****************************",
	  "redirect": "https:\/\/example.com\/callback",
	  "personal_access_client": false,
	  "password_client": false,
	  "revoked": false,
	  "updated_at": "YYYY-mm-dd HH:ii:ss",
	  "created_at": "YYYY-mm-dd HH:ii:ss",
	  "id": 1
	},{},...
]
```
##### 修改Client
```http request
PUT https://osu.ppy.sh/oauth/clients/{client-id}
X-CSRF-TOKEN: {Your CSRF Token here}
Content-Type: application/json

{
    "name": "Example client",
    "redirect": "https://example.com/callback"
}
```
返回：
```json
{
  "user_id": 9367540,
  "name": "Example client",
  "redirect": "https:\/\/example.com\/callback",
  "personal_access_client": false,
  "password_client": false,
  "revoked": false,
  "updated_at": "YYYY-mm-dd HH:ii:ss",
  "created_at": "YYYY-mm-dd HH:ii:ss",
  "id": 1
}
```

##### 撤销Client
```http request
DELETE https://osu.ppy.sh/oauth/clients/{client-id}
X-CSRF-TOKEN: {Your CSRF Token here}
Content-Type: application/json
```
返回：
```
200 ok
```
需要注意的是，撤销一个已经撤销的Client也会返回200，而撤销其他人的Client返回的是404而不是401。


### 5.3 osu!游戏更新信息接口

URL：https://osu.ppy.sh/web/check-updates.php?action=check&stream=stable40

参数：stream可以切换为fallback、cutting edge（请自行抓包获取）。

返回内容：一个完整的、可直接运行的、不带本地化数据的osu! 客户端的每个文件的信息和下载地址。

**如果你有一台流量充足、位于国外的服务器，可以使用此API定时爬取客户端，进而搭建客户端镜像。**

```json
[
    {
        "file_version":"1349",
        "filename":"avcodec-51.dll",
        "file_hash":"734e450dd85c16d62c1844f10c6203c0",
        "filesize":"4416072",
        "timestamp":"2015-04-07 05:42:04",
        "patch_id":null,
        "url_full":"http://m1.ppy.sh/r/avcodec-51.dll/f_734e450dd85c16d62c1844f10c6203c0"
    },
    {
        "file_version":"1350",
        "filename":"avformat-52.dll",
        "file_hash":"9c492c0792c4302b4ffaf5f27e48c443",
        "filesize":"717896",
        "timestamp":"2015-04-07 05:45:29",
        "patch_id":null,
        "url_full":"http://m2.ppy.sh/r/avformat-52.dll/f_9c492c0792c4302b4ffaf5f27e48c443"
    },
    {
        "file_version":"1351",
        "filename":"avutil-49.dll",
        "file_hash":"8462c4461db36bc03392ee9576e3d34e",
        "filesize":"68680",
        "timestamp":"2015-04-07 05:46:37",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/avutil-49.dll/f_8462c4461db36bc03392ee9576e3d34e"
    },
    {
        "file_version":"4",
        "filename":"bass.dll",
        "file_hash":"d7f05d3fa5e745e02e1de41821ccccaf",
        "filesize":"98872",
        "timestamp":"2014-08-18 08:17:03",
        "patch_id":"1352",
        "url_full":"http://m3.ppy.sh/r/bass.dll/f_d7f05d3fa5e745e02e1de41821ccccaf",
        "url_patch":"http://m3.ppy.sh/r/bass.dll/p_d7f05d3fa5e745e02e1de41821ccccaf_5ac9068249ad443dfa7b0211330681a5"
    },
    {
        "file_version":"1353",
        "filename":"bass_fx.dll",
        "file_hash":"ede8b6bfd91068ee477e1b6cd438cd0c",
        "filesize":"36000",
        "timestamp":"2015-04-07 05:46:56",
        "patch_id":"2207",
        "url_full":"http://m3.ppy.sh/r/bass_fx.dll/f_ede8b6bfd91068ee477e1b6cd438cd0c",
        "url_patch":"http://m3.ppy.sh/r/bass_fx.dll/p_ede8b6bfd91068ee477e1b6cd438cd0c_3975f6195c3e01c78c97dfc27d75ff06"
    },
    {
        "file_version":"1354",
        "filename":"Microsoft.Ink.dll",
        "file_hash":"340a75fd6e365fc85cfbd04ae295fa28",
        "filesize":"522312",
        "timestamp":"2015-04-07 05:47:22",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/Microsoft.Ink.dll/f_340a75fd6e365fc85cfbd04ae295fa28"
    },
    {
        "file_version":"3108",
        "filename":"osu!.exe",
        "file_hash":"16cf7d54234e4d155310a5b4732a130c",
        "filesize":"4188864",
        "timestamp":"2019-04-10 12:12:14",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/osu!.exe/f_16cf7d54234e4d155310a5b4732a130c"
    },
    {
        "file_version":"3101",
        "filename":"osu!ui.dll",
        "file_hash":"92f3d4a63961c1af25761c9ba63d7fbc",
        "filesize":"36204736",
        "timestamp":"2019-04-01 02:09:20",
        "patch_id":null,
        "url_full":"http://m2.ppy.sh/r/osu!ui.dll/f_92f3d4a63961c1af25761c9ba63d7fbc"
    },
    {
        "file_version":"1347",
        "filename":"pthreadGC2.dll",
        "file_hash":"1d5ab200f8d19cd9e65eb06ccdf6d59a",
        "filesize":"66496",
        "timestamp":"2015-04-07 05:38:06",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/pthreadGC2.dll/f_1d5ab200f8d19cd9e65eb06ccdf6d59a"
    },
    {
        "file_version":"2487",
        "filename":"osu!gameplay.dll",
        "file_hash":"8e04440f100b4389f99b94689b6c3727",
        "filesize":"31843896",
        "timestamp":"2016-04-03 10:19:00",
        "patch_id":null,
        "url_full":"http://m2.ppy.sh/r/osu!gameplay.dll/f_8e04440f100b4389f99b94689b6c3727"
    },
    {
        "file_version":"2428",
        "filename":"OpenTK.dll",
        "file_hash":"87d7774466c2efa2cb801e5fb2ce7b52",
        "filesize":"4306112",
        "timestamp":"2016-02-17 11:30:37",
        "patch_id":null,
        "url_full":"http://m1.ppy.sh/r/OpenTK.dll/f_87d7774466c2efa2cb801e5fb2ce7b52"
    },
    {
        "file_version":"1753",
        "filename":"d3dcompiler_47.dll",
        "file_hash":"c5b362bce86bb0ad3149c4540201331d",
        "filesize":"3466856",
        "timestamp":"2015-08-14 08:35:25",
        "patch_id":null,
        "url_full":"http://m2.ppy.sh/r/d3dcompiler_47.dll/f_c5b362bce86bb0ad3149c4540201331d"
    },
    {
        "file_version":"2426",
        "filename":"libEGL.dll",
        "file_hash":"1b3a1cf8aed3f567a1b4ac6b6696cd05",
        "filesize":"138432",
        "timestamp":"2016-02-17 11:29:50",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/libEGL.dll/f_1b3a1cf8aed3f567a1b4ac6b6696cd05"
    },
    {
        "file_version":"2427",
        "filename":"libGLESv2.dll",
        "file_hash":"6d52dba415b86bb99268b0156b835451",
        "filesize":"3356352",
        "timestamp":"2016-02-17 11:30:11",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/libGLESv2.dll/f_6d52dba415b86bb99268b0156b835451"
    },
    {
        "file_version":"2993",
        "filename":"osu!spooky.dll",
        "file_hash":"efb604a2b5f4b69c1f8d3f29645cba2b",
        "filesize":"18375360",
        "timestamp":"2018-10-23 15:21:13",
        "patch_id":null,
        "url_full":"http://m3.ppy.sh/r/osu!spooky.dll/f_efb604a2b5f4b69c1f8d3f29645cba2b"
    },
    {
        "file_version":"3042",
        "filename":"osu!seasonal.dll",
        "file_hash":"bd12579112f0e44a3c4bc6a2b4aee1c7",
        "filesize":"16965312",
        "timestamp":"2018-12-12 07:30:36",
        "patch_id":null,
        "url_full":"http://m1.ppy.sh/r/osu!seasonal.dll/f_bd12579112f0e44a3c4bc6a2b4aee1c7"
    },
    {
        "file_version":"2870",
        "filename":"discord-rpc.dll",
        "file_hash":"250e4d35ed51ac293527865ca2080c2b",
        "filesize":"304728",
        "timestamp":"2017-12-03 03:38:22",
        "patch_id":null,
        "url_full":"http://m1.ppy.sh/r/discord-rpc.dll/f_250e4d35ed51ac293527865ca2080c2b"
    }
]
```

> 本篇贡献者: Mother Ship