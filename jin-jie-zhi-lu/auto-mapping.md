---
description: Auto-mapping 综述
---

# Auto-mapping 综述

## 简介

本文是基于 AI 的 osu! 自动作图模型综述。该模型的功能是输入一段音频，输出符合音乐节奏的、类似人类 mapper 制作出的 beatmap。一些商业音游已经采用了这些手段辅助人类作图（比如 Love Live![^Takada]），但我们主要关注与 osu! 相关的自动作图进展。另外我们排除 BPM 变速图、跑道图等的自动作图工具，我们希望模型能够学习到人类作图的知识而不仅仅是生成。

说是综述其实只是一些论文的汇总介绍，行文没有仔细打磨。另外也还没有直接对 std 模式的论文，基本都是下落式模式，毕竟 std 作为二维平面相比一维的下落式或者太鼓建模麻烦点。。。

## 相关工作

### Beatmap generator for Osu Game using machine learning approach

目前找到的最早的有关 osu 的自动作图论文是2015年一位印尼人写的[^Maulidevi]。游戏平台是 mania4K。通过一些库提取音频特征然后摆放音符，虽然可以成功检测到节拍和旋律，但无法控制生成的 beatmap 的难度。这篇论文写得比较粗糙（公式甚至用位图而且非常糊。。），而且不是基于深度学习的方法好像是传统机器学习（SVM）的方法，感觉价值不大了。

### Procedural Content Generation of Rhythm Games Using Deep Learning Methods

比较有价值的工作来自于2019年 Yubin Liang 等人写的使用深度学习实现音乐游戏的 PCG（procedural content generation）[^Liang]。游戏平台是 mania4K。系统流程是用一套模型从音频生成 timpstamp ，然后基于 timestamp 再用另一套模型生成 action type，结合 timestamp 和 action type 可以生成 beatmap。

所以其实我们想要的自动作图是一个叫做 PCG 方向下的具体实现。深度学习在 PCG 上的应用有一篇综述可以看看[^Liu]，这里就不只是音游了。PCG 其实更早是用在 2d 横板过关游戏的关卡自动生成上。

### TaikoNation: Patterning-focused Chart Generation for Rhythm Action Games

重量级的工作是 2021 年 Emily Halina 等提出的 TaikoNation 模型[^Halina]。游戏平台是 osu! taiko。作者的 osu id 是 [Kaifin](https://osu.ppy.sh/users/2596942)（从导师推特的 follower 找到作者推特才发现这位就是 Kaifin。。。）。这项工作有3个主要贡献：

- 提出了一个给定音频自动生成 taiko beatmap 的 LSTM 架构；
- 建立了一个有 110 个 charts 的 taiko beatmap 数据集；
- 与 Dance Dance Convolution 模型（见下文）的对比

模型训练时先将音频分成 23ms 的小段提取特征，然后将对应的这一段人类制作的 beatmap 提取特征，两个东西扔进模型里训练预测后4个 notes；生成 heatmap 时将预测窗在音频上滑动，得到预测的 notes 从而得到最终完整的 beatmap。
作者强调了 human-like patterning 这一概念，指人类作图时往往重复出现某些特定的组合（以表现音乐主题），这一模式将是模型主要的学习对象。
对音频的处理：23ms 大约是 163BPM 下两个 1/64 拍的 notes 时间间隔，作者认为这样的切片对大部分 BPM 音乐而言都比较合适。切片的音频使用 short-time Fourier transform 提取特征。
对 beatmap 的处理：即对 .osu 文件的解析，不多介绍。
网络这一块反而没什么好说的了，就 ReLu、dropout、卷积、全连接、LSTM，softmax，本人完全不懂。训练用的 CPU 是 AMD Ryzen 5 3600x，GPU 是 NVIDIA GeForce 1080 Ti GPU，花了2个半小时。生成时会用较长的滑动窗口对音乐进行预测，在每个 timestamp 上得到多个预测，选择其中概率最高的作为最终结果，这种预测方式提高了模型的记忆性。生成的 beatmap 还需要一些处理实现最终可玩。

Evaluation 也是比较重要的部分。以前的模型会把检测起始点和作图分成两个任务，但 TaikoNation 是这两个任务一起做的。选择的 baseline 一个是 DDC，因为其他人也用而且和 taiko 比较像；另一个是在 timestamp 上随机生成 note 形成的 beatmap。作者收集了两个游戏平台都收录的10首音乐，以及这些音乐的上架 beatmap。作者制定了5条标准衡量生成 beatmap 的质量（没看懂说实话）：

- DCRand：与随机生成的 beatmap 的直接对比，衡量模型引入了多少结构；
- DCHuman：与人类制作的 beatmap 的直接对比，后者相当于是 golden standard，因此这条相当于是模型的准确率；
- OCHuman：相当于更加宽松的 DCHuman，如果生成的 beatmap 没有在人类制作的 beatmap 的 timestamp 上有 note，就看前后的 timestamp 上有没有 note，这条相当于是模型的相似性；
- Over.P-Space：找出独特模式，旨在揭露模型覆盖了多少模式空间；
- HI P-Space：衡量模式使用的 Human-like pattern 的比例。

基于以上标准，作者实验的结果表明 TaikoNation 相比 DDC 使用了更多的 human-like pattern，并且生成的 beatmap 中的 note 密度分布和人类制作的更加相似。作者指出由于 DDR 和 taiko 的 note 类型存在区别，导致无法直接比较两种方法的 note 类型摆放选择，但从 note 类型的分布来看，TaikoNation 和人类还是比较接近的。在起始点检测任务上二者没有显著差异，作者认为这和用于预测的滑动窗口有关。进一步的实验需要更多的人类评价，这将留待未来完成。（2021年8月左右作者发了招收 taiko 志愿者的推，不过我没有看到后续）

作者提到模型目前生成的 beatmap 只能算作是一个“近似值”。可以让玩家通过双盲测试对 beatmap 的特定方面进行评分，进一步帮助研究者深入了解模型的学习能力。

作者认为未来的发展方向应当是人类与 AI 合作制图，例如 AI 可以作为新手 mapper 的指导老师，或者为成熟 mapper 提供灵感。另外，其他音乐游戏（SDVX）具有独特的游戏对象（旋钮），可以开发出一种通用架构能够针对不同音游进行调整。以上就是这篇论文的主要内容。

有兴趣的还可以看一下 [workshop](http://www.pcgworkshop.com/index.php)，录像在 [这里](https://herts-ac-uk.zoom.us/rec/play/DXcbDiDn2yEtgZHpYqdrDWQRR0UtShnSzOBCkoG0fBHXmU2JuDKWy_OWvqokDZy4gEuaNxS3FEZN5DU3.WIYDoCo3KPNpakwQ?continueMode=true&_x_zm_rtaid=SY9h4bt_QRGq__AH9ce7-g.1648229651961.c796ecd306dd04a07f848d03b4178d7d&_x_zm_rhtaid=146)，大概2小时的地方，听作者介绍自己的工作和 Q&A。
TaikoNation 代码已公开：<https://github.com/emily-halina/TaikoNationV1>
TakoNation 参考了很多 Dance Dance Convolution[^Donahue]，一个基于 Dance Dance Revolution 游戏平台的模型（可能外国人玩 DDR 的比较多）DDC 的代码也是公开的：<https://github.com/chrisdonahue/ddc>。

作者推特上公开了下一步的工作是 KiaiTime（A Demonstration of KiaiTime: A Mixed-Initiative PCGML Rhythm Game Editor），部分论文已经可以看到[^Halina2]。游戏平台仍然是 taiko。这是一个辅助人类作图的系统，mapper 可以从 AI 生成的 beatmap 中产生灵感，共同完成制作。

### 一些不是很有用但还可以的工作

2019年 Zhiyu Lin 等人提出的 GenarationMania[^Lin]。游戏平台是 Beatmania。这个模型要解决的问题是自动生成 keysound（按下按键重现音乐的某些声音），相对来说 osu 不是很看重 keysound，所以这个问题在 osu 里不是很重要，这点 Halina 也提到了。

然后看到比较有意思的还有2021年 Kim 等人提出的通过视频就可以提取音游 beatmap 的模型，用于生成数据集[^Kim]。当然 osu 的 beatmap 相当于是全公开的，所以我们用不到。

一些研究者选择直接“自制”音游，这些音游本身就具有给定音频自动生成 beatmap 的功能，例如[^Salsabilla][^Yeh] 。

### 其他项目

- [Ar3](https://osu.ppy.sh/users/989563) 做的 osu! ai beatmap generator "osumapper"，全模式可用，可以用 colab 跑，比较完整
  - 帖子：<https://osu.ppy.sh/community/forums/topics/791481?n=1>
  - 代码：<https://github.com/kotritrona/osumapper>
  - 生成谱面示例：<https://osu.ppy.sh/beatmapsets/1290030#osu/2678072>
- 前几年有人做的自动生成 beatmap，作者已经弃坑
  - 博客：<https://www.nicksypteras.com/blog/aisu.html>：
  - 代码：<https://github.com/Syps/osu_beatmap_generator>
  - 生成谱面示例：<https://www.youtube.com/watch?v=E7yl1_HKcxw&list=PLXHqh2k-tZQeTHQ_s_kbrfhYQaYajfbpg>
- 基于前者
  - 代码：<https://github.com/NalianLive/AIMapper>
- 17年的东西，看起来也没维护过了
  - 代码：<https://github.com/Ehaschia/osu-mapper>

参考文献直接从 Zotero 里拉的，格式就不管了。

[^Maulidevi]: Perkasa D B, Maulidevi N U. Beatmap generator for Osu Game using machine learning approach[C]//2015 International Conference on Electrical Engineering and Informatics (ICEEI). 2015: 77-81Denpasar, Bali, Indonesia: IEEE, 2015: 77-81.

[^Liang]: Anonymous. Entertainment Computing and Serious Games: First IFIP TC 14 Joint International Conference, ICEC-JCSG 2019, Arequipa, Peru, November 11–15, 2019, Proceedings[M]. Van Der Spek E, Göbel S, Do E Y-L, et al., eds.. Cham: Springer International Publishing, 2019.

[^Liu]: Liu J, Snodgrass S, Khalifa A, et al. Deep learning for procedural content generation[J]. Neural Computing and Applications, 2021, 33(1): 19-37.

[^Takada]: Takada A, Yamazaki D, Liu L, et al. Gen\’eLive! Generating Rhythm Actions in Love Live![J]. arXiv:2202.12823 [cs, stat], 2022.

[^Halina]: Halina E, Guzdial M. TaikoNation: Patterning-focused Chart Generation for Rhythm Action Games[J]. The 16th International Conference on the Foundations of Digital Games (FDG) 2021, 2021: 1-10.

[^Donahue]: Donahue C, Lipton Z C, McAuley J. Dance Dance Convolution[J]. [no date]: 10. .

[^Halina2]: Halina E, Guzdial M. A Demonstration of KiaiTime: A Mixed-Initiative PCGML Rhythm Game Editor[J]. [no date]: 3. .

[^Kim]: Kim Y, Choi S. Vision-based beatmap extraction in rhythm game toward platform-aware note generation[C]//2021 IEEE Conference on Games (CoG). 2021: 1-5Copenhagen, Denmark: IEEE, 2021: 1-5.

[^Lin]: Lin Z. GenerationMania: Learning to Semantically Choreograph[J]. [no date]: 7. .

[^Salsabilla]: Salsabilla G A, Fabroyir H, Herumurti D, et al. Tachyon: Multiplatform Rhythm Game with Automatic Beatmap Generation[C]//2020 International Conference on Computer Engineering, Network, and Intelligent Multimedia (CENIM). 2020: 162-167Surabaya, Indonesia: IEEE, 2020: 162-167.

[^Yeh]: Yeh T-C, Jang J-S R. AutoRhythm : A Music Game With Automatic Hit-Timing Generation and Percussion Identification[J]. IEEE Transactions on Games, 2020, 12(3): 291-301.

> 本篇贡献者: Miracle
