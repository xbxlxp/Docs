# 资源管理

Bithumb Chain客户端 `bithumbchain-cli` 提供了资源管理模块，可以在命令行中通过 `resource` 命令实现：

- 用 `BT` 抵押获取带宽/ `cpu` 
- 取消抵押 `BT` 释放带宽/ `cpu` 
- 当取消抵押释放完成3天之后，退回抵押的 `BT` 到账户
- 查询抵押账户的信息

此外，你可以通过 `help` 命令获取信息获取模块的帮助信息 。

```shell
xtar-cli resource help
```

## 抵押获取带宽/CPU

要获取带宽/CPU，可使用 `delegate` 命令来抵押 `BT` 获取。
- `--delegator`：指定抵押者资产地址或索引。
- `--receiver`：指定获取带宽/CPU接受者地址。
- `--net-amount` ：指定用于抵押获取带宽的 `BT` 总量。
- `--cpu-amount`：指定用于抵押获取CPU的 `BT` 总量。
- `--disable-wait-evtlog`：关闭`event log`日志。
- `--print-tx, -t` ：预执行并打印十六进制的交易内容，不发送到网络。

你可以在第一个终端启动本地测试网。

```shell
xtar-cli --testnet
```

然后在第二个终端中抵押本地测试网中账户的 `BT` 来获取带宽/CPU。

```shell
$ xtar-cli resource delegate --delegator=0 --receiver=1 --net-amount=10 --cpu-amount=10
Resource delegator account
Please input password
Password:
DelegateResource
Delegator:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
Receiver:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
NetAmount:10
CpuAmount:10
TxHash:6ffab2168e5114a30cc4b08ca426c572b8cfaef7f926ff29ff9e75fc96a86f6c
Waiting for eventLog...
EventLog:=>
{
   "height": 1214,
   "tx_hash": "6ffab2168e5114a30cc4b08ca426c572b8cfaef7f926ff29ff9e75fc96a86f6c",
   "error_code": 0,
   "net_usage": 216,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "ram_usage": "126"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000001",
         "data": [
            "transfer",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AFmseVrdL9f9oyCzZefL9tG6UbviEH9ugK",
            "20"
         ]
      },
      {
         "contract": "0000000000000000000000000000000000000007",
         "data": [
            "delegate",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "10",
            "10",
            "false"
         ]
      }
   ]
}
```

## 抵押信息查询

在资源管理模块中，`delegateInfo` 命令用于查询抵押信息。

```shell
$  xtar-cli resource delegateInfo 0
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
DelegateInfo:=>
{
   "TotalStake": 22,
   "DelegateList": [
      {
         "Receiver": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "NetAmount": 1,
         "CpuAmount": 1
      },
      {
         "Receiver": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
         "NetAmount": 10,
         "CpuAmount": 10
      }
   ]
}
```

## 取消抵押

要取消抵押，可使用 `undelegate` 命令来释放抵押的 `xtar` 。

```shell
$ xtar-cli resource undelegate --delegator=0 --receiver=1 --net-amount=5 --cpu-amount=10
Resource delegator account
Please input password
Password:
UndelegateResource
Delegator:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
Receiver:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
NetAmount:5
CpuAmount:10
TxHash:13369a003515fed63814e986bb5f49517a218d257306ddc9d07e462b284c270f
Waiting for eventLog...
EventLog:=>
{
   "height": 1431,
   "tx_hash": "13369a003515fed63814e986bb5f49517a218d257306ddc9d07e462b284c270f",
   "error_code": 0,
   "net_usage": 216,
   "cpu_usage": 40,
   "ram_usage": [
      {
         "address": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "ram_usage": "48"
      }
   ],
   "event_logs": [
      {
         "contract": "0000000000000000000000000000000000000007",
         "data": [
            "unDelegate",
            "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
            "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
            "5",
            "10"
         ]
      }
   ]
}
$ xtar-cli resource delegateInfo 0
Address:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
DelegateInfo:=>
{
   "TotalStake": 7,
   "DelegateList": [
      {
         "Receiver": "AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn",
         "NetAmount": 1,
         "CpuAmount": 1
      },
      {
         "Receiver": "AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9",
         "NetAmount": 5,
         "CpuAmount": 0
      }
   ]
}
```

## 退回抵押

取消抵押之后，一般系统默认3天之后可以提取抵押的 `xtar` ，`refund` 命令用于获取抵押的 `xtar`。

```shell
$ xtar-cli resource refund --delegator=0
Resource delegator account
Please input password
Password:
Refund
Delegator:AQihuq7oRHfdTH4f3dfKApa357zM4UYmnn
TxHash:2f835f434c084e43443fa4b5ef36ff4018a1f84280d969cc5d8fea7f05e1d582
Waiting for eventLog...
EventLog:=>
{
   "height": 1525,
   "tx_hash": "2f835f434c084e43443fa4b5ef36ff4018a1f84280d969cc5d8fea7f05e1d582",
   "error_code": 2009,
   "net_usage": 200,
   "cpu_usage": 40,
   "ram_usage": null,
   "event_logs": null
}
```

## 查询可用带宽

抵押过程总就能产生可用的带宽，`getNet` 命令用于获取抵押产生的可用带宽。

```shell
$ xtar-cli resource getNet 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Net:=>
{
   "NetAmount": 5,
   "TotalNet": 15099494400000,
   "NetUsage": 0,
   "UsableNet": 15099494400000
}
```

## 查询可用CPU

抵押过程中就能产生可用的CPU，`getCpu` 命令用于获取抵押产生的可用CPU。

```shell
$ xtar-cli resource getCpu 1
Address:AKVhDYofgsN9aoYKSev1uPMAW64NxYeRr9
Cpu:=>
{
   "CpuAmount": 0,
   "TotalCpu": 0,
   "CpuUsage": 0,
   "UsableCpu": 0
}
```

