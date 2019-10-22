# 账户管理

Bithumb Chain链上的账户就是地址，跟其他区块链一样地址是整个系统的最小单位，上面存放了钱和其他资源。Bithumb Chain链上的账户分为`普通账户`和`合约账户`，`普通账户`就是单纯的地址，有公私钥；`合约账户`是由系统账户合约发行的标准化合约，有特定的方法集，符合合约账户的标准；合约账户不是必须存在的，仅在用户对账户权限有更细粒度的控制需求时可以特别申请使用。

每个合约账户都有三个默认权限，称为`owner`、`active`和`payer`。同时开发者们还可以提出自定义action授权。每个权限的使用都需要对应的公钥地址授权，必须有对应的私钥进行签署授权。

为了方便用户记忆，每个账户支持申请别名（也就是域名），一个地址可以绑定多个别名。具体请参照XNS管理。

Bithumb Chain客户端 `Bithumbchain-CLI` 提供了钱包管理模块，可以在命令行中通过 `wallet` 命令实现：

- 对钱包账户的增加、删除、查看、修改。
- 对钱包账户的导入导出。

此外，你可以通过 `help` 命令获取钱包管理模块的帮助信息 。

```shell
$ xtar-cli wallet help
NAME:
  xtar-cli wallet - Manage all keys in key store

USAGE:
  xtar-cli wallet [options] command [command options] [arguments...]
  
VERSION:
  0.0.1
  
COMMANDS:
  new     Create a new key
  list    Print all keys list exist in key store
  update  Update an existing key
  import  Import a private key into a new key
  help, h  Shows a list of commands or help for one command
  
MISC OPTIONS:
  --help, -h   show help
```

## 签名算法

Bithumb Chain客户端 Bithumbchain-CLI` 目前支持 `ECDSA` 签名算法。

要了解更多关于ECDSA 密钥曲线的信息，可以访问 [NIST Digital Signature Standard (FIPS 186-3)](https://csrc.nist.gov/csrc/media/publications/fips/186/3/archive/2009-06-25/documents/fips_186-3.pdf)。

## 创建账户

要创建账户，使用 `new` 命令：

```bash
$ xtar-cli wallet new
```

在添加账户的过程中，有输入密码和不输入密码两种模式，如果不想输入密码则需要使用 `--passwd <path>` 指定密码文件，也可以使用 `--lightkdf` 降低手机轻钱包的密钥导出级别。Keystore文件通过密码以加密形式存储在<DATADIR>/keystore。

- 每个 `Keystore` 文件都应该有一个默认账户，一般情况下是第一个添加的账户，可以使用 `list` 命令查看默认账户。

```bash
$ xtar-cli wallet list
```

- 默认账户不能被删除。

在账户管理模块中， `new` 命令所支持的选项如下表所示，也可以通过 `--help` 选项获取帮助信息。

```shell
$ xtar-cli wallet new --help
XTAR OPTIONS:
  --datadir <path>    Data <path> for block chan data and keystore (default: "Database")
  --keystore <path>   Key store <path>. (default = inside the data dir)

ACCOUNT OPTIONS:
  --passwd <path>     Password file <path> for non-interactive input.
  --lightkdf          Reduce key-derivation crypt level at mobile device
```


> - --passwd: 指定默认密码文件的路径；
> - --lightkdf: 指定降低手机轻钱包的密钥导出级别。
> - 同一个 `Keystore` 文件下，不能出现重复的账户标签。
> - 未设置账户标签的账户为空字符串。

## 查看账户

要查看 `Keystore` 文件中的账户列表，使用 `list` 命令：

```shell
$ xtar-cli wallet list
```

- 账户在钱包中的索引 `Index` 从 0 开始。
- `default` 对应的账户为默认账户。
- 客户端 `Bithumbchain-CLI` 支持通过索引 `Index`、账户地址 `Address` 以及非空账户标签 `Label` 来查找账户。

在账户管理模块中，`list` 命令所支持的选项如下表所示，也可以通过 `--help` 选项获取帮助信息。

```shell
$ xtar-cli wallet list --help
XTAR OPTIONS:
  --datadir <path>    Data <path> for block chan data and keystore (default: "Database")
  --keystore <path>   Key store <path>. (default = inside the data dir)
```

## 修改账户	

要对账户进行修改，使用`update` 命令：

```shell
xtar-cli wallet update
```

在账户管理模块中，`update` 命令所支持的选项如下表所示，也可以通过 `--help` 选项获取帮助信息。

```shell
xtar-cli wallet update help
XTAR OPTIONS:
  --datadir <path>    Data <path> for block chan data and keystore (default: "Database")
  --keystore <path>   Key store <path>. (default = inside the data dir)

ACCOUNT OPTIONS:
  --passwd <path>     Password file <path> for non-interactive input.
  --change-passwd     Change the password of a exist account
  --lightkdf          Reduce key-derivation crypt level at mobile device
```

> - --passwd: 指定默认密码文件的路径；
> - --change-passwd: 修改账户密码；
> - --lightkdf: 指定降低手机轻钱包的密钥导出级别；
> - 默认的钱包路径为 `.Database/keystore/*****.keystore`；
> - 同一个 `Keystore` 文件中，不能有两个相同的钱包标签。

## 导入账户

在账户管理模块中，`import` 命令用于将 `Keystore` 文件导入到钱包账户之中。

```shell
xtar-cli wallet import --passwd
```

请在导入完成后彻底清除设备中的 `keystore` 文件或将其置于安全的位置。

## 常见问题

- 什么是明文私钥？

  明文私钥是由（伪）随机数生成的，用来解锁对应钱包的一串字符。在交易场景下, 私钥用于生成交易所必须的签名，以证明资产的所有权。任何具有明文私钥的人，就能控制该私钥所对应的钱包账户。

- 什么是 `Keystore`？

  `Keystore` 允许你以加密的方式存储私钥。私钥被自定义密码所加密，一旦忘记密码, 就失去了对 `Keystore` 的使用权。因此，一定要妥善保管好 `Keystore` 以及密码。

- 什么是助记词？

  助记词是私钥的另一种表现形式。最早是由 `BIP39` 提案提出, 其目的是为了帮助用户记忆复杂的私钥字符串。助记词一般由 12、15、18、21 个单词构成, 这些单词都取自一个固定词库, 其生成顺序也是按照一定算法而来。
