



## Name Service(dApp)合约

name_service合约是管理`Bithumb Chain链上域名系统（VDNS）`的合约之一。name_service合约对链上域名进行管。

域名管理：VDNS合约只管理一级域名的所有权，多级域名的控制权完全由一级域名的所有人/合约决定。

域名划分：

* 域名根据分隔符"."数量，划分为FirstLevel（一级）,SecondLevel（二级）,MoreLevel（多级），"dapp"是一级域名，"x.dapp"是二级域名，"www.x.dapp"是三级域名，三级以及三级以上域名统称为多级域名。
* 域名根据不同的后缀，分为不同的类型：
  * 1. 12个小写字母或数字1-9组成的域名为`普通域名`，每个地址可免费注册一个；
    2. 以"dapp"为后缀的是`TypeSpecialName（特殊类型域名）`，需要按不同长度的定价向系统合约购买；
    3. 以"xtar"为后缀的是`TypeOfficialName（官方域名）`，由基金会分配;
    4. 以 "com", "cn", "app", "x"为后缀的目前是`TypeReservedName（保留域名）`，目前无实际用途，仅做保留；

域名阶梯竞拍：

* 每24小时仅能成交一个报价，且最高报价标的必须报价超过24小时才具备成交条件
* 当报价标的价格，从高到低排名超过100名，可随时撤销竞拍，撤销需要收取1%的手续费（具体功能将在第二期实现）
* 每次报价都必须达到或超过当前域名最高报价的10%
* 基金会可以开启或者暂停竞拍
  * 如果竞拍中途暂停，那么开启后必须至少24小时才能开放成交
  * 竞拍暂停期间，已经成交的域名不受影响，但无法报价并成交新的域名

域名状态：

* 0：未被注册的
* 1：可转让的
* 2：不能转让的
* 3：受限制的
* 4：已拉黑的

### dapp合约

"dapp"作为后缀的二级域名即为`DappName(dapp类域名)`，主要是为了方便开发Dapp的用户快速注册域名，降低域名使用门槛。`DappName`根据域名长度不同，价格不同，相同长度的域名只需要支付一定数量token，即可购买并自动注册，先到先得,不可转让。

合约功能：购买.dapp域名。

合约地址：XagqqFetxiDb9wbartKDrXgnqLaiBN6f2f

### 可调用合约方法

1、**查询域名对应的地址buyDappName**

* 方法名称：buyDappName

* 描述：购买`DappName`类域名，是一个跨合约调用的方法，将调用`name_service`合约中的registerName方法及相关事件

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)结构体参数和支付者账户

    - 1. 该参数是一个结构体，具有2个成员：
         1. 域名名称
         2. 接收者账户
      2. 该参数为支付者账户为account类型

```json
      "inputs": [
        {
          "name": "registerNameParam",
          "type": "struct",
          "components": [
            {
              "name": "xnsName",
              "type": "string"
            },
            {
              "name": "account",
              "type": "account"
            }
          ]
        },
        {
          "name": "xnsNamePayer",
          "type": "account"
        }
      ]
```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 无

* 触发event：
  * [name_service合约的registerName事件](# name_service-registerName)
  * BT token合约的transfer事件

### 可触发合约事件event

* 无

