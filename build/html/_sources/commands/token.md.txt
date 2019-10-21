# 一键资产映射

区别于传统的智能合约发币，Bithumb Chain Chain内置了一键资产映射模块，使资产映射更加高效和安全。用户无需许可即可发布或映射Token（包括但不限于稳定币）。

Bithumb Chain客户端 `Bithumb Chain CLI` 提供了token管理模块，可以非常方便的在链上注册自己的 `token` 和发行任意数量的token。可以在命令行中通过 `token` 命令实现：

- 对 `token` 的注册、发行、销毁。
- 对 `token` 的信息查询。

此外，你可以通过 `help` 命令获取钱包管理模块的帮助信息 。

```shell
xtar-cli token help
```

## [token注册](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=注册)

要注册token，使用 `register` 命令：

```bash
$ xtar-cli token register --symbol eos --decimal 8 --total-supply 10000000000000000 --issuer 1 --version 1.0 --name eos --author AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9 --email ******@**.com --desc eos1
Issuer account
Please input password
Password:
Register token:
Symbol:eos
Decimal:8
Total supply:10000000000000000
Issuer:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
TxHash:d3103148c096c22dbf40fc46904aa17da6fb2ecad57af4b3c88242890f72d3e6
Waiting for eventLog...
EventLog:=>
{
   "height": 2892,
   "tx_hash": "d3103148c096c22dbf40fc46904aa17da6fb2ecad57af4b3c88242890f72d3e6",
   "error_code": 0,
   "net_usage": 272,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
         "ram_usage": "266"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000003",
         "data": [
            "register",
            "ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4",
            "eos",
            "8",
            "10000000000000000"
         ]
      }
   ]
}
```

在注册模块中， `register` 命令所支持的选项如下表所示，也可以通过 `--help` 选项获取帮助信息。

```shell
xtar-cli token register --help
```

| 选项             | 描述                                                |
| ---------------- | --------------------------------------------------- |
| `--symbol`       | 指定 `token` 的象征符号                             |
| `--decimal`      | 指定 `token` 的小数位，最大支持的小数位是8，默认：8 |
| `--total-supply` | 指定 `token` 的总发行量，最大支持1000亿，默认：0    |
| `--issuer`       | 指定 `token` 的发行者地址或者地址索引               |
| `--version`      | 指定 `token` 合约的版本                             |
| `--name`         | 指定 `token` 合约名称                               |
| `--author`       | 指定 `token` 合约的所有者                           |
| `--email`        | 指定 `token` 合约的联系信箱                         |
| `--desc`         | 指定 `token` 合约的描述信息                         |

## [token发行](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=发行)

`token` 注册之后可以根据需要发行token，使用 `issue` 选项即可：

```shell
$ xtar-cli token issue --amount 10 --issuer 1 --address ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Issuer account
Please input password
Password:
Issue token
TokenAddress:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Issuer:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Issue amount:10
Source:
TxHash:79d101db1ea861ad3af94f26f7510b39a1df83e863a566fdaee702a1d58f75ba
Waiting for eventLog...
EventLog:=>
{
   "height": 2910,
   "tx_hash": "79d101db1ea861ad3af94f26f7510b39a1df83e863a566fdaee702a1d58f75ba",
   "error_code": 0,
   "net_usage": 200,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
         "ram_usage": "63"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000003",
         "data": [
            "issue",
            "ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "10",
            ""
         ]
      }
   ]
}
```

token发行后的查询：

```shell
$ xtar-cli asset balance 1 --asset-type=ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Token:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Balance:=>
10
```

在发行模块中， `issue` 命令所支持的选项如下表所示，也可以通过 `--help` 选项获取帮助信息。

```shell
xtar-cli token issue --help
```

| 选项        | 描述                                                        |
| ----------- | ----------------------------------------------------------- |
| `--amount`  | 指定 `token` 发行的数量                                     |
| `--issuer`  | 指定 `token` 的发行者地址或者地址索引，需和注册的issuer相同 |
| `--address` | 指定 `token` 合约的地址                                     |
| `--source`  | 指定 `token` 的发行者地址或销毁的transaction地址            |

## [销毁token](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=销毁)

如果要销毁发行账户的 `token` ，可以使用 `destroy` 命令来减少发行量：

```shell
查询发行token的余额:
$ xtar-cli asset balance 1 --asset-type=ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Token:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Balance:=>
10

销毁token:
$ xtar-cli token destroy --amount 5 --issuer 1 --address ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Issuer account
Please input password
Password:
Destroy token
TokenAddress:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Issuer:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Destroy amount:5
Source:
TxHash:3d627dfb3cf194b5e1631b75b6f985cc3da4e7a30c2295cf21cc7a9169341f41
Waiting for eventLog...
EventLog:=>
{
   "height": 2942,
   "tx_hash": "3d627dfb3cf194b5e1631b75b6f985cc3da4e7a30c2295cf21cc7a9169341f41",
   "error_code": 0,
   "net_usage": 200,
   "cpu_usage": 40,
   "ram_usage": [],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000003",
         "data": [
            "destroy",
            "ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "5",
            ""
         ]
      }
   ]
}

再次查询余额：
$ xtar-cli asset balance 1 --asset-type=ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Token:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
Balance:=>
5
```

## [信息查询](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=信息查询)	

要对`token` 信息进行查询，使用 `info` 命令：

```shell
$ xtar-cli token info ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4
TokenAddress:ab645d0af8c30309c7a8e2fa89503fef8c4cc4d4 =>
Symbol:eos
Decimal:8
Supply:5
TotalSupply:10000000000000000
Issuer:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
```
