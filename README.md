# 电商平台用户行为分析
## 1.项目背景
项目的数据集来自天池，是阿里巴巴提供的淘宝用户行为数据集，其中包含了2017年11月25日至2017年12月3日之间，
有行为的约一百万随机用户的所有行为（行为包括点击、购买、加购、喜欢）。
数据集的每一行表示一条用户行为，由用户ID、商品ID、商品类目ID、行为类型和时间戳组成，并以逗号分隔。有关每个字段的介绍如下所示：
![字段说明](https://upload-images.jianshu.io/upload_images/15578485-31f57c3890f3300a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用户行为的类型共有四种，具体说明见下表：

![行为类型说明](https://upload-images.jianshu.io/upload_images/15578485-bb7eda1391023a72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.分析的思路与目的

![分析思路](https://upload-images.jianshu.io/upload_images/15578485-260e2893d23cff99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.数据概览

### 3.1读取数据

数据总量有1亿多，所以这里选择了500万条数据来进行分析。

![](https://upload-images.jianshu.io/upload_images/15578485-eccffd90e5ee394a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.2 查看数据信息

![](https://upload-images.jianshu.io/upload_images/15578485-5d239e7e959e5a40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.3 描述性统计分析

![](https://upload-images.jianshu.io/upload_images/15578485-230c14bc05c09fda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由于该数据集中会出现一个用户ID多次浏览的情况，因此这里“用户ID”的count数和max值不代表用户数量，“商品ID“和“商品类目ID“类似

[](https://upload-images.jianshu.io/upload_images/15578485-bded8cb034dc9c3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用户行为类型分为4种，其中浏览量pv最多，达到4475232次

## 4.数据预处理

### 4.1 重复值处理

对重复值直接用删除的方式处理。

![](https://upload-images.jianshu.io/upload_images/15578485-09e29aa46498ea97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到删除了5条重复的数据。


### 4.2 缺失值处理

在处理缺失值之前，先查看有多少缺失值


![](https://upload-images.jianshu.io/upload_images/15578485-ff09e1b4cac249b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到没有缺失值，因此也不用处理了。

### 4.3 异常值处理

根据数据介绍，可知道数据的日期包含在2017年11月25日至2017年12月3日之间，因此可根据这条规则对数据进行异常处理。

![](https://upload-images.jianshu.io/upload_images/15578485-a88e5f74f74202c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

剔除异常值后数据还剩下4997363条

### 4.4 数据清洗

将时间戳转换为datetime格式。

![](https://upload-images.jianshu.io/upload_images/15578485-66e316679063cb6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

提出日期、时间和小时

![](https://upload-images.jianshu.io/upload_images/15578485-3fd680fb950f304e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

查看处理的结果

![](https://upload-images.jianshu.io/upload_images/15578485-93071c06fdc87c78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5. AARR模型分析

### 5.1 获取用户

###### 1. 日pv、日人均pv和日uv

![](https://upload-images.jianshu.io/upload_images/15578485-e1dd02bd2f1c7622.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-f68339be6cb2deb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 日pv和日uv两者走势相类似，也进一步说明日人均pv波动较平缓，其平均水平为13.34

- 日pv和日uv均呈现上升趋势，且均在12月2日突然升高至九日内最高水平，而12月2日是周六，但11月25日也是周六，因此可能不是周末的原因，又由于12月2日距离双十一较近且多数人会在双十一购买近期所需物品，因此初步推测12月2日~3日的突然升高是因为商家进行促销、宣传推广等活动。

###### 2. 日新增uv和日新增uv的pv

![](https://upload-images.jianshu.io/upload_images/15578485-b1b4383795c6bade.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-7ded1523cebcac32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

日新增uv和日新增uv的pv均呈现明显下降趋势，且在12月2日新增uv的人均pv为627/62=10.11（低于日人均pv的平均水平），说明日pv的突然升高不是由12月2日当日新增的uv带来的，而是由老uv带来的，另外，12月2日新增uv为62人，环比增长-0.44，从侧面反映了此次活动的目的可能不是拉新。

### 5.2用户活跃度

###### 1.日活跃用户数

![](https://upload-images.jianshu.io/upload_images/15578485-0fd2569731041bbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-c58ed6adee8c990c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

日活跃用户数呈现明显的增长趋势，且在12月2日取得最大值，说明此次活动的目的可能是促活。

###### 2.时活跃用户数

![](https://upload-images.jianshu.io/upload_images/15578485-7b68ead1cb890611.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-4e8ee651e894cf3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

19时~22时为用户活跃高峰期， 而2时~5时则为用户活跃低峰期，可在用户活跃高峰期加大活动宣传力度。

### 3 用户留存分析

![](https://upload-images.jianshu.io/upload_images/15578485-e84d0e20e7ac4aed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-038a19a044d7c865.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-f00ced520d3df13c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 就时间窗口来说，次日留存和3日留存均表现出先减后增的趋势，而7日留存则相比之前略有减少；

- 就某一天来说，11月25日新增的活跃用户3日留存<次日留存<7日留存，11月26日新增的活跃用户次日留存<3日留存<7日留存，且其他日期3日留存均大于次日留存。

总体来说，留存呈现增长的趋势，反映出用户粘性在上升。

### 4 营收分析
###### 1. 日购买行为


![](https://upload-images.jianshu.io/upload_images/15578485-f686c4a2b67c96aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-56858aaf9cb659f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在12月1日之前，购买人数和购买率走势相类似，而在12月1日之后购买人数有所增加，但与之前相比购买率却在减少，商家应当优化产品本身并加大宣传推广。

###### 3. 9日复购率


![](https://upload-images.jianshu.io/upload_images/15578485-419219857858fcb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果以0.6作为合格标准的话，说明用户忠诚度表现一般，有大幅增长空间。

###### 4. 3日复购率与回购率
![](https://upload-images.jianshu.io/upload_images/15578485-eaa253a649c0ee90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](https://upload-images.jianshu.io/upload_images/15578485-24771cd89ae7de60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 用户回购率整体高于复购率，其波动性也明显强于复购率；

- 用户复购率呈现先减后增的趋势，而用户回购率则是增加趋势 , 即第二周期购买用户的忠诚度较第一期高，整体说明用户忠诚度在增加。

###### 5. 时购买行为

![](https://upload-images.jianshu.io/upload_images/15578485-5f6c86651f7b3818.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-ac8d0bd8c4ce22ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

购买人数和购买率的走势大致相似，且均呈现明显的双峰走势，其中21时购买人数最多，而10时购买率最高，应当继续保持10时的活动，加大21时的活动力度。

## 6. 漏斗转化模型分析

![](https://upload-images.jianshu.io/upload_images/15578485-cd6c2dcf90b15cc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/15578485-d99f16306ce358e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 总的点击量中，有6.25%加入购物车，有3.24%收藏，而到最后只有2.24%购买，整体来看，购买的转化率最低，有很大的增长空间；

- 就颜色来看，红色部分的变化最大，即“点击-加入购物车“这一环节的转化率最低，按照“点击-加入购物车-收藏-购买”这一用户行为路径，我们可通过优化“点击-加入购物车”这一环节进而提升购买的转化率。

