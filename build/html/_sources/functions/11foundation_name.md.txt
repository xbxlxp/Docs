



## Name Service(Foundation)合约

name_service合约是管理`Bithumb Chain链上域名系统（XNS）`的合约之一。name_service合约对链上域名进行管。

域名管理：VDNS合约只管理一级域名的所有权，多级域名的控制权完全由一级域名的所有人/合约决定。

域名划分：

* 域名根据分隔符"."数量，划分为FirstLevel（一级）,SecondLevel（二级）,MoreLevel（多级），"dapp"是一级域名，"x.dapp"是二级域名，"www.x.dapp"是三级域名，三级以及三级以上域名统称为多级域名。
* 域名根据不同的后缀，分为不同的类型：
  * 1. 12个小写字母或数字1-9组成的域名为`普通域名`，每个地址可免费注册一个；
    2. 以"dapp"为后缀的是`TypeSpecialName（特殊类型域名）`，需要按不同长度的定价向系统合约购买；
    3. 以"Bithumbchain"为后缀的是`TypeOfficialName（官方域名）`，由基金会分配;
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

### Foundation合约

以"xtar"为后缀的foundation类型域名，由基金会（foundation）分配。

合约功能：基金会管理员为用户注册xtar域名。

合约账户：XagqqFetxiDb9wbartKDrXgnqLaiPAS8zU

### 可调用合约方法

1、**基金会管理员为用户注册xtar域名**

* 方法名称：registerXtarName

* 描述：注册TypeOfficialName`类域名，是一个跨合约调用的方法，只能由基金会管理员进行注册，将调用`name_service`合约中的registerName方法并触发相关事件

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    - 1. 该参数是一个结构体，具有四个成员：
         1. 域名
         2. 接收者账户

```json
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
}
```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 无

* 触发event：
  * [name_service合约的registerName事件](# name_service-registerName)

### 可触发合约事件event

* 无

