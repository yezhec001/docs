---
简介: 在线节点IP地址通过种子服务获取
---

# 种子服务

通过团队正在维护的一个种子服务，可以简单的获取在线节点的IP地址。

{% hint style="warning" %}
通过种子服务获取的节点有可能提供错误信息， 开发者可以通过`asgardex-midgard` 部件来筛选节点. 
{% endhint %}

在线节点的IP地址可以通过以下链接获取:

{% tabs %}
{% tab title="测试网" %}
[https://testnet-seed.thorchain.info](https://testnet-seed.thorchain.info)
{% endtab %}

{% tab title="主网" %}
[https://seed.thorchain.info](https://seed.thorchain.info)
{% endtab %}
{% endtabs %}

## 筛选节点IP地址

网络安全模型基于2/3网络节点的共识，这代表了有些节点可以不正当的提供错误的地址。开发者可以通过咨询1/3的节点来确保自己获得的地址无害。

具体过程如下:

1. 获取所有在线的节点IP
2. 获取至少1/3节点提供的宝库地址
3. 确保地址一样，如果不同，重新进行 \(2\)
4. 如果地址一样，可以直接使用1/3节点中的任意一个使用
5. 经常性的进行这个过程，从 \(1\) 开始

{% hint style="danger" %}
如果你对安全性的需求很高，可以自己维护一个无需质押的节点(non-consensus)。
{% endhint %}

## 分布式计划

团队可以在未来将种子服务移植到一个WEB3平台的智能合同，开发者可以直接使用WEB3来获取节点的IP地址。
