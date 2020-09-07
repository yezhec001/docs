---
简介: 为闪链提供资金流动性
---

# 权益质押者

质押者为闪链提供资金流动性来获取手续费分红和系统奖励。奖励的多少由资金池和网络运行状态决定。

{% hint style="warning" %}
用户在质押的同时需要知道每个资金池的底层资产的市场价是有波动性的，并且质押者会承担价格波动的风险。

系统的奖励也是波动性的，在资产市场价波动剧烈的情况下，奖励可能会少于资产的无常损失（impermanent loss)。

在取回质押的资产时，用户应该了解他们取回的资产不是固定的，而是该资金池的一个百分比。 
{% endhint %}

## 奖励

流动性提供者通过质押资产来获取利息。如果用户质押资产在BTC/RUNE资金池，他们获得的奖励将会是BTC和RUNE。

利息在每一个区块产出时计算，用户在取回质押资产时将会获取利息。利息的产出和每一个区块时间内该资金池进行的交易次数关联，交易费用会分配给质押者和节点操作员。如果一个区块时间内没有交易，由该资金池内的资金数量决定奖励大小。

{% tabs %}
{% tab title="BLOCK WITH SWAPS" %}
一个产生交易费用的区块是怎么分配奖励的。
以下例子中，1000个RUNE正在被分配：

| **资金池深度 \(RUNE\)** | **交易费**  | **交易费百分比** | 奖励 |
| :--- | :--- | :--- | :--- |
| 1,000,000 | 1000 | 33% | 333 |
| 2,000,000 | 0 | 0% | 0 |
| 3,000,000 | 2000 | 67% | 667 |
| **6,000,000** | **3000** | **100%** | **1000** |
{% endtab %}

{% tab title="BLOCK WITH NO SWAPS" %}
一个没有产生交易费用的区块是怎么分配奖励的。
以下例子中，1000个RUNE正在被分配：

| **资金池深度 \(RUNE\)** | **交易费**  | **交易费百分比** | 奖励 |
| :--- | :--- | :--- | :--- |
| 1,000,000 | 0 | 17% | 167 |
| 2,000,000 | 0 | 33% | 333 |
| 3,000,000 | 0 | 50% | 500 |
| **6,000,000** | **0** | **100%** | **1000** |
{% endtab %}
{% endtabs %}

通过这样的方式，交易需求越大的资金池就能带来越大的区块奖励。

### 影响利息的因素

**质押者的资金池所有权百分比** – 在一个资金池内质押的越多，获取该资金池的奖励越多。

**交易量** – 交易量越大资金池获取的手续费越高，质押者获取的奖励也越高。

**单次交易的大小** – 当用户在平台上进行大单交易时，如果他们没有耐心将一次交易分成好几份，就会产生更多的手续费。

**钟摆模式** – 根据质押资产和节点绑定资产的比例，奖励分配也不同。有时分配更多给质押者来促进网络的安全。 [Learn more](../how-it-works/incentive-pendulum.md).

**资产价格变动 --** 随着用户使用资金池来进行交易和资产价格的变动，质押者获取的两种资产数量也会不同。 

### 计算资金池所有权百分比

当用户质押资产时，资金池所有权百分比如下计算：

$$
(R + A) * (r * A + R * a) \over 4 * R * A
$$

* r = 质押RUNE数量
* R = 资金池内RUNE数量
* a = 质押的配对货币数量
* A = 资金池内配对货币数量

用户获取的奖励和他资金池所有权百分比成正比。

## 运行原理

### **质押资产**

闪链上质押资产无需经过第三方监管或者允许。

质押者可以提议新的资金池或者给现有资金池增加流动性。任何人只要质押新货币就能提议该新货币的资金池，详情 [asset listing/delisting](https://)。当新资金池被创建时，所有人都可以为其增加流动性。

质押和取出资产完全没有第三方监管。只有质押者本人可以取出被质押的资产，节点收到网络协议控制所以无法动用质押资产。

#### 流程

流动性可以添加到现有的资金池来增加资金深度和吸引交换者。资金池越深，费用越小；费用越小，交易量一般会更大，给资金池带来更多收益。

一般来说，质押者应该对称质押，但是在资金池已经不平衡的情况下，不对称质押更加有利。

{% hint style="info" %}
**对称和不对称质押**

**对称质押** 用户质押同等价值的两种资产，比如说1000美金的BTC和1000美金的RUNE。

**不对称质押** 用户质押不同价值的两种资产，比如说2000美金的BTC和0美金的RUNE。在这种情况下，套利者就会将他的1000美金BTC换成RUNE，所以质押者最后的两种资产都会小于1000美金。  
  
_Note: 自己先使用资金池交换资金达成对称然后质押和直接不对称质押让套利者来对称你的资金效果是一样的。_ 
{% endhint %}

### 取出资产

质押者可以随时取出他们的资产。用户发起取款请求然后获取他们资金池所有权的百分比，网络费用会自动扣除，并添加到网络费用储备 [the network reserve](../how-it-works/emission-schedule.md).

### **交易费和区块奖励带来的利息**

Stakers earn a yield on the assets they deposit. This yield is made up of fees and rewards.

**Fees** are paid by swappers and traders. Most swaps cause the ratio of assets in the liquidity pool to diverge from the market rate.

{% hint style="info" %}
The ratio of assets in a liquidity pool is comparable to an exchange rate.
{% endhint %}

This change to the ratio of assets is called a 'slip'. A proportion of each slip is kept in the pool. This is allocated to stakers and forms part of their staking yield. Learn more about [swapping](swapping.md).

**Rewards** come from THORChain's own [reward emissions](../how-it-works/emission-schedule.md). Reward emissions follow a predetermined schedule of release.

Rewards also come from a large token reserve. This token reserve is continuously filled up from[ network fees](../how-it-works/fees.md#network-fee). Part of the token reserve is paid out to stakers over the long-term. This provides continuous income even during times of low exchange volume.

Learn about how [factors affecting yield and how yield is calculated](staking.md#compensation).

{% hint style="info" %}
See here for an [interactive example](https://rebase.foundation/network/thorchain/system-component/providing-liquidity) of the staking process.
{% endhint %}

### 策略

**保守** 质押者选择流动性已经很高的资金池来最小化风险和最大化收益。

**激进** 质押者选择流动性低，但是需求高的资金池（更多手续费）来最大化收益。

## 要求和费用

Stakers must have assets to deposit and their assets must be native to a supported chain. There is no minimum amount to stake in existing pools. However new assets must win a competition to be listed – larger value deposits will be listed over smaller value deposits.

The only direct cost to stakers is the [network fee](../how-it-works/fees.md#network-fee), charged for depositing and withdrawing assets. An indirect cost to stakers comes in the form of impermanent loss. Impermanent loss is common to Constant Function Market Makers like THORChain. It leads to potential loss of staker purchasing power as a result of price slippage in pools. However, this is minimised by THORChain's  [slip-based fee](../how-it-works/fees.md#slip-based-fee).

Stakers are not subject to any direct penalties for misconduct.
