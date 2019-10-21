## 	Staking合约

staking合约在Bithumb Chain平台进行抵押XTAR获得平台上网络资源（net）、cpu资源以获得将数据传输到链上并计算，以及抵押BT获得Bithumb Chain收益，类似于银行存款。

管理资源分为：

* net：网络资源，用于区块链上的网络通信
* cpu：cpu资源，用于的Bithumb Chain平台进行计算
* mine: 用于获得DEX（去中心化交易所）的每日挖矿上限

抵押可获得收益，抵押总量的基数为：

​	`net+cpu+mine`的总量，如：抵押net:100BT,cpu:100BT,mine:100BT，则能获得收益的总量为300BT。

抵押的资源给reciver接收方，抵押获得的收益则给抵押人from

抵押收益权重：

* 抵押收益权重为抵押时每单位BT的放大倍数；权重为1则不放大；权重为2则乘2；权重为3则乘3

产品ID（表示staking抵押周期）：

* `0`表示活期：可随时提取抵押的**BT**。抵押收益权重为1
* `1`表示3个月抵押时间：必须在抵押3个月之后才能提取抵押的BT，抵押收益权重为2
* `2`表示6个月抵押时间：必须在抵押6个月之后才能提取抵押的BT，抵押收益权重为3

合约账户：XagqqFetxiDb9wbartKDrXgnqLaihXtcBn

### 可调用合约方法

1.抵押资源

* 方法名称：stake
* 描述：抵押人from为接收者抵押cpu、net和mine

* 传入一个[ContractArg](#xtar合约设计文档)合约参数

  - 1. 该参数是一个抵押参数的结构体，代表其中的转账实体，具有
       1. 抵押人from的账户
       2. 接收者reciver的账户
       3. 抵押net资源的BT额度
       4. 抵押cpu资源的BT额度
       5. 抵押用于获取dex挖矿上限的BT额度
       6. 产品Id

  * 返回值：
    * 无
  * event log：
    * stake
      * 该event log参数为
        * from
        * reciver
        * netAmount
        * cpuAmount
        * mineAmount
        * productId

  2.申请返回抵押的BT

  * 方法名称：unstake
  * 描述：抵押人申请返还抵押的BT
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数是一个返还抵押参数的结构体，代表其中的转账实体，具有
         1. 抵押人from的账户
         2. 接收者reciver的账户
         3. 抵押net资源的BT额度
         4. 抵押cpu资源的BT额度
         5. 抵押用于获取dex挖矿上限的BT额度
         6. 产品Id
  * 返回值：
    * 无
  * 触发event log：
    * unstake：
      * 该event log参数为：
        * from
        * reciver
        * netAmount
        * cpuAmount
        * mineAmount
        * productId

3.申请抵押获得的利息：

* 方法名称claim：
* 描述：抵押人向staking合约申请通过stake获得的XTAR利息
* 传入一个account参数：
  * 表示用户账户
* 返回值：
  * 无
* 触发event log：
  * refund：
    * 该event log参数为：
      * user：用户
      * amount：金额

4.unstake度过周期锁定期后提款：

* 方法名称refund：
* 描述：在抵押人unstake动作之后进行提款操作
* 传入一个account参数：
  * 表示用户账户
* 返回值：
  * 无
* 触发event log：
  * refund
    * 该event log参数为：
      * user
      * amount