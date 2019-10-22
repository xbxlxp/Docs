# 节点部署

在Bithumb Chain的节点模型中，节点分为共识节点、候选共识节点、同步节点和图书馆节点。

- 共识节点节点是所有用户投票所产生的，投票的前x名为共识节点，参与共识以及出块流程，分享系统收益。
- 候选共识节点是在投票的前100名，候选节点并不参与共识流程，但是也参与系统收益分成。
- 同步节点在网络中参与区块同步。
- 图书馆节点保存系统最全的交易以及区块信息，不会做区块裁剪，并开放给外部查询。

## 部署共识节点

默认情况下，客户端 `Bithumb Chain CLI` 不会启动共识模块，需要通过 `--consensus` 选项来开启共识。

```shell
xtar-cli  --consensus
```

默认情况下，输出的共识日志级别为3-info，可以使用`--consensus-log-level` 来调整日志级别(0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace)。

```shell
xtar-cli  --consensus-log-level <level>
```

默认情况下，单个区块包含的最大交易是10000，可以使用`max-tx-num` 来调整交易数。

```shell
xtar-cli  --max-tx-num <number>
```

客户端 `Bithumb Chain CLI` 不会启动 RPC 服务器，启动RPC服务可能会存在安全问题。因此，如果没有特殊要求，推荐使用关闭 `RPC` 模块。可以使用`--rpc` 开启 RPC 服务。

```shell
xtar-cli  --rpc
```

可以使用`--rpc-addr` 指定 RPC 服务器地址，默认：localhost。

```shell
xtar-cli  --rpc-addr <address>
```

可以使用`--rpc-port` 指定 RPC 服务器端口，默认：43026。

```shell
xtar-cli  --rpc-port <port>
```

客户端 `Bithumb Chain CLI` 不会启动 WebSocket 服务器，启动WebSocket 服务可能会存在安全问题。因此，如果没有特殊要求，推荐使用关闭 `WebSocket ` 模块。可以使用`--ws` 开启 WebSocket  服务。

```shell
xtar-cli  --ws
```

可以使用`--ws-addr` 指定 WebSocket 服务器地址，默认：localhost。

```shell
xtar-cli  --ws-addr <address>
```

可以使用`--ws-port` 指定 WebSocket 服务器端口，默认：43027。

```shell
xtar-cli  --ws-port <port>
```

因此，我们推荐共识节点使用的启动参数如下：

```shell
xtar-cli --consensus
```

## 部署同步节点

使用以下命令启动同步节点。

- 主网

  ```shell
  xtar-cli
  ```

- 测试网

  ```shell
  xtar-cli 1
  ```

- 测试模式

  测试模式用于搭建开发测试环境，为单节点网络。

  ```shell
  xtar-cli 0
  ```
  
  可以使用`--block-inval` 指定出块时间间隔（单位是秒），默认是5秒。
  
  ```shell
  xtar-cli  --block-inval value
  ```

在测试模式下，共识模块、RPC 模块、Restful 模块以及 WebSocket 模块会同时开启。

## 常见问题

- 如何为节点指定创世区块？

  使用 `--config` 选项指定创世区块的配置文件。

- 如何为节点指定区区块链数据及keystore文件？

  使用 `--datadir` 选项。

- 如何为节点指定账户？

  使用 `--account` 选项。

- 如何修改整体日志输出级别？

  使用 `--log-level <level>` 参数来修改。(0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace)

- 如何关闭终端ANSI颜色？

  使用 `--dis-color` 参数来修改。