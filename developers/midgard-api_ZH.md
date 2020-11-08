---
简介: Midgard 详情
---

# Midgard API

## 概括

Midgard是第二层的REST API，主要为前端使用者提供网络数据。大部分对闪链网络数据的请求将通过Midgard来执行，这样可以减少对网络本身资源的需求。

### 文档

Midgard 的文档可以通过 `http://<host>:8080/v1/doc` 获取

### 区块链代理

Midgard同时可以作为各个区块链的代理，可以广播签名的交易到闪链的网络上。
一个钱包无需自己运行一个比特币节点，可以直接发送签名交易或者使用RPC和THORNode沟通：
`http://<host>:8332`
