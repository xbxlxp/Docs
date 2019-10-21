## RAM合约

ram合约是管理Bithumb Chain平台的`ram(内存)`的合约。

ram的功能是用于存储状态数据

ram合约功能：购买一定字节数的ram、购买一定xtar token的ram、卖出ram、查询某地址持有的ram信息的功能。

合约地址：XagqqFetxiDb9wbartKDrXgnqLai8awY64

### 可调用合约方法

1、**购买一定字节数的ram**

* 方法名称：buyRamBytes
* 描述：为某一账户用BT token抵押cpu和net资源
* 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数是一个抵押资源的结构体，代表其中的购买ram结构，具有三个成员：
         1. 购买者
         2. 接收者
         3. 购买ram的数量

```json
{
  "name":"buyParam",
  "type":"struct",
  "components":[
    {
      "name":"buyer",
      "type":"account"
    },
    {
      "name":"receiver",
      "type":"account"
    },
    {
      "name":"ramAmount",
      "type":"uint64"
    }
  ]
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * [bithumb-transfer](#bithumb-transfer)
  * buyRam

2、**购买一定BT token的ram**

* 方法名称：buyRam
* 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数是一个抵押资源的结构体，具有三个成员：
         1. 购买者的地址
         2. 接收者的地址
         3. 付出的xtar token的额度

```json
{
  "name":"buyParam",
  "type":"struct",
  "components":[
    {
      "name":"buyer",
      "type":"account"
    },
    {
      "name":"receiver",
      "type":"account"
    },
    {
      "name":"tokenAmount",
      "type":"uint64"
    }
  ]
}
```

* 返回值及其意义
  * 无
* 触发event：
  * [xtar-transfer](#xtar-transfer)
  * buyRam

3、**卖出ram**

* 方法名称：sellRam
* 描述：卖出ram
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数是一个抵押资源的结构体，具有两个成员：
         1. 卖出ram的地址
         2. 卖出ram数量

```json
        {
          "name":"sellParam",
          "type":"struct",
          "components":[
            {
              "name":"user",
              "type":"account"
            },
            {
              "name":"ramAmount",
              "type":"uint64"
            }
          ]
        }
```

* 返回值及其意义：
  * 无

* 触发event：
  * [bithumb-transfer](#bithumb-transfer)
  * sellRam

4、**查询某地址持有的ram**

* 方法名称：getRam
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 该参数表示被查询的地址

```json
{
  "name": "user",
  "type": "account"
}
```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 返回信息是一个结构体，代表该地址持有ram的信息：
         3. 已经使用的cpu总量
         4. 剩余可使用的cpu总量

```json

        {
          "name":"ramInfo",
          "type":"struct",
          "components":[
            {
              "name":"totalUsed",
              "type":"uint64"
            },
            {
              "name":"totalReserved",
              "type":"uint64"
            }
          ]
        }
```

* 触发event:
  * 无

### 可触发合约事件event

1.**购买ram事件buyRam**

* event名称: buyRam

* 描述：记录购买ram时产生的收据

* event_logs参数及其意义：

  * 第一个参数为event名称

    * 1.buyRam

  * 第二个参数开始代表其中的转账实体参数:

    * 2.购买者地址
    * 3.接收者地址
    * 4.购买ram的xtar token的数量
    * 5.购买ram的手续费0.5%
    * 6.购买所得的ram数量

```json
{
  "name":"buyer",
  "type":"address"
},
{
  "name":"receiver",
  "type":"address"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"fee",
  "type":"uint64"
},
{
  "name":"ram",
  "type":"uint64"
}
```

2.**卖出ram事件sellRam**

* event名称: sellRam

* 描述：记录释放抵押资源的收据

* event_logs参数及其意义：

  * 第一个参数为event名称

    - 1.sellRam

  * 第二个参数开始代表其中的授权提现实体参数:

    - 2.ram卖出者地址
    - 3.卖出ram所得的xtar token的数量
    - 4.手续费0.5%
    - 卖出ram的数量
```json
{
  "name":"user",
  "type":"address"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"fee",
  "type":"uint64"
},
{
  "name":"ram",
  "type":"uint64"
}
```
