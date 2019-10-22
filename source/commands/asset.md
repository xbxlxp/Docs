# 资产管理

Bithumb Chain客户端 `xtar-CLI` 提供了资产管理模块，可以在命令行中通过 `asset` 命令实现：

- 查询账户资产信息
- 对账户资产进行控制，如执行 BT 转账，以及其他token的转账等

此外，你可以通过 `help` 命令获取信息获取模块的帮助信息 。

```shell
xtar-cli asset help
```

## 账户余额

要查询指定账户在所接入网络中的余额信息，使用`balance` 命令。

你可以在第一个终端启动本地测试网。

```shell
xtar-cli --testnet
```

然后在第二个终端中查询指定账户在本地测试网中的账户余额。

```shell
$ xtar-cli asset balance 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
XTAR balance:20
$ xtar-cli asset balance AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
XTAR balance:20
```

默认情况下查询的是指定账户的xtar账户余额，还可以使用`--asset-type` 命令指定查询其他token的余额，参数`--asset-type`后跟上其他token的地址。

```shell
$ xtar-cli asset balance --asset-type 004ed82971fabb900ec1d582380e0fe11daf83a9 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Token:004ed82971fabb900ec1d582380e0fe11daf83a9
Balance:99990
$ xtar-cli asset balance --asset-type 004ed82971fabb900ec1d582380e0fe11daf83a9 AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Token:004ed82971fabb900ec1d582380e0fe11daf83a9
Balance:99990
```

## 转账

在资产管理模块中，`transfer` 命令用于构造转账交易，可以转移 `BT` 或其他 `token` ，其包含以下参数：

- `--asset-type`：指定资产类型（`BT` 与 `其他token`）。
- `--from`：指定转账交易的扣款账户或账户索引。
- `--to` ：指定转账交易的收款账户或账户索引。
- `--amount`：指定转账金额。
- `--disable-wait-evtlog`：关闭`event log`日志。
- `--print-tx, -t` ：预执行并打印十六进制的交易内容，不发送到网络。

转移指定账户在所接入网络中的 `BT`。

```shell
$  xtar-cli asset transfer --from 0 --to 1 --amount 10
Transfer-out account
Please input password
Password:
Tranfer
From:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
To:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:10
TxHash:9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659
Waiting for eventLog...
EventLog:=>
{
   "height": 798,
   "tx_hash": "9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659",
   "error_code": 0,
   "gas_consumed": 0,
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "10"
         ]
      }
   ]
}
```

- 由于 `BT` 的（小数点后）精度是 0，因此如果输入浮点数，那么小数部分将会被丢弃。

```shell
$ xtar-cli info log 9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659
Transaction:9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659 =>
{
   "height": 798,
   "tx_hash": "9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659",
   "error_code": 0,
   "gas_consumed": 0,
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "10"
         ]
      }
   ]
}
```

也可以完成指定账户在所接入网络中的 `BT` 以外的其他 `token` 的转账。

```shell
$ xtar-cli asset transfer --asset-type 004ed82971fabb900ec1d582380e0fe11daf83a9 --from 1 --to 2 --amount 10
Transfer-out account
Please input password
Password:
Tranfer
From:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
To:AHd6d9SFVWsXPa7BPmz7hhBaZzKeWGMoPP
Amount:10
TxHash:1ae808b3fd9c1e65fc8f4dee87254dcf96af52d28ab56b4f9976cda2b807694f
Waiting for eventLog...
EventLog:=>
{
   "height": 1510,
   "tx_hash": "1ae808b3fd9c1e65fc8f4dee87254dcf96af52d28ab56b4f9976cda2b807694f",
   "error_code": 0,
   "gas_consumed": 0,
   "event_logs": [
      {
         "contract": "004ed82971fabb900ec1d582380e0fe11daf83a9",
         "data": [
            "transfer",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "AHd6d9SFVWsXPa7BPmz7hhBaZzKeWGMoPP",
            "10"
         ]
      }
   ]
}
```

## 授权转账

在资产管理模块中，`approve` 命令用于授权转账。

授权转移指定账户在所接入网络中的 `BT`。

```shell
$ xtar-cli asset balance 0
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
XTAR balance:999999980
$ xtar-cli asset approve --from 0 --to 1 --amount 20
Transfer-out account
Please input password
Password:
Approve
From:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
To:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:20
TxHash:eb9b270a10efb7458c4792ec3d98803392c3d76d16ef7cb8852b95d1bdb327f3
Waiting for eventLog...
EventLog:=>
{
   "height": 1910,
   "tx_hash": "eb9b270a10efb7458c4792ec3d98803392c3d76d16ef7cb8852b95d1bdb327f3",
   "error_code": 0,
   "gas_consumed": 0,
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "approve",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "20"
         ]
      }
   ]
}
```

## 授权查询

在资产管理模块中，`allowanceOf` 命令用于授权之后的转账查询。

```shell
$ xtar-cli asset allowanceOf --from 0 --to 1
AllowanceOf
From:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
To:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
XTAR balance:20
```

## 授权账户转账

在资产管理模块中，`transferFrom` 命令用于构造授权账户转账交易。

注意: 授权账户转账需由接收方签名并支付响应的系统资源。

```shell
$ xtar-cli asset balance 0
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
XTAR balance:999999980
$ xtar-cli asset transferFrom --from 0 --to 1 --amount 20
Transfer-in account
Please input password
Password:
TranferFrom
From:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
To:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Amount:20
TxHash:b4377825a6a83130d149630b15a75ff487146a3bddbd2c3819cbaf8d27450c53
Waiting for eventLog...
EventLog:=>
{
   "height": 1956,
   "tx_hash": "b4377825a6a83130d149630b15a75ff487146a3bddbd2c3819cbaf8d27450c53",
   "error_code": 0,
   "gas_consumed": 0,
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "20"
         ]
      }
   ]
}
$ xtar-cli asset balance 0
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
XTAR balance:999999960
```

## 常见问题

- 如何确认对账户资产的操作是否成功？

  所有对账户资产的操作，本质上都对应着交易的发送，在完成账户资产的操作后，你可以查看返回的交易哈希，确认操作是否成功。

  ```shell
  $ xtar-cli info log 9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659
  Transaction:9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659 =>
  {
     "height": 798,
     "tx_hash": "9bb36c6af86c6dc9a928626e3ef451d8af49d78b1b5be28e8a0ffefb6721c659",
     "error_code": 0,
     "gas_consumed": 0,
     "event_logs": [
        {
           "contract": "0000000000000000000000000000000000000001",
           "data": [
              "transfer",
              "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
              "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
              "10"
           ]
        }
     ]
  }
  ```

- Bithumb Chain 网络中所有的交易是免费的，但是需要花费系统资源，如带宽和CPU，如何获得带宽和CPU？

  只需要抵押 BT 就可以获得带宽和CPU。理论上系统的带宽和CPU是有限的，系统会根据个人抵押的 BT 跟系统抵押的比例适时进行分配，同时也会适时释放之前锁定的带宽和CPU。

- Bithumb Chain网络中的每笔交易需要消耗RAM，如何获得RAM?

  需要花费 BT 购买RAM，具体可参考 RAM Management。

- 如何在本地测试网中获取 `NET/CPU/RAM`？

  在第一次使用本地测试网时，你的默认账户会拥有所有的 `BT`，但是会有4个 `BT` 被用于购买资源，其中2个 `BT` 用于购买 `ram` (1个用于手续费，1个用于实际购买 `ram` )，1个用于抵押获得全网资源 `cpu`，1个用于抵押获得全网资源 `net`。

  ```shell
  $ xtar-cli asset balance 0
  Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
  XTAR balance:=>
  999999996
  ```
  
  同时，你的可使用的 `ram` 为 68719。
  
  ```shell
  $ xtar-cli ram balance 0
  Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
  Ram:=>
  {
     "Used": 0,
     "Reserved": 68719
  }
  ```
  