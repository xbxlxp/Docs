RPC
============

具体用法可以浏览 [Bithumb Chain RPC Introduction](https://documenter.getpostman.com/view/7127222/SVYkvg4t?version=latest#intro) 。

## bithumbchain.GetTransaction

交易信息查询。根据提供的hash值返回交易信息和所在区块高度

**Parameters**

1. `DATA` HexString - 交易的Hash值
2. `"0"|"1"` String - `"0"`返回Json格式数据，`"1"`返回Raw数据

**Example**

```json
"params": ["953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca","0"]
```

**Returns**

1. `Height` Number - 区块高度

2. `tx_type` Number - 交易类型（0:Invoke/1:Deploy）

3. `version` Number - 交易版本ID

4. `nonce` Number - nonce

5. `payload` Object - 交易详细内容

   5.1 `contract_account` String - 交易所调用的合约账户

   5.2 `method` String - 交易所调用的合约方法

   5.3 `args` Encode String - 调用合约方法所传入的参数（encode）

6. `attributes` Array - 交易附属信息

7. `payer` String - 该交易的网络/存储支付者

8. `sig` Object - 交易签名信息

   8.1 `public_keys`  Array[HexString] - 交易的发起者/payer的公钥地址

   8.2 `m` Number - 多签生效的阈值

   8.3 `sig_data`  Array[HexString] - 公钥对应的签名

9. `hash`   HexString - 交易的hash值

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "Height": 43,
        "Tx": {
            "tx_type": 0,
            "version": 0,
            "nonce": 16955790161879403521,
            "payload": {
                "contract_account": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                "method": "transfer",
                "args": "0c010d030a0127178e5d8c56480007761454953b1ce93f134d280a01d5898e85b770073769984f7b2aa17865c60e9dd1060a"
            },
            "attributes": [],
            "payer": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
            "sig": [
                {
                    "public_keys": [
                        "0103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218"
                    ],
                    "m": 1,
                    "sig_data": [
                        "018bea1bcd11ed29ef203259681434a40aeeb51014c8ab43a04da019751d7170134229842dd2638a073396dfebbc96097d13c3ad56415e270b869fa550f3164237"
                    ]
                }
            ],
            "hash": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca"
        }
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

**Example For RAW**
`"params": ["953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca","1"]`

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": "2b000088eb4f11510ce28c01010000000000000000000000000000000000000001087472616e73666572320c010d030a0127178e5d8c56480007761454953b1ce93f134d280a01d5898e85b770073769984f7b2aa17865c60e9dd1060a0027178e5d8c56480007761454953b1ce93f134d2801010103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218010141018bea1bcd11ed29ef203259681434a40aeeb51014c8ab43a04da019751d7170134229842dd2638a073396dfebbc96097d13c3ad56415e270b869fa550f3164237",
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetBlock

区块信息查询。根据提供的区块hash值或者高度值返回区块信息

**Parameters**

1. `DATA` HexString | String - 区块的hash值（hex string）或者高度（Number->String）
2. `"0"|"1"` String - `"0"`返回Json格式数据，`"1"`返回Raw数据

**Example**

```json
	"params": ["43","0"]
```

```json
	"params": ["7a3d220d4f8c922b7a7c4e10e6dee6ca77f72447a14464826e5b72becdf09b97","0"]
```

**Returns**

1. `Header` Object - 区块头

   1.1. `version` Number - 区块版本ID

   1.2. `prev_block_hash` HexString - 前一区块的hash值

   1.3. `prev_proposal_block_hash` HexString - 前一提案区块的hash值

   1.4. `transaction_root` HexString - 当前区块内所有交易的hash值

   1.5. `block_root` HexString - 区块根

   1.6. `incr_state_root` HexString - 区块状态hash

   1.7. `state_root` HexString - 区块状态根hash

   1.8. `nonce` Number - 当前区块的随机数

   1.9. `timestamp` Number - unix timestamp

   1.10. `consensus_no` Number - 共识序号

   1.11. `consensus_index` Number - 当前区块在当前共识批次中的索引序号

   1.12. `consensus_num` Number - 共识共识批次中的区块数量

   1.13. `height` Number - 该区块所在高度

   1.14. `vrf_value` HexString - VRF值

   1.15. `vrf_proof` HexString - VRF证明

   1.16. `next_bookkeeper` String - vrf计算出的下一个出块者，base58显示

   1.17. `bookkeepers` Array[hexString] - 该区块验证者的公钥

   1.18. `sig_data` Array[hexString] - 该区块验证者的签名列表

   1.19. `hash` HexString - 该区块的Hash值

2. `TxList` Array[交易] - 该区块内打包的交易列表，详细内容参考`bithumbchain.GetTransaction`

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "Header": {
            "version": 1,
            "prev_block_hash": "ba9cdc5704171b299cfd3a6ea0da14cea1ec4ef71604861ce3b5e599c9776e12",
            "prev_proposal_block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
            "transaction_root": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca",
            "block_root": "d12f32c42e47c3e77d902453ecaa493add7a441d9172b843f0e4b61ad0878138",
            "incr_state_root": "7fa64f3434c8ca63260188b6bb4f5a3a5e08427b3556a03187c88da313562bde",
            "state_root": "4245491c402cde1b256c475ebba39d31eefe2acae089f8844528783731fb8247",
            "nonce": 8946061438353727430,
            "timestamp": 1565061435,
            "consensus_no": 43,
            "consensus_index": 0,
            "consensus_num": 0,
            "height": 43,
            "vrf_value": "",
            "vrf_proof": "",
            "next_bookkeeper": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
            "bookkeepers": [
                "0103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218"
            ],
            "sig_data": [
                "0133ac97e5fa348d49dc9f551a85e3cf5130f03290d8e348981bcba0fa71280e3c0db23e5116a7d9b35f14f8218c4704d728b4acfdd63887468fcbbbe5b234d87c"
            ],
            "hash": "7a3d220d4f8c922b7a7c4e10e6dee6ca77f72447a14464826e5b72becdf09b97"
        },
        "TxList": [
            {
                "tx_type": 0,
                "version": 0,
                "nonce": 16955790161879403521,
                "payload": {
                    "contract_account": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                    "method": "transfer",
                    "args": "0c010d030a0127178e5d8c56480007761454953b1ce93f134d280a01d5898e85b770073769984f7b2aa17865c60e9dd1060a"
                },
                "attributes": [],
                "payer": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                "sig": [
                    {
                        "public_keys": [
                            "0103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218"
                        ],
                        "m": 1,
                        "sig_data": [
                            "018bea1bcd11ed29ef203259681434a40aeeb51014c8ab43a04da019751d7170134229842dd2638a073396dfebbc96097d13c3ad56415e270b869fa550f3164237"
                        ]
                    }
                ],
                "hash": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca"
            }
        ]
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetHeader

区块头部信息查询。根据提供的区块hash值或者高度值返回区块头部信息

**Parameters**

1. `DATA` HexString | String - 区块的hash值（hex string）或者高度（Number->String）
2. `"0"|"1"` String - `"0"`返回Json格式数据，`"1"`返回Raw数据

**Example**

```json
	"params": ["43","0"]
```

```json
	"params": ["7a3d220d4f8c922b7a7c4e10e6dee6ca77f72447a14464826e5b72becdf09b97","0"]
```

**Returns**

1. 区块头部信息参考`bithumbchain.GetBlock`的返回值中的Header

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "version": 1,
        "prev_block_hash": "ba9cdc5704171b299cfd3a6ea0da14cea1ec4ef71604861ce3b5e599c9776e12",
        "prev_proposal_block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
        "transaction_root": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca",
        "block_root": "d12f32c42e47c3e77d902453ecaa493add7a441d9172b843f0e4b61ad0878138",
        "incr_state_root": "7fa64f3434c8ca63260188b6bb4f5a3a5e08427b3556a03187c88da313562bde",
        "state_root": "4245491c402cde1b256c475ebba39d31eefe2acae089f8844528783731fb8247",
        "nonce": 8946061438353727430,
        "timestamp": 1565061435,
        "consensus_no": 43,
        "consensus_index": 0,
        "consensus_num": 0,
        "height": 43,
        "vrf_value": "",
        "vrf_proof": "",
        "next_bookkeeper": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
        "bookkeepers": [
            "0103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218"
        ],
        "sig_data": [
            "0133ac97e5fa348d49dc9f551a85e3cf5130f03290d8e348981bcba0fa71280e3c0db23e5116a7d9b35f14f8218c4704d728b4acfdd63887468fcbbbe5b234d87c"
        ],
        "hash": "7a3d220d4f8c922b7a7c4e10e6dee6ca77f72447a14464826e5b72becdf09b97"
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetBlockHash

根据区块高度查询区块hash

**Parameters**

1. `DATA` Number - 区块的高度值

**Example**

```json
	"params": [43]
```

**Returns**

1. HexString - 该区块的hash值

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": "7a3d220d4f8c922b7a7c4e10e6dee6ca77f72447a14464826e5b72becdf09b97",
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetCurrentBlockHeight

查询系统当前最新区块高度

**Parameters**
none

**Example**

```json
	"params": []
```

**Returns**

1. Number - 系统当前区块高度

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": 1144,
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetCurrentBlockHash

查询系统当前最新区块的hash

**Parameters**
none

**Example**

```json
	"params": []
```

**Returns**

1. HexString - 系统当前区块hash

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": "057efcb5d239a3a761be72d8cb8019118b53206658c77da2c5d7207f7a7bccbb",
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetEventLog

根据交易hash值或者区块高度查询对应的EventLog

**Parameters**

1. `DATA` HexString | String - 交易的Hash值或者区块的高度（Number->String）
2. `"0"|"1"` String - `"0"`返回Json格式数据，`"1"`返回Raw数据

**Example For Tx**

```json
	"params": ["953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca","0"]
```

**Returns For Tx**

1. `height` Number - 该交易所在区块的高度

2. `tx_hash` HexString - 该交易的hash值

3. `error_code` Number - 该交易执行的错误码

4. `net_usage` Number - 该交易执行所花费的NET(bytes)

5. `cpu_usage` Number - 该交易执行所花费的CPU(微秒)

6. `ram_usage` Object

   6.1 `address` String - 该交易的RAM提供者(base58)

   6.2 `ram_usage` String - 该交易执行所花费的RAM

7. `event_logs` Array[event]

   7.1 `contract` String - 该交易所调用的合约地址

   7.2 `data` Array[String] - 该交易执行返回的event数据，默认第一个参数为调用合约的方法名称

**Example For Tx**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "height": 43,
        "tx_hash": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca",
        "error_code": 0,
        "net_usage": 217,
        "cpu_usage": 40,
        "ram_usage": [
            {
                "account": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                "ram_usage": "41"
            }
        ],
        "event_logs": [
            {
                "contract": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                "data": [
                    "transfer",
                    "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                    "Xv9vYirEPVTDrd1L5YtBSuRR5TeMmruAzr",
                    "10"
                ]
            }
        ]
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

**Example For Block**

```json
	"params": ["43","0"]
```

**Returns For Block**

1. Array[tx event] - 该区块内所有交易的eventlog，详细内容参考交易的EventLog

**Example For Block**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": [
        {
            "height": 43,
            "tx_hash": "953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca",
            "error_code": 0,
            "net_usage": 217,
            "cpu_usage": 40,
            "ram_usage": [
                {
                    "account": "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                    "ram_usage": "41"
                }
            ],
            "event_logs": [
                {
                    "contract": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                    "data": [
                        "transfer",
                        "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                        "Xv9vYirEPVTDrd1L5YtBSuRR5TeMmruAzr",
                        "10"
                    ]
                }
            ]
        }
    ],
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetRangeEventLog

查询一定区块范围内的EventLog

**Parameters**

1. `startHeight` String - 查询起始区块（Number->String）
2. `endHeight` String - 查询结束区块（Number->String）
3. `contractAddress` String - 指定合约地址
4. `method` String - 指定合约方法
5. `"0"|"1"` String - `"0"`返回Json格式数据，`"1"`返回Raw数据

**Example**

```json
	"params": ["1","100","0000000000000000000000000000000000000001","transfer"]
	//查询区块1到100内，所有Bithumb的transfer交易的event log
```

**Returns**

1. `Array[Tx Event]` Array - 返回范围内符合条件的交易Event Log，详细参照`GetEventLog`中查询TX

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": [
        {
            "height": 51,
            "tx_hash": "aec1c2d9d9245f6367e7ec283aa9f143ca4ef8db3bc0ebc2d07bc39ea6cf2773",
            "error_code": 0,
            "net_usage": 449,
            "cpu_usage": 40,
            "ram_usage": [
                {
                    "address": "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                    "ram_usage": "47"
                }
            ],
            "event_logs": [
                {
                    "contract": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                    "data": [
                        "transfer",
                        "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                        "XagqqFetxiDb9wbartKDrXgnqLai8awY64",
                        "10000000000"
                    ]
                }
            ]
        },
        {
            "height": 51,
            "tx_hash": "f8fc7ee06fc02bf2ee473af137bbec6ec04bc39b2955f0a4a3af26ae35377179",
            "error_code": 0,
            "net_usage": 460,
            "cpu_usage": 40,
            "ram_usage": [
                {
                    "address": "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                    "ram_usage": "149"
                }
            ],
            "event_logs": [
                {
                    "contract": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                    "data": [
                        "transfer",
                        "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                        "XagqqFetxiDb9wbartKDrXgnqLahuf6AQq",
                        "20000000000"
                    ]
                }
            ]
        },
        {
            "height": 52,
            "tx_hash": "83f31c60424cd2b3a135eb719d9a061f89db3aeffcb2efec6d281f430111729b",
            "error_code": 0,
            "net_usage": 455,
            "cpu_usage": 40,
            "ram_usage": [
                {
                    "address": "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                    "ram_usage": "48"
                }
            ],
            "event_logs": [
                {
                    "contract": "0000000000000000000000000000000000000001",
                    "data": [
                        "transfer",
                        "XiiouyaZ1vDVckJUCBfTAeVPitaTvb31cz",
                        "Xv6HVMLy5fScUED9mBiiUxBDtovtct2nXA",
                        "10000000000000000"
                    ]
                }
            ]
        }
    ],
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.SendRawTx

发送交易

**Parameters**

1. `Data` HexString - Encode后的交易
2. `"0"|"1"` String - `"0"`（默认）发送交易，`"1"`预执行交易

**Example**

```json
	"params": ["0000886f03410293850a080000000000000000000000000000000000000001087472616e73666572300c010d030a27178e5d8c56480007761454953b1ce93f134d280ad5898e85b770073769984f7b2aa17865c60e9dd1060a0027178e5d8c56480007761454953b1ce93f134d2801010103776933b620272510599da0dd3c817a68f7b5e01eb5adb989f6fe1346b0235218010141017e704c14e1b0ee6f55ed5644d75c6f867956ea6e99fdb4a8f22683bcbf1f076171c15a85f2dc00831e6322c7f98c5865b216f5b13523836decb923e9cf3fbd52"]
```

**Returns**

1. `hash` HexString - 该交易的hash值

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": "34f12a49e730e939ca9d545e88a837ab0d2279d2d5d643bd26939470e7531542",
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

**Returns For 预执行**

1. `Data` TxEvent - 该交易的预执行结果，详细参考`GetEventLog`

**Example For 预执行**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "result": "00",
        "evt_log": {
            "height": 12365,
            "tx_hash": "34f12a49e730e939ca9d545e88a837ab0d2279d2d5d643bd26939470e7531542",
            "error_code": 0,
            "net_usage": 214,
            "cpu_usage": 40,
            "ram_usage": [],
            "event_logs": [
                {
                    "contract": "XagqqFetxiDb9wbartKDrXgnqLah9fKoTx",
                    "data": [
                        "transfer",
                        "XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw",
                        "Xv9vYirEPVTDrd1L5YtBSuRR5TeMmruAzr",
                        "10"
                    ]
                }
            ]
        }
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.ContractCall

本地账本查询

**Parameters**

1. `Data` HexString - Encode后的查询交易

**Example**

```json
	"params": ["00008823faa9e646fc054100000000000000000000000000000000000000010962616c616e63654f66150a27178e5d8c56480007761454953b1ce93f134d2800000000000000000000000000000000000000000000"]
```

**Returns**

1. `data` HexString - 查询结果（根据ABI定义结构，encode后的数据）

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": "0687038d63c5bf5f99",
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.BalanceOf

查询账户余额

**Parameters**

1. `ContractAddress` HexString - 合约地址
2. `Address` Base58 - 账户地址

**Example**

```json
	"params": ["XagqqFetxiDb9wbartKDrXgnqLah9fKoTx","XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw"]
```

**Returns**

1. `data` Number - 账户余额

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": 999884589064089,
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.AllowanceOf

查询两个账户之间的授权额度

**Parameters**

1. `ContractAddress` HexString - 合约地址
2. `Address` Base58 - 授权地址
3. `Address` Base58 - 被授权地址

**Example**

```json
	"params": ["XagqqFetxiDb9wbartKDrXgnqLah9fKoTx","XeFYQScWSznQGMv9i9QL1ukMnLeEe11Bdw","Xv9vYirEPVTDrd1L5YtBSuRR5TeMmruAzr"]
```

**Returns**

1. `data` Number - 授权余额

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": 10,
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetContract

查询合约信息

**Parameters**

1. `ContractAddress` HexString - 合约地址
2. `"0"|"1"` String - `"0"`(默认)返回Json格式数据，`"1"`返回Raw数据

**Example**

```json
	"params": ["XagqqFetxiDb9wbartKDrXgnqLah9fKoTx","0"]
```

**Returns**

1. `code` HexString - 合约地址
2. `name` String - 合约名称
3. `author` String - 作者
4. `version` String - 版本
5. `email` String - 邮箱地址
6. `description` String - 描述
7. `type` String - 合约类型(native/wasm)
8. `tx_hash` HexString - 该合约发布时的TXID

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "code": "0000000000000000000000000000000000000001",
        "name": "Bithumb",
        "author": "Bithumb",
        "version": "1.0",
        "email": "",
        "description": "Bithumb governance token",
        "type": "native",
        "tx_hash": "0000000000000000000000000000000000000000000000000000000000000000"
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```

## bithumbchain.GetResourceLimit

查询链上资源总量

**Parameters**
  none

**Example**

```json
	"params": []
```

**Returns**

1. `VirtualNetLimit` Number - 最新区块Net总量
2. `VirtualCpuLimit` Number - 最新区块CPU总量

**Example**

```json
{
    "jsonrpc": "2.0",
    "id": "1",
    "result": {
        "VirtualNetLimit": 15190902,
        "VirtualCpuLimit": 2897399
    },
    "error": {
        "code": 0,
        "message": "ok"
    }
}
```