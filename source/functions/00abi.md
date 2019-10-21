# 智能合约ABI设计文档

Bithumb Chain合约ABI设计文档用于指导Bithumb Chain合约ABI生成，以及合约调用。ABI文档以JSON格式表示。

一个合约的ABI对象即为一个JSON对象，可以在ABI对象中内嵌子对象, 每一个对象都有一个type字段属性，以表示对象类型，以及若干个跟子对象相关的其他字段类型。

* [Bithumb-Chain合约ABI设计文档](#bithumb-chain合约abi设计文档)
  * [1、数据类型](#1、数据类型)
  * [2、合约参数类型](#2、合约参数类型)
  * [3、函数类型](#3、函数类型)
  * [4、Event对象](#4、event对象)
  * [5、合约对象](#5、合约对象)
  * [6、合约参数编码](#6、合约参数编码)

### 1、数据类型

Bithumb Chain合约支持以下几种基本数据类型：

1. **nil**		空类型
2. **int8**		8位有符号整型
3. **uint8**	8位无符号整型
4. **int32** 	32位有符号整型
5. **uint32**	32位无符号整型
6. **int64**	64位有符号整型
7. **uint64**  	无符号64位整型  
8. **string**  	字符串类型  
9. **bool**    	布尔类型 
10. **bytes**  	byte数组类型 
11. **account**  账户类型（20byte数组或者字符串域名）
12. **uint256**  哈希类型（32byte数组）
13. **array**       数组类型
14. **struct**      结构体类型

### 2、合约参数类型
ContractArg 合约参数类型用于合约函数和合约事件的输入输出的数据类型。

合约参数的JSON表示：

```
{
    "name":"arg name",
    "type":"arg type",
    "components":[
        {
            "name":"arg name",
            "type":"arg type",
        },
        ...
    ]
}
```
其中：
"name":表示参数名称，
"type":参数类型，
"components":用于当type是array和struct时指定成员类型，是一个合约参数数组。当type是array和struct之外的基础类型，则没有components字段。

如:

```
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
```

### 3、函数类型

ContractFunction 合约函数是合约的基本类型，用于实现合约功能。合约函数的输入输出都是合约参数ContractArg类型

函数的JSON表示：  

```
{
    "name":"function name",
    "intputs":[
       ContactArgs
        ...
    ],
    "outputs":[
       ContactArgs
    ]
}
```

其中：  
"name":"function name" 定义函数的名称  
"intputs":[]定义了函数的输入参数，是一个合约参数对象数组   
"outputs":[] 定义了函数的返回值类型，是一个合约参数对象数组，目前XTAR 合约只支持单返回值。

如：  

```
{
    "name":"add",
    "intputs":[
        {
            "name":"a",
            "type":"uint64"
        },
        {
            "name":"b",
            "type":"uint64"
        }
    ],
    "outputs":[
        {
            "name":"c",
            "type":"uint64"
        }
    ]
}
```

### 4、Event对象

Event对象用于输出合约函数的执行结果，一个合约函数可以输出零个或多个Event对象

Event对象JSON表示：   

```
{
    "name":"event name",
    "inputs":[
        ContractArg
        ...
    ]
}
```
其中:      
"name":"event name" 表示定义的event对象名称   
"inputs":[] 定义Event输出的结果，是一个合约参数对象数组   
如：   

```
{
    "type":"event",
    "name":"transfer",
    "inputs":[
        {
            "name":"from",
            "type":"account"
        },
        {
            "name":"to",
            "type":"account"
        },
        {
            "name":"value",
            "type":"uint64"
        }
    ]
}
```

### 5、合约对象
合约对象是合约abi的顶层对象，一个合约abi就是一个合约对象。

Contract 合约对象JOSN表示:

```
{
    "version":"abi version",
    "contract_account":"contract address",
    "functions":[
        ContractFunction,
        ...
    ]
    "events":[
        ContractEvent,
        ...
    ]
}
```

其中：
"version":表示的是合约abi的版本，目前统一使用"1.0"
"contract_account":表示的是合约的地址, 合约地址由合于code生成
"functions":表示的是合约函数，是一个合约函数数组
"events":"表示的是合约事件"，是一个合约事件数组

如:

```
{
    "verison":"1.0",
    "contract_account":"XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
    "functions":[
        "name":"transfer",
        "intputs":[
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
                        "name":"value",
                        "type":"uint64"
                    }
                ]
            }
        ],
        "outputs":[],
    ],
    "events":[
        "name":"transfer",
        "inputs":[
            {
                "name":"from",
                "type":"account"
            },
            {
                "name":"to",
                "type":"account"
            },
            {
                "name":"value",
                "type":"uint64"
            }
        ]
    ]
}
```

### 6、合约参数编码

在编码合约参数时，首先编码参数类型，数据类型nil、int8、uint8、int32、uint32、int64、uint64、string、bool、bytes、account、uint256、array以及struct分别用从0-13的uint8的数字表示。

对于array和struct类型，还需编码一个uint32类型的长度数据，对array来说是数组长度，对于struct来说是struct的字段数量。注意，对于array类型，为了方便处理，每个数组元素都需要编码数据类型。

最后是编码实际的数据。对实际数据的编码采用Bithumb Chain统一数据编码方式。

