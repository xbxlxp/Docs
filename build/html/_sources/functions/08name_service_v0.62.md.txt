



## Name Service合约

name_service合约是管理`Bithumb Chain链上域名系统（VDNS）`的合约之一。name_service合约对链上域名进行管。

域名管理：VDNS合约只管理一级域名的所有权，多级域名的控制权由**一级域名的所有人及该多级域名的父域名所有人**共同决定。
> *所有人可以是普通用户，也可以是一本合约*

#### 域名划分：

* 域名根据分隔符"."数量，划分为FirstLevel（一级）,SecondLevel（二级）,MoreLevel（多级），"bithumbchain"是一级域名，"x.bithumbchain"是二级域名，"www.x.bithumbchain"是三级域名，三级以及三级以上域名统称为多级域名。

* 域名根据不同的后缀，分为不同的类型：
  * 1. 12位小写字母或数字1-9组成的域名为`TypeCommonName（普通域名或者免费域名）`，每个地址可免费注册一个；
    2. 以"system"为后缀的是系统域名，仅由Native Contract才有资格拥有
    3. 以"xtar"为后缀的是`TypeOfficialName（官方域名）`，由基金会合约分配;
    4. 以 "com", "cn"等为后缀的目前是`TypeReservedName（保留域名）`，目前无实际用途，仅做保留；
    > - "com", "cn", "net", "org", "io", "one", "global", "us", "co", "edu"
    > - "star", "dex", "system", "spbithumbchain", "bithumbchainsp", "dapp", "app", "prime"
    
  5. 3位到11位长度的所有合法一级域名，均为`TypeReservedName（可购买域名）`，通过购买获得
  
* 域名特殊名称
  - RootName(根域名),TopName(顶级域名)是一级域名的别称，`a.b.bithumbchain`的根域名是bithumbchain`，`bithumbchain`的根域名也是bithumbchain`
  - FatherName(父亲域名或父域名)，是多级域名的上一级域名，`a.b.bithumbchain`的父域名是`b.bithumbchain`
    - 特别强调的是，<u>一级域名的父域名是自己，例如，`bithumbchain`的父域名也是`bithumbchain`</u>
  - Signature(签名)
    - 私钥签名，与其他区块链的签名，无任何区别
    - 当Signer(签名者)是一本合约时，由于合约地址没有对应私钥，当且仅当Signer合约调用name_service合约时，在name_service合约中，Signer验签通过
  - registerSign(承诺，注册授权或异步签名)
    - 通过调用registerSign方法产生的异步签名，仅可在name_service合约中的registerName方法里，替代Signer的签名，详情见方法说明
  - Agent(代理)
    - 根域名可以设置自己的代理人，代替自己签名或者异步签名，需要注意代理适用域名范围，详情见setRootAgent方法说明

#### 域名状态：

* 0：Empty-未被注册的
* 1：Used-正常使用中的
  * 转让冻结中（域名冻结只能通过解冻时刻判断，域名状态依然属于Used，详情见下方getAddressInfo方法说明）
    * 当前时间小于<u>解冻时刻</u>
  * 未冻结
    * 当前时间大于等于<u>解冻时刻</u>
* 2：Blocked-已拉黑的

### 5.1、name_service合约

##### 合约功能：
  - 查询各类域名及相关信息
  - 购买、注册、设置主域名、转让、拉黑功能
  - 授权注册多级域名(*异步签名*)，授权代理用户(*可代替本人签名*)

合约地址：XagqqFetxiDb9wbartKDrXgnqLahx9xZhB

### 可调用合约方法

1.**查询域名对应的地址getAddressInfo**

* 方法名称：getAddressInfo
* 描述：查询域名对应的地址、域名解冻时刻(当前时间大于等同于解冻时刻，即正常状态，否则处于转让冻结状态)、域名状态
* 传入参数及其意义：

  * 传入一个[ContractArg](#xtar合约设计文档)合约参数

    * 表示域名

  ```json
  {
    "name": "xnsName",
    "type": "string"
  }
  ```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 该参数是一个结构体，具有三个成员：
         1. 域名对应的地址账户
         2. 域名的解冻时刻
         3. 域名状态

  ```json
  {
    "name": "addressInfo",
    "type": "struct",
    "components": [
      {
        "name": "account",
        "type": "account"
      },
      {
        "name": "unfreezeTime",
        "type": "uint32"
      },
      {
        "name": "status",
        "type": "uint8"
      }
    ]
  }
  ```
2.**查询顶级域名的购买价格**

* 方法名称：getXnsNamePrice
* 描述：查询某个顶级域名的购买价格
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 表示域名

  ```json
  {
    "name": "xnsName",
    "type": "string"
  }
  ```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    * 1. 该参数是一个数字，表示域名价格，精度是8位，token是xtar

  ```json
  {
    "name": "price",
    "type": "uint64"
  }
  ```

* 触发event：
  
  * 无

3.**购买域名buyXnsName**

* 方法名称：buyXnsName
* 描述：购买顶级域名，并且自动注册绑定该域名
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 该参数是一个结构体，具有2个成员：
      * 1. 顶级域名
        2. 域名绑定账户（同时也是购买者，需要校验该账户签名，账户中要有足够的钱）

  ```json
  {
    "name": "buyNameParam",
    "type": "struct",
    "components": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "buyer",
        "type": "account"
      }
    ]
  }
  ```

* 返回值及其意义：
  
  * 无
* 触发event：
  
  * buyXnsName

4.**注册域名registerName**

* 方法名称：registerName
* 描述：注册域名
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    * 该参数是一个结构体，具有2个成员：
      * 1. 域名
        2. 域名绑定账户（需要校验该账户签名）

* 备注：仅可注册免费普通的一级域名以及多级域名，注册任何多级域名需要同时获得其父亲域名、其根域名、其本人三方的同意

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
        "name": "address",
        "type": "account"
      }
    ]
  }
  ```

* 返回值及其意义：
  
  * 无
* 触发event：
  
  * registerName

5.**设置主域名setMainName**

* 方法名称：setMainName
* 描述：设置主域名
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
  - * 表示域名

  ```json
  {
    "name": "xnsName",
    "type": "string"
  }
  ```
* 备注：设置主域名需要获得域名使用者的签名

* 返回值及其意义：
  
* 无
  
* 触发event：
  
  * setMainName

6.**获取一个账户的域名信息getXnsNameInfo**

* 方法名称：getXnsNameInfo
* 描述：获取一个账户的域名信息
* 传入参数及意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 表示该账户的地址
  ```json
  {
    "name": "account",
    "type": "account"
  }
  ```

* 返回值及其意义：
  * 返回一个[ContractArg](#xtar合约设计文档)合约参数
    - 1. 该参数是一个结构体，具有3个成员：
         1. 该账户的主域名
         2. 该账户所拥有的域名数量
         3. 该账户是否已经注册过免费顶级域名

  ```json
  {
    "name": "xnsNameInfo",
    "type": "struct",
    "components": [
      {
        "name": "mainXnsName",
        "type": "string"
      },
      {
        "name": "totalNames",
        "type": "uint32"
      },
      {
        "name": "hasCommonName",
        "type": "bool"
      }
    ]
  }
  ```

* 触发event：
  
  * 无

7.**转让域名transferName**

* 方法名称：transferName
* 描述：转让域名
* 传入参数及其意义：
  * 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有2个成员：
      - 1. 域名
        2. 接收账户

```json
{
  "name": "transferNameParam",
  "type": "struct",
  "components": [
    {
      "name": "xnsName",
      "type": "string"
    },
    {
      "name": "dstAccount",
      "type": "account"
    }
  ]
}
```
* 备注：
  - 仅可以转让一级收费域名，并且该域名不能绑定在合约上；转让域名需要同时获得，域名拥有者，接受者双方签名
  - <u>转让域名之后，域名会冻结解析90天时间
  - 冻结期间，域名不能接受转账，不能再次转让，但可以继续授权子孙域名注册，设置代理人</u>

* 返回值及其意义：
  * 无
* 触发event：
  * transferName

8.**放弃域名（拉黑域名）blockName**

* 方法名称：blockName
* 描述，设置域名状态为拉黑状态（拉黑也即放弃域名状态）
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个域名，类型为字符串
  ```json
          {
            "name": "xnsName",
            "type": "string"
          }
  ```

* 备注：
  - 域名拥有者可以拉黑自己的域名；根域名拥有者可以拉黑自己的子孙域名；除此之外，均不具备拉黑权力
  - 域名一旦拉黑，永久不可逆

* 触发event：
  
  * blockName

9.**转出域名支票transferNameTo**

* 方法名称：transferNameTo
* 描述：域名拥有者授权其他用户可以提取该域名拥有者的域名
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有两个成员：
      - 1. 域名
        2. 接收方账户

  ```json
  {
    "name": "transferNameParam",
    "type": "struct",
    "components": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "dstAccount",
        "type": "account"
      }
    ]
  }
  ```
  
* 备注：
  - 转出域名支票，仅需要xnsName拥有者签名即可，**同一个域名只支持一张支票，后开的支票会覆盖前面的支票**
  - 转出域名，仅代表域名拥有者同意，并不代表一定会转让成功，最终取决于接收者提取域名时，能否通过TransferName的校验

* 触发event：
  
  * transferNameTo

10.**接收授权转让的域名transferNameAccept**

* 方法名称：transferNameAccept
* 描述：接收者提取“转出域名支票”
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有两个成员：
      - 1. 域名
        2. 接收方账户

  ```json
  {
    "name": "transferNameParam",
    "type": "struct",
    "components": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "dstAccount",
        "type": "account"
      }
    ]
  }
  ```
* 备注：
  - 仅需要接受者签名，即可提取域名支票，无需再验证域名拥有者签名，但是需要此时刻的域名满足转让域名的条件，比如域名必须满足正常无冻结状态
  - <u>转让域名之后，域名会冻结解析90天时间
  - 冻结期间，域名不能接受转账，不能再次转让，但可以继续授权子孙域名注册，设置代理人</u>


* 返回值：
  * 无
* 触发event log：
  * transferNameAccept

11.**查询是否有父级域名**

* 方法名称：hasFatherName
* 描述：查询`account`是否拥有某个域名,该域名的父域名是`fatherName`
* 说明：`xyz.abc.xtar`的父级域名为`abc.xtar`,而`xtar`不是`xyz.abc.xtar`的父级域名。父级域名必须是比该域名少一个`.`且父级域名完全包括在该域名中；有一种特殊情况，顶级域名的父域名是自身，即`xtar`的父域名是`xtar`
* 传入参数及其意义：
  * 传入两个参数：
    * 查询的账户
    * 查询的父级域名

```json
        {
          "name": "account",
          "type": "account"
        },
        {
          "name": "fatherName",
          "type": "string"
        }
```
* 备注：同一个账户地址，可以有多个域名，这些域名允许拥有相同的父域名，例如，user可以注册`1.xtar`,也可以注册`2.xtar`，即使`1.xtar`域名被拉黑，hasFatherName(user, "xtar")依然返回`true`

* 返回值：
  * 返回一个参数，类型为bool，是否具有该父级域名的域名

```json
        {
          "name": "hasFatherName",
          "type": "bool"
        }
```

* 触发合约事件：
  * 无

12.**授权注册子域名的承诺（异步签名）**

* 方法名称：registerSign
* 描述：在XTAR链上由域名持有人为接收者提供异步签名，可替代注册域名时的在线签名
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有四个成员：
      - 1. 授权receiver用户可注册的域名范围（例如参数填写"abc.xtar",表明receiver可注册的域名必须是"abc.xtar"的直接子域名）
        2. 授权方签名方账户
        3. 接收方账户
        4. 默认`false`, 如果为`true`表明<u>取消之前已授权的签名</u>

  ```json
  {
    "name": "registerSignParam",
    "type": "struct",
    "components": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "signerAccount",
        "type": "account"
      },
      {
        "name": "receiverAccount",
        "type": "account"
      },
      {
        "name": "isCancelled",
        "type": "bool"
      }
    ]
  }
  ```

* 备注：
  - 该方法提供的异步签名，只在注册子域名时有效，无法应用于转让域名等其他方法
  - 如果Signer，既是根域名拥有者，也是父域名拥有者，那么相当于两份签名同时生效
  - Signer异步签名成功后，只要不主动取消签名，即使Signer自身未来被拉黑或者转让冻结，也不影响之前的异步签名的有效性
  - Signer授权给Receiver的可注册域名范围，只能是**直接子域名**范围，不是**子孙域名**范围
    - 例如只授权了"abc.xtar"后，Receiver可以选择注册"1.abc.xtar"，不可以注册"1.1.abc.xtar"

* 返回值：
  
  * 无
* event log：
  
  * registerSign

13.**根域名设置代理人**

* 方法名称：setRootAgent
* 名称：根域名拥有者，可设置代理人代替自己，进行注册子域名的签名授权
* 说明：同一个代理域名范围下的代理人只能设置一个，如果需要取消代理人则把代理人设置为自己的账户，若选项为永久代理则不可变更代理人。
* 传入参数及意义：
  - 传入一个[ContractArg](#xtar合约设计文档)合约参数
    - 该参数是一个结构体，具有3个成员：
      - 1. 代理域名范围
        2. Agent账户
        3. 是否永久代理

  ```json
  {
    "name": "setRootAgentParam",
    "type": "struct",
    "components": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "receiverAccount",
        "type": "account"
      },
      {
        "name": "isPermanent",
        "type": "bool"
      }
    ]
  }
  ```
* 备注：
  - 第一个参数是代理范围，例如参数`abc.xtar`，表明代理范围是`*.abc.xtar`，即`abc.xtar`域名的所有<u>子孙域名</u>
  - 当第一个参数代理范围是`abc.xtar`，授权者只能是`xtar`的拥有者，只有根域名拥有才能是授权者，所以参数无需填入授权者，只需要通过授权范围，可以求出授权者是谁
  - 当Receiver得到代理身份后，其可以对其他人提供`abc.xtar`,`1.abc.xtar`,`1.1.abc.xtar`等级别的可注册域名授权，但是不能提供`1.xtar`,`xtar`的授权范围
    - 因为`1.xtar`,`xtar`不属于代理范围`abc.xtar`

* 返回值：
  
  * 无
* event log：
  
  * setRootAgent




### 可触发合约事件event

1.**购买域名buyXnsName事件**
* event名称：buyXnsName
* 描述：注册域名的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.registerName
  - 第二个参数开始代表token注册后相关实体:
    - 2.购买的域名
    - 3.购买者账户
    - 4.购买价格

  ```json
  {
    "name": "buyXnsName",
    "inputs": [
      {
        "name": "xnsName",
        "type": "string"
      },
      {
        "name": "buyer",
        "type": "account"
      },
      {
        "name": "price",
        "type": "uint64"
      }
    ]
  }
  ```


2.**注册域名registerName事件**

* event名称：registerName

* 描述：注册域名的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.registerName
  - 第二个参数开始代表注册域名事件返回的数据:
    - 2.域名
    - 3.绑定的账户
    - 4.该域名是否被设置成了该账户的主域名
    - 5.该域名状态
      - 只有值是1（Used）才是合法状态

  ```json
  {
    "name": "xnsName",
    "type": "string"
  },
  {
    "name": "account",
    "type": "account"
  },
  {
    "name": "isMainXnsName",
    "type": "bool"
  },
  {
    "name": "nameStatus",
    "type": "uint8"
  }  
  ```
* 备注：
  - 当该账户没有主域名的时候，该域名会被自动设置为主域名
  - 如果返回false，表明该账户之前已经有主域名了，除非账户拥有者setMainName，否则不会更改


3.**设置主域名setMainName事件**

* event名称：setMainName
* 描述：设置主域名的收据
* event_logs参数及其意义：
  * 第一个参数为event名称
    * 1.setMainName
  * 第二个参数开始代表token发行的相关参数：
    * 2.账户
    * 3.主域名

  ```json
  {
    "name": "account",
    "type": "account"
  },
  {
    "name": "mainName",
    "type": "string"
  }
  ```

4.**转让域名transferName事件**

* event名称：transferName
* 描述：转让时返回的收据
* event_logs参数及其意义：
  - 第一个参数为event名称
    - 1.transferName
  - 第二个参数开始代表token发行的相关参数：
    - 2.域名
    - 3.转出账户
    - 4.接收账户

  ```json
  {
    "name": "xnsName",
    "type": "string"
  },
  {
    "name": "srcAddress",
    "type": "account"
  },
  {
    "name": "dstAddress",
    "type": "account"
  }
  ```

7.**拉黑域名blockName事件**

- event名称：blockName
- 描述：拉黑域名状态时的收据
- event_logs及其参数：
  - 第一个参数为event名称
    - 1.setNameStatus
  - 第二个参数开始代表:
    - 2.域名
    - 3.拉黑之前域名的状态
    - 4.拉黑之后域名的状态，只能是2（Blocked）

  ```json
  {
    "name": "nameContent",
    "type": "string"
  },
  {
    "name": "statusBefore",
    "type": "uint8"
  },
  {
    "name": "statusAfter",
    "type": "uint8"
  }
  ```

7.**转出域名支票transferNameTo**

- event名称：transferNameTo
- 描述：授权转出域名时的收据
- event_logs及其参数：
  - 第一个参数为event名称
    - 1.transferNameTo
  - 第二个参数开始代表:
    - 2.域名
    - 3.授权转出地址
    - 4.授权接收地址

  ```json
  {
    "name": "xnsName",
    "type": "string"
  },
  {
    "name": "srcAccount",
    "type": "account"
  },
  {
    "name": "dstAccount",
    "type": "account"
  }
  ```

8.**提取授权转让的域名transferNameAccept事件**

- event名称：transferNameAccept
- 描述：提取授权转让的域名时的收据
- event_logs及其参数：
  - 第一个参数为event名称
    - 1.transferNameAccept

  - 第二个参数开始代表:
    - 2.域名
    - 3.授权转出地址
    - 4.授权接收地址

  ```json
  {
    "name": "xnsName",
    "type": "string"
  },
  {
    "name": "srcAddress",
    "type": "account"
  },
  {
    "name": "dstAddress",
    "type": "account"
  }
  ```

9.**异步注册签名registerSign事件**

* event名称:registerSign

* 描述：异步签名时的收据

* event_logs参数：

* 第一个参数为event名称

  - ```
    registerSign
    ```

* 第二个参数开始代表:

  - 2.授权可注册的域名范围
  - 3.授权签名账户
  - 4.接收者账户
  - 5.授权Or取消授权
  - 6.签名结果

10.**设置根域名签名的代理人**

- event名称:setRootAgent

- 描述：根域名签名的代理时的收据

- event_logs参数：

- 第一个参数为event名称

  - ```
    setRootAgent
    ```

- 第二个参数开始代表:
  - 2.代理范围
  - 3.授权签名账户
  - 4.接收者账户
  - 5.是否是永久代理