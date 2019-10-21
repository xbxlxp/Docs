



## DEX合约

Dex合约是在Bithumb Chain公链平台上进行的去中心化交易所的合约。

合约地址：XagqqFetxiDb9wbartKDrXgnqLahYeM9Eg

### 可调用合约方法

1、**查询在dex中的余额**

* 方法名称：balanceOf

* 描述：查询Bithumb Chain上去中心化交易所中余额

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数，该参数具有两个参数

    * 用户账户
    * 资产账户

```json
        {
          "name": "balanceArg",
          "type": "struct",
          "components": [
            {
              "name": "user",
              "type": "account"
            },
            {
              "name": "asset",
              "type": "account"
            }
          ]
        }
```

* 返回值及其意义：
  * 返回一个uint64表示余额：

```json
        {
          "name":"balance",
          "type":"uint64"
        }
```

* 触发event：
  * 无

2、**往dex充值**

* 方法名称：deposit
* 描述：将链上资产充值到dex
* 传入参数及意义：
  * 传入一个结构体，包含4个参数：
    * 发行资产的账户
    * 资产流出账户
    * dex中的账户
    * 充值大小

```json
        {
          "name": "depositArg",
          "type": "struct",
          "components": [
            {
              "name": "asset",
              "type": "account"
            },
            {
              "name": "from",
              "type": "account"
            },
            {
              "name": "to",
              "type": "account"
            },
            {
              "name": "value",
              "type": "uint64"
            }
          ]
        }
```

* 返回值及其意义：
  * 无
* 触发event：
  * transfer

3、**从dex提现**

* 方法名称：withdraw
* 描述：将token从dex提现到账户
* 传入参数及意义：
  
- 
  - - 传入一个结构体，包含4个参数：
      - 发行资产的账户
      - 资产流出账户
      - dex中的账户
      - 提现大小
  
  ```json
          {
            "name": "withdrawArg",
            "type": "struct",
            "components": [
              {
                "name": "asset",
                "type": "account"
              },
              {
                "name": "from",
                "type": "account"
              },
              {
                "name": "to",
                "type": "account"
              },
              {
                "name": "value",
                "type": "uint64"
              }
            ]
          }
  ```
  
* 返回值及其意义：
  * 无
    * 1.表示管理员地址

* 触发event：
  * transfer

4、**代理提现**

* 方法名称：delegateWithdraw
* 描述：通过代理人`relay`代理提现
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数,为一个结构体

```json
    {
      "name": "delegateWithdraw",
      "inputs": [
        {
          "name": "dWithdrawArg",
          "type": "struct",
          "components": [
            {
              "name": "asset",
              "type": "account"
            },
            {
              "name": "from",
              "type": "account"
            },
            {
              "name": "to",
              "type": "account"
            },
            {
              "name": "amount",
              "type": "uint64"
            },
            {
              "name": "fee",
              "type": "uint64"
            },
            {
              "name": "salt",
              "type": "uint64"
            },
            {
              "name": "extra",
              "type": "string"
            },
            {
              "name":"sig",
              "type":"struct",
              "components":[
                {
                  "name":"public_keys",
                  "type":"array",
                  "components":[
                    {
                      "name":"public_key",
                      "type":"publickey"
                    }
                  ]
                },
                {
                  "name":"m",
                  "type":"uint8"
                },
                {
                  "name":"sig_data",
                  "type":"array",
                  "components":[
                    {
                      "name":"sig_data",
                      "type":"bytes"
                    }
                  ]
                }
              ]
            },
            {
              "name": "relay",
              "type": "account"
            }
          ]
        }
```
* 返回值：
  * 无
* 触发event：
  * Transfer

5、**取消订单**

* 方法名称：cancel
* 描述：取消交易订单
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数,为一个结构体
    * 表示下定单者
    * 订单id

```json
{
  "name": "cancelArg",
  "type": "struct",
  "components": [
    {
      "name": "from",
      "type": "account"
    },
    {
      "name": "id",
      "type": "string"
    }
  ]
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * cancelOrder

6、**设置代理人**

* 方法名称：setRelay

* 描述：查询全局参数

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数，为一个结构体


```json
        {
          "name": "setterArgs",
          "type": "struct",
          "components": [
            {
              "name": "from",
              "type": "account"
            },
            {
              "name": "target",
              "type": "account"
            },
            {
              "name": "value",
              "type": "bool"
            }
          ]
        }
```

* 返回值及其意义：
  * 无
* 触发event：
  * setRelay

7、**查询所有代理人**

- 方法名称：relays
- 描述：查询所有代理人
- 传入参数及其意义：
  - 无
- 返回值及其意义：
  - 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数为一个数组
         1. 数组成员为string,表示relay

```json
        {
          "name": "relays",
          "type": "array",
          "components": [
            {
              "name": "relay",
              "type": "string"
            }
          ]
        }
```

8、**查询订单状态**

* 方法名称：orderState
* 描述：查询订单状态
* 传入参数及意义：
  * 传入string表示订单id

* 返回值及其意义：
  * 一个结构体，包含三个成员：
  * 用户
    * 已成交量
    * 是否已经取消

```json
{
  "name": "orderState",
  "type": "struct",
  "components": [
    {
      "name": "user",
      "type": "account"
    },
    {
      "name": "filled",
      "type": "uint64"
    },
    {
      "name": "canceled",
      "type": "bool"
    }
  ]
}
```

* 触发event：
  * 无

9、**撮合交易**

* 方法名称：trade
* 传入参数及其意义：
  * 传入一个结构体，具有三个结构体：
    * maker下单信息
    * taker下单信息
    * relay代理人信息

```json
        {
          "name": "tradeArg",
          "type": "struct",
          "components": [
            {
              "name": "maker",
              "type": "struct",
              "components": [
                {
                  "name": "version",
                  "type": "uint32"
                },
                {
                  "name": "user",
                  "type": "account"
                },
                {
                  "name": "pair",
                  "type": "string"
                },
                {
                  "name": "side",
                  "type": "string"
                },
                {
                  "name": "price",
                  "type": "string"
                },
                {
                  "name": "amount",
                  "type": "string"
                },
                {
                  "name": "channel",
                  "type": "account"
                },
                {
                  "name": "fee",
                  "type": "uint64"
                },
                {
                  "name": "expire",
                  "type": "uint32"
                },
                {
                  "name": "salt",
                  "type": "uint64"
                },
                {
                  "name":"sig",
                  "type":"struct",
                  "components":[
                    {
                      "name":"public_keys",
                      "type":"array",
                      "components":[
                        {
                          "name":"public_key",
                          "type":"publickey"
                        }
                      ]
                    },
                    {
                      "name":"m",
                      "type":"uint8"
                    },
                    {
                      "name":"sig_data",
                      "type":"array",
                      "components":[
                        {
                          "name":"sig_data",
                          "type":"bytes"
                        }
                      ]
                    }
                  ]
                }
              ]
            },
            {
              "name": "taker",
              "type": "struct",
              "components": [
                {
                  "name": "version",
                  "type": "uint32"
                },
                {
                  "name": "user",
                  "type": "account"
                },
                {
                  "name": "pair",
                  "type": "string"
                },
                {
                  "name": "side",
                  "type": "string"
                },
                {
                  "name": "price",
                  "type": "string"
                },
                {
                  "name": "amount",
                  "type": "string"
                },
                {
                  "name": "channel",
                  "type": "account"
                },
                {
                  "name": "fee",
                  "type": "uint64"
                },
                {
                  "name": "expire",
                  "type": "uint32"
                },
                {
                  "name": "salt",
                  "type": "uint64"
                },
                {
                  "name":"sig",
                  "type":"struct",
                  "components":[
                    {
                      "name":"public_keys",
                      "type":"array",
                      "components":[
                        {
                          "name":"public_key",
                          "type":"publickey"
                        }
                      ]
                    },
                    {
                      "name":"m",
                      "type":"uint8"
                    },
                    {
                      "name":"sig_data",
                      "type":"array",
                      "components":[
                        {
                          "name":"sig_data",
                          "type":"bytes"
                        }
                      ]
                    }
                  ]
                }
              ]
            },
            {
              "name": "relay",
              "type": "struct",
              "components": [
                {
                  "name": "from",
                  "type": "account"
                },
                {
                  "name": "trade_amount",
                  "type": "string"
                },
                {
                  "name": "maker_fee",
                  "type": "string"
                },
                {
                  "name": "taker_fee",
                  "type": "string"
                }
              ]
            }
          ]
        }
```

* 返回值及其意义
  * 无

* 触发event：
  - trade

10、**查询交易挖矿奖励**

* 方法名称：bounus
* 输入参数：
  * 查询的账户

```json
        {
          "name": "account",
          "type": "account"
        }
```

* 返回值：
  * 一个结构体,含有2成员
    * 锁定的奖励
    * 解锁的奖励

```json
        {
          "name": "userBonus",
          "type": "struct",
          "components": [
            {
              "name": "lockedBonus",
              "type": "uint64"
            },
            {
              "name": "unlockedBonus",
              "type": "uint64"
            }
          ]
        }
```

11、**申请奖励解锁**

* 方法名称：claimBonus
* 输入参数：
  - 请求的账户
* 返回值：
  - 一个uint64,表示此次解锁的额度

12、**设置可挖矿的用户及对应资产**

- 方法名称：setMineable
- 输入参数：
  - 一个结构体，含有三个参数：

```json
        {
          "name": "setterArgs",
          "type": "struct",
          "components": [
            {
              "name": "from",
              "type": "account"
            },
            {
              "name": "target",
              "type": "account"
            },
            {
              "name": "value",
              "type": "bool"
            }
          ]
        }
```

- 返回值：
  - 无

13、**查询可挖矿的资产列表**

- 方法名称：mineable
- 输入参数：
  - 无
- 返回值：
  - 一个列表
    - 成员为string
    - 表示可挖矿的资产列表

14、**清除之前的挖矿奖励**

- 方法名称：updateBonus
- 输入参数：
  - 被清除的剩余挖矿奖励的账户

```json
        {
          "name": "account",
          "type": "account"
        }
```

- 返回值：
- 无

### 可触发合约事件event

1.**请求转换管理员transferAdmin事件**

* event名称：transferAdmin

* 描述：请求转换管理员的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.transferAdmin
  - 第二个参数开始代表token注册后相关实体:
    - 2.旧的管理员地址
    - 3.新的管理员地址

```json
{
  "name":"oldAdmin",
  "type":"account"
},
{
  "name":"newAdmin",
  "type":"account"
}
```

2.**接受成为新管理员acceptAdmin事件**

* event名称：acceptAdmin
* 描述：接受成为新管理员的收据
* event_logs参数及其意义：
  * 第一个参数为event名称
    * 1.acceptAdmin
  * 第二个参数开始代表token发行的相关参数：
    * 2.旧管理员地址
    * 3.新管理员地址

```json
{
  "name":"oldAdmin",
  "type":"account"
},
{
  "name":"newAdmin",
  "type":"account"
}
```

3.**设置全局参数setParams事件**

* event名称：setParams
* 描述：设置全局参数时返回的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.setParams
  - 第二个参数开始代表token发行的相关参数：
    - 2.设置的全局参数的字符串表示

```json
{
  "name":"newParams",
  "type":"string"
}
```

4.**使得全局参数生效applyParams事件**

* event名称：applyParams
* 描述：使得合约中方法setParams设置的全局参数生效
* event_logs及其参数：
  * 第一个参数为event名称
    - 1.applyParams
  * 第二个参数开始代表:
    - 2.所有全局参数及其值的字符串表示

```json
{
  "name":"params",
  "type":"string"
}
```

5.**设置操作人setOperator事件**

* event名称：setOperator
* 描述：设置操作人
* event_logs及其参数：
  - 第一个参数为event名称
    - 1.setOperator
  - 第二个参数开始代表:
    - 2.新操作人地址

```json
{
  "name":"newOperator",
  "type":"account"
}
```





