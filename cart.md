树模型是机器学习中一类强有力的模型。曾经有个Princeton大学的教授来我校做报告的时候说过，

    “tree model是最保险的模型。当你不知道该用什么模型好的时候，Tree model一定是一个理想的选择。它可能不会是最好，但是一定不会很坏。”

在这里我想总结一下三种最基本、最常用的决策树模型 ID3，C4.5和CART，因为所做的项目中用到了CART模型，所以想梳理一下，加深对决策树模型的理解。

决策树模型最核心的intuition就是在树的每一层对数据集进行切分，分成两个子集合，切分的依据就是“使得分开的两类样本在各自的集合中有序度最高（无序度最低）”。这样的切分方式对分类问题来说才是最好的切分。既然要使集合内样本有序度高（也就是样本纯度高），数学上必须引入一个概念来衡量有序程度，那就是**信息熵(Information Entropy)**。这是信息论祖师爷**克劳德 ⋅香农Claude Shannon**提出来的。

　　熵被用来衡量一个随机变量出现的期望值。熵越大，一个变量的不确定性就越大（也就是可取的值很多），把它搞清楚所需要的信息量也就越大，熵是整个系统的**平均消息量**。 信息熵是信息论中用于度量信息量的一个概念。一个系统越是有序，信息熵就越低；反之，一个系统越是混乱，信息熵就越高。它的计算公式为：

$$Entropy = H(D)=E[I(D)] = -\sum_i^{n} p_ilog_2(p_i)$$
其中，D是样本集合，$I(D)=-log_2p_i$是样本集合的信息量, $p_i$是任意样本属于类i的*非零概率*。

    “熵越大，说明系统越混乱，携带的信息就越少。熵越小，说明系统越有序，携带的信息就越多。信息的作用就是在于消除不确定性”
（基于此可以证明Gaussian的熵为$H[x] = \frac{1}{2}{1+ln(2\pi\sigma^2)}$）

这三个模型最根本的区别就是对树枝进行分裂的时候分裂信息（或者说标准）不同。但是都是基于信息熵的。

# ID3
采用信息增益（Information Gain）作为节点的分裂标准。
信息增益在统计学中成为互信息。互信息(mutual Information)是条件概率与后验概率的比值。
- 信息增益计算：
 $$IG(A) = H(D)-H(D|A)$$
 $$where，H(D|A)=\sum_i \frac{|D_j|}{D}H(D_j)$$
 A可以看做分类特征，以这个特征对D集合切分的时候就要计算她的IG。条件熵H(D|A)表示的是以A特征切分的时候，样本集D的混乱程度。如果以A特征切分时正好能把D的类别完全分开，那么分开的类别最有序，H(D|A)=0.


# C4.5
采用信息增益率（Information Gain Ratio）作为节点的分裂标准
ID3中用信息增益有一个缺点就是训练样本类别不平衡，倾向选择取值多的属性
所以C4.5在以下几个方面进行了改进：
- 以信息增益率为标准选择切分属性。
- 在树的构造过程中完成剪枝，防止过拟合。
- 可以对连续属性离散化处理。
- 可以处理不完整数据集

信息增益率计算公式：
$$IGR(A)=\frac{IG(A)}{Split(D|A)}$$
# CART
采用基尼指数（Gini Index）作为节点的分裂标准
CART全称Classification and Regression Tree，所以它既可以做分类也可以做回归。这里主要讲分类。
基尼指数定义：

分类问题中， 假设有K个类，样本属于第k个类的概率为p_k,则概率分布的基尼指数为
$$Gini(p) = \sum_{k=1}^Kp_k(1-p_k)$$
由定义可以看到，当类别确定性高的的时候基尼系数趋近于0.
对于给定样本集合D,其基尼系数为
$$Gini(D)=1-\sum_{k=1}^K(\frac{|C_k|}{D})^2$$
根据特征A对数据集D切分，得到的基尼指数：
$$Gini(D,A)=\frac{|D_1|}{D}Gini(D_1)+\frac{|D_2|}{D}Gini(D_2)$$
