---
简介: 闪链针对资产交换者的价值定位
---

# 资产交换者

用户可以使用闪链来交换数字资产。 网络为用户提供以下功能：

* 通过跨链技术和简易上币功能，为用户提供多种多样的数字资产
* 通过开源协议为用户提供一流的无需第三方许可的交易体验
* 一键操作Binance Chain, Ethereum, Bitcoin, 和Monero

## 交换是怎么完成的

### 可供交易的资产

用户可以交换与闪链连接的区块链上的任意资产，比如ETH换BTC；也可以将支持的任意资产换成 [RUNE](../rune.md).

{% hint style="info" %}
了解网络添加区块链和资产的机制：[the Governance section](../how-it-works/governance.md).

用户在质押新资产时，该新资产自动加入系统资产添加的等待列表。用户只有在资产被添加到网络并且完成了前期的资金储备以后才能开始交易该资产。

{% endhint %}

### 去中心化结构



THORChain manages the swaps in accordance with the rules of the state machine - which is completely autonomous. Every swap that it observes is finalised, ordered and processed. Invalid swaps are refunded, valid swaps ordered in a transparent way that is resistant to front-running. Validators can not influence the order of trades, and are punished if they fail to observe a valid swap. 

Swaps are completed as fast as they can be confirmed, which is around 5-10 seconds. 

### Continuous Liquidity Pools

Swaps on THORChain are made possible by liquidity pools. These are pools of assets deposited by Stakers, where each pool consists of 1 connected asset, for example Bitcoin, and THORChain's own asset, RUNE. They're called Continuous Liquidity Pools because RUNE, being in each pool, links all pools together in a single, continuous liquidity network.

When a user swaps 2 connected assets on THORChain, they swap between two pools:

1. Swap to RUNE in the first pool,
2. Move that RUNE into the second pool,
3. Swap to the desired asset in the second pool with the RUNE from \(2\)

The THORChain state machine handles this swap in one go, so the user is never handles RUNE. 

See [this example](swapping.md#example-connected-asset-binance-coin-to-connected-asset-bitcoin) for further detail and the page below for broader detail on Continuous Liquidity Pools.

{% page-ref page="../how-it-works/continuous-liquidity-pools.md" %}

### Calculating Swap Output

The output of a swap can be worked out using the formula

$$
y = \frac{ xYX} {(x+X)^2 }
$$

where

* x is input asset amount
* X is input asset balance
* y is output asset amount
* Y is output asset balance

#### Example

The BTC.RUNE pool has 100 BTC and 2.5 million RUNE. A user swaps 1 BTC into the pool. Calculate how much RUNE is output:

$$
\frac {1 * 2500000 * 100 } {(1 + 100)^2} = 24,507.40
$$

This user swaps 1 BTC for 24,507.40 RUNE.

{% hint style="info" %}
Run through an [interactive tutorial of an asset swap](https://app.bepswap.com/swap).
{% endhint %}

### Costs

The cost of a swap is made up of two parts:

1. Network Fee
2. Price Slippage

All swaps are charged a network fee. The network fee is dynamic – it's calculated by averaging a set of recent gas prices. Learn more about [Network Fees](../how-it-works/fees.md#network-fee).

Note that users who force their swaps through quickly cause large slips and pay larger fees to stakers.
