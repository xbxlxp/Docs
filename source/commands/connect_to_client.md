# 连接到Bithumb Chain

Bithumb Chain客户端提供了大量的调用接口，你可以通过 RPC、 Restful 或者 WebSocket 进行访问。

默认情况下，Restful 接口监听在 30088 端口，Websocket 接口监听在 43027 端口，RPC 接口监听在 43026 端口。

## 基于客户端进行调用

Bithumb Chain客户端提供了大量调用命令，这些命令默认会向本地启动的节点按照 JSON-RPC 协议发送调用命令。

一个简单的例子是分别在测试模式和主网同步模式查询账户余额。

1. 在第一个终端中开启测试模式：

   ```shell
   ./bithumbchain-cli 0
   ```

2. 在第二个终端中查询账户余额：

   ```shell
   ./bithumbchain-cli asset balance 1
   ```

3. 将第一个终端切换到主网同步模式：

   ```shell
   ./bithumbchain-cli
   ```

4. 在第二个终端中查询账户余额：

   ```shell
   ./bithumbchain-cli asset balance 1
   ```

## 基于 SDK 进行调用

Bithumb Chain提供了众多 SDK 供开发者使用，你可以参考下表选择自己熟悉的语言，更多关于 SDK 的信息你可以点击[这里](http://10.0.151.70/cn/sdks/overview.html)访问文档中心的 SDK 部分。

- [Github of Java SDK](https://github.com/bithumb-network/bithumb-chain-java-sdk)

- [Github of Typescript SDK](https://github.com/bithumb-network/bithumb-chain-ts-sdk)

- [Github of Python SDK](https://github.com/bithumb-network/bithumb-chain-py-sdk)

- [Github of Golang SDK](https://github.com/bithumb-network/bithumb-chain-go-sdk)

  

一般情况下，你只需要连接到 RPC、Restful 或 WebSocket 中的一个接口，并使用该接口与节点进行交互即可

关于函数库的更多信息可以在下列章节中找到：

- [Java SDK](https://bithumbchain.readthedocs.io/zh_CN/latest/sdks/java.html)
- [TS SDK](https://bithumbchain.readthedocs.io/zh_CN/latest/sdks/ts.html)
- [Python SDK](https://bithumbchain.readthedocs.io/zh_CN/latest/sdks/python.html)
- [Go SDK](https://bithumbchain.readthedocs.io/zh_CN/latest/sdks/go-sdk.html)

## 使用公开节点

通常情况下，开发者自己运行节点是极为不便的。因此，Bithumb Chain提供了 `testnet` 测试网节点以及主网节点供开发者使用，它们均支持 RPC、 Restful 以及 WebSockek 调用，并使用默认的端口号。

- 测试网节点
  - [http://xgalaxy1.bithumb.netwotk](http://xgalaxy1.bithumb.netwotk)
  - [http://xgalaxy2.bithumb.netwotk](http://xgalaxy2.bithumb.netwotk)
- 主网节点
  - [http://dappnode1.bithumb.netwotk](http://dappnode1.bithumb.netwotk)
  - [http://dappnode2.bithumb.netwotk](http://dappnode2.bithumb.netwotk)

如果你希望基于 `testnet` 测试网进行开发，你可以在[这里](https://developer.bithumbchain.io/applyResource)申请测试所需的 `NET` 、 `CPU` 和 `RAM` 。

