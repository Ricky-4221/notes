- [
  首页](https://km.woa.com/?kmref=km_header)
- [发现](https://km.woa.com/discovery?kmref=km_header)
- [悦读](https://km.woa.com/read?kmref=km_header)
- [乐问](https://km.woa.com/q?kmref=km_header)
- [直播](https://tencent.lexiangla.com/lives?company_from=tencent&kmref=km_header)
- [应用](javascript:void(0))
- [我的K吧](javascript:void(0))
- [![img](https://dayu.oa.com/avatars/rickyrqzhao/profile.jpg?0) ](https://km.woa.com/user/rickyrqzhao)
- 

[![img](https://km.woa.com/files/groups/icons/49100.jpg)](https://km.woa.com/group/yybalg)[应用宝算法](https://km.woa.com/group/49100) 

[加入K吧](javascript:void(0);) 

- [首页](https://km.woa.com/group/49100)
- [微博](https://km.woa.com/group/49100/twitters)
- [讨论](https://km.woa.com/group/49100/topics)
- [文章](https://km.woa.com/group/49100/articles)
- [活动](https://km.woa.com/group/49100/events)
- [投票](https://km.woa.com/group/49100/surveys)
- [日历](https://km.woa.com/group/49100/calendars)
- [相册](https://km.woa.com/group/49100/photos/albums)
- [视频](https://km.woa.com/group/49100/videos)
- 

目录

目录大纲

1、推荐系统背景

1.1 推荐系统本质

1.2 推荐系统分类

1.2.1长内容推荐

1.2.2短/即时内容推荐

1.2.3用户行为的重要性

1.3 应用宝推荐场景

\2. 推荐系统如何理解用户行为

2.1 传统序列特征

2.2 hard search

2.3 soft search

2.4 search方法融合

2.5 search分析&总结

3、排序模型如何建模用户行为

3.1 target attention模型

3.2 self-attention模型

4、推荐模型的泛化能力探索

4.1 share embedding机制

4.2 引入行为向量embedding

4.2.1 引入更长周期预训练模型 

4.2.2 预训练模型为模型带来了什么

4.3 引入文本向量embedding

4.4 multi-channel embedding

\5. 总结

[![img](https://km.woa.com/img/km_pictures/km_headline_icon.png)](https://km.woa.com/base/headlines)

【万字长文】应用宝推荐排序综述

- [jimberxin](https://km.woa.com/user/jimberxin)
-  

- 2021年09月06日 09:55
-  

- 浏览(771)
-  

- [收藏(105)](javascript:void(0);)
-  

- [评论(17)](javascript:void(0);)
-  

- [分享](javascript:void(0);)

| 导语 用户行为作为推荐系统最为重要的特征，在大部分排序模型里是缺乏个性化的。对于推荐的长尾item，业界主流推荐模型也是缺乏泛化能力的。本文结合大规模用户稀疏场景的推荐系统，以应用宝的推荐排序发展为线索，讲述下我们在用户行为序列检索、序列模型建模、以及模型的泛化能力的一些探索和尝试。

文末总结了47页的PDF附件：[应用宝推荐排序综述.pdf](https://km.woa.com/base/attachments/attachment_view/244885)

全文约1.6万字，预计需要花费40-60分钟阅读

本文以应用宝推荐系统的探索之路作为主线，浅谈个人对稀疏低频推荐场景的一些理解，仅代表个人看法。

行文匆匆，难免有错漏之处，还请指正赐教

------

# **目录大纲**

[1、推荐系统背景](https://km.woa.com/group/49100/articles/show/485104#_Toc81771693)

[ 1.1 推荐系统本质](https://km.woa.com/group/49100/articles/show/485104#_Toc81771694)

[ 1.2 推荐系统分类](https://km.woa.com/group/49100/articles/show/485104#_Toc81771695)

[ 1.2.1长内容推荐](https://km.woa.com/group/49100/articles/show/485104#_Toc81771696)

[ 1.2.2短/即时内容推荐](https://km.woa.com/group/49100/articles/show/485104#_Toc81771697)

[ 1.2.3用户行为的重要性](https://km.woa.com/group/49100/articles/show/485104#_Toc81771698)

[1.3 应用宝推荐场景](https://km.woa.com/group/49100/articles/show/485104#_Toc81771699)

[2、推荐系统如何理解用户行为](https://km.woa.com/group/49100/articles/show/485104#_Toc81771700)

[ 2.1 传统序列特征](https://km.woa.com/group/49100/articles/show/485104#_Toc81771701)

[ 2.2 hard search](https://km.woa.com/group/49100/articles/show/485104#_Toc81771702)

[ 2.3 soft search](https://km.woa.com/group/49100/articles/show/485104#_Toc81771703)

[ 2.4 search方法融合](https://km.woa.com/group/49100/articles/show/485104#_Toc81771704)

[ 2.5 search分析&总结](https://km.woa.com/group/49100/articles/show/485104#_Toc81771705)

[3、排序模型如何建模用户行为](https://km.woa.com/group/49100/articles/show/485104#_Toc81771706)

[ 3.1 target attention模型](https://km.woa.com/group/49100/articles/show/485104#_Toc81771707)

[ 3.2 self-attention模型](https://km.woa.com/group/49100/articles/show/485104#_Toc81771708)

[4、推荐模型的泛化能力探索](https://km.woa.com/group/49100/articles/show/485104#_Toc81771709)

[ 4.1 share embedding机制](https://km.woa.com/group/49100/articles/show/485104#_Toc81771710)

[ 4.2 引入行为向量embedding](https://km.woa.com/group/49100/articles/show/485104#_Toc81771711)

[ 4.2.1 引入更长周期预训练模型](https://km.woa.com/group/49100/articles/show/485104#_Toc81771712)

[ 4.2.2 预训练模型为模型带来了什么](https://km.woa.com/group/49100/articles/show/485104#_Toc81771713)

[ 4.3 引入文本向量embedding](https://km.woa.com/group/49100/articles/show/485104#_Toc81771714)

[4.4 multi-channel embedding](https://km.woa.com/group/49100/articles/show/485104#_Toc81771715)

[5、总结](https://km.woa.com/group/49100/articles/show/485104#_Toc81771716)

------

 

# 1、推荐系统背景

## 1.1 推荐系统本质

​    在信息过载的时代，传统的电商货架式、门户网站分类索引等信息检索的方式已经越来越难以满足用户的信息获取需求。过去几年的发展中，推荐业务迎来了很多爆发性增长，如图1.1所示，从早期Netflix的电影和图书推荐，到后面各大互联网公司相关的推荐产品如图文feeds流推荐、电商推荐、视频推荐、社交推荐、短视频推荐、O2O推荐、音乐推荐、app推荐等。

![img](https://km.woa.com/files/photos/pictures/b01/65a1ddd955c755303b3a37a3f2b25_w830_h414.png)

​                   图1.1 推荐系统广泛应用于互联网产品

  推荐系统作为中间的物品分发平台，主要起到的作用是连接用户以及item，如图1.2所示。我个人在前面的文章[《推荐系统中的深度匹配模型》](https://km.woa.com/articles/show/439871)中，也从技术的角度解释了推荐系统的本质，其实就是做好user和item的match匹配。

![img](https://km.woa.com/files/photos/pictures/899/c4c075e99cb1896c8c02b6659524b_w640_h274.png)

​           图1. 推荐系统起到连接用户和item作用

   图1.2左侧代表的是推荐系统的C端生态，是推荐系统服务面向的主要目标，也是决定推荐产品能否存活的关键。其中用户来源、用户群分布、用户心智、用户意图、用户所处生命周期、用户在产品中的交互反馈行为等，对于推荐系统来说是非常重要的信息输入。一个推荐产品的成功与否，最重要的一个决定因素是产品是否能满足左侧用户的需求。

   图1.2右侧代表的是推荐系统的B端生态，代表的是满足用户需求的物料供给情况。物料item的候选集大小、物料item的更新频次、新物品的更新速度、item背后的作者/开发者/创作者的活跃/留存/激励等情况，都决定了整个推荐系统的B端item库的生态情况。一个推荐系统如果没有足够丰富和良好的item供给，也很难长期生存下去。

## 1.2 推荐系统分类

   在我个人的认知和理解范围内，推荐系统大体上可以分为以下两类，分别是短内容推荐和长内容推荐（仅代表个人看法，欢迎探讨），两者区别如下图1.3所示：

![img](https://km.woa.com/files/photos/pictures/796/32bc5747864e726bd84c755d59241_w810_h692.png)

​              图1.3 短内容推荐算法友好，短内容推荐算法不友好

### 1.2.1长内容推荐

   这类推荐系统特点是右侧item库相对来说变化频次慢、左侧user特点是用户决策成本高、信息反馈慢、推荐的决策周期长。典型推荐产品代表有：爱奇艺/优酷/腾讯视频等长视频app的电影和连续剧推荐、番茄/七猫/起点读书等读书app的小说推荐、淘宝/拼多多/京东等电商app的大宗商品推荐、安居客/贝壳/中原等房产app的租房买房推荐、腾讯应用宝的应用app和游戏推荐等。

   对于电影推荐来说，用户点击开始播放推荐系统推荐的一部电影，表面看是一个播放动作，背后代表的决策是用户愿意用1-2个小时去花在这部电影上。对应用推荐来说，用户决定下载一个app，不是简单的反映在数据上花不到一秒钟的点击动作那么简单，背后代表的是用户未来几个月甚至更长时间愿意花时间和精力在一款软件或者游戏app上。对于大宗商品如租房、买房、电视机等来说，其背后更是代表未来几年甚至几十年用户的选择抉择。这种决策成本高、反馈周期长的长内容推荐系统，用户可能好几天甚至更久才过来体验一次产品，也决定了用户的反馈一定是大规模且非常稀疏的。

### 1.2.2短/即时内容推荐

   这类推荐系统特点是右侧item库相对来说物料更为丰富、变化频次高，左侧user特点是用户决策成本低、信息反馈快、推荐的决策周期短。典型的代表有抖音/快手/微视的短视频和直播推荐、腾讯新闻/网易新闻/今日头条等的feeds图文推荐。用户在信息流里刷新闻，看到标题觉得不错点进来看完就走，点进来发现不是自己想看的退出就是；刷一个短视频，觉得好看就看完甚至多看几遍，高兴了点个赞发个评论，不好看直接划走。

   由于反馈周期短、用户和推荐系统交互代表的用户背后决策和心里选择成本也很低，用户沉浸在这类短内容中，很容易留下非常丰富的反馈和兴趣，留给推荐系统进一步完善用户体验。

### 1.2.3用户行为的重要性

   都说数据决定了模型的上限，我个人的观念里，无论是长内容还是短内容推荐，用户的行为决定了整个数据的上限。以视频推荐为例，过去看过很多美食制作视频的用户会倾向于接着看美食视频。对于推荐模型来说，学到的信息就是：待推荐item为美食视频类目时，转化率显著高于均值，打分会更高，系统会倾向于向用户推荐美食制作视频。如果用户是用脚投票的，或者用户的点击行为是完全随机的，比如用户过去不管有过什么历史行为，对于当前推荐系统推荐的视频，点击转化率都一样。那么推荐系统对于曾经看过美食制作视频的人来说，再也不会推荐出来美食制作了，因为这个时候推荐其他类型的视频，转化率是一样的，推荐系统这个时候的推荐就是随机推荐。

   因此，用户和推荐系统的交互反馈数据，对于模型能学多精准是非常非常关键的。用户交互过程数据越稀疏、随机性和不确定性越大，对推荐来说越难以学习。

## 1.3 应用宝推荐场景

   腾讯应用宝作为一个应用分发平台，连接着用户和app开发者，承担的C端职责是给用户推荐软件和游戏app。如图1.4所示，应用宝是个非常典型的长内容推荐产品，有发现首页、游戏首页、软件首页、必备弹窗等很多推荐场景。

![img](https://km.woa.com/files/photos/pictures/b7f/89a67e96e49c7d8ffaef261ae5f67_w852_h388.png)

​           图1.4应用宝推荐场景是典型的长内容推荐场景

   作为长内容推荐系统，应用宝推荐一样面临着大规模稀疏的问题。大部分用户将应用商店作为工具类产品，有下载近期新热app、重大版本更新等需求的时候才会打开应用宝，整个应用宝月活跃天只有xx天（业务脱敏）。同时由于下载app代表着用户决定在这个软件或者游戏app花时间去体验，注定了用户在应用宝留下的行为也是很稀疏的，用户月下载中位数只有xx个（业务脱敏）。

​    除了行为稀疏性以外，大部分推荐系统整个数据链路都是闭环的，也就是item侧内容、用户对item的交互反馈都是发生在推荐系统之内的。而对于应用宝来说，整个推荐的链路是不闭环的。用户选择下载app的渠道除了应用宝，还有其他厂商应用商店、短视频、信息流和搜索引擎等，来自应用宝的安装占比只有不到xx（业务脱敏）。

![img](https://km.woa.com/files/photos/pictures/528/aa4290f46acdddfc1d15a1ae84832_w644_h294.png)

​           图1.5 有很多行为发生在应用宝推荐系统之外

   这些大量发生在推荐系统之外的行为，对我们来说，一方面既是非常重要的用户兴趣表达，例如用户安装了迷你世界，无论是来自应用宝还是其他地方，都体现了用户对沙盒游戏的偏好，这些外部行为是我们做好建模很重要的数据来源。另一方面，由于数据发生在推荐系统之外导致的数据的不可控，这里面一定又充斥着大量的噪音，如何理解好用户的这些行为对应用推荐来说也是不小的挑战。

   接下来的几章中，我将围绕着如何做好这些稀疏行为用户理解、模型如何抽象建模行为特征、如何做好长尾流量的泛化等工作做详细的介绍。

# 2. 推荐系统如何理解用户行为

## 2.1 传统序列特征

   前面第1章提到了，用户的历史行为特征，对于排序模型来说，是最为重要的特征，没有之一。传统的用户历史行为序列，主要有以下几个问题：

   **（1）行为序列没有区分分度**

​      我们说个性化推荐系统，体现在用户之间，对于用户A和用户B来说，由于用户画像以及用户的历史行为不同，推荐列表是不一样的。但是在排序模型中，对于同一用户来说，不管预估的item是什么，输入到模型中的历史行为都是一样的，缺乏个性化。

   **（2）历史行为中存在着很多无关的噪音**

​      在应用推荐领域，比如说当前要预估用户下载欢乐斗地主这个游戏的概率，那么用户历史行为中下载苏宁易购、前尘无忧这些行为对模型来说大概率是没有帮助的。在电商领域，要预估用户购买华为手机pro的概率，那么用户历史上对母婴产品的购买行为，其实很大概率也是没有什么用的。这些用户历史行为中，和target相关性较低的历史行为，如果全部都作为特征引入模型，在大规模和稀疏性并存的推荐系统领域，很多时候由于与item的共现pair对出现次数太少，对模型来说很难学习好。

![img](https://km.woa.com/files/photos/pictures/7ce/6cc84ed6fc5e59d8ee17d91d08d66_w830_h438.png)

​           图2.1 传统模型对历史行为序列特征使用方式

   **（3）按照最近N个行为做截断的缺点**

​      业界大部分使用用户历史行为特征的方法，绝大部分都是拿用户最近一段时间的N个行为做截断。这种方法的最大问题在于，很多时候出于线上存储和计算开销角度考虑，N的取值都不会太大，比如几十、几百这样。如果N取得太大，达到几千甚至更高，除了计算和存储压力变大，也会引入更多噪音，加大模型的学习负担。

​      这种按照近期历史行为做截断的做法，也和推荐系统的业务形态有关。对大部分推荐系统来说，用户的近期历史行为都是最重要的。甚至说，当行为足够多的时候，用户更长时间以前的历史行为，其实就显得没那么重要了。如果非要考虑长时间以前的历史兴趣，可以根据时间对历史行为兴趣做衰减然后做成用户的长期兴趣。在信息流推荐里面，用户几个月前浏览了什么文章、看了什么短视频、点赞了哪条动态，可能用户自己都已经忘了，即便模型里有关于兴趣权重的表达，可能权重也几乎衰减到0了。几个月前用户在看NBA相关资讯，然后赛季结束了用户不关注了，推荐系统对NBA的推荐也会变少。而等新赛季开始用户继续关注，这个时候随着用户短期行为发生变化，系统也自然会有更多NBA相关资讯的推荐。我个人理解，在这种短期行为（例如信息流、直播、短视频、o2o外卖/餐饮等业务）足够丰富且非常重要的场景，长期历史行为的丢失其实影响没那么大。

​     我们既然知道用户不同历史行为，对于预估当前不同item的影响不同，那么一种很自然的想法，就是根据预估item的不同，用来做排序特征的用户历史行为序列也应该是不同的。

![img](https://km.woa.com/files/photos/pictures/e93/71c87c68e72e9d3a29999a8522a34_w830_h452.png)

​           图2.2 基于检索的历史行为序列特征模式

   具体做法是，在排序模型内部，我们将整个排序过程看成两阶段。第一阶段是检索，把当前要预估的item作为query，用户的历史行为做为doc文档。从用户历史行为中，检索出来我们认为相关的历史行为，其余部分直接丢弃。如图2.2所示，当前要预估的item是绿色的item1，只检索出绿色相关的历史行为；要预估的item是红色的item4，只检索出红色相关的历史行为。这个思路其实和阿里妈妈2020年的SIM模型思路是类似的。

   在接下来的模型中，我们将其分为两步，第一步是行为检索，也就是Behavior Search，第二步是精准匹配，也就是传统意义上的排序模型。

## 2.2 hard search



   对于第一阶段的行为检索，核心就是定义相关性。一种非常启发性的思路，就是用item所在的类目作为相关性的标准。

   应用市场的分类体系作为三级类目体系，我们统计了下用户历史行为，和预估的item是否match的cvr。可以发现，类目相关match特征为强兴趣特征。下面的表表示，给用户推荐他历史上有过行为的同三级类目app，对比其他类目的app，前者是后者cvr的2.3倍。

| tag         | match平均cvr | not-match cvr | cvr提升比例 |
| ----------- | ------------ | ------------- | ----------- |
| app三级类目 | xx%          | xx%           | 2.30        |
| app二级类目 | xx%          | xx%           | 2.10        |
| app一级类目 | xx%          | xx%           | 2.03        |

   因此，我们上线的第一个版本，以类目作为相关性检索，同类目就是相关，不同类目直接丢弃。

 ![img](https://km.woa.com/files/photos/pictures/7e7/a6d4a97cc8177baca24e4b26f2181_w776_h510.png)

​              图2.3 Hard search检索框架

   以图2.4为例，待预估的item是城市飞车手游，检索出的历史行为序列是QQ飞车、跑跑卡丁车以及登山赛车。

![img](https://km.woa.com/files/photos/pictures/578/3fd6b7b078aef9063c97e5b386726_w728_h366.png)

​       图2.4 待预估item作为query检索出的同类目序列

​    由于引入了search，在行为数据上我们引入了用户的全量安装数据，以及将用户的历史行为从90天，扩到用户的全生命周期（用户安装应用宝至今的所有历史数据）。行为数据的大量扩充也必然引入了大量噪音，我们通过同类目相关实现数据噪音的过滤。

   整个hard search实现框架非常简单，整体用户的行为数据构建为一个key1-key2-value的结构，一级索引key1 为user，二级索引key2为类目category，value为该类目下的行为序列，也可扩展为类目相关的行为序列。在我们的线上实现中，实际上这个过程是个特征抽取match算子的过程。Match算子基本逻辑为：

   Input：用户历史行为数据、target item、item类目信息

   Output：用户历史hard match item。

在hard search模型中我们引入了全生命周期的用户历史行为，在业务中我们知道不同时间段的行为影响是不同的，因此在模型中我们实际上将用户的历史行为分成了3种，7天内是短期行为；7-180天是中长期历史行为，超过180天是全生命周期行为。

   从离线AUC结果来看，不同周期序列的影响是不同的。如图2.5所示，短期（7D）的行为如果做hard search，效果反而是负向的，对于中长期以上的行为AUC则分别用2个以上千分点的提升。经过分析，我们发现其实用户短期历史行为普遍较短，中位数不到XX个（业务脱敏）且大多和预估item有较强相关性，因此对于短期行为序列我们不做search，保留全部行为；对于中长期&全生命周期历史行为数据，由于序列长且噪音多，通过hard search效果有所提升。

![img](https://km.woa.com/files/photos/pictures/2c5/a920bcebb350296e4818dfad0e3c4_w826_h422.png)

​      图2.5 不同周期序列特征做hard search的AUC提升结果

   也就是说，对于中长期特征，如果引入全部特征，对比hard search特征，效果反而是下降的。现在问题来了：在模型里加入了一个特征，有可能导致模型效果下降么？

   在早期LR线性模型时代还是以特征工程为主的时代里，我们认为一个特征的引入，如果它没有区分度，最多没有收益，至少不会引起负向效果。没有区分度的特征，从LR权重分布来看，因为它不足以区分正负样本，正向权重和负向权重应该分布在0左右。一个特征加入模型要拿到正向收益，我认为至少需要满足两个条件：

   第一个是它和当前预估的item有相关性，也就是说对于正负样本具有较强的区分能力。比如，当前处于一个小时的第几分钟，从先验角度来说，对于用户是否会下载一个游戏显然转化率是没有区分度的，那么它就不是个有区分度的特征，引入模型是没有帮助的。再比如说，男性如果对于篮球新闻的点击率，远远高于女性对于篮球新闻的点击率，那么性别对于推荐篮球新闻来说，就是个很有区分度的很好的特征。

   第二是在当前的特征体系中没有过表达，也就是新的特征必须给系统引入新的信息量。一个强特征，如果在模型里其他特征已经有过表达，比如说，对于用户的安装列表数据，在早期的特征工程工作中，我们将其看成doc进行LDA主题概率分解，得到每个app的隐主题向量后，可以统计得到用户的安装列表隐主题兴趣，在LR模型加入该特征后拿到了一定收益。后期随着我们特征体系的不断完善，从多个角度如各种统计、itemCF、word2vec/DeepWalk等序列模型不断进行安装列表的挖掘，发现LDA主题兴趣特征去除以后，对模型效果已经基本没有影响。通过一些数据分析，我们猜测大概率是因为早期的模型中没有安装列表的一些表达，引入LDA能拿到收益。而后期安装列表在模型中已经有了多个维度的特征表达，LDA已经很难再给模型带来新的信息量，自然就没有收益。

   而在深度学习推荐系统时代，面对black box的深度学习网络结构，以及大规模和稀疏性并存的时代，是不是无限制的引入模型就能学习好呢？我个人的思考是，由于长尾流量的存在，以及深度网络结构的不可解释和复杂的交叉性质，如果引入的额外的数据信息增益，小于模型容量带来的收益，那么收益有可能是负向的。对于用户历史行为来说，a1,a2,a3,…an，当前预估item是b，a与b的共现次数符合power-law长尾分布的，大量长尾的pair对得不到充分的训练，一定程度上是有可能导致模型泛化能力下降，收益下降的。

   为了验证这个想法，我们做了以下的特征重要性实验。

![img](https://km.woa.com/files/photos/pictures/2e5/435db985633e018916dd73a74f921_w878_h376.png)

​           图2.6 hard search检索出的match序列重要性提升

   以hard search为例，我们以用户历史安装特征为例，根据hard search将其分成两种序列，match序列，以及not match序列。根据xgboost的特征重要性评估，原始的中长周期安装特征、全生命周期安装特征，重要性分别在第4和第8位；而每种时间序列特征拆分为match和not match之后，我们发现match序列特征重要性提升到第3和第5位，也就是去除了没有match那部分特征之后，match特征的特征重要性有所提升。而not match的那部分序列我们直观感受上认为应该是序列里的噪音，从特征重要性上来看也确实很靠后，略微好于时间、省份等相对区分度很弱的特征。

   这个特征重要性分析，从一定程度上也验证了我们的假设，通过hard search被丢弃的那部分not match的特征，确实相比整个序列来说更像是噪音，hard search提取了更为干净的行为序列拿到了收益。但是这种通过类目作为相关性的方法过于hard，举个例子，下载和平精英（第一人称射击游戏）的用户，可能也会下载变声器（工具类软件），用于解说、主播等场景。这种跨类目的关系，通过hard search关系是很难捕捉的。而从特征重要性分析来看，not match那部分序列虽然重要性下降，但也不是完全没有信息。

## 2.3 soft search



  通过hard search的方法是利用了当前item的分类side info，去计算和历史行为序列所在的分类side info信息，分类side info信息相同就是相关，是个0-1的hard独热编码检索。而既然我们把这个过程看成检索，很自然可以引入向量embedding的思路：把当前query encode成向量，和历史每个item encode计算相似度。这里利用的side info 信息就是item的embedding信息。例如，当前待预估的app为同城旅行，只找出历史上和同城旅行embedding最相似的top K 的行为item。由于将item编码成向量，自然可以复用向量embedding的相似度，该过程称为soft search。

   整个soft search流程如图所示

![img](https://km.woa.com/files/photos/pictures/cec/f37cb02e9a2b2f1ca6c3d20264584_w830_h368.png)

​                图2.7 soft search检索框架

   对于soft search的向量embedding如何得到，有两种思路：第一种是直接端到端训练得到。这种做法，需要在离线阶段传入全生命周期的行为特征，训练空间和时间开销都很大；对于线上预估，也需要在精排模型阶段传入全部特征，线上耗时会是很大的瓶颈，需要结合业务收益和性能开销做平衡。第二种方法，是可以借助各种预训练的app embedding模型。比如，基于用户行为数据训练的word2vec、DeepWalk、Node2vec、GraphSage等都可以，或者像最基础的基于行为共现的itemCF、swing，或者基于文本信息训练的TextCNN、bert embedding，或者如果能拿到CV信息训练item的视觉embedding信息都可以。

   在应用宝的实践中，我们采用的是第二种方法，也就是预训练embedding。首先，它的扩展性非常强，本质上是只要能计算item和item之间两两之间的相似度的方法都可以用。其次，离线实现和在线部署都非常简单，对于embedding信息更新不频繁的，可以离线提前算好i2i的相似度分数，构建每个item的top k倒排索引，线上直接查询到排索引检索，线上耗时可以大大降低。

   而对于预训练的embedding，由于扩展性强可使用的数据源比较多，不是全部embedding都对我们线上的排序模型有用，因此需要有方法来评估预训练embedding和线上cvr任务的相关性。

![img](https://km.woa.com/files/photos/pictures/0b2/8b5ab7821a69ca17080bfbc7d475b_w674_h448.png)

​       图2.8 soft search 序列embedding相似度和cvr高度相关

​    embedding能作为相似度进行检索，前提是embedding计算的相似度和cvr任务应该有高度相关关系。我们分析了基于用户历史行为预训练得到的DeepWalk embedding向量为例，关于DeepWalk向量如何训练，我们后续会讲到。我们将候选target作为query，和用户的历史行为item作为keys计算平均相似度sim。从图2.8可见，相似度sim越高，cvr越高，两者呈现非常正向的相关关系。同时，这个sim分数作为截断的副产物，还有个重要作用，相当于是通过pretrain得到的用户对当前target item的score，对于精排模型来说也是非常重要的特征。

   同样，我们沿用2.1关于hard search的特征重要性分析，也做了关于soft search的特征重要性分析。将中长期和全生命周期特征序列分别拆分成match和not match两种特征，对比hard search，soft search检索得到的match特征从第3和第5提升到2和4位置，重要性有所提升。而对于not match部分的特征重要性则下降更多。

   这说明通过soft search方法，确实比起hard search，能够挖掘更相关的历史行为，丢弃更加没用的噪音行为。

![img](https://km.woa.com/files/photos/pictures/826/28a8c3c07d140c890c3e56c8bc38d_w890_h324.png)

   图2.9 soft search检索相比hard search得到的match序列重要性更高

## 2.4 search方法融合

   通过2.2和2.3，我们实践验证了通过hard search和soft search分别都是有收益的。那是否能够融合这两者呢？答案是肯定的，实际上，整个行为检索可以抽象成很通用的行为检索引擎，通过不同的特征抽取算子，可以实现不同的序列抽取。整个框架如下图所示

![img](https://km.woa.com/files/photos/pictures/168/f7298841d06cacedecc270143dbad_w830_h392.png)

   图2.10 通用Behavior Search框架

   实际上，按照不同的检索维度，我们可以抽取出想要的不同的用户行为序列，例如：

- 根据时间：抽取成短期、中期、长期、超长期行为序列
- 根据类目相关性：抽取同类目相关的hard search序列
- 根据开发者相关性：抽取用户在同个开发者下载过的hard search序列
- 根据DeepWalk相似度：抽取DeepWalk相似度soft search子序列
- 根据bert相似度：抽取bert相似度soft search子序列
- …

   不同的子序列可以作为单独的特征slot、或者取交集、并集，根据实际效果进行尝试。而在我们的实际尝试中，还发现了很有意思的现象，就是可以作为query的不仅仅是当前的候选item，也可以扩展到当前的context，如当前的网络环境、当前时间（是否周末、是否节假日、早上/下午/晚上/深夜等）。

   举个例子，对于应用推荐市场来说，用户在节假日下载游戏的需求会大于非游戏，假如当前是五一放假期间，context是节假日，在做预估的时候，候选item为游戏app，我们可以只抽取用户在context=节假日所下载的游戏app，这样的行为序列对于模型预估来说相关性更高。类似的，对于开学季来说，用户会有下载教育app需求，我们可以关注抽取用户在上个开学季安装的app序列。类似的context信息还有，用户在3g/4g网络下倾向于安装小软件/游戏。

   对电商推荐来说，用户在冬季会有购买冬服需求，如果取近期的行为，哪怕拉取到一年都没法覆盖用户相关的历史行为（去年购买冬服行为）。如果我们以当前context时间作为query，去检索同样context=winter的行为就可以解决该问题，这种换季推荐问题通过这种search框架可以得到有效解决。

## 2.5 search分析&总结

   和阿里SIM模型类似，在关于时间数据分析上，在应用推荐领域，我们也有类似的一些结论。

![img](https://km.woa.com/files/photos/pictures/8cd/b6977767a59d9e0ea7a8a9cad434d_w862_h548.png)

​           图2. 11 BSM VS baseline提升效果

   上面这张图2.11，横轴代表用户仅仅在该时间段有下载。比如，7d-15d这个时间，代表模型推荐且用户下载了的app，用户仅仅在第7-15天内有过该类目的下载，其他时间没有该类目的下载行为。我们可以看到，在7d以内的下载占比，行为检索模型BSM对比baseline，几乎没有提升。在7天到180天这段时间内，行为检索模型BSM对比baseline，下载占比提升14.8%；而在超过180天的下载占比则提升131%。

   这个分析说明BSM显著提升了更长周期以前用户兴趣类目的下载占比。

   我们再来看下同时间窗口内的cvr提升比例。在7天以内的推荐，cvr均值处于波动状态；而在7天-180天cvr提升7.65%；对于超过180天的推荐，cvr提升19.5%。

   这个分析说明，对于更长周期以前用户兴趣类目，BSM推荐更为精准。

   整体上来说，用户的短期行为，依然是占据推荐的主体。对于近期有行为的用户，系统大部分推荐还是会基于近期行为推荐；但是BSM对于用户更长时间以前的行为能够比baseline推荐得更多（下载占比提升），也推荐得更精准（cvr提升）。这在一定程度上也达到了用户全生命周期行为序列建模的目的，在短期为王的推荐系统中，我们能够兼顾用户的长周期兴趣。

![img](https://km.woa.com/files/photos/pictures/992/594e90876f0830a052c67477f3b8d_w826_h570.png)

​         图2.12 BSM VS baseline cvr提升效果

   另外，我们和阿里SIM探索时间几乎是同时独立在做，仔细对比两者，如图2.13，有以下几个不同点：

- 目的：阿里作为电商推荐，累积了用户历史上足够丰富几千几万的行为，SIM模型初衷很大程度是为了解决引入全生命周期行为后，序列过长导致的性能问题。而在我们的场景，哪怕拉长到全生命周期，用户行为序列也不会太长。我们的出发点主要是为了解决如全量安装、卸载、全生命周期中的大量噪音信息。
- 序列划分方法：阿里SIM模型对于整个用户序列是统一处理的。在我们的业务实践中，进行了不同周期的划分。短周期行为序列全保留不做search，中长周期和全生命周期的行为做search。
- Hard search方法：阿里SIM和我们在初期都使用了类目作为相关性的判断标准。实际上这个方法也是可以扩展的，对应用商店来说，如同作者、同包大小下的行为序列；对电商推荐来说，如同品牌、同个商铺下的行为序列等。
- Soft search方法：阿里SIM提到的是end2end来抽取和target相关的子序列，无论是离线训练还是在线存储计算开销都会非常大，对线上工程要求很高。在一开始我们就放弃了end2end的想法，而是借用了各种pretrain model来作为soft search检索，目前线上主要是基于deepwalk embedding以及bert embedding。同时，计算得到的soft score我们验证了对于精排模型也是个很强的特征信号。
- Search融合方法：阿里SIM模型只用到了hard search。不同的search检索出来的方向，是独立做成特征喂给精排模型，还是融合去重后一起输入，需要结合业务方向进行尝试。我们在实践中对于同类型的会做merge，差异较大的单独作为特征。
- 扩展query：阿里SIM模型用的是待预估item作为query，实际上这个方向可以扩展，在我们的实践中，context类特征，如网络环境和时间特征就是个比较有区分度的query特征。

  ![img](https://km.woa.com/files/photos/pictures/67a/af3a654b4e2f838013c3669066baa_w664_h500.png)

图2.13 阿里SIM模型和应用宝行为检索模型BSM对比

 

### 

# 3、排序模型如何建模用户行为

## 3.1 target attention模型

   在推荐系统模型中，我们的baseline是wide&deep模型，wide侧是bias类别特征、user侧和item侧特征；deep侧特征对于用户历史行为这种multi-hot特征，采用average pooling的方式。

   而通过数据分析，我们发现，用户的不同历史行为，对于当前预估target的影响是不同的。我们用安装差异指数来衡量这个影响。

![img](https://km.woa.com/files/photos/pictures/cdc/73a88c27f568b3a933540e6682eb3_w962_h106.png)

   由上式可知，安装差异指数越大，说明该app的个性化差异越大，用户历史安装app对当前app影响越高。

   我们看下两个case。图3.1所示，淘宝app top20安装差异指数为2.88，横坐标前几个表示，安装过蓝晶社、好省、美逛、粉象生活等购物app的用户，对比没有安装过这些app的用户，下载淘宝的意愿更强烈。

![img](https://km.woa.com/files/photos/pictures/cb5/192453f5dafadad553ffb3b5749b2_w830_h420.png)

​         图3.1 淘宝在不同安装app下的分发系数差异

   同样，对于腾讯视频app来说，如图3.2所示，安装差异指数只有1.28。说明用户是否安装top20这些app，对于用户是否下载腾讯视频，影响并不是很大，这个和我们的直觉认知也是差不多的。

![img](https://km.woa.com/files/photos/pictures/18e/bbf59ac33db3822f9266f0897912d_w830_h482.png)

​           图3.2 腾讯视频app在不同安装app的分发系数

   通过上面的分析，我们可以知道，用户历史行为对待预估的item的影响是不同的。这里我们借鉴DIN的思路，考虑在模型使用target attention，来建模用户历史行为，与当前item的关系。

![img](https://km.woa.com/files/photos/pictures/15a/eacf56bcf4d55ae78222705de840f_w830_h494.png)

​           图3.3 同样的安装列表对不同的app权重不同

   如上图3.3 所示，虎扑app是体育社区，毒（得物）app是装备社区，两者都有年轻、潮流、装备的特性，可以看出虎扑对于毒的attention权重要显著高于腾讯视频等。对于待预估item是小红书，可以发现美丽说app的权重也会明显较高。

   在target attention的探索中，我们也尝试了AutoInt等网络结构，也就是所有特征filed都有机会作为query，去和所有特征做self-attention。从实验结果来看并没有太大的收益。对于用户的不同历史行为，按照短期、中长期、全生命周期行为分别尝试target attention：

| 特征             | Aver pooling AUC | target attention AUC | AUC lift |
| ---------------- | ---------------- | -------------------- | -------- |
| 短期行为序列特征 | 0.84153          | 0.84154              | 0.00001  |
| 中长周期序列特征 | 0.84153          | 0.84255              | 0.00102  |
| 全生命周期特征   | 0.84153          | 0.84359              | 0.00206  |

   可以看到，随着时间窗户的拉长，全生命周期行为特征做attention收益最大，而短期行为序列引入target attention则完全没有收益。我们猜测大概率是因为短期行为少且重要，中位数只有XX个(业务脱敏)，而对于全生命周期行为序列来说，做完search也存在较大噪音，引入target attention模型能学习更为充分。

   target attention的思路本质上是在一个对历史行为进行加权表达的过程，这个加权的score是通过模型end2end进行学习得到的。第一步在我们做soft search的时候，已经有了每个历史item和当前item通过pretrain学习得到的权重。这里我们有个trick，在实际中我们将soft search得到的score和当前din网络学习得到的score进行融合效果是最好的，AUC大概有0.001的提升。

​        Score = average(scoresoft_search，scoredin)

## 3.2 self-attention模型

   第一版的target attention考虑了不同历史行为对于当前target的影响，但没有考虑不同历史行为序列时间的关系。换句话说，历史行为是顺序不敏感的。在我们对不同历史行为进行时间粒度的下钻分析后发现，越是近期的历史行为，对于当前item的影响越大。

   如图3.4所示，横坐标代表用户最近一次在该类目上有下载距离当前时间，纵坐标表示相比baseline的cvr提升倍数。以绿色曲线为例，用户如果在最近1天内下载过棋牌类目，那么给他推荐棋牌同类目的app，cvr是其他类目的6.8倍；如果是7天前下载过棋牌类目，cvr提升是4.7倍，如果是180天以前下载过，cvr提升为1.9倍。这意味着，对于棋牌类目来说，越近期的行为影响越大。

![img](https://km.woa.com/files/photos/pictures/a57/186b95409f1ef129b396509c0d8b2_w830_h494.png)

​      图3.4 不同类目match特征cvr提升倍数时间曲线

   当然，也有些类目是时间不敏感的，比如生活类、工具类、办公类app等，用户的历史行为，基本不会随着时间而对当前预估item有较大的影响。一种很自然而然的想法，我们引入了transformer中的position encoding，来建模这种时间序列关系。对于position的建模，也尝试了两种方法，一种是和原始transformer一样的函数化思路，用sin/cos来表示；另一种是参数化的思路，每个item距离当前item的时间关系先离散化后用embedding学习表示得到。实际中我们用了后者的方法。

![img](https://km.woa.com/files/photos/pictures/6ab/a59dbe3ad1d0d0c50fd2130adb9dc_w640_h342.png)

​        图3.5 体育竞速和办公类目attention map结果

   学习到的模型我们抽case看了下一些attention map的可视化结果，如图3.5所示，横坐标表示距离当前时间，越靠左表示时间越近。亮度表示attention weight，颜色越浅表示权重越高。可以发现体育竞速类距离越近的行为权重越大；对于办公类目来说则没有特别明显的时间区分度。

   除了时间关系，同样的我们引入了self-attention来建模历史行为序列之间的关系。每个历史行为序列都会作为query，来得到每个历史行为序列相互之间的attention score，如图3.6所示

![img](https://km.woa.com/files/photos/pictures/19a/efd21b0bf81679c343fed322f35fa_w666_h338.png)

​           图3.6 self-attention建模用户历史行为序列之间关系

![img](https://km.woa.com/files/photos/pictures/93d/9ca957a499f537f488003db0e6190_w578_h214.png)

   同样的，我们通过抽样case来查看attention map。如图3.7所示，坐标（i, j）代表用户历史第i个行为和第j个行为归一化后的attention权重。方块颜色越浅，代表权重越大。左边的图是global step在大概5w步的时候，可以看到从左到右颜色越来越暗，第一列的权重最大，模型学习到了近期的行为更为重要。右边的图是global step在100w步的时候，可以看到除了从左到右颜色越来越暗，还有就是左上到右下对角线的趋势。每个item可以学习到距离当前item更近的行为权重更大，体现了self-attention的思想。

![img](https://km.woa.com/files/photos/pictures/044/31238678268598ed26f219b37ca74_w830_h422.png)

   图3.7 体育竞速类app不同global step下的attention map

   引入transformer后我们可以得出以下结论：对于同类目来说，时间越近的行为越为重要，这个无论是对于target attention还是self attention来说都是适用的。对不同类目行为不直接可比，由模型网络结构来学习类目以及时间的交叉关系。例如母婴类目app180天以前的行为，比起工具类app近3天的行为更为重要。

   最终我们的整个网络如下所示，底层是原始的行为序列层，第二层是embedding层后，每个历史行为有两个表达，分别是item本身的表达e和位置信息的表达p。第三层是transformer层，每个历史行为经过transformer后，可以充分表达历史行为序列之间的self attention关系。最后一层我们还引入了target attention，让每个历史行为序列，和最终的待预估item做target attention。

![img](https://km.woa.com/files/photos/pictures/d55/e6ebe189b393762e95f41fa16cf2a_w830_h348.png)

​            图3.8 应用宝序列推荐模型网络结构

   在建模中，有一下几点经验可以分享下：

- muti-head数量：从1到4个逐步增加一直有收益，超过4个head后没有明显收益
- 不同周期序列做差异化处理
- 短期序列：不做search，原始序列直接做average pooling
- 中长周期&全生命周期序列：先做search，序列内部使用transformer后，再和target item做attention
- 其他参数：block数量为2；正则化上采用batch norm加速收敛等

   整体上来说，通过target attention和self attention，在应用宝的首页线上推荐中，整体分发系数提升5%，游戏分发系数提升11%。

# 4、推荐模型的泛化能力探索

   在机器学习领域中，模型的泛化能力如何一直也是评估模型好坏的重要标准。在我个人的认知范围里，推荐领域比较早提及到泛化性这个概念的，还是早在2016年google的经典论文wide&deep模型里。当时的论文提到，模型的wide部分擅长记忆能力，也就是记住历史上出现过的样本；模型的deep部分擅长泛化能力，也就是记住历史上出现的少甚至是没出现过的样本。在应用宝的推荐系统中，以待预估的item为例，对于出现次数很少甚至没有出现过的item的预估能力如何，是我们衡量模型泛化能力的一个指标。我们将流量分为高频item和长尾item，泛化能力重点评估长尾流量部分的AUC和COPC(cvr/pcvr)。

## 4.1 share embedding机制

   在应用宝推荐系统中，用户行为是非常低频的，平均一个月来应用宝的天数只有x天（业务脱敏）。但从不同的特征filed来分析，这些低频行为在不同的field（如点击、搜索、安装、下载、更新等）的共现概率其实是非常高的。这些类似的特征field都是用户主动兴趣的表达，从直觉上理解，它们表达的embedding应该也是类似的，理论上是可以共用的。

   因此，在模型泛化能力的探索上，我们采用了如下的share embedding机制来解决模型对于长尾item的泛化能力表达。如图4.1所示，相似的field共享embedding矩阵，如用户下载、安装、在玩等，在field共享embedding权重后的网络是独享的。这种share embedding机制有两个好处：第一个方面，不同field共享embedding，类似于多任务学习中，相似的任务共享底层的网络权重，可以提升中长尾item的泛化能力（类比于多任务学习中提升中长尾任务的泛化能力）。另一方面也极大减少了模型的大小，embedding层的参数大小减少了50%。

![img](https://km.woa.com/files/photos/pictures/0b7/6851faa825783872935dc9fc2d169_w830_h496.png)

​           图4.1 share embedding机制

   做了share embedding后，超长尾分布问题也有一定程度的缓解。从图4.2可以看到，特征频次低于5次的item减少了21%。

![img](https://km.woa.com/files/photos/pictures/7b3/bc932534bfedecb283058e92848db_w824_h472.png)

​        图4.2 share embedding对低频特征分布有所缓解

   不过值得一提的是，我们在实践中试过将用户卸载行为这种显式负向行为一起做share，效果是负向的。share embedding机制本质上是类似于多任务学习中的share bottom机制的，它能work的很重要前提是，不同filed之间具有很高的相关性和相似性。哪些特征field可以做group放在一起share embedding，哪些不能share，一定需要结合业务的特性来设计和做实验。整个share embedding设计，对比baseline，在长尾流量AUC上提升了0.79%，COPC从1.15下降到了1.13。 

## 4.2 引入行为向量embedding

### 4.2.1 引入更长周期预训练模型 

   在推荐系统中，数据决定了模型的上限。长尾item表达的不充分，一定程度上可以通过增加更多的样本来缓解。一种intuitive的思路是拉长样本周期，比如一个item在过去一个月只出现过2次，如果拉长到半年可能出现的次数超过20次，学习起来当然就相对充分和准确些。在训练框架支持的情况下，这条路几乎很难不work。另一种思路，长尾item在模型里学习表达不准确某种程度上是因为在cvr任务里，item完全是end2end，在出现频次不多的情况下很难学习充分。如果我们有办法得到item的其他表示，比如说，通过预训练得到的训练更为充分的行为向量，也许可以缓解这个问题。

   以DeepWalk为代表的行为预训练模型，我们用180天的用户历史行为作为doc来构建行为样本，通过随机游走构件图生成样本训练得到每个app的embedding表达。如下图4.3所示，构建了一个无向图G(V,E,W)，其中V为所有节点的集合，E为所有边的集合，W为每条边的权重。具体方式为：初始化V，E为空，W为0。扫描所有用户的下载序列，假设当前用户的下载序列为A，B，C。那么往节点结合V中加入节点A、B、C，向边集合E中加入A-B、B-C、A-C，同时存储每条边出现的次数。在边的权重这里我们做了些优化，例如，app1和app2的weight=app1和app2的共现次数/（app1出现次数+app2出现次数-app1和app2的共现次数）

![img](https://km.woa.com/files/photos/pictures/418/0cbc74ac1020d2f07b3bcf60e51f4_w830_h242.png)

![img](https://km.woa.com/files/photos/pictures/54c/bea7936b07b4720e477404e0dc0c1_w550_h72.png)

​        图4.3 用户180天行为序列DeepWalk模型

   得到每个DeepWalk的向量表达后，如何在模型里使用是个值得探索和思考的问题。简单来说，有三种思路：

- DeepWalk embedding初始化embedding并且固定
- DeepWalk embedding初始化embedding并且finetune
- DeepWalk embedding作为特征和app embedding concat

   以下是我们的实验结果

![img](https://km.woa.com/files/photos/pictures/e1a/326cdf6043ee7ded1841e19312277_w910_h242.png)

   从结果上来看，方法一frozen的效果最差，而方法二初始化后finetune或者方法三当做特征，长尾AUC都有所上升，COPC有所下降。这给了我们一些启发，看起来预训练得到的行为向量embedding还是可能给模型带来一些收益的。直接frozen住效果很差，大概率是因为DeepWalk训练目标，和当前我们模型cvr主模型差异实在是过大，那是否可以融合这两个目标呢？

   沿着融合两个任务的想法，我们尝试了特征蒸馏的思路，设置了两个任务：主任务还是cvr任务，需要保证loss下降和主任务一致；增加了一个辅助任务，用DeepWalk embedding作为输入，来指导当前app embedding的更新，两者相似度越高，loss越小。

![img](https://km.woa.com/files/photos/pictures/fd9/0878452e6073e5e66333b2c74329f_w418_h52.png)

   其中lambda是超参数，用来控制辅助任务的影响。实际结果，特征蒸馏思路效果优于固定初始化，但是对比finetune效果仍然要差。

### 4.2.2 预训练模型为模型带来了什么

   上面的几个实验，我们得到了几个经验教训。

   首先是强扭的瓜并不甜，DeepWalk学习得到的app embedding分布，和cvr模型学习目标差异较大，如果通过固定初始化后不再更新，或者说辅助任务强行保证app embedding要和DeepWalk 学习得到的embedding分布要一致，效果会很差。

   其次，我们单独去分析了下DeepWalk任务以及cvr任务学习得到的embedding信息，可以发现两者的可视化语义信息截然不同。

![img](https://km.woa.com/files/photos/pictures/d52/8e70ac4de5a09c82a5c848a5d3a5a_w856_h484.png)

​      图4.4 w&d模型和DeepWalk 学习的app embedding结果

​    图4.4中，左图是wide&deep模型学习得到的app embedding的TSNE二维可视化结果，可以发现整个分布是杂乱无章的；右图是通过DeepWalk初始化得到的app embedding，可以发现哪怕经过了cvr任务的finetune，整个分布也是具有明显的类目聚集性。关于item embedding的语义信息以及模型的表达能力两者的关系，其实业界很少有答案。

   我个人在《deep learning》一书的第15章中，发现了有一部分相关的描述，就是预训练的模型，一定程度可以将模型带到了通过随机初始化（Xavier、均匀、正态分布等本质上还是随机）无法带到的地方。

![img](https://km.woa.com/files/photos/pictures/71e/387e6860fabc9e28a688e60723422_w708_h360.png)

​    图4.5 《深度学习》第15.1章预训练的IsoMap可视化结果

   从该书第15章这张图4.5可以看到，坐标圆形的点表示没有经过pretrain embedding初始化的参数，整个训练轨迹更加发散；右边下三角的点表示经过了pretrain embedding初始化的参数，可以发现整个训练轨迹更加狭窄集中。

   我个人猜测，如果没有预训练的信息引入（作为特征concat或者finetune），通过正常的end2end训练，item的embedding的表达很难收敛到右边这种狭窄集中的空间去。当然，embedding的可视化结果和模型结果之间的关系目前我们也无法判断给出结论，这个方向值得整个业界去持续探索。

## 4.3 引入文本向量embedding

   前面提到的行为embedding通过预训练的DeepWalk向量拉长了周期，给模型带来了新的信息量。但是对于拉长周期依然没有行为的item，比如完全新的item则无能为力了。很自然的想法是引入和行为无关的其他泛化信息。对于app推荐系统来说，就是app的文本描述信息，如图4.6所示。

![img](https://km.woa.com/files/photos/pictures/227/b6afa478e83c99870f9d43c64fc7e_w316_h658.png)![img](https://km.woa.com/files/photos/pictures/92c/ae7aee8571ba8901e6235f10f7e86_w318_h654.png)

​    图4.6 软件soul和英雄杀的文本描述界面

   在我们的app分类任务中，利用app的文本描述信息作为样本，用bert进行三级分类任务训练得到的分类，在搜索场景有很不错的提升，因此我们借用文本信息训练的bert模型，取CLS作为embedding作为输入。

![img](https://km.woa.com/files/photos/pictures/7ac/12f7d24cb4f5f2a362b0600d08617_w626_h392.png)

​      图4.7 基于app描述信息学习的bert文本 embedding

   如图4.7所示，通过app描述文本训练得到的CLS作为app embedding得到的原始的bert向量有768维，如果直接在模型里concat使用或者finetune，会导致原始的embedding信息有所湮没，效果也不好。因此这里我们也探索了在模型外降维、以及通过一层MLP进行降维后concat的方法，后者效果会更好。引入了bert 文本向量后，我们对比下DeepWalk的行为向量之前的区别

- 覆盖率：文本向量的覆盖率会更全，对于没有行为或者非常行为很稀疏的app也可以覆盖学习
- 新item表达：DeepWalk行为向量对于新item由于没有用户行为因此无法表达，但对于文本向量来说，新item也有文本描述可以得到其文本向量embedding
- 类目聚集性：我们同样做了行为向量embedding和文本向量embedding的TSNE可视化结果，发现两者都具有明显的类目聚集性，如下图所示。而且可以发现，行为向量embedding对于头部app的表达更为精准，猜测可能是因为头部app的行为更为丰富训练更加充分。

![img](https://km.woa.com/files/photos/pictures/2b2/161b86763471a63055b86544b067f_w832_h428.png)

​    

   图4.8 bert embedding和DeepWalk embedding TSNE可视化结果

   由于app推荐领域，除了文本信息以外，视觉、音频相关的side info信息对推荐并不关键，所以除了文本以外我们并没有用到其他信息。在短视频、电商、feeds流等场景，除了文本，可以充分利用例如视觉信息embedding、音频embedding、封面图embedding等信息来加强模型的泛化能力表达。当然不是所有这些side info信息直接加到模型都一定有收益，也和当前模型的特征体系、业务形态等有关。

## 4.4 multi-channel embedding



   通过前面我们引入行为向量embedding，文本向量embedding都有一定收益，怎么在模型里进一步融合是个问题。我们通过行为向量得到的embedding是60维，通过文本向量得到的embedding是768维。因此我们考虑在模型里引入矩阵进行映射。

   如下图4.9所示，我们尝试了concat和pooling两种思路，将两个不同维度的bert向量和DeepWalk向量，映射到维度为d的相同向量。实验中证明用concat效果会优于sum/average pooling。

![img](https://km.woa.com/files/photos/pictures/e00/ddc226b8a7e7cb827d8ce2f27456b_w830_h382.png)

![img](https://km.woa.com/files/photos/pictures/12c/a1abee89f3735f8a5e690a71ab123_w696_h150.png)

​       图4.9 bert向量和DeepWalk向量的融合

   这种融合了行为channel，以及文本channel的embedding方法，我们称这种方法为multi-channel embedding。后续如果有新的pretrain embedding信息融入，可以统一在该框架中。通过可视化结果，发现multi-channel的embedding，文本channel的bert向量和行为channel的DeepWalk向量呈现互补的趋势。

![img](https://km.woa.com/files/photos/pictures/6ec/2cc69e00f264d7814a6be67ddc1bc_w830_h430.png)

​         图4.10 multi-channel embedding TSNE可视化结果

   值得注意的是，离线阶段我们会提前计算好每个app经过Wbert和Wdeepwalk矩阵映射后的向量，在线的话相当于是个lookup操作，无需存储每个app的768维向量并经过矩阵映射，减少计算和存储。

   最终上线我们也用的multi-channel的版本，同时融合行为向量和文本向量，从离线结果来看，整体AUC有1.53%的提升，整体COPC下降7.19%。长尾AUC提升2.69%，长尾流量的COPC下降9.92%。线上效果也有大概7个百分点的提升

![img](https://km.woa.com/files/photos/pictures/93d/5fbfd4694101c5d17f44988eb62fc_w906_h326.png)

# 5. 总结

   本文主要讲述了对于大规模稀疏推荐场景如何做推荐，并以应用宝的推荐发展探索之路进行了描述。

   第一章探讨了推荐系统的一些本质，按照长短内容进行推荐系统分类，以及应用宝的推荐面临的挑战。

   第二章针对全生命用户行为，如何通过hard search、soft search以及两者的融合来进行序列的抽取。

   第三章针对前面提到的search抽取出来的行为特征，如何通过模型的方式，用target attention、self attention的方法进行行为建模。

   第四章针对大部分推荐系统会面临的长尾item泛化能力不足问题，如何通过引入预训练的行为向量、文本向量等外部信息来提升模型的泛化能力。

   时间有限，其实很多尝试过还比较有意思的探索如系统偏差不一致的解决、多场景如何协同建模、样本reweight等问题没有来得及在这里一一开展，部分工作可以参考下团队其他同学的相关文章。行文匆匆，多有错漏，欢迎加微探讨指正。

 

最后更新于 2021-09-06 09:56

[![img](https://kmdocs-30058.preview.qpic.cn/5469becd932c1817c184b07781d65a46.pdf?cmd=doc_preview&sign=q-sign-algorithm%3Dsha1%26q-ak%3Dqp1nIu1F2nLan9t7paPoR0ij%26q-sign-time%3D1631069860%3B1631156260%26q-key-time%3D1631069860%3B1631156260%26q-header-list%3Dhost%26q-url-param-list%3D%26q-signature%3De68110d2b84f38c33065adc67be0d96cad4581eb&page=1)应用宝推荐排序综述.pdf查看全部（共44页）](https://km.woa.com/group/49100/attachments/attachment_view/244885)

附件：

[应用宝推荐排序综述.pdf ](https://km.woa.com/group/49100/attachments/attachment_view/244885)[预览](https://km.woa.com/group/49100/attachments/show/244885)

![img](https://km.woa.com/img/articles_know/has_apply_img.png)已申报

如果觉得我的文章对您有用，请随意赞赏

赏

[4人已赞赏](javascript:void();)

[![img](https://dayu.oa.com/avatars/petershen/profile.jpg) ](https://km.woa.com/user/petershen)[![img](https://dayu.oa.com/avatars/gavinnie/profile.jpg) ](https://km.woa.com/user/gavinnie)[![img](https://dayu.oa.com/avatars/jeangao/profile.jpg) ](https://km.woa.com/user/jeangao)[![img](https://dayu.oa.com/avatars/maxwellcao/profile.jpg)](https://km.woa.com/user/maxwellcao)



仅供内部学习与交流，未经公司授权切勿外传

分类：[推荐算法](https://km.woa.com/group/yybalg/articles?cat_id=185711) 标签：[推荐系统](https://km.woa.com/group/yybalg/articles?tag_id=31576)(7) [深度学习](https://km.woa.com/group/yybalg/articles?tag_id=70010)(1) [精排模型](https://km.woa.com/group/yybalg/articles?tag_id=166280)(1) [泛化能力](https://km.woa.com/group/yybalg/articles?tag_id=149664)(1) [Embeddin...](https://km.woa.com/group/yybalg/articles?tag_id=174084)(2)

![img](https://km.woa.com/operations/get_wechat_image?target_url=https://km.woa.com/group/49100/articles/show/485104)

本文专属二维码，扫一扫还能分享朋友圈

想要微信公众号推广本文章？[点击获取链接](javascript:void(0);)

#### 相关阅读

-  [推荐系统中的深度匹配模型](https://km.woa.com/posts/show/439871?kmref=related_post)
-  [搜索中的深度匹配模型](https://km.woa.com/posts/show/445795?kmref=related_post)
-  [深度学习在CTR预估中的应用](https://km.woa.com/posts/show/359518?kmref=related_post)
-  [深度CTR预估模型在应用宝推荐系统中的探索](https://km.woa.com/posts/show/470205?kmref=related_post)
-  [应用宝推荐系统系列--位置偏差学习](https://km.woa.com/posts/show/518794?kmref=related_post)
-  [应用宝推荐系统系列——多场景迁移学习](https://km.woa.com/posts/show/512582?kmref=related_post)
-  [Embedding技术在应用宝推荐场景中的应用](https://km.woa.com/posts/show/467274?kmref=related_post)

我顶 (23)

收藏 (105)

- [转载](javascript:void(0);)

   

- [收录](javascript:void(0);) 

- [评论 ](javascript:void(0);)(17) 

- [反馈](javascript:void(0);)

分享到 

大家评论

[![img](https://dayu.oa.com/avatars/youjiangxu/profile.jpg)](https://km.woa.com/user/youjiangxu)

[youjiangxu](https://km.woa.com/user/youjiangxu)

2021-09-06 10:02:00

66666，膜拜！

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/jessezli/profile.jpg)](https://km.woa.com/user/jessezli)

[jessezli](https://km.woa.com/user/jessezli)

2021-09-06 10:02:49

![img](https://km.woa.com/img/face/76.gif)![img](https://km.woa.com/img/face/76.gif)![img](https://km.woa.com/img/face/76.gif)

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/frankiehu/profile.jpg)](https://km.woa.com/user/frankiehu)

[frankiehu](https://km.woa.com/user/frankiehu)

2021-09-06 10:11:40

真不错，赞。
9.6还贡献了这么高质量的文章。

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/petershen/profile.jpg)](https://km.woa.com/user/petershen)

[petershen](https://km.woa.com/user/petershen) 

2021-09-06 10:13:31

学习学习，俊波老师太6了！

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/chuanjunli/profile.jpg)](https://km.woa.com/user/chuanjunli)

[chuanjunli](https://km.woa.com/user/chuanjunli)

2021-09-06 10:31:36

火钳刘明

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/gavinnie/profile.jpg)](https://km.woa.com/user/gavinnie)

[gavinnie](https://km.woa.com/user/gavinnie) 

2021-09-06 10:33:15

牛！

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/jeangao/profile.jpg)](https://km.woa.com/user/jeangao)

[jeangao](https://km.woa.com/user/jeangao)

2021-09-06 10:44:22

膜拜大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/wenjunli/profile.jpg)](https://km.woa.com/user/wenjunli)

[wenjunli](https://km.woa.com/user/wenjunli)

2021-09-06 11:15:13

膜拜大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/yanqianzhou/profile.jpg)](https://km.woa.com/user/yanqianzhou)

[yanqianzhou](https://km.woa.com/user/yanqianzhou)

2021-09-06 16:52:23

高屋建瓴～ ![img](https://km.woa.com/img/face/76.gif)

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/lynasliu/profile.jpg)](https://km.woa.com/user/lynasliu)

[lynasliu](https://km.woa.com/user/lynasliu)

2021-09-06 19:26:47

膜拜大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/tinkleguo/profile.jpg)](https://km.woa.com/user/tinkleguo)

[tinkleguo](https://km.woa.com/user/tinkleguo)

2021-09-06 19:52:17

%大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/haoxinglin/profile.jpg)](https://km.woa.com/user/haoxinglin)

[haoxinglin](https://km.woa.com/user/haoxinglin)

2021-09-06 20:59:59

膜拜大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/sigmoidguo/profile.jpg)](https://km.woa.com/user/sigmoidguo)

[sigmoidguo](https://km.woa.com/user/sigmoidguo)

2021-09-06 21:36:35

膜拜大佬

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/waynetao/profile.jpg)](https://km.woa.com/user/waynetao)

[waynetao](https://km.woa.com/user/waynetao)

2021-09-06 22:00:01

膜拜大佬！

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/stevezhu/profile.jpg)](https://km.woa.com/user/stevezhu)

[stevezhu](https://km.woa.com/user/stevezhu)

2021-09-07 00:21:04

有个问题请教下。
search应该是对批量item一起search的吧，如果候选item分布比较宽泛，是不是search跟不search区分度就不大了？

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/joviwang/profile.jpg)](https://km.woa.com/user/joviwang)

[joviwang](https://km.woa.com/user/joviwang)

2021-09-07 19:47:03

学习学习，俊波老师太6了！

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/zhengronwen/profile.jpg)](https://km.woa.com/user/zhengronwen)

[zhengronwen](https://km.woa.com/user/zhengronwen)

2021-09-07 21:57:42

tql 学习了

 顶 [ 回复](javascript:void(0);)

[![img](https://dayu.oa.com/avatars/rickyrqzhao/avatar.jpg)](https://km.woa.com/user/rickyrqzhao)

[ ](javascript:void(0);)[切换到更多功能](javascript:void(0);)

关于作者

- [![img](https://dayu.oa.com/avatars/jimberxin/profile.jpg?1583656147)](https://km.woa.com/user/jimberxin)

  [jimberxin(辛俊波)](https://km.woa.com/user/jimberxin) PCG\商业产品部\混排策略组员工

作者文章

-  [【万字长文】应用宝推荐排序综述](https://km.woa.com/posts/show/520684?kmref=author_post)
-  [搜索中的深度匹配模型](https://km.woa.com/posts/show/445795?kmref=author_post)
-  [推荐系统中的深度匹配模型](https://km.woa.com/posts/show/439871?kmref=author_post)
-  [深度学习在CTR预估中的应用](https://km.woa.com/posts/show/359518?kmref=author_post)



Copyright©1998-2021 Tencent Inc. All Rights Reserved

腾讯公司研发管理部 版权所有

[广告申请](https://km.woa.com/chartlets/km/) [反馈问题](javascript:void(0);)

[563/600/465 ms]