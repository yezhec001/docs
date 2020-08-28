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

Start with the fixed-product formula:

$$
Eqn 1: X*Y = K
$$

Derive the raw "XYK" output:

$$
Eqn 2: \frac{y}{Y} = \frac{x}{x+X} \rightarrow y= \frac{xY}{x+X}
$$

Establish the basis of Value \(the spot purchasing power of `x` in terms of `Y` \) and slip, the difference between the spot and the final realised `y`:

$$
Eqn 3: Value_y = \frac{xY}{X}
$$

$$
Eqn 4: slip =\frac{Value_y - y}{Value_y} =\frac{( \frac{xY}{X})-y}{ \frac{xY}{X}} = \frac{x}{x+X}
$$

Derive the slip-based fee:

$$
Eqn 5: fee = slip * output =  \frac{x}{x+X} * \frac{xY}{x+X} = \frac{x^2Y}{(x+X)^2}
$$

Deduct it from the output, to give the final CLP algorithm:

$$
Eqn 6: y = \frac{xY}{x+X} - \frac{x^2Y}{(x+X)^2} \rightarrow y= \frac{ xYX} {(x+X)^2 }
$$

Comparing the two equations \(Equation 2 & 6\), it can be seen that the Base XYK is simply being multiplied by the inverse of Slip \(ie, if slip is 1%, then the output is being multiplied by 99%\). 

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
The slip-based fee model breaks path-independence and incentivises traders to break up their trade in small amounts.   
For protocols that can't order trades \(such as anything built on Ethereum\), this causes issues because traders will compete with each other in Ethereum Block Space and pay fees to miners, instead of paying fees to the protocol.   
It is possible to build primitive trade ordering in an Ethereum Smart Contract in order to counter this and make traders compete with each other on trade size again.   
THORChain is able to order trades based on fee & slip size, known as the Swap Queue. This ensures fees collected are maximal and prevents low-value trades. 
{% endhint %}

## Benefits of the CLP Model

Assuming a working Swap Queue, the CLP Model has the following benefits:

* The fee paid asymptotes to zero as demand subsides, so price delta between the pool price and reference market price can also go to zero. 
* Traders will compete for trade opportunities and pay maximally to liquidity providers.
* The fee paid for any trade is responsive to the demand for liquidity by market-takers.
* Prices inherit an "inertia" since large fast changes cause high fee revenue
* Arbitrage opportunities are democratised as there is a diminishing return to arbitrage as the price approaches parity with reference
* Traders are forced to consider the "time domain" \(how impatient they want to be\) for each trade. 

The salient point is the last one - that a liquidity-sensitive fee penalises traders for being impatient. This is an important quality in markets, since it allows time for market-changing information to be propagated to all market participants, rather than a narrow few having an edge. 

## Virtual Depths

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

