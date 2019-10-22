# 内存管理

Bithumb Chain客户端 `bithumbchain-cli` 提供了RAM管理模块，可以在命令行中通过 `ram` 命令实现：

- 用 `BT` 购买RAM及特殊字节的RAM
- 出售RAM
- 查询账户RAM余额

此外，你可以通过 `help` 命令获取信息获取模块的帮助信息 。

```shell
xtar-cli ram help
```

## 账户查询RAM

要查询指定账户在所接入网络中的RAM信息，使用`balance` 命令。

你可以在第一个终端启动本地测试网。

```shell
xtar-cli --testnet
```

然后在第二个终端中查询指定账户在本地测试网中的账户余额。

```shell
$ xtar-cli ram balance 1
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
Ram:=>
{
   "Used": 0,
   "Reserved": 68719
}
$ xtar-cli ram balance AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Ram:=>
{
   "Used": 0,
   "Reserved": 0
}
```

## 购买RAM

在RAM管理模块中，`buy` 命令用于购买RAM交易，购买时需要使用 `BT`，其包含以下参数：

- `--buyer`：指定购买者资产地址（`BT`）。
- `--receiver`：指定接受者地址。
- `--amount` ：指定用于购买的 `BT` 额度。
- `--disable-wait-evtlog`：关闭`event log`日志。
- `--print-tx, -t` ：预执行并打印十六进制的交易内容，不发送到网络。

你可以在第一个终端启动本地测试网。

```shell
xtar-cli --testnet
```

然后在第二个终端中购买一定数量的RAM。

```shell
$  xtar-cli ram buy --buyer 0 --receiver 1 --amount 100
Ram buyer account
Please input password
Password:
BuyRam
Buyer:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
Receiver:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:100
TxHash:d256744eccf06e40b609aa354319459cdbf658402d8254f80face6618773f094
Waiting for eventLog...
EventLog:=>
{
   "height": 2436,
   "tx_hash": "d256744eccf06e40b609aa354319459cdbf658402d8254f80face6618773f094",
   "error_code": 0,
   "net_usage": 210,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "ram_usage": "46"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AFmseVrdL9f9oyCzZefL9tG6UbviRj6Fv6",
            "100"
         ]
      },
      {
         "contract": "0000000000000000000000000000000000000009",
         "data": [
            "buyRam",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "100",
            "1",
            "6802541"
         ]
      }
   ]
}
```

## 购买指定字节RAM

```shell
$ xtar-cli ram buyBytes --ram-amount=10000 --buyer=0 --receiver=1 
Ram buyer account
Please input password
Password:
BuyRamBytes
Buyer:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
Receiver:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:10000
TxHash:5b6a2074ba1a7848990136c49970ebaff25ab5427630f84e63b8818cc74dc031
Waiting for eventLog...
EventLog:=>
{
   "height": 9,
   "tx_hash": "5b6a2074ba1a7848990136c49970ebaff25ab5427630f84e63b8818cc74dc031",
   "error_code": 0,
   "net_usage": 217,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "ram_usage": "45"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AFmseVrdL9f9oyCzZefL9tG6UbviRj6Fv6",
            "29103847"
         ]
      },
      {
         "contract": "0000000000000000000000000000000000000009",
         "data": [
            "buyRam",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "29103847",
            "145520",
            "9950"
         ]
      }
   ]
}
```

## 出售RAM

在RAM管理模块中，`sell` 命令用于出售RAM。

```shell
$ xtar-cli ram balance 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Ram:=>
{
   "Used": 329,
   "Reserved": 9624
}
$ xtar-cli asset balance 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
XTAR balance:=>
100
$ xtar-cli ram sell --seller=1 --ram-amount=100
Ram seller account
Please input password
Password:
SellRam
Seller:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:100
TxHash:ebc79645aff8ec04d37f313b83c1cd0323caace563d18896b63e48339a23268b
Waiting for eventLog...
EventLog:=>
{
   "height": 3155,
   "tx_hash": "ebc79645aff8ec04d37f313b83c1cd0323caace563d18896b63e48339a23268b",
   "error_code": 0,
   "net_usage": 200,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
         "ram_usage": "3"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AFmseVrdL9f9oyCzZefL9tG6UbviRj6Fv6",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "289600"
         ]
      },
      {
         "contract": "0000000000000000000000000000000000000009",
         "data": [
            "sellRam",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "291056",
            "1456",
            "100"
         ]
      }
   ]
}
$ xtar-cli ram balance 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Ram:=>
{
   "Used": 332,
   "Reserved": 9521
}
```
