---
简介: '交易过程, 交易MEMO, 退款和货币注释.'
---

# 交易MEMO

### 概括

闪链一直在观察着所有和系统宝库相关钱包地址的流水，宝库地址可以通过Midgard API 查询到。
如果要使用闪链生态，就需要在发送到宝库地址的交易MEMO上使用正确的格式。如果交易MEMO不符合系统规则，该交易会自动退回给用户。

### 各个链的MEMO机制

每条链都有它自己的添加MEMO的方式：

| Chain | Mechanism | Notes |
| :--- | :--- | :--- |
| Bitcoin | OP\_RETURN | Limited to 80 bytes, long memos need to use two OP\_RETURN outputs. |
| Ethereum | Smart Contract Input | The user can pass in the memo in the `deposit(asset, value, memo)` function, where `memo` is bytes32. This is emitted as an event.  |
| Binance Chain | MEMO | Each transaction has an optional memo, limited to 128 bytes.  |
| Monero | Extra Data | Each transaction can have attached `extra data` field, that has no limits.  |

### 交易类别

The following transactions are permitted:

<table>
  <thead>
    <tr>
      <th style="text-align:left">类别</th>
      <th style="text-align:left">携带货币</th>
      <th style="text-align:left">MEMO</th>
      <th style="text-align:left">系统结果</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">STAKE（质押）</td>
      <td style="text-align:left">
        <p><code>RUNE 和/或者 Token</code>
        </p>
        <p>可以单货币或者双货币进行质押</p>
      </td>
      <td style="text-align:left"><code>STAKE:ASSET</code>
      </td>
      <td style="text-align:left">在制定资金池质押</td>
    </tr>
    <tr>
      <td style="text-align:left">WITHDRAW（取出）</td>
      <td style="text-align:left">
        <p><code>0.00000001 BNB</code>
        </p>
        <p>任意大于0的数量</p>
      </td>
      <td style="text-align:left">
        <p><code>WITHDRAW:ASSET:PERCENT</code>
        </p>
        <p>取出相应比例的资金(0-10000, 10000=100%)</p>
      </td>
      <td style="text-align:left">从指定资金池解除质押</td>
    </tr>
    <tr>
      <td style="text-align:left">SWAP（交换）</td>
      <td style="text-align:left">
        <p><code>RUNE 或者 Token</code>
        </p>
      </td>
      <td style="text-align:left">
        <p><code>SWAP:ASSET:DESTADDR:LIM</code>
        </p>
        <p>如果<code>DESTADDR</code>为空白，默认发送给交易发起者:</p>
        <p><code>SWAP:ASSET::LIM</code>
          <br />
        </p>
        <p>设定交易上限，如果系统预估超过上限，自动退回。
          10000000等于1个单位的货币.</p>
        <p></p>
        <p>如果省略<code>LIM</code>，没有交易滑点保护</p>
        <p> <code>SWAP:ASSET:DESTADDR</code>
        </p>
        <p></p>
        <p>如果以上两个都为空白：</p>
        <p> <code>SWAP:ASSET</code>
        </p>
      </td>
      <td style="text-align:left">和指定货币进行交换</td>
    </tr>
    <tr>
      <td style="text-align:left">ADD Assets（捐款）</td>
      <td style="text-align:left">
        <p><code>RUNE 或者/和 Token</code>
        </p>
        <p>Can be either, or just one side.</p>
      </td>
      <td style="text-align:left"><code>ADD:ASSET</code>
      </td>
      <td style="text-align:left">向资金池免费注入资金</td>
    </tr>
  </tbody>
</table>

### 退款

以下条件构成自动退款:

| 情况 | 重点 |
| :--- | :--- |
| 错误的 `MEMO` | 如果`MEMO` 格式不对，自动退款 |
| 错误的资产 | 如果交易的资产和实际不符， \(比如在BUSD池添加BNB\) 自动退款 |
| 错误的交易类别 | 如果在一个交换交易中携带了两种货币，自动退款 |
| 超过上限 | 如果用户在交易中明确了上限并且系统预估数量小于上限，自动退款  |

为了防止DDOS， 退款从退款金额中自动扣除（1 RUNE）。

| Asset | Amount |
| :--- | :--- |
| RUNE | 1 RUNE |
| Non-RUNE Asset | 1 RUNE equivalent |

### 额外的MEMO格式

用户可以使用以下格式：

| Transaction | Long-form MEMO | Short-form MEMO | Symbol MEMO |
| :--- | :--- | :--- | :--- |
| STAKE | `STAKE` | `st` | `+` |
| WITHDRAW | `WITHDRAW` | `wd` | `-` |
| SWAP | `SWAP` | `s` | `=` |
| ADD | `ADD` | `a` | `%` |

### 货币

以下为闪链系统使用的货币注释：

![](https://docs.google.com/drawings/u/1/d/skidhZPIsMKQ-XWJWb3EJaQ/image?w=698&h=276&rev=23&ac=1&parent=1ZoJQKvyATQekFbWMk_rqX96K9BSmCArh9e-A_g66wDQ)

#### 例子 

| 货币 | 注释 |
| :--- | :--- |
| Bitcoin | BTC.BTC |
| Ethereum | ETH.ETH |
| USDT | ETH.USDT-0xdac17f958d2ee523a2206206994597c13d831ec7 |
| BNB | BNB.BNB |
| RUNE \(BEP2\) | BNB.RUNE-B1A |
| RUNE \(NATIVE\) | THOR.RUNE |
