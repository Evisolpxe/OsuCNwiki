---
description: osu相关工具一览
---

# Review of osu! Tools

### ranking

#### 基于pp的排名

* 官方pp
  * 项目网址：[https://github.com/ppy/osu-performance](https://github.com/ppy/osu-performance)
  * 简介：官方的pp计算器，不需要介绍了。
* pp+
  * 项目网址：[https://syrin.me/pp+](https://syrin.me/pp+)
  * 简介：从aim、speed等多个角度计算某个成绩的pp，相比官方的单pp更加科学。
* osuskills
  * 项目网址：[http://osuskills.com/](http://osuskills.com/)
  * 简介：类似pp+，但是其算法看上去不如pp+靠谱。
* delta\_t's PP Rework Calculator
  * 项目网址：[https://newpp.stanr.info/](https://newpp.stanr.info/)
  * 简介：类似于官方pp，主要削弱了所谓的pp图。目前只能重算bp上的100个成绩，因此仅供参考。

**计算pp的插件**

* ezpp
  * 项目网址：[https://github.com/oamaok/ezpp](https://github.com/oamaok/ezpp)
  * 简介：一个浏览器插件，当访问谱面的官网地址时可以方便计算该图的pp（而不必下载谱面）。
* oppai
  * 项目网址：[https://github.com/Francesco149/oppai-ng](https://github.com/Francesco149/oppai-ng)
  * 简介：经常被调用的pp计算器。

#### 基于mp表现的排名

* elo
  * 项目网址：[http://otsu.fun/home](http://otsu.fun/home)
  * EloWeeklyCup群：738401694
  * 简介：elo等级分制度是一个衡量各类对弈活动水平的评价方法，广泛用于棋牌、球类等体育赛事。在osu!中，elo系统将根据玩家mp的分数表现更新玩家的elo，随着玩家mp次数的增加，elo应当将收敛至符合该玩家水平分段的数值。目前elo系统由[Explosive](https://osu.ppy.sh/users/245276)负责主要开发，敬请期待！
* maidbot（osu!matchmaking）
  * 项目网址：[https://oma.hwc.hr/](https://oma.hwc.hr/)
  * discord邀请链接：[https://discord.gg/CehChep](https://discord.gg/CehChep)
  * 简介：osu!matchmaking是一个面向osu!std的mp系统，同样使用elo作为评价标准。主要特点在于搭建的自动mp机器人（maidbot）以及自带图池，玩家可以随时进行排位mp。具体使用方法请加入discord服务器了解。

### mapping

* osumapper
  * 项目网址：[https://github.com/kotritrona/osumapper](https://github.com/kotritrona/osumapper)
  * 简介：基于深度学习的自动作图器。
* mappingtools
  * 项目网址：[https://mappingtools.seira.moe/](https://mappingtools.seira.moe/)
  * 简介：将各种作图辅助软件集合到这一个程序中（顺便一提我是从[fanzhen](https://osu.ppy.sh/users/418699)关于XNOR的[推文](https://twitter.com/fanzhen0019/status/1297837187984064512?s=20)里找到的这个）。

### collections

* osu Collections Editor
  * 项目网址：[https://github.com/Kurocon/Osu-Collections-Editor](https://github.com/Kurocon/Osu-Collections-Editor)
  * 简介：一个本地收藏夹编辑器，可以方便地编辑collections.db文件，例如添加、删除歌曲，合并收藏夹等，适合批量操作。可以与 [https://osustats.ppy.sh/collections](https://osustats.ppy.sh/collections) 联动，下载别人分享的收藏夹然后与本地的合并。
* collection sharing
  * 项目网址：[https://collections.osustuff.ri.mk/](https://collections.osustuff.ri.mk/)
  * 简介：[arily](https://osu.ppy.sh/users/1123053)写的。同样是一个收藏夹分享网站，可以上传自己的收藏夹或下载别人的分享。可以用它来传赛图。
* Collection-Helper
  * 项目网址：[https://github.com/nzbasic/Collection-Helper](https://github.com/nzbasic/Collection-Helper)
  * 简介：比较新的收藏夹管理软件，特点在于可以自定义筛选谱面。

### replay

* circleguard
  * 项目网址：[https://github.com/circleguard/circleguard](https://github.com/circleguard/circleguard)
  * 简介：著名的查rep挂软件。可视化做得很好，也可以用来分析自己为什么miss。
* circlevis
  * 项目网址：[https://github.com/circleguard/circlevis](https://github.com/circleguard/circlevis)
  * 简介：circleguard使用的replay播放器。
* osuReplayAnalyzer
  * 项目网址：[https://github.com/firedigger/osuReplayAnalyzer](https://github.com/firedigger/osuReplayAnalyzer)
  * 简介：查rep挂。
* replay to video
  * 项目网址：[https://github.com/uyitroa/osr2mp4-app](https://github.com/uyitroa/osr2mp4-app)
  * 简介：将replay转成视频。

### stream

* streamCompanion
  * 项目网址：[https://github.com/Piotrekol/StreamCompanion](https://github.com/Piotrekol/StreamCompanion)
  * 简介：主要可以显示实时pp与谱面信息，还有其他一些功能例如Key counter等等。
* osuSync
  * 项目网址：[https://github.com/OsuSync](https://github.com/OsuSync)
  * 简介：一系列osu直播插件合集，包含pp计算器、当前谱面信息抓取、同步聊天等等。
* Bongo Cat
  * 项目网址：[https://github.com/kuroni/bongocat-osu](https://github.com/kuroni/bongocat-osu)
  * 另一个版本？：[https://github.com/ZeCryptic/osu-cat](https://github.com/ZeCryptic/osu-cat)
* kps
  * 项目网址：[https://github.com/yugecin/osukps](https://github.com/yugecin/osukps)
  * 简介：显示按键状态、每秒按键数（kps）、累计按键次数。
* gosumemory
  * 项目地址：[https://github.com/l3lackShark/gosumemory](https://github.com/l3lackShark/gosumemory)
  * 简介：CPOL录视频以及许多top直播使用的工具，可以自己定制出非常好看的直播界面。还支持比赛直播。

### bot

这部分其实笔者不是很熟悉，但是又觉得有介绍的必要，所以只是列出了一些开源的，还在持续更新的，同时功能比较完善的或者比较有名的项目。

#### QQ

* 白菜
  * 项目地址：[https://github.com/Mother-Ship/cabbageWeb](https://github.com/Mother-Ship/cabbageWeb)
  * 简介：可能是最早的osu的qq bot（指点江山以下略，，）
* osu! 新人群 bot
  * 项目地址：[https://github.com/b11p/OsuQqBotForNewbieGroup](https://github.com/b11p/OsuQqBotForNewbieGroup)
  * 简介：只能说看着介绍写得比较全而且还在更新，笔者没有用过

#### Discord

Discord 是国外osu社区用的比较多的聊天软件，因为是hosted chat server的模式（而不是IM），很适合用来组织比赛，而且开发bot比qq方便很多。但是需要梯子。

* flowabot
  * 项目地址：[https://github.com/LeaPhant/flowabot](https://github.com/LeaPhant/flowabot)
  * 简介：除了基本的查询信息之外，有很多有意思的功能，比如渲染谱面视频、输出谱面star/BPM的时域变化折线图等等。

#### osu! irc

* osu-ahr
  * 项目地址：[https://github.com/Meowhal/osu-ahr](https://github.com/Meowhal/osu-ahr)
  * 简介：通过 osu irc 在mp时实现自动轮换房主的bot，包括创建房间、开始游戏、关闭房间等等功能，使用房内玩家投票的方式决定是否跳过房主、是否abort等等。这个bot可以迁移到discord上，通过discord频道方便地设置bot的各种参数。

### statistics

#### 数据追踪

* osu!track
  * 网址：[https://ameobea.me/osutrack/](https://ameobea.me/osutrack/)
  * 简介：可能是比较早的爬个人数据的网站。能看到非常古老的个人bp，很适合老玩家怀旧。
* osuTracker
  * 网址：[https://osutracker.com/](https://osutracker.com/)
  * 简介：主要特点是除了个人之外还有基于国家（或地区）的统计数据，比如可以看 CN 所有玩家的 bp 合成一个人的话是多少 pp。此外还有一些关于 osu! 有趣的数据统计结果可以欣赏。
* osu! Stats
  * 网址：[https://osustats.ppy.sh/](https://osustats.ppy.sh/)
  * 简介：统计玩家所有全球排名前200左右的谱面，一天更新一次。另外页面最下面会显示前一天最高的25个pp记录。相比bp来说，上榜的成绩也许更能体现出实力。

#### PP图统计

有一些网站统计了玩家最常刷的pp图；当然也可以通过这些网站来找pp图。

* [https://osu-pps.com/#/osu/maps](https://osu-pps.com/#/osu/maps)
* [https://pp.bcs.dev/](https://pp.bcs.dev/)

#### 时光机-Wayback Machine

如果你曾经点过维基百科的参考资料链接，你可能会知道「[保存整个互联网](https://web.archive.org/)」计划（需要梯子），它爬取了从1996年起的很多网页并存了档（当然笔者对这个计划也是一知半解，如介绍有错误还请见谅）。你可以从这里看到十年前的b站、微博的网页是什么样的。当然，我们回到osu!。很遗憾的是我们看不到过去每一天的自己的个人主页，因为时光机没有去爬当时的osu官网。但幸运的是2017年12月15日左右，似乎osu.ppy.sh的所有页面都被存档了一次，如果你那时已经入坑了，可以搜搜自己的主页，也可以看看当时的世界排名榜和国内排名榜，比如下面这张图是 2017.11.3 的全球top10玩家。2017.12.15可能是最早的有全部页面存档的日期，这几年爬取的频率变高了，也许五年或者十年后，你可以再回来看看。

![这里有一张 2017.11.3 Top10玩家的截图](https://s3.bmp.ovh/imgs/2021/12/0a4026ab9f0770d2.png)
<!-- https://imgur.com/a/mPClCR1 -->

### mirrors

* bloodcat
  * [https://bloodcat.com/osu/](https://bloodcat.com/osu/)
  * 备注：过去的血猫已经倒了，现在会跳转到[https://chimu.moe/](https://chimu.moe/)，也能用。
* sayo
  * [https://osu.sayobot.cn/home](https://osu.sayobot.cn/home)
  * 浏览器插件（配合[TamperMonkey](https://www.tampermonkey.net/)使用）：[https://osu.sayobot.cn/sayobot.user.js](https://osu.sayobot.cn/sayobot.user.js)
  * sayo还开发了[地图下载器](https://osu.sayobot.cn/download/)，可以更方便地批量下载谱面，有需可用。
* inso
  * [http://inso.link/yukiho/](http://inso.link/yukiho/)

### 播放器

* osu player
  * 项目地址：[https://github.com/Milkitic/Osu-Player](https://github.com/Milkitic/Osu-Player)
  * 简介：[yf_bmp](https://osu.ppy.sh/users/1243669)写的osu播放器，除了音乐外还可以播放hitsound、video和storyboard等。
