# HTLC-\(CN\)

注意：本文档仅适用于BitShares 的公共**测试网**。 HTLC 尚未在**主网**上进行实现。

注意：BitShares 的 HTLC 实现目前仍是测试版软件，仅用于实验目的。软件可能无法按造设计意图执行，您有可能会因此损失资产。

注意：BitShares UI 网页钱包尚未实现 HTLC 功能。因此，必须使用 BitShares CLI（命令行钱包）直接与 HTLC 进行交互。 UI 团队正在与 Core 团队合作，以在主网发布之前实现所需的图形用户界面。

## 介绍

HTLC 是一种**需要满足特定条件**才能从“存款人”转移到“收款人”的**转账**，其中两个独立的条件都将影响转账结果。 1）**哈希锁**要求区块链提供适当的“preimage”（原像），这需要发生在2）**时间锁**到期之前，否则该转账额将自动返回“存款人”。

这使得双方能够在无信任但是安全的情况下，在互相独立的两个平台上交换代币，从而实现跨链原子交换（ACCS）以及其他有用的功能。

## HTLC 入门

使用 HTLC 时，需要完成5个步骤：

1. 生成“原像”（存款人） 
2. 计算“原像” 哈希值（存款人） 
3. 创建 HTLC（存款人） 
4. 验证 HTLC（收款人） 
5. 兑换 HTLC（收款人）

### 0. 前提条件

1. “存款人”必须拥有一个已解锁的`cli_wallet`及账户（以 `alice` 为例）
   * `alice` 的钱包里必须有 TEST 代币的余额
   * 任何人都可以发出此命令来验证 `alice` 的余额：`$> list_account_balances alice`
2. “收款人”必须拥有一个已解锁的`cli_wallet`及其帐户（例如 `bob`）
   * `bob` 可以有 0 余额

**注意：出于测试目的，两个帐户可以位于同一个钱包中。**

### 1. 生成“原像”（存款人）

1. `alice` 需要生成一个“原像”（类似密码）并一直向 `bob` 保密到需要揭开它的时候。 
2. 使用 [这个外部网站](https://passwordsgenerator.net/sha256-hash-generator/) 生成“原像”
3. 在文本框中输入“原像”文本 

   ```text
   警告：使用常见的原像会带来很大的安全风险。部分收款人可以通过将原像的哈希值与常见的哈希字典进行比较从而兑换 HTLC。最好选择一个长且随机的原像。
   ```

4. 数一下您输入的字符数，包括空格，这是您的“原像长度”（以字节为单位） 
5. 保留“原像”和“原像长度”供以后使用 
6. 示例：例如字符串“preimage” 是我的原像，其原像长度为 `11` 个字节

### 2. 计算“原像哈希值”（存款人）

1. `alice` 必须从生成的**原像**中计算出“原像哈希值”
2. 文本框正下方是**原像**字符串的 SHA256 哈希值
3. 将其保留为“原像哈希值”供以后使用
4. 示例：“原像哈希值” 为`650BFCEF53BD8E6E030613E0B75EC0CBA4FCD25C53BF0D15A6283593B269DF79`

### 3. 创建 HTLC（存款人）

1. `alice` 将使用 `htlc_create` 命令创建 HTLC，语法为 `htlc_create <from> <to> <amount> <symbol> <hash_algo> <preimage_hash> <preimage_length> <expiration> <broadcast>`
   * a. 是 `alice`，我们的存款人
   * b. 是 `bob`，我们的收款人
   * c. 是锁定在合约中的代币数量（比如 `100`）
   * d. 是代币的标志（比如 `TEST`）
   * e. `<hash_algo>` 将是 SHA256 的哈希值（未来版本可能支持其他哈希算法）
   * f. `<preimage_hash>` 是来自 ＃2 的计算值（不是仍在保密中的原像）
   * g. `<preimage_length>` 是 ＃1 的字节数
   * h. 是时间锁，表示 HTLC 在向区块链广播时保持有效的秒数（例如 `300` 表示为 5 分钟）
   * i. 是一个布尔值，必须设置为 `true`，`cli_wallet` 才能正常广播命令
2. `alice` 将执行以下命令：`$（unlocked）> htlc_create alice bob 100 TEST SHA256“650BFCEF53BD8E6E030613E0B75EC0CBA4FCD25C53BF0D15A6283593B269DF79”11 300 true`
3. 操作正常的情况下，系统将返回 JSON 响应（否则会出现断言错误 - 请检查参数）
4. HTLC 现在处于激活状态，同时哈希锁正在进行倒数，`bob` 需要在哈希锁过期之前完成验证和兑换。
5. 100个 TEST 代币被从 `alice` 的账户余额中扣除，并被锁定到HTLC，同时受到哈希锁和时间锁条件的保护。

### 4. 验证 HTLC（收款人）

1. `bob` 必须验证 `alice` 创建的 HTLC 是否符合他的要求。他可以通过查看其帐户的**最近帐户历史操作**来查找激活状态的 HTLC。
2. `bob` 将执行以下命令：`$（unlocked）> get_account_history bob 1`
   * a. 这将返回最近的操作，该操作应以 “Create HTLC to bob” 开头 
   * b. 找到格式为 1.16.XX 的 HTLC  "database\_id"（数据库 ID）
     * “XX” 是 `alice` 创建的 HTLC 中的一个唯一数值 
     * 保留完整的 HTLC “database\_id”（例如 `1.16.99`） 
3. `bob` 将使用 “database\_id” \(数据库号\) 执行此命令：`$（unlocked）> get_htlc 1.16.99`
4. `bob` 将核实以下值是否符合他的期望，和先前与 `alice` 约定的是否相符：
   * a. from（来自）
   * b. to（发给）
   * c. amount（数额）
   * d. asset（资产）
5. `bob` 将看到 `alice` 对该 HTLC 的两种条件限制：
   * 对于哈希锁： 
     * a. hash\_algo 
     * b. preimage\_hash
     * c. preimage\_length 
   * 对于时间锁：
     * d. expiration\_string 
6. 如果 `bob` 验证并确认了所有内容，他可以接下来继续兑换 HTLC 
7. 否则，`alice` 将在时间锁失效时收到锁定在 HTLC 中的资产

### 5. 兑换 HTLC（收款人）

1. `bob` 将在执行商定好的任务后获得 `alice` 的原像
2. `htlc_redeem` 命令具有以下语法：`htlc_redeem <htlc_database_id> <fee_paying_account> <preimage> <broadcast>`
3. `bob` 将执行以下命令：`$（unlocked）> htlc_redeem 1.16.99 bob “My Preimage” true`
4. 操作正常的情况下，系统将返回 JSON 响应（否则出现断言错误 - 请检查参数）
5. 由于 `bob` 提供的原像满足 HTLC 上的**哈希锁**条件，**100 TEST** 被移入他的账户余额（其中的 `0.4 TEST` 将作为交易手续费被扣除，此外还将减去 `0.8 TEST` 作为存储原像的费用）。
6. 任何人都可以发出此命令来验证 `bob` 的余额：`$> list_account_balances bob`

## 使用两个 HTLC 进行原子交换

**原子交换**中包括了一对由相同的哈希锁绑定起来的HTLC。原子交换使得在无信任情况下不同账户间不同类代币的交易成为可能。关于如何在测试网上执行此操作的更多信息将在近期放出。

## 执行原子交叉链交换（ACCS）

HTLC 实现的最终目标是允许 BitShares 区块链上的代币可以在无需信任的前提下和外部区块链上的代币进行交换。我们可能会在未来几周内开始从 TESTBTC 教程开始。如果您想要尝试的话，请您先在 BitShares 链上进行练习。

