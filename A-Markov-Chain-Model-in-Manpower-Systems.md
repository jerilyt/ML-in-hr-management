---
title: A Markov Chain Model in Manpower Systems
date: 2020-08-04 00:22:09
tags:
- 人力资源
- Markov Chain
categories:
- Stochastic Model
---

# 如何根据互联网员工年龄

## 基于 Markov 过程预测未来公司人员规模

其实不管是互联网、金融还是其他行业，拟定一个合理员工年龄结构战略对任何一家企业的长期发展都有着极其重要的影响，同时也是人力规划的核心目标。在制定未来一段时期人员补充方案时，企业有必要对不同方案下的员工年龄结构变化趋势进行预测，从而判断不同方案下员工队伍的变化是否能支撑未来发展需要。
<!--more-->

### 从Markov Chain到模型构建

这个模型我是基于Markov Chain 的思想来构建的。何为Markov Chain？简单来说就是假设某一时刻状态转移的概率只依赖于它的前一个状态。

在年龄预测模型中，基本的几个状态为不同的年龄段。Markov Chain 模型主要是分析一个人在某一阶段内由一个年龄段调到另一个年龄段的可能性。从一个年龄段转移到另一个年龄段的状态属于内部流动，意味着该员工仍在该企业就职。但还存在外部流动的状态，离职与退休。属于员工流失。通过运用Markov 过程原理，分析每个年龄段的员工流动到不同状态的流动趋势和概率，构建完整的预测周期运算过程，以便为人力资源在新增人员对企业总量规模、人员年龄结构的规划中提供依据。



这么说不太直观，举个例子。在项目中，我将内部流动分为7个阶段，分别为20-24，25-28，29-31，32-35，36-40，41-50，51-60。外部流动为离职率和退休率。

根据Markov Chain原理，转移矩阵中的概率就是某一状态到另一状态的可能性。下表就是项目中转移矩阵的结构。我们就用这个转移矩阵就表示这一年内发生的事情。其中，p_23表示今年处于[25,28]这一年龄阶段的员工在下一年流向[29,31]的概率。为了方便计算，我用频率表示，即今年处于[25,28]这一年龄阶段而在下一年流向[29,31]的员工总数与今年处于[25,28]这一年龄阶段的员工总数比值。为什么大部分概率为0其实很好理解，这些概率为0的状态转移情况是不可能发生的。今年[25,28]年龄段的员工明年可能是26,27,28,29，因此在内部流动中只能有两种状态。而先前说的p_23为今年28岁的员工明年没有离职流向[29,31]年龄的概率。

**Transition Matrix from Preview Year to Next Year**

|       | 20-24 | 25-28 | 29-31 | 32-35 | 36-40 | 41-50 | 51-60 | Dismission | Retirement | Recruitment |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ---------- | ---------- | ----------- |
| 20-24 | p_11  | p_12  | 0     | 0     | 0     | 0     | 0     | p_18       | 0          | p_1_10      |
| 25-28 | 0     | p_22  | p_23  | 0     | 0     | 0     | 0     | p_28       | 0          | p_2_10      |
| 29-31 | 0     | 0     | p_33  | p_34  | 0     | 0     | 0     | p_38       | 0          | p_3_10      |
| 32-35 | 0     | 0     | 0     | p_44  | p_45  | 0     | 0     | p_48       | 0          | p_4_10      |
| 36-40 | 0     | 0     | 0     | 0     | p_55  | p_56  | 0     | p_58       | 0          | p_5_10      |
| 41-50 | 0     | 0     | 0     | 0     | 0     | p_66  | p_67  | p_68       | 0          | p_6_10      |
| 51-60 | 0     | 0     | 0     | 0     | 0     | 0     | p_77  | p_78       | p_79       | p_7_10      |



### 模型假设与定义

![images](hypotheses.png)

假设，

* $N(t)$ 为第t年年初各年龄段员工总数，$N(t) = [n_1(t), n_2(t), ... , n_7(t)]$，其中$n_1(t),n_2(t),...,n_7(t)$ 分别为第t年20-24岁，25-28岁，29-31岁，32-35岁，36-40岁，41-50岁，51-60岁的员工人数。
* $L(t)$ 为在第t整年各年龄段离职员工人数，$L(t) = [l_1(t), l_2(t), ... , l_7(t)]$，而$P(t) = [p_1(t), p_2(t), ... , p_7(t)]$分别为发生概率，即$L(t)=N(t)\times P(t)$
* $R_t(t)$ 为在第t整年各年龄段退休员工人数，$R_t(t)=[0,0,0,0,0,0,r_7(t)]$ 
* $R_c(t)$ 为在第t整年各年龄段新增员工人数
* $P_{ij}(t+1)$ 第t年从状态$i$到下一年转移到状态$j$的概率

**模型假设** : 下一年年初的员工总数=这一年年末剩余+下一年年初新增。而这一年年末剩余就等于这一年年初的总数乘上转移矩阵。这就是markov process在这个算法中的运用。$$N(t+1)=N(t)*P_{ij}(t+1)+R_c(t+1)$$












