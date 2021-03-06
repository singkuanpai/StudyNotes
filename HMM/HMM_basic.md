HMM

[TOC]

### 定义
隐马尔可夫模型（Hidden Markov Model，HMM）是统计模型，它用来描述一个含有隐含未知参数的马尔可夫过程。其难点是从可观察的参数中确定该过程的隐含参数。
隐马尔可夫模型是马尔可夫链的一种，它的状态不能直接观察到，但能通过观测向量序列观察到，每个观测向量都是通过某些概率密度分布表现为各种状态，每一个观测向量是由一个具有相应概率密度分布的状态序列产生。所以，隐马尔可夫模型是一个双重随机过程----具有一定状态数的隐马尔可夫链和显示随机函数集。

### 基本算法
针对以下三个问题，人们提出了相应的算法
1 评估问题： 前向算法
2 解码问题： Viterbi算法
3 学习问题： Baum-Welch算法(向前向后算法)

### 模型表达
隐马尔可夫模型（HMM）可以用五个元素来描述，包括2个状态集合和3个概率矩阵：
1. 隐含状态 S
这些状态之间满足马尔可夫性质，是马尔可夫模型中实际所隐含的状态。这些状态通常无法通过直接观测而得到。（例如S1、S2、S3等等)
2. 可观测状态 O
在模型中与隐含状态相关联，可通过直接观测而得到。(例如O1、O2、O3等等，可观测状态的数目不一定要和隐含状态的数目一致。）
3. 初始状态概率矩阵 π
表示隐含状态在初始时刻t=1的概率矩阵，(例如t=1时，P(S1)=p1、P(S2)=P2、P(S3)=p3，则初始状态概率矩阵 π=[ p1 p2 p3 ].
4. 隐含状态转移概率矩阵 A。
描述了HMM模型中各个状态之间的转移概率。
其中Aij = P( Sj | Si ),1≤i,,j≤N.表示在 t 时刻、状态为 Si 的条件下，在 t+1 时刻状态是 Sj 的概率。
5. 观测状态转移概率矩阵 B （英文名为Confusion Matrix，直译为混淆矩阵不太易于从字面理解）。
令N代表隐含状态数目，M代表可观测状态数目，则：
Bij = P( Oi | Sj ), 1≤i≤M,1≤j≤N.
表示在 t 时刻、隐含状态是 Sj 条件下，观察状态为 Oi 的概率。
总结：一般的，可以用λ=(A,B,π)三元组来简洁的表示一个隐马尔可夫模型。隐马尔可夫模型实际上是标准马尔可夫模型的扩展，添加了可观测状态集合和这些状态与隐含状态之间的概率关系。



