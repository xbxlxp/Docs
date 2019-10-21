



## Token合约

token合约是Bithumb Chain发行token和管理token合约。token合约对注册发行的进行token生命周期的管理：注册token、发行token、销毁token、变更token发行人、接受成为新的token发行人、查询token发行人、查询token符号、查询token精度、查询token总发行量、token转账、查询token余额、授权token转账、授权token转账提现、查询token授权提现剩余额度的功能。

合约地址：XagqqFetxiDb9wbartKDrXgnqLahUovwfs

### 可调用合约方法

1、**注册token**

* 方法名称：register

* 描述：注册登记发行token的信息，注册的token将存入到issuer发行人的地址中

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    * 1.是一个结构体struct代表注册登记token的信息，该结构体含有9个成员：

      * 1. symbol指定`token`的象征符号。
      * 2. decimal指定 `token` 的小数位。
      * 3. totalSupply指定 `token` 的发行总量
      * 4. issuer指定 `token` 的发行人地址
      * 5. name指定`token`合约名称
      * 6. author指定 `token` 的合约所有者 

      * 7. version指定`token`合约的版本

      * 8. email指定`token`合约的联系邮箱

      * 9. description指定`token`合约的描述信息

```json
{
  "name":"tokenInfo",
  "type":"struct",
  "components":[
    {
      "name":"symbol",
      "type":"string"
    },
    {
      "name":"decimal",
      "type":"uint8"
    },
    {
      "name":"totalSupply",
      "type":"uint64"
    },
    {
      "name":"issuer",
      "type":"account"
    },
    {
      "name":"name",
      "type":"string"
    },
    {
      "name":"author",
      "type":"string"
    },
    {
      "name":"version",
      "type":"string"
    },
    {
      "name":"email",
      "type":"string"
    },
    {
      "name":"description",
      "type":"string"
    }
  ]
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * register

2、**发行token**

* 方法名称：issue
* 描述：将登记注册的token发行到指定的账户
* 传入参数及意义：
  * 传入三个[ContractArg](#xtar合约设计文档)合约参数
    * tokenAddress指定token合约地址:
    * amount指定token发行的数量
    * source指定token的发行者地址或者销毁token的地址

```json
{
  "name":"tokenAddress",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"source",
  "type":"string"
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * issue

3、**销毁token**

* 方法名称：destroy
* 描述：销毁发行账户的 `token` ，可以使用 `destroy` 命令来减少发行量
* 传入参数及意义：
  - 传入三个[ContractArg](#xtar合约设计文档)合约参数
    - tokenAddress指定token合约地址:
    - amount指定销毁token发行的数量
    - source指定token的发行者地址

```json
{
  "name":"tokenAddress",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"source",
  "type":"string"
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * destroy

4、**申请变更token发行人**

* 方法名称：transferIssuer

* 描述：变更token的发行人

* 传入参数及意义：

  - 传入两个[ContractArg](#xtar合约设计文档)合约参数
    - tokenAddress指定token合约地址:
    - 地址的新发行人的地址

  ```json
  {
    "name":"tokenAddress",
    "type":"account"
  },
  {
    "name":"newIssuer",
    "type":"account"
  }
  ```

* 返回值及其意义：
  
  * 无
* 触发event：
  
  * transferIssuer

5、**接受成为新的token发行人**

* 方法名称：acceptIssuer

* 描述：接受成为心新的token发行人

* 传入参数及意义：

  - 传入一个[ContractArg](#xtar合约设计文档)合约参数：
    - 1.表示token合约地址

  ```json
  {
    "name":"tokenAddress",
    "type":"account"
  }
  ```

* 返回值及其意义：
  
  - 无
* 触发event：
  
  - acceptIssuer

6、**查询token发行人**

* 方法名称：getIssuer
* 描述：查询token发行人

7、**查询xtar token符号**

* 方法名称：symbol

* 传入参数及意义：
  * 无（空数组）
* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.代表xtar token的符号

```json
{
    "name":"symbol",
    "type":"string"
}
```

* 触发event：
  * 无

8、**查询精度**

* 方法名称：decimal
* 传入参数及其意义：
  * 无（空数组）
* 返回值及其意义
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.代表xtar token的精度
      * 预期返回值为：8

```json
{
    "name":"decimal",
    "type":"uint8"
}
```

* 触发event：
  - 无

9、**查询总发行量**

* 方法名称：totalSupply
* 传入参数及其意义：
  * 无（空数组）
* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.代表token的总发行量

```json
{
  "name":"totalSupply",
  "type":"uint64"
}
```

* 触发event：
  - 无

10、**查询已经发行的总量**

* 方法名称：supply
* 传入参数及其意义：
  - 无（空数组）
* 返回值及其意义：
  - 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1.代表token的已经发行量的数量

11、**转账**

* 方法名称：transfer
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 该参数为一个数组，代表一笔交易的多个转账
         1. 数组成员是一个转账结构，代表其中的转账实体，具有
            1. 转出方
            2. 接收方
            3. 额度

```json
{
  "name":"transferArgs",
  "type":"array",
  "components":[
    {
      "name":"transferArg",
      "type":"struct",
      "components":[
        {
          "name":"from",
          "type":"account"
        },
        {
          "name":"to",
          "type":"account"
        },
        {
          "name":"amount",
          "type":"uint64"
        }
      ]
    }
  ]
}
```

* 返回值及其意义：
  * 无
* 触发event:
  * transfer
* 返回eve_logs及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 成员是一个转账结构的输入参数，第一个参数为event名称第二个开始代表其中的转账实体参数，具有
         1. 合约方法
         2. 转出方
         3. 接收方
         4. 额度

```json
{
  "name":"from",
  "type":"account"
},
{
  "name":"to",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
}
```

12、**查询余额**

* 方法名称：balanceOf
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.表示查询余额的地址

```json
{
  "name":"account",
  "type":"account"
}
```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.表示锁查询的地址余额
    * 说明：8位精度

```json
{
  "name":"balance",
  "type":"uint64"
}
```

* 触发event：
  * 无

13.**授权转账(开支票)**

* 方法名称：approve
* 描述：授权其他用户可从自己的账户提取一定额度的xtar token
* 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数为一个数组，代表一笔交易的多个转账
         1. 数组成员是一个转账结构，代表其中的转账实体，具有
            1. 转出方
            2. 接收方
            3. 额度

```json
{
  "name":"transferArgs",
  "type":"array",
  "components":[
    {
      "name":"transferArg",
      "type":"struct",
      "components":[
        {
          "name":"from",
          "type":"account"
        },
        {
          "name":"to",
          "type":"account"
        },
        {
          "name":"amount",
          "type":"uint64"
        }
      ]
    }
  ]
}
```

* 返回值及其意义：
  * 无

* 触发event_log:
  * approve

14.**授期转账提现（用支票）**

* 方法名称：transferFrom
* 描述：从授权提现的地址中提取xtar token
* 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数为一个数组，代表一笔交易的多个转账
         1. 数组成员是一个转账结构，代表其中的转账实体，具有
            1. 转出方
            2. 接收方
            3. 额度

```json
{
  "name":"transferArgs",
  "type":"array",
  "components":[
    {
      "name":"transferArg",
      "type":"struct",
      "components":[
        {
          "name":"from",
          "type":"account"
        },
        {
          "name":"to",
          "type":"account"
        },
        {
          "name":"amount",
          "type":"uint64"
        }
      ]
    }
  ]
}
```

- 返回值及其意义：
  - 无

- 触发event_log:
  - transfer

15.**查询授权提现剩余额度**

* 方法名称：allowance
* 描述：从授权提现的地址中查询剩余可提现的额度
* 传入参数及其意义：
  - 传入两个[ContractArg](#xtar合约设计文档)合约参数
    - 1.授权地址
    - 2.提现地址

```json
{
  "name":"from",
  "type":"account"
},
{
  "name":"to",
  "type":"account"
}
```

* 返回值及其意义：
  * 剩余可提现额度

```json
{
  "name":"balance",
  "type":"uint64"
}
```

* 触发event_log:
  * 无

16.**接收域名**

* 方法名称：acceptXnsName
* 描述：接收域名
* 传入参数及意义：
  * 传入一个参数xnsName，string类型

* 返回值及其意义：
  * 无

17.放弃域名（拉黑域名）

- 方法名称：blockXnsName
- 描述：拉黑/放弃域名
- 传入参数及意义：
  - 传入一个参数xnsName，string类型

- 返回值及其意义：
  - 无

18.设置主域名

- 方法名称：setMainXnsName
- 描述：设置主域名
- 传入参数及意义：
  - 传入一个参数mainName，string类型
- 返回值及其意义：
  - 无

19.注册域名

- 方法名称：registerXnsName
- 描述：注册域名
- 传入参数及意义：
  - 传入一个参数xnsName，string类型
- 返回值及其意义：
  - 无

### 可触发合约事件event

1.**token注册事件register**

* event名称：register

* 描述：记录token注册记录的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.register
  - 第二个参数开始代表token注册后相关实体:
    - 2.token合约地址
    - 3.token符号
    - 4.token精度
    - 5.token发行总量

```json
{
  "name":"symbol",
  "type":"string"
},
{
  "name":"decimal",
  "type":"string"
},
{
  "name":"totalSupply",
  "type":"uint64"
},
{
  "name":"tokenAddress",
  "type":"account"
}
```

2.**issue发行token事件**

* event名称：issue
* 描述：记录token发行记录的收据
* event_logs参数及其意义：
  * 第一个参数为event名称
    * 1.issue
  * 第二个参数开始代表token发行的相关参数：
    * 2.token的合约地址
    * 3.token发行人地址
    * 4.token发行数量
    * 5.token发行的接收人

```json
{
  "name":"tokenAddress",
  "type":"account"
},
{
  "name":"issuer",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"source",
  "type":"string"
}
```

3.**destroy销毁token事件**

* event名称：destroy
* 描述：销毁发行账户的 `token` ，减少发行量
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.destroy
  - 第二个参数开始代表token发行的相关参数：
    - 2.token的合约地址
    - 3.token发行人地址
    - 4.token销毁的数量
    - 5.token销毁的地址

```json
{
  "name":"tokenAddress",
  "type":"account"
},
{
  "name":"issuer",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
},
{
  "name":"source",
  "type":"string"
}
```

4.**transferIssuer更换token发行人事件**

* event名称：transferIssuer
* 描述：请求更换token发行人事件
* event_logs及其参数：
  * 第一个参数为event名称
    - 1.transferIssuer
  * 第二个参数开始代表:
    - 2.token地址
    - 4.新发行者地址

```json
{
  "name":"tokenAccount",
  "type":"account"
},
{
  "name":"newIssuer",
  "type":"account"
}
```

5.**acceptIssuer接受成为token新发行人事件**

* event名称：acceptIssuer
* 描述：接受成为token新发行人的事件
* event_logs及其参数：
  - 第一个参数为event名称
    - 1.transferIssuer
  - 第二个参数开始代表:
    - 2.token地址
    - 3.旧发行者地址
    - 4.新发行者地址

```json
{
  "name":"tokenAccount",
  "type":"account"
}
```

6.**转账事件transfer**

* event名称: transfer

* 描述：记录发生过转账记录的收据

* event_logs参数及其意义：

  * 第一个参数为event名称

    * 1.transfer

  * 第二个参数开始代表:

    * 2.转出方

    * 3.接收方

    * 4.额度

```json
{
  "name":"from",
  "type":"account"
},
{
  "name":"to",
  "type":"account"
},
{
  "name":"amount",
  "type":"uint64"
}
```

7.**授权提现事件approve**

* event名称: approve

* 描述：记录发生过的授权提现记录的收据

* event_logs参数及其意义：

  * 第一个参数为event名称

    - 1.approve

  * 第二个参数开始代表其中的授权提现实体参数:

    - 2.授权转出方

    - 3.授权接收方

    - 4.额度

```json
        {
          "name":"from",
          "type":"account"
        },
        {
          "name":"to",
          "type":"address"
        },
        {
          "name":"amount",
          "type":"uint64"
        }
```
