



## Prime合约

name_service合约是管理`Bithumb Chain链上域名系统（VDNS）`的合约之一。name_service合约对链上域名进行管。

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

### Prime合约（第一版）

描述：

* "prime"作为后缀的是`primeName`,拥有primeName即成为prime用户，不可转让，最多只能注册一个prime，primeName只能是二级域名。普通Prime用户的primeName长度必须大于等于9位，即前缀必须大于2位。
* prime邀请其他人而获得的奖励称为InviteReward，每次注册一个新的Prime，其上级邀请人可以获得PrimeInviteReward奖励；上上级邀请人可以获得PrimeSecondLevelReward，这些奖励锁仓一周时间，会通过DistributeInviteReward来检测并发放。
* 每次注册Prime都会有部分奖励进入到TopPrimes奖池，TopPrime会取获得InviteReward奖励前十的Prime用户，发放最后的奖励。TopPrimes.TotalReward即为总奖池，由DistributeTopPrimeReward发放，根据用户的直接邀请人数比例来发放，直接邀请人数越多，奖励越多。
* prime启动：如果需要启动Prime活动，那么初始，只需要把'prime'，作为自己的上级邀请人即可。
* prime作为基础服务开放给所有的应用或者机构
  * 拥有二级primeName即可使用系统提供的Prime开放服务 
    * 第三方应用通过Prime开放服务，可以自由扩展自己应用的邀请奖励规则，并且设置特殊激励吸引用户；
    * Bithumb Chain的用户只要注册该二级域名为后缀的三级域名既可以成为该应用的prime会员
    * 同一个用户，可以成为多个应用或机构的prime会员
    * 默认奖励规则与XtarPrime一致

合约功能：注册`primeName`类域名registerPrime、获得目前排名top10名的邀请奖励用户getTopPrimeList、分配邀请奖励distributeInviteReward、分配top10名的奖励distributeTopPrimeReward的功能。

合约调用：`prime`合约将调用`name_service`合约

合约地址：XagqqFetxiDb9wbartKDrXgnqLaiUUKzLS

### 可调用合约方法

1、**注册`primeName`类域名registerPrime**

* 方法名称：registerPrime

* 描述：注册`primeName`类域名，需要花费10个BT

* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    - 1. 该参数是一个结构体，具有三个成员：
         1. prime域名名称
         2. 注册用户账户
         3. 邀请人的prime域名

```json
        {
          "name": "registerPrimeParam",
          "type": "struct",
          "components": [
            {
              "name": "newPrimeName",
              "type": "string"
            },
            {
              "name": "userAccount",
              "type": "account"
            },
            {
              "name": "inviterPrimeName",
              "type": "string"
            }
          ]
        }
```

* 返回值及其意义：
  * 无

* 触发event：
  * [name_service合约的registerName事件](# name_service-registerName)

2、**获得目前排名top5名的邀请奖励用户getTopPrimeList**

* 方法名称：getTopPrimeList
* 描述：*获得目前排名top10名的邀请奖励用户
* 传入参数及意义：
  * 无

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有4个成员：
      - 1. 数组，该数组的的成员：
           1. 一个结构体，该结构体有是三个成员：
              1. prime奖励信息，是一个结构体，有4个成员：
                 1. prime域名
                 2. 邀请人的地址
                 3. 邀请人的奖励
                 4. 一级邀请人数
              2. 将获得奖励的地址
              3. top5奖励奖池的总金额
           2. top5中的邀请奖励的最小值（进入top5的门槛）
           3. 总奖励
           4. 奖励可发放时间

```json
        {
          "name": "topPrimes",
          "type": "struct",
          "components": [
            {
              "name": "topPrimesList",
              "type": "array",
              "components": [
                {
                  "name": "topPrimeInfo",
                  "type": "struct",
                  "components": [
                    {
                      "name": "primeInfo",
                      "type": "struct",
                      "components": [
                        {
                          "name": "primeName",
                          "type": "string"
                        },
                        {
                          "name": "inviterAccount",
                          "type": "account"
                        },
                        {
                          "name": "inviteReward",
                          "type": "uint64"
                        },
                        {
                          "name": "directInvites",
                          "type": "uint64"
                        }
                      ]
                    },
                    {
                      "name": "account",
                      "type": "account"
                    },
                    {
                      "name": "topPrimeReward",
                      "type": "uint64"
                    }
                  ]
                }
              ]
            },
            {
              "name": "minReward",
              "type": "uint64"
            },
            {
              "name": "totalReward",
              "type": "uint64"
            },
            {
              "name": "rewardTime",
              "type": "uint32"
            }
          ]
        }
```

* 触发event：
  * 无

3、**分配邀请奖励distributeInviteReward**

* 方法名称：distributeInviteReward
* 描述：分配邀请奖励
* 传入参数及意义：
  - 无

* 返回值及其意义：
  * 无

* 触发event：
  * distributeInviteReward

4、**分配top5名的奖励distributeTopPrimeReward**

* 方法名称：distributeTopPrimeReward
* 描述：分配top10名的奖励
* 传入参数及意义：
  * 无

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有4个成员：
      - 1. 数组，该数组的的成员：
           1. 一个结构体，该结构体有是三个成员：
              1. prime奖励信息，是一个结构体，有4个成员：
                 1. prime域名
                 2. 邀请人的地址
                 3. 邀请人的奖励
                 4. 一级邀请人数
              2. 将获得奖励的地址
              3. top10奖励奖池的总金额
           2. top10中的邀请奖励的最小值（进入top10的门槛）
           3. 总奖励
           4. 奖励可发放时间

```json
        {
          "name": "topPrimes",
          "type": "struct",
          "components": [
            {
              "name": "topPrimesList",
              "type": "array",
              "components": [
                {
                  "name": "topPrimeInfo",
                  "type": "struct",
                  "components": [
                    {
                      "name": "primeInfo",
                      "type": "struct",
                      "components": [
                        {
                          "name": "primeName",
                          "type": "string"
                        },
                        {
                          "name": "inviterAccount",
                          "type": "account"
                        },
                        {
                          "name": "inviteReward",
                          "type": "uint64"
                        },
                        {
                          "name": "directInvites",
                          "type": "uint64"
                        }
                      ]
                    },
                    {
                      "name": "account",
                      "type": "account"
                    },
                    {
                      "name": "topPrimeReward",
                      "type": "uint64"
                    }
                  ]
                }
              ]
            },
            {
              "name": "minReward",
              "type": "uint64"
            },
            {
              "name": "totalReward",
              "type": "uint64"
            },
            {
              "name": "rewardTime",
              "type": "uint32"
            }
          ]
        }
```

* 触发event：
  * [xtar-transfer](#xtar-transfer)

### 可触发合约事件event

1.**注册prime域名registerPrime事件**

* event名称：registerPrime

* 描述：注册prime域名的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.registerPrime
  - 第二个参数开始代表prime域名注册相关实体:
    - 2.注册的prime域名
    - 3.绑定的用户地址
    - 4.邀请人的prime域名

```json
        {
          "name": "newPrimeName",
          "type": "string"
        },
        {
          "name": "userAccount",
          "type": "account"
        },
        {
          "name": "inviterPrimeName",
          "type": "string"
        }
```

2.**分配邀请奖励distributeInviteReward事件**

* event名称：distributeInviteReward
* 描述：设置主域名的收据
* event_logs参数及其意义：
  * 第一个参数为event名称
    * 1.distributeInviteReward
  * 第二个参数开始代表token发行的相关参数：
    * 2.奖励笔数
    * 3.第一笔奖励的索引
    * 4.最后一笔奖励的索引

```json
        {
          "name": "countRewards",
          "type": "uint64"
        },
        {
          "name": "distributeTime",
          "type": "string"
        },
        {
          "name": "invitedRewardQueueHeadIndex",
          "type": "uint64"
        },
        {
          "name": "invitedRewardQueueTailIndex",
          "type": "uint64"
        }
```

