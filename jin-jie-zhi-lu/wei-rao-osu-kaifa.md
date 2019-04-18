#围绕osu!进行开发

本篇可能涉及到一些Web开发相关的知识，如果你有兴趣基于osu!的数据构建一款自己的应用，可以参考本篇！

### 5.1 osu! API

这是ppy在2013年7月公布的一组API。

文档：[https://github.com/ppy/osu-api/wiki](https://github.com/ppy/osu-api/wiki)

开始使用： 在 [https://osu.ppy.sh/p/api](https://osu.ppy.sh/p/api) 申请一个API KEY，信息随意填写。

限制：每分钟1200次，最高瞬时1400次。

共有的父URL：[https://osu.ppy.sh/api/](https://osu.ppy.sh/api/)

#### 5.1.1谱面信息

/api/get\_beatmaps

概览：返回通用的谱面信息。

URL：/api/get\_beatmaps

参数：

k - api key （必须）

since - 返回在该日期之后ranked的所有谱面。必须是MySQL格式。

s - 指定一个谱面的SetID。\(/s/xxxxx\)

b - 指定一个谱面的Beatmap ID。（/b/xxxxx）

u - 指定一个用户名/用户数字id。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

m - 模式 \(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania\). 默认返回所有模式的谱面。

a - 指定是否包括被转换的谱面（？） \(0 = 不包括, 1 = 包括\). 只在包含了m参数，并且不为0的情况下有效.被转换的谱面显示它们转换后的难度。默认为0。

h - 谱面哈希值。举个栗子，如果你尝试获取某个rep对应的谱面，而osr文件只包含谱面的哈希值。\(例子：a5b99395a42bd55bc5eb1d2411cbdf8b\). 默认情况下， 返回的谱面与Hash值无关。

limit - 返回值的数量. 默认值（同样是最大值）是500。

返回值：一个包含所有符合指定条件的、ranked谱面的JSON列表。每个难度一个列表。

```javascript
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

/api/get\_user

概览：返回用户信息。

URL：/api/get\_user

参数：

k - api key （必须）

u - 指定一个用户名/用户数字id。

m - 模式\(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania\).默认值为0。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

event\_days - 打出最后成绩的日期（last event date），距离现在的最大天数。取值范围为1-31，默认值为1。 【实际使用时好像没有用。同时指定u参数和该参数，和直接指定u参数没有区别。单指定该参数没有返回。】

返回值： 包含用户信息的JSON列表。

```javascript
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

/api/get\_scores

概览：获取某张地图前100名的分数信息。

URL：/api/get\_scores

参数：

k - api key （必须）

b - 指定一个谱面的Beatmap ID。（/b/xxxxx）（必须）

u - 指定一个要返回分数的用户名/用户数字id。

m - 模式 \(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania\)，默认为0。

mods -指定一个或者一些mod \(具体枚举映射参见后文\)

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

limit - 返回值的数量. 默认值是50，最大值是100。

返回值：包含选定谱面前100分数信息的JSON列表。

```javascript
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

```text
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

/api/get\_user\_best

获取指定玩家的BP

URL：/api/get\_user\_best

参数：

k - api key （必须）

u - 指定一个要返回分数的用户名/用户数字id。

m - 模式 \(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania\)，默认为0。

limit - 返回值的数量. 默认值是10，最大值是100。

type - 指定u参数是数字id还是用户名。对于数字id，该参数值为id，而用户名则参数值为string。默认为智能识别，在纯数字用户名时可能出现问题。

返回值：包含了指定用户的BP前10的JSON列表。

```javascript
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

/api/get\_user\_recent

概览：获取玩家最近（24小时内）的10次游戏记录。

URL：/api/get\_user\_recent

参数：与获取BP一样，只不过limit的最大值是50。

返回值：包含玩家最近10次游戏记录的JSON列表。

字段与BP一致，不再赘述

#### 5.1.6 MP房间信息

/api/get\_match

概览：返回一把mp的历史记录。

URL：/api/get\_match

参数： k - api key （必须）

mp - 房间id（必须）【也就是官网MP Link的参数】

返回值：包括房间信息和玩家成绩的JSON列表。

```javascript
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

/api/get\_replay

概览

获取指定玩家在指定谱面的rep。

并发请求限制说明： 该请求对服务器负担较大，每分钟只允许10次查询。同样请注意该请求不是为批处理预设的。

URL：/api/get\_replay

参数

k - api key （必须）

m - 模式 \(0 = osu!, 1 = Taiko, 2 = CtB, 3 = osu!mania\)，默认为0。（必须）

b - 指定谱面id（注意！不是BeatmapSet ID，也就是说不是/s/xxxxx而是/b/xxxxx）（必须）

u - 指定玩家。（必须）

返回值：一个包含"content"值的JSON列表，该值中含有base-64加密的rep。

```javascript
{"content":"喵喵喵","encoding":"base64"}
```

请注意，这段二进制数据用base-64解码后，并不是.osr文件的目录。这是LZMA流，在osu-wiki中可以参看定义：

The remaining data contains information about mouse movement and key presses in an wikipedia:LZMA stream \([https://osu.ppy.sh/wiki/Osr\_\(file\_format\)\#Format](https://osu.ppy.sh/wiki/Osr_%28file_format%29#Format)\)

### 5.1.8 osu://协议

osu://mp//\[\]

加入某个mp房间的连接。前文搞错了一点，这个mpID和前文API用的id不同。如果存在密码，就加上密码参数。

osu://edit/\[ \(x,x,x,x...\)\]

\(x为0-9的整数。\) xx:xx:xxx 是某首歌的具体时间点。x,x,x,x,x 没有数量限制 ，是被选中的一些Object，例如滑条，note，转盘等。

通常用于mod，直接在编辑器中选中这些Object，按Ctrl+C复制，再复制到论坛里，就是这样的连接了。

osu://chan/\#

加入osu!聊天频道的连接。\(例如osu://chan/\#italian\)

osu://dl/

打开某个谱面的osu!Direct下载。

osu://spectate/ 围观某人。

注意：使用这些连接时，参数并不会包含&lt;&gt;！

举个栗子：\[osu://spectate/7679162 点击围观\]

将这个链接发送到osu!中任何频道（当然为了社区礼仪，建议发送到私聊或者\#announce）再点击，就可以开始围观uid是7679162的人，只要他在线。也可以直接在浏览器访问osu://spectate/7679162这个链接。

### 5.2 osu! API v2

这是一些由[kj415j45](https://osu.ppy.sh/users/9367540)在[新官网的web项目中](https://github.com/ppy/osu-web)发掘的API，虽然随着新官网上线，但是并没有公布，功能也非常有限。

相比获取信息，API V2更有用的地方是OAuth，也就是玩家可以授权开发者的网站访问他的数据，而不需要直接输入osu!账号密码的机制。

共有的父URL：[https://osu.ppy.sh/api/v2](https://osu.ppy.sh/api/v2)

#### 5.2.1 一次标准的鉴权+访问API流程

OAuth认证中，Client指用户和osu!官网之间的第三方网站。第三方网站可以以用户的身份访问osu!的数据，同时用户可以控制Client能访问的osu!数据内容的范围，而不至于需要向第三方网站提供密码。

如果你的项目不涉及其他玩家对你的授权，而只是你个人使用需要使用API V2返回的某些字段，也仍然需要创建Client。

你需要授权自己的Client访问自己的账户，获得Client访问账户用的access\_token，进而以自己账户的身份来访问API V2；还需要编写定时任务，使用refresh\_token来刷新自己的access\_token。

**创建OAuth Client**

\`\`\`http request POST [https://osu.ppy.sh/oauth/clients](https://osu.ppy.sh/oauth/clients) X-CSRF-TOKEN: {Your CSRF Token here} Content-Type: application/json Cookie: osu\_session={Your osu\_session in cookies};

{ "name": "Example client", "redirect": "[https://example.com/callback](https://example.com/callback)" }

```text
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

**引导用户授权**

创建一个链接：

`https://osu.ppy.sh/oauth/authorize?response_type=code&client_id={$client_id}&redirect_uri={$redirect_uri}&state={$state}&scope={$scope}`

其中client\_id和redirect\_uri与创建Client时完全一致，state会在稍后回调时传回，而scope则可以控制Client要求访问用户内容的范围。

`identify` 将要求所有权限， `friends.read`则只能获取好友。 多个权限使用空格（%20）分割。

**接受回调**

用户点击后，你预先填写的地址将会收到回调：`{$redirect_uri}?code={$code}&state={$state}`，其中code将会被用来鉴别点击了授权的用户。

\`\`\`http request POST [https://osu.ppy.sh/oauth/token](https://osu.ppy.sh/oauth/token) Content-Type: application/x-www-form-urlencoded

grant\_type=authorization\_code &client\_id={$client\_id} &client\_secret={$client\_secret} &redirect\_uri={$redirect\_uri} &code={$code}

```text
```json
{
    "token_type": "Bearer",
    "expires_in": 86400,
    "access_token": "****",
    "refresh_token": "****"
}
```

至此，你已经可以使用access\_token以用户的身份访问osu! API v2了。

**附带Token**

在请求头中添加`Authorization: {$token_type} {$token}`即可。

#### 5.2.2 API V2

完整的列表参见：[ppy/osu-web:/routes/web.php@line//API](https://github.com/ppy/osu-web/blob/master/routes/web.php#L296-L378)

网站开发者用于鉴别用户身份时，常用`/me`接口：

```javascript
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

**刷新Token**

\`\`\`http request POST [https://osu.ppy.sh/oauth/token](https://osu.ppy.sh/oauth/token) Content-Type: application/x-www-form-urlencoded

grant\_type=refresh\_token &client\_id=1 &client\_secret=**\*** &refresh\_token=**\*\***

```text
```json
{
    "token_type": "Bearer",
    "expires_in": 86400,
    "access_token": "****",
    "refresh_token": "****"
}
```

**查询Client**

\`\`\`http request GET [https://osu.ppy.sh/oauth/clients](https://osu.ppy.sh/oauth/clients) X-CSRF-TOKEN: {Your CSRF Token here} Content-Type: application/json

```text
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

**修改Client**

\`\`\`http request PUT [https://osu.ppy.sh/oauth/clients/{client-id}](https://osu.ppy.sh/oauth/clients/{client-id}) X-CSRF-TOKEN: {Your CSRF Token here} Content-Type: application/json

{ "name": "Example client", "redirect": "[https://example.com/callback](https://example.com/callback)" }

```text
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

**撤销Client**

\`\`\`http request DELETE [https://osu.ppy.sh/oauth/clients/{client-id}](https://osu.ppy.sh/oauth/clients/{client-id}) X-CSRF-TOKEN: {Your CSRF Token here} Content-Type: application/json

```text
返回：
```

200 ok

```text
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