# 信息获取

Bithumb Chain客户端 `bithumbchain-CLI` 提供了信息获取模块，可以在命令行中通过 `info` 命令进行以下操作：

- 查询区块信息
- 查询交易信息
- 查询日志信息
- 查询合约信息

此外，你可以通过 `help` 命令获取信息获取模块的帮助信息 。

```shell
./bithumbchain-cli info help
```

## [区块信息](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/11-block-info?id=区块信息)

要根据区块高度或者区块哈希查询区块信息，使用`block` 命令。

你可以在第一个终端将客户端连接到测试网。

```shell
./bithumbchain-cli --testnet
```

然后在第二个终端中查询测试网中的区块信息。

```shell
$ ./bithumbchain-cli info block 1
Block:1 =>
{
   "Header": {
      "version": 1,
      "prev_block_hash": "867b45bbd4c63d398f595548f4d554d7ab360efddfc92c6e0df04e991fdac2d0",
      "prev_proposal_block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
      "transaction_root": "0000000000000000000000000000000000000000000000000000000000000000",
      "block_root": "968babe26b524f3cd3a585356be27583533f869d7229906c04a6054b4953c0b8",
      "state_root": "0000000000000000000000000000000000000000000000000000000000000000",
      "nonce": 7629934305395839314,
      "timestamp": 1563256051,
      "consensus_no": 1,
      "consensus_index": 0,
      "consensus_num": 0,
      "height": 1,
      "vrf_value": "",
      "vrf_proof": "",
      "next_bookkeeper": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
      "bookkeepers": [
         "01025c381425dc4f7f5dc84e44272092293f18ff6d2e3b408b0b7efdb55d2544dc82"
      ],
      "sig_data": [
         "01bf8e7319c10eea9ac562827b6ef276878cc628633208098bfbd48ca140da0a10fba1bb756deb4fd5d7f3312d93c081f0ba32bf3f688491c7957d805a22b06162"
      ],
      "hash": "5a3f892d827210b8ae9ae39dc30a45bf92d9127efc43d997b222942e7d0efd17"
   },
   "TxList": []
}
$ ./bithumbchain-cli info block d85180c488dac6987c94c3c3f11667c58df3eadfdff68010b5c8c2075d8c5cd9
Block:d85180c488dac6987c94c3c3f11667c58df3eadfdff68010b5c8c2075d8c5cd9 =>
{
   "Header": {
      "version": 1,
      "prev_block_hash": "415ec035b839b8318f5ca7c5da264c9940d26185096864a7b66eb09c4619848a",
      "prev_proposal_block_hash": "0000000000000000000000000000000000000000000000000000000000000000",
      "transaction_root": "0000000000000000000000000000000000000000000000000000000000000000",
      "block_root": "0000000000000000000000000000000000000000000000000000000000000000",
      "state_root": "0000000000000000000000000000000000000000000000000000000000000000",
      "nonce": 6671621713384406800,
      "timestamp": 1560764432,
      "consensus_no": 2,
      "consensus_index": 0,
      "consensus_num": 0,
      "height": 2,
      "vrf_value": "",
      "vrf_proof": "",
      "next_bookkeeper": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
      "bookkeepers": [
         "01025c381425dc4f7f5dc84e44272092293f18ff6d2e3b408b0b7efdb55d2544dc82"
      ],
      "sig_data": [
         "01a9283896bad9bc93b0cca99bec6e0803ea58e8d5a2789ae5f23863b0b71d3674aa99655a2b994bc9dbed6ea451580b238bcecf2bc0477391dddf59fbb1810e6f"
      ],
      "hash": "d85180c488dac6987c94c3c3f11667c58df3eadfdff68010b5c8c2075d8c5cd9"
   },
   "TxList": []
}
```

## [交易信息](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/11-block-info?id=交易信息)

要根据交易哈希查询交易信息，使用`tx` 命令。

你可以在第一个终端将客户端连接到测试网。

```shell
./bithumbchain-cli --testnet
```

然后在第二个终端中查询测试网中的交易信息。

请求正文如下：

```shell
$ ./bithumbchain-cli info tx b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92
Transaction:b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92 =>
{
   "Height": 2804,
   "Tx": {
      "tx_type": 0,
      "version": 0,
      "nonce": 9559358314341740493,
      "payload": {
         "address": "0000000000000000000000000000000000000001",
         "method": "transfer",
         "args": "0c010d030a621fc1db6be573451def1271530d843e378c2a450a28d1241e6b77872eca282a18e68d55a4344726690664"
      },
      "attributes": [],
      "payer": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
      "sig": [
         {
            "public_keys": [
               "01025c381425dc4f7f5dc84e44272092293f18ff6d2e3b408b0b7efdb55d2544dc82"
            ],
            "m": 1,
            "sig_data": [
               "01bdf5bca24dc267e01e92a2641b30fe9fe305c278dd95cd5bfcd4f825dded553700101dc87a696774c1e93802002dfd52021f918681eb8c7db0e7c64e677a1ed4"
            ]
         }
      ],
      "hash": "b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92"
   }
}
```

在返回的交易状态中，各字段的含义如下：

- `Nonce`：随机数。
- `Payer`：指定支付交易资源的钱包账户（默认使用转账交易的扣款账户）。
- `Hash`：交易哈希。

## [日志信息](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/11-block-info?id=日志信息)

要根据交易哈希查询交易状态，使用`log` 命令。

你可以在第一个终端将客户端连接到测试网。

```shell
./bithumbchain-cli --testnet
```

然后在第二个终端中查询测试网中的交易状态。

请求正文如下：

```shell
$ ./bithumbchain-cli info log b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92
Transaction:b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92 =>
{
   "height": 2804,
   "tx_hash": "b19419eb744ddf544beb6544c0062c94c61af259b0c11f2fda7c2affcf698c92",
   "error_code": 0,
   "net_usage": 214,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "ram_usage": "41"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "100"
         ]
      }
   ]
}
```

在返回的交易状态中，各字段的含义如下：

- `error_code`：交易执行结果，若值为 `0`，表示交易执行成功。
- `gas_consumed`：执行交易所消耗的资源。
- `event_logs` ：交易执行时所触发的事件。

## [合约信息](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/11-block-info?id=合约信息)

要获取所接入网络的智能合约信息，使用`contract` 命令。

你可以在第一个终端将客户端连接到测试网。

```shell
./bithumbchain-cli --testnet
```

然后在第二个终端中查询测试网的当前区块高度。

```shell
$ ./bithumbchain-cli info contract ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Contract:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4 =>
{
   "code": "03656f7308872386f26fc1000028d1241e6b77872eca282a18e68d55a434472669d3103148c096c22dbf40fc46904aa17da6fb2ecad57af4b3c88242890f72d3e6",
   "name": "eos",
   "author": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
   "version": "1.0",
   "email": "******@**.com",
   "description": "eos1",
   "type": "native"
}
```

在返回的信息中，各字段的含义如下：

- `name`：合约的名称。
- `author` ：合约发行人的信息。
- `version` ：版本信息。
- `email` ：合约发行人的联系邮箱。
- `description` ：合约描述信息。
- `type` ：合约类型。

