---
layout: post
title: 合肥总结 && 山寨旅游
categories:
    - 活着&生活
tags:
    - ACM
---

合肥赛区是国内第一个赛区，我们队披荆斩棘，艰难的拿到了一个银奖。首先毫无疑问要感谢我的两个队友水浩和chyx，他们的稳定发挥是CoffeeBean成功的关键因素；感谢郭老师和其他集训队员的支持和鼓励，在你们身上我真的学到了好多好多。 

先说说热身赛。比赛前一天中科大照例举办一个试机的热身赛，题目不是很难。A题是问平面中一条线段穿过多少个方格，chyx推了一下上去1y了。B题是2人的TSP问题，问2个人走完n个点走的较远那个人走的最短距离。我写了个DP，调了一会也过了。C题转化一下后是一个树同构判定，看完题目我就觉得十分郁闷。因为合肥网络赛就有道树同构，还有个图同构！那时候想应该不会再出同构了吧，，没想到。。水浩没体力最终没敲完。热身赛就这样水过，大家表现得都比较放松，没有太紧张的情绪，这是一个好的开始。 

比赛那天早上7点就起床了，已经很久没试过这么早起来了，而且还要去比赛！比赛时间安排在8点半实在是早了点。坐车去到现场，发现可以提前开机调环境，这个情况好像有点不合逻辑。8点半比赛准时开始，水浩和chyx分别从两头开始看，我从D开始看。先说说各题题意： 

- A：n个球分别从某些时刻开始从各自起点以各自速度匀速直线运动到各自的终点，问整个过程中会不会发生碰撞。
- B：问2^n-1是不是素数，n<257。
- C：貌似是很复杂的模拟，没看。
- D：问空间中一条线段穿过多少个方格。
- E：给一个文本和一些短串，问在文本中出现次数最多的短串及出现次数。
- F：给两个只包含字母a和b的长度不超过10万的串，问在第一个串中有多少个长度和第二个串长相等且曼哈顿距离不超过k的字串。
- G：给一个有向无环图，每个点有些棋子，两人在上面博弈。每次每人取任意一个棋子并将其放到取棋点的任意一个后继上。不能走者输。给出棋子总数s，问有多少种分配棋子的方法使得后手必败。
- H：貌似是很复杂的剪纸游戏，没看。
- I：n个城市排列在一条直线上，从中选m个建邮局。城市之间有距离，在第i个城市建邮局的费用为ci，第i个城市可以被距离不超过li的邮局服务到。如果城市i没有邮局能服务到则要付出费用di。问建m个邮局的最小费用。
- J：一棵n个点的树，点和边都带权，从中选一个有m个点的连通块，使其他点的权值*到连通块的最短距离最小。 

我看完D，是昨天A的加强版，真是神奇（前一天热身赛时我就想合肥这么喜欢出同样的题，会不会明天比赛时就是今天的某些题改版？果然被我猜中）。马上扔给chyx。chyx推了一下公式，容斥原理，直接上去1y，盛赞！这时我和水浩讨论B，他跟我说了题意我第一个想法就是高精/Java大整+米勒测试，无奈Java不熟，不敢轻易尝试，怕遇到什么地方不会写就会浪费很多时间，但是这时场上已经有两三支队过了，这么快过的应该是水题啊，于是也不敢轻易敲高精，想想是不是有其他办法。徘徊了很久，未果。水浩看了E，说是kmp，我也看了一遍，多串匹配，觉得会超，问他是不是后缀数组或者别的什么能做；转念一想现在空机，还是先敲着吧。于是水浩上去敲完，调了一下，1y！大赞！期间我和chyx继续讨论B，chyx说梅森数判定有确定的算法的，我一翻黑书，上面赫然写着Lucas-Lehmer测试，我顿时傻了眼，无奈的放下了B。。（黑书真是黑啊！！）等水浩ac了E，我看完了F，没多想就准备上去暴力了，幸好被水浩叫住了，发现了算法的错误这时我抬头往周围看了看，发现hdu的mm队旁边赫然飘着三个气球！！好猛的SailorMoon啊！！顿时压力剧增。我又刷了下board，看G有些队过了，而场上除了B也没别的题目可做。我很不甘心，心想这么多队过B，怎么也得写一下，这时正好空机，我于是下定决心上去水一下B，水浩和chyx想G。我把高精模板和米勒测试贴了上去，敲到手都酸了。敲完发现小数据不过，十分崩溃。调试的时候打印出来发现是3页半纸，真是体力活啊！调了二十分钟，总算能跑了，然后开两个console，一个让我的程序跑一个用来编译。这里不得不赞下中科大的机器，一点也不卡。等水浩敲好G时，我的表也打出来了，然后写了个程序提交，1y！！我激动的心都要跳出来了，居然有这么假的题目！这时离比赛结束大概还有一个多小时。chyx跟我讲G的解法，很囧的是我没能听懂。。于是我们把程序打印出来人手一份，看看有什么地方可以降低复杂度。但是可悲的是，水浩把能想到的优化都试过了，交上去还是TLE。正当我们绝望的时候，突然想到一个很猛的优化，就是DP的时候不需要枚举每个点放的棋子个数，而只需要枚举0和1，最后用组合公式乘一下就行了，原因在于sg函数是异或操作的！然后水浩改了下，交后发现一个bug，改了再交。第一次wa，我当时紧张的都在胸口划十字。然后，一个大大的
Yes出现在我们面前，我们都惊呆了。时间定格在这一刻，我们相拥而笑，一切都来得太精彩！ 

赛后中科大组织我们看《建国大业》，我晕~真是无语啊= =。。。我们都没去，和大家讨论了下题目，发现B原来《具体数学》上就有一个现成的表！我都要哭出来了……另外发现原来Java的大整数类就有现成的判素数的函数，不会Java真的很亏啊！！下定决心这次一定好好学Java！另外原来G是chyx和我说解法时大家出现了理解偏差，赛后想起来好像不会太难，但是比赛时一切又不一样了。还好所有这些失误都没有对结果造成太大影响。 

总结这次比赛，一个就是心态很重要。这次比赛对于我们队3人来说都是首战，没有什么经验，而大家在比赛时都没有太大的情绪波动，我想这对于整个团队的相互配合是相当重要的。另外这次比赛也暴露了我们很多不足，譬如Java就是一个很大失误，还有调试能力和coding能力不强也是一个心病。接下来都是我们要共同努力的。 

最后还是要感谢我的队友，水浩和chyx，跟你们一队让我学到了很多，你们使我我感受到跳动的思想的火花。感谢合肥和中科大给CoffeeBean带来的美好的比赛征途。

PS：杂事和山寨旅游篇

杂事是有关美少女战士的，，以后再补上，哈哈~~~

比赛完的第二天，我们早早就退了房，开始了赛后旅游。但万万没想到的是这次旅游会这么囧。首先，由于人生地不熟，我们在七天连锁放下行李就马上朝合肥汽车总站出发。我糊里糊涂的跟着大队走，不知不觉上了一辆很猥琐的车（拐卖小孩那种，后面有个蓬一堆人坐两边那种），然后摇摇晃晃的来到一个很山寨的私人汽车站。令人气愤的是他们说有个什么约定，一定要前一辆车开了我们坐那辆才能开！于是等啊等，那辆等足了人开走了，我们还要赔上50快司机才肯开！我顶啊~！然后坐了n久，终于到达巢湖市中心了，然后下车问路巢湖怎么走，结果一看地图，发现我们兜了一个大弯，我们几乎是沿着巢湖湖边走的。。然后又找辆车坐回去，终于到了一个叫宗庙的地方。想上wc然后去了一个跟《贫民窟的百万富翁》中那个小孩上的茅坑差不多的地方憋气闭目待了一分钟还给了一块钱，我的天呀！本想这该完了吧，可以坐船回去宾馆，结果船夫说太远不肯开！！极度无奈之下花了40块去巢湖上一个岛上准备转一圈。坐船过程中，发现巢湖水质特差，表面都是蓝藻，一片青绿，实在没法看。风倒是凉嗖凉嗖的，值得一提的是我终于看见了《腾王阁序》所描述的“秋水共长天一色”的景象了！远方真的海天相接，十分飘渺美妙动人。 

到了岛上，发现那座山比白云山都矮上好几倍，没一会就爬到顶了，发现一座塔，定睛一看，共七层，名曰“锁妖塔”~大伙就往前冲，赵牛带头。那阶梯真的很窄，要是赵牛一个不小心失足，那我们大家都完了。。战战兢兢的爬了许久终于到塔顶，伸头往外一看，发现飞檐蹦了一块，那是重楼打的。。下塔的时候出现了一个灵异事件，就是到第三层大家都突然发现没有下去的路了！！迷宫啊！！后来不知谁碰了什么机关，通道突然显现，大家总算活着走出此塔。 

回去后晚上在“庐州太太”餐厅大吃了一顿，这是在合肥吃过的最好吃的了。然后回去一堆男人们逛了一下，就各奔东西了。 

第二天一早就收拾好行装，趁还有一个多小时和赵牛他们出来逛了下步行街，发现了很多山寨食品，譬如铜锣烧，譬如果真加水变成的苹果c，譬如烧仙草+奶茶+雪糕+珍珠+一堆其他东西混一起的豪华奶茶。吃饱喝足然后就坐车到火车站，发现治安真乱，一堆小孩围着人转要给他钱，还好我没给= =。。 

坐火车真是一件痛苦的事情，一路打牌然后睡觉然后再打牌，时间就这样被消磨光，就像永远都下不了车一辈子都在车上度过一样，那种感觉很难形容。 

合肥之旅到此结束，lambda字。
