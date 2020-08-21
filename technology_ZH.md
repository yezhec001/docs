---
简介：闪链使用的单向状态叠，状态机器，和阈值签名方案
---

# 科技

## 概况

闪链融合并且创新了几项现有的技术来完成它的使命：

1. 单项状态叠完善了跨链协议
2. 状态机器主要负责资产的交换和赎回逻辑
3. Bifröst 将赎回的逻辑翻译成特定区块链链能理解的语言
4. 通过TSS来分配签名的任务

![How THORChain works](.gitbook/assets/image%20%284%29.png)

## Bifröst协议: 单向状态叠

每条和闪链连接的区块链都有一个单独的Bifröst模块，用以适配不同链的需求。
当节点和区块链完成同步时，他们将开始观察每条链的宝库地址，宝库地址是闪链储存链上资产的地址。打个比方，节点完成和比特币区块链同步以后，会观察闪链的比特币宝库。当比特币链上有和宝库相关的资金流动时，节点会解码这些资金流动，并且将他们翻译成闪链独特的见证交易，见证交易有统一的格式：

```text
type Tx struct {
	ID          TxID    `json:"id"`
	Chain       Chain   `json:"chain"`
	FromAddress Address `json:"from_address"`
	ToAddress   Address `json:"to_address"`
	Coins       Coins   `json:"coins"`
	Gas         Gas     `json:"gas"`
	Memo        string  `json:"memo"`
}
```
闪链在观察区块链交易的同时会从不同的节点收集签名。当大多数的节点见证的交易是一模一样的时候，见证交易就会从等待的状态转换为完成的状态。

```text
type ObservedTx struct {
	Tx             common.Tx        `json:"tx"`
	Status         status           `json:"status"`
	OutHashes      common.TxIDs     `json:"out_hashes"` 
	BlockHeight    int64            `json:"block_height"`
	Signers        []sdk.AccAddress `json:"signers"` 
	ObservedPubKey common.PubKey    `json:"observed_pub_key"`
}
```
总体的过程如下图，每条链的独有模块尽可能做到轻量，只包含连接到某条链的必要信息。

![](.gitbook/assets/image%20%286%29.png)

## 闪链的状态机器
状态机器负责计算最终交易结果，排列交易顺序，然后将所有的交易分工到不同的对外宝库。最后，txOut会被产出并被储存。

![](.gitbook/assets/image%20%2816%29.png)

txOut如下：
```text
type TxOutItem struct {
	Chain       common.Chain   `json:"chain"`
	ToAddress   common.Address `json:"to"`
	VaultPubKey common.PubKey  `json:"vault_pubkey"`
	Coin        common.Coin    `json:"coin"`
	Memo        string         `json:"memo"`
	MaxGas      common.Gas     `json:"max_gas"`
	InHash      common.TxID    `json:"in_hash"`
	OutHash     common.TxID    `json:"out_hash"`
}
```
txOut标识了交易的最终细节，比如说具体是哪条链，收款人地址，宝库地址，和GAS上限;同时也包括了发起这次交易和结束这次交易的Hash代码。

## 签名人 \(Bifröst\)
当完整的交易被创造以后，签名人会下载到本地然后使用对应的区块链模块来完成对应的交易。所有签名将会经过TSS，并最终发布到母链上。

![](.gitbook/assets/image%20%2810%29.png)


