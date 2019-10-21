



## Global Params合约

global_params合约是Bithumb Chain发行管理链上全局参数的合约。合约对链上的全局参数进行的管理：转移管理员transferAdmin、接受成为新的管理员acceptAdmin、查询管理员getAdmin、设置全局参数setParams、应用全局参数applyParams、获取全局参数getParams、设置操作人setOperator、查询操作人getOperator的功能。

全局参数包括：

* 1. freeFlg：资源使用是否免费，bool类型
  2. validatorNum：验证节点的数量
  3. proposerNum：提案节点数量
  4. assetLockTime：资产锁定时间
  5. ...

合约地址：XagqqFetxiDb9wbartKDrXgnqLahmbZRXT

### 可调用合约方法

1、**转移管理员transferAdmin**

* 方法名称：transferAdmin

* 描述：转移系统的管理员

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    * 表示新的管理员地址

```json
{
  "name":"newAdmin",
  "type":"account"
}
```

* 返回值及其意义：
  * 无
* 触发event：
  * transferAdmin

2、**接受成为新的管理员acceptAdmin**

* 方法名称：acceptAdmin
* 描述：接收成为新的管理员
* 传入参数及意义：
  * 无
* 预期：交易需要新的管理员进行签名，否则将验证失败

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
  * acceptAdmin

3、**查询管理员getAdmin**

* 方法名称：getAdmin
* 描述：查询系统管理员
* 传入参数及意义：
  - 无

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.表示管理员地址

```json
{
  "name":"admin",
  "type":"account"
}
```

* 触发event：
  * 无

4、**设置全局参数setParams**

* 方法名称：setParams
* 描述：设置全局参数
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数为一个数组
         1. 成员是个结构体，表示一个全局参数对象

```json
{
  "name":"params",
  "type":"array",
  "components":[
    {
      "name":"param",
      "type":"struct",
      "components":[
        {
          "name":"key",
          "type":"string"
        },
        {
          "name":"value",
          "type":"string"
        }
      ]
    }
  ]
}
```
* 触发event：
  * setParams

5、**应用全局参数applyParams**

* 方法名称：applyParams
* 描述：是的设置的全局参数生效
* 传入参数及其意义：
  * 无
* 返回值及其意义：
  * 无
* 触发event：
  * applyParams

6、**获取全局参数getParams**

* 方法名称：getParams

* 描述：查询全局参数

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    * 1. 该参数为一个数组

      * 1. 成员是一个全局参数对象的名称

```json
{
  "name":"keyList",
  "type":"array",
  "components":[
    {
      "name":"key",
      "type":"string"
    }
  ]
}
```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 该参数为一个数组
         1. 成员表示一个全局参数对象的值

```json
{
  "name":"values",
  "type":"array",
  "components":[
    {
      "name":"value",
      "type":"string"
    }
  ]
}
```

7、**获取所有全局参数getAllParams**

- 方法名称：getParams
- 描述：查询全局参数
- 传入参数及其意义：
  - 无
- 返回值及其意义：
  - 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数为一个数组
         1. 数组成员为一个结构体，具有两个成员：
            1. key：string类型
            2. value：string类型

```json
        {
          "name": "params",
          "type": "array",
          "components": [
            {
              "name": "param",
              "type": "struct",
              "components": [
                {
                  "name": "key",
                  "type": "string"
                },
                {
                  "name": "value",
                  "type": "string"
                }
              ]
            }
          ]
        }
```

8、**设置操作人setOperator**

* 方法名称：setOperator
* 描述：设置操作人operator，operator可以修改全局参数
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 表示新的操作人的地址

```json
{
  "name":"newOperator",
  "type":"account"
}
```

* 返回值及其意义：
  * 无

* 触发event：
  * setOperator

9、**查询操作人getOperator**

* 方法名称：setOperator
* 传入参数及其意义：
  * 无（
* 返回值及其意义
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1.代表操作人地址

```json
{
  "name":"operator",
  "type":"account"
}
```

* 触发event：
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





