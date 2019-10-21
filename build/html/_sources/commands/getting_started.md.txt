# 快速开始

## 概述

Bithumb Chain价值网络客户端 `bithumbchain-cli` 是一个用 `Go` 语言编写的命令行应用程序，其主要功能如下：

- 运行和管理全节点
- 管理帐户密码
- 管理原生资产及发行Token
- 管理区块数据
- 显示区块链信息
- 构建、签署和发送交易
- 抵押 BT 获得资源、释放资源、赎回BT、查询资源等
- 域名注册、查询等



## 安装

### [下载安装包](https://dev-docs.bithumbchain.io/#/docs-cn/xtar-cli/01-install?id=下载安装包)

基于 Go 语言，我们提供了通用的Bithumb Chain客户端安装程序，你可以通过下载和使用源码编译快速获取：

- 点击 [这里](https://github.com/bithumb-chain/BithumbChain/releases) 下载Bithumb Chain客户端安装程序。

  

### [使用源码编译](https://dev-docs.bithumbchain.io/#/docs-cn/xtar-cli/01-install?id=使用源码编译)

此外，你也可以从源码编译Bithumb Chain客户端。

先决条件：Golang >= 1.9 (The Go Programming Language) 。

1. 获取源码：

   ```shell
   cd $GOPATH/src/github.com/bithumb-chain
   git clone https://github.com/bithumb-chain/BithumbChain.git
   ```

   or

   ```shell
   cd $GOPATH/src
   go get -u -v github.com/bithumb-chain/BithumbChain.git
   ```

2. 然后：

   ```shell
   cd $GOPATH/github.com/bithumb-chain/BithumbChain.git
   git checkout master
   ```

3. 编译源码：

   ```shell
   make clean
   make bithumbchain-cli
   ```

源码成功编译后会生成两个可执行程序：

- bithumbchain-cli：Bithumb Chain客户端。



## 快速开始

当你安装好Bithumb Chain的客户端后，便可以使用以下命令进行一些常用操作。

**查看帮助信息**

```shell
./bithumbchain-cli --help
```

**连接到主网**

```shell
./bithumbchain-cli
```

或者

```shell
./bithumbchain-cli 1
```

**连接到测试网**

```shell
./bithumbchain-cli 2
```

**启动测试模式**

在测试模式下，节点独立运行，默认的 `资源` 为 0。

```shell
./bithumbchain-cli 0
```

第一次启动测试模式之前，需要创建钱包文件。

```÷shell
./bithumbchain-cli wallet new
```