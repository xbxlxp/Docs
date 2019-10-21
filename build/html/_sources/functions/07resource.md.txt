## Resource合约

resource合约用于查询Bithumb Chain平台的通过抵押（staking合约）获得的net、cpu、mine（挖矿）以及vote(投票)信息的合约。

管理的资源分为：

* net：网络资源，用于网络通信
* cpu：cpu资源，用于在XTAR平台进行计算

resource_manager合约功能：抵押资源、释放抵押资源、释放抵押资源的BT token、查询Cpu资源、查询Net资源、查询抵押信息的功能。

合约地址：XagqqFetxiDb9wbartKDrXgnqLahuf6AQq

### 可调用合约方法

1、**查看net**

* 方法名称：getNet
* 描述：为某一账户net网络资源的状况
* 传入参数及其意义：
  
- 传入一个account类型参数，表示被查询的用户
  
* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
  - 1. 返回信息是一个结构体，代表该地址持有抵押的net信息：
         1. net的总量
         2. 已经使用的net的数量
         3. 剩余可使用的net总量
  
* 触发event：
  
  * 无

4、**查询Cpu资源**

* 方法名称：getCpu
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
    - 1. 返回信息是一个结构体，代表该地址持有抵押的cpu信息：
         2. 持有的cpu总量
         3. 已经使用的cpu总量
         4. 剩余可使用的cpu总量

```json
{
  "name":"cpuInfo",
  "type":"struct",
  "components": [
    {
      "name": "totalCpu",
      "type": "uint64"
    },
    {
      "name": "cpuUsage",
      "type": "uint64"
    },
    {
      "name": "usableCpu",
      "type": "uint64"
    }
  ]
}
```

* 触发event:
  * 无

5、**查询Mine挖矿资源**

- 方法名称：getMineInfo
- 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数表示被查询的账户

```json
{
  "name": "user",
  "type": "account"
}
```

- 返回值及其意义：
  - 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 返回信息是一个数组
         1. 第一个参数表示该用户的挖矿权重
         2. 表示总的挖矿权重

```json
    {
      "name":"getMineInfo",
      "inputs":[
        {
          "name": "user",
          "type": "account"
        }
      ],
      "outputs":[
        {
          "name": "mineInfo",
          "type": "array",
          "components": [
            {
              "name": "stake",
              "type": "uint64"
            }
          ]
        }
      ]
    }
```

- 触发event:
  - 无

6、**查询投票权重**

- 方法名称：getVoteWeight
- 传入参数及其意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数表示被查询的账户

```json
{
  "name": "user",
  "type": "account"
}
```

- 返回值及其意义：
  - 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 返回信息是一个uint64，表示投票权重
         1. 第一个参数表示该用户的挖矿权重
         2. 表示总的挖矿权重

```json
    {
      "name":"getMineInfo",
      "inputs":[
        {
          "name": "user",
          "type": "account"
        }
      ],
      "outputs":[
        {
          "name": "mineInfo",
          "type": "array",
          "components": [
            {
              "name": "stake",
              "type": "uint64"
            }
          ]
        }
      ]
    }
```

- 触发event:
  - 无

