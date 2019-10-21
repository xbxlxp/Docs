## Authorization合约

authorization合约是一本权限合约，可由授权账户为被授权账户添加权限。权限包括设置Active，该Active可代表授权账户授权，作为添加该账户作为payer;添加授权，可以代替权限账户进行指定合约方法的调用；或者将该权限账户作为调用某合约方法的payer

合约地址：XagqqFetxiDb9wbartKDrXgnqLaiWHViiH

### 可调用合约方法

1、**设置Active**

* 方法名称：setActive

* 传入参数及意义：
  * account：account类型，表示添加授权的账户
  * newActive：account类型，表示被添加为Active的账户
* 返回值及其意义：
  * 无

* 触发event：
  * setActive

```json
    {
      "name": "setActive",
      "inputs":[
        {
          "name": "account",
          "type": "account"
        },
        {
          "name": "newActive",
          "type": "account"
        }
      ]
    }
```

2、**设置payer**

* 方法名称：setPayer
* 传入参数及其意义：
  * account：account类型，表示添加授权的账户
  * newPayere：account类型，表示被添加为Payer的账户
* 返回值及其意义
  * 无
    * 1.代表xtar token的精度
      * 预期返回值为：8



* 触发event：
  - setPayer

```json
{
  "name": "setPayer",
  "inputs":[
    {
      "name": "account",
      "type": "account"
    },
    {
      "name": "newPayer",
      "type": "account"
    }
  ]
}
```

3、**为某一账户添加某合约执行方法的权限**

* 方法名称：addAuth
* 传入参数及其意义：
  * 传入一个结构体，该结构体具有5个成员：
    * account：account类型，表示添加权限的账户
    * contractAccount：合约账户
    * method：合约账户的中方法名称，string类型
    * permissionAccount：被添加权限账户，account类型
    * isPayer：是否设置为payer，bool类型。若为true则被授权的账户只能将授权账户作为该方法的payer
* 返回值及其意义：
  * 无

* 触发event：
  - addAuth

```json
    {
      "name": "addAuth",
      "inputs":[
        {
          "name": "account",
          "type": "account"
        },
        {
          "name": "contractAccount",
          "type": "account"
        },
        {
          "name": "method",
          "type": "string"
        },
        {
          "name": "permissionAccount",
          "type": "account"
        },
        {
          "name": "asPayer",
          "type": "bool"
        }
      ]
    }
```

4、**删除为某一账户添加某合约执行方法的权限**

* 方法名称：delAuth
* 传入参数及其意义：
  - 传入一个结构体，该结构体具有5个成员：
    - account：account类型，表示添加权限的账户
    - contractAccount：合约账户
    - method：合约账户的中方法名称，string类型
    - permissionAccount：被添加权限账户，account类型
    - isPayer：是否设置为payer，bool类型。若为true则被授权的账户只能将授权账户作为该方法的payer
* 返回值及其意义：
  - 无
* event log:
  * delAuth

```json
    {
      "name": "delAuth",
      "inputs":[
        {
          "name": "account",
          "type": "account"
        },
        {
          "name": "contractAccount",
          "type": "account"
        },
        {
          "name": "method",
          "type": "string"
        },
        {
          "name": "permissionAccount",
          "type": "account"
        },
        {
          "name": "asPayer",
          "type": "bool"
        }
      ]
    }
```

5、**查询账户的授权信息**

* 方法名称：getAuthInfo
* 传入参数及其意义：
  * 传入一个account参数，表示被查询的账户

* 返回值及其意义：
  * 返回一个结构体表示授权信息，该结构体有4个成员：
    * 被设置为active的账户
    * 被设置为payer的账户
    * 被添加合约执行权限的列表
      * 该列表成员是一个结构体，具有3个参数
        * 合约账户
        * 合约方法名称
        * 被添加具有执行权限的账户列表
          * 被添加具有该合约方法执行权限的账户
    * 被添加可将授权账户作为指定合约方法的payer的列表
      - 该列表成员是一个结构体，具有3个参数
        - 合约账户
        - 合约方法名称
        - 被添加具有执行权限的账户列表
          - 被添加具有该合约方法执行权限的账户

```json
        {
          "name": "authInfo",
          "type": "struct",
          "components": [
            {
              "name": "active",
              "type": "account"
            },
            {
              "name": "payer",
              "type": "account"
            },
            {
              "name": "actionAuths",
              "type": "array",
              "components": [
                {
                  "name": "actionAuth",
                  "type": "struct",
                  "components": [
                    {
                      "name": "contract",
                      "type": "account"
                    },
                    {
                      "name": "method",
                      "type": "string"
                    },
                    {
                      "name": "permissionList",
                      "type": "array",
                      "components": [
                        {
                          "name": "permissionAccount",
                          "type": "account"
                        }
                      ]
                    }
                  ]
                }
              ]
            },
            {
              "name": "payerAuths",
              "type": "array",
              "components": [
                {
                  "name": "payerAuth",
                  "type": "struct",
                  "components": [
                    {
                      "name": "contract",
                      "type": "account"
                    },
                    {
                      "name": "method",
                      "type": "string"
                    },
                    {
                      "name": "permissionList",
                      "type": "array",
                      "components": [
                        {
                          "name": "permissionAccount",
                          "type": "account"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
```

* 触发event：
  * 无
