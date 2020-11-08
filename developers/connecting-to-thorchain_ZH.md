---
简介: '如何连接 Midgard, THORNode 和 Tendermint layer.'
---

# 连接闪链

在线的节点IP地址可以下方链接获取：

{% tabs %}
{% tab title="测试网 TESTNET" %}
[https://testnet-seed.thorchain.info](https://testnet-seed.thorchain.info)
{% endtab %}

{% tab title="混沌网 CHAOSNET" %}
[https://chaosnet-seed.thorchain.info](https://chaosnet-seed.thorchain.info)
{% endtab %}

{% tab title="主网 MAINNET" %}
[https://seed.thorchain.info](https://seed.thorchain.info)
{% endtab %}
{% endtabs %}

有三个获取网络信息的渠道：

1. MIDGARD:  面向使用者的信息，如资金池，交易信息。DeFi面板，钱包，和交易所主要跟MIDGARD连接。
2. THORNODE: 跟闪链状态机器相关的原始区块链信息。闪链的区块链浏览器主要跟THORNODE连接。
3. TENDERMINT: Tendermint的标准信息，所有区块链浏览器都可以从此获得基础信息。

{% tabs %}
{% tab title="MIDGARD" %}
Midgard提供关于资金池，交易量，用户，和流动性提供者的信息。

Port: `8080`

RPC Guide:  
[http://&lt;host&gt;:8080/v1/doc](http://<host>:8080/v1/doc)

Port: `8080`

RPC Guide:  
[http://:8080/v1/doc](http://<NODE_IP>:8080/v1/doc)

Example:  
[http://:8080/v1/stats](http://<NODE_IP>:8080/v1/stats)
{% endtab %}

{% tab title="THORNODE" %}
THORNode提供关于闪链网络状态的信息，比如说网络资金，交易信息等等。

Port: `1317`

RPC Guide:  
[https://gitlab.com/thorchain/thornode/-/blob/master/x/thorchain/query/query.go](https://gitlab.com/thorchain/thornode/-/blob/master/x/thorchain/query/query.go)

Example:  
[http://:1317/thorchain/constants](http://<NODE_IP>:1317/thorchain/constants)
{% endtab %}

{% tab title="TENDERMINT" %}
## **RPC**

RPC allows base blockchain information to be returned.

TESTNET Port: `26657`

MAINNET Port: `27147`

RPC Guide:  
[https://docs.tendermint.com/master/rpc/](https://docs.tendermint.com/master/rpc/)

Example:  
[http://:26657/genesis](http://<NODE_IP>:26657/genesis)

## **P2P**

P2P is the network layer between nodes, useful for network debugging.

TESTNET Port: `26656`

MAINNET Port: `27146`

P2P Guide  
[https://docs.tendermint.com/master/spec/p2p/](https://docs.tendermint.com/master/spec/p2p/)
{% endtab %}
{% endtabs %}
