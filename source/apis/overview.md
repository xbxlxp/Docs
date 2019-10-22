概述
============
Bithumb Chain节点的 RPC 接口、WebSocket 接口遵循着一套公开的接口规范。

默认情况下，Websocket 接口使用 43027 端口，RPC 接口使用 43026 端口。

## 使用公开节点

通常情况下，开发者自己运行节点是极为不便的。因此，Bithumb Chain提供了 xGalaxy 公开测试网节点以及主网节点供开发者使用，它们均支持 RPC、WebSocket 调用，并使用默认的端口号。

xGalaxy 公开测试网第一个节点和主网第一个节点的 30088 端口支持  HTTPS 。

- 公开测试网节点

http://xgalaxy1.bithumb.network

http://xgalaxy2.bithumb.network

- 主网节点

http://dappnode1.bithumb.network

http://dappnode2.bithumb.network

如果你希望基于 xGalaxy 公开测试网进行开发，你可以在 [公开测试网BT申请](https://developer.bithumb.network/applyBT) 申请测试所需的 BT。

## 部署个人节点

根据开发者需要，我们也提供了搭建节点的方法。

[节点部署](https://github.com/bithumb-network/BithumbChain)