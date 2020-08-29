---
简介: '闪链是如何提供连续不间断并且有经济激励性的流动资金池的？'
---

# 喷泉式流动资金池

喷泉式流动资金池(Continuous Liquidity Pools)是闪链的标志性功能；相比普通交易所的订单系统，CLP有以下优势：
* 为所有类型的资产提供不停歇的流动资金池。
* 为用户创造一个透明公正的交易环境，告别传统中心化交易所。
* 为内部和外部提供链上资产价格信息
* 给所有用户差价套利交易的机会
* 因为手续费是建立在一个趋向于零的公式上，链上资金池的价格可以自然得趋向于外部市场价格
* 公正得为资金提供者谋取红利
* 根据市场对资金的需求变化而变化

## CLP 公式

| 变量 | 含义 | 变量 | 含义 |
| :--- | :--- | :--- | :--- |
| x | 输入货币数量 | X | 输入货币余额 |
| y | 输出货币数量 | Y | 输出货币余额 |

从固定的乘积开始:

$$
Eqn 1: X*Y = K
$$

算出 `XYK`:

$$
Eqn 2: \frac{y}{Y} = \frac{x}{x+X} \rightarrow y= \frac{xY}{x+X}
$$

算出价值基础 \( `x`相对`y` 的购买力 \) 和滚动差价（输出数量`y`和现货数量的差别）

$$
Eqn 3: Value_y = \frac{xY}{X}
$$

$$
Eqn 4: slip =\frac{Value_y - y}{Value_y} =\frac{( \frac{xY}{X})-y}{ \frac{xY}{X}} = \frac{x}{x+X}
$$

计算基于滚动差价的系统收费:

$$
Eqn 5: fee = slip * output =  \frac{x}{x+X} * \frac{xY}{x+X} = \frac{x^2Y}{(x+X)^2}
$$

将收费从输入数量扣除：

$$
Eqn 6: y = \frac{xY}{x+X} - \frac{x^2Y}{(x+X)^2} \rightarrow y= \frac{ xYX} {(x+X)^2 }
$$

对比公式2和6我们可以发现, CLP公式算出的输入货币数量就是XYK公式乘以（1-slip)

## CLP模型的演变历史

### 叠盘模型

这是最简单的模型，将两种资产一比一的进兑换。如果流动资金池采用这种模型可能会导致资不抵债，这种模型不能更改资产的价格也没有内置的收费功能。

$$
Eqn 8: y = x
$$

### 定价模型

这种模型预先设置好资产的交换比例，但是资金池同样可能会资不抵债。

$$
Eqn 9: y = \frac{xY}{X}
$$

### 固定乘积模型

固定乘积模型就是我们一开始使用的XYK公式。这种模型可以动态的改变资产价格同时防止资不抵债，但是没有内置的收费功能。

$$
Eqn 10: y= \frac{xY}{x+X}
$$

### 固定收费模型

以下公式所展现的模型固定得收取每笔交易的百分之0.3为手续费，但是手续费和资产流动性不挂钩。

$$
Eqn 11: y= 0.997 * \frac{xY}{x+X}
$$

### 滚动收费模型 \(CLP\)

这是闪链使用的模型，每笔交易收取的费用跟资金池流动性需求挂钩，需求越大，收费越高。

$$
Eqn 12: y= \frac{ xYX} {(x+X)^2 }
$$

{% hint style="warning" %}
滚动收费模型鼓励交易者分批次交易来减少费用。如果一个协议不能为进入的交易排序，比如ETH，交易者们就会通过使用更多的gas来加速他们交易通过的时间，但是这样是付费给挖矿者而不是协议本身。
闪链根据交易者付出的费用来排列交易顺序（交换队列），保证系统获取最大盈利。
{% endhint %}

## CLP收费模型的优势

CLP配合一个有效的交换队列有以下优势：

* 当对流动性需求慢慢变少时，手续费会趋向于零，所以资金池的币价可以趋向与真正的市场价
* 交易者需要付出更多费用来保证他们的交易首先通过，为系统和资金提供者带来最大收益
* 手续费与资金流动性需求挂钩
* 因为快速和大量的交易会导致更多的费用，所以资金池币价也拥有了惯性
* 让更多人能参与套利交易，因为一个人不能一次性完成套利交易（想要一次完成需要付出巨大费用，得不偿失）
* 交易者需要考虑时间因素，越没有耐心，手续费越高

最后一点是最重要的，一个和流动性需求挂钩的收费系统保证了市场变化信息可以传递给每一个在市场交易的人，而不是几个有内部消息的人。

## 虚拟资金深度

Balances of the pool \(X and Y\), are used as inputs for the CLP model. An amplification factor can be applied \(to both, or either\) in order to change the "weights" of the balances:

| Element | Description |
| :--- | :--- |
| a | Input Balance Weight |
| b | Output Balance Weight |

$$
Eqn 7: y= \frac{ xYbXa} {(x+Xa)^2 }
$$

If `a = b = 2` then the pool behaves as if the depth is twice as deep, the slip is thus half as much, and the price the swapper receives is better. This is akin to smoothing the bonding curve, but it does not affect pool solvency in any way. Virtual depths are currently not implemented

If `a = 2, b = 1` then the `Y` asset will behave as though it is twice as deep as the `X` asset, or, that the pool is no longer 1:1 bonded. Instead the pool can be said to have 67:33 balance, where the liquidity providers are twice as exposed to one asset over the other. 

{% hint style="info" %}
Virtual Depths and Dissimilar Weighting have not been added to THORChain, because their impact on the  [Incentive Pendulum](incentive-pendulum.md) as well as the loss of revenue to Liquidity Providers has not yet been investigated. 

THORChain ruthlessly maximises revenue for itself, taking the perspective that liquidity pools are an incentive **race-to-the-top** as opposed to a fee **race-to-the-bottom**. In typical markets, market-takers are value-extractive from market-makers, whilst in THORChain, market-takers pay handsomely for the privilege of access to liquidity. 
{% endhint %}

