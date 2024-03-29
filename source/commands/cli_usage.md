# 命令行索引

星际价值网络客户端Bithumb Chain CLI提供了丰富的功能，在这一部分，我们将手把手地教你使用客户端。



## 命令

目前支持的命令如下表所示，你可以在需要的时候使用 `help` 命令在终端中查看。

```shell
xtar-cli help
```

```shell
COMMANDS:
  account   Manage accounts in key store
  asset     Manage xtar asset of account
  ram       Manage ram of account
  resource  Manage net&cpu resource of account
  contract  Deploy and invoke native or wasm contract
  token     Register, issue and destroy standard token via token contract
  tx        Utils methods for transaction
  info      Show information of blockchain
  utils     Utils methods for xtar
  help, h   Shows a list of commands or help for one command
```

## 参数

目前支持的命令行参数如下所示，你可以在需要的时候使用 `help` 命令在终端中查看。

```shell
XTAR OPTIONS:
  --config <path>                 Genesis config file <path>. If don't specified, using default genesis config instead
  --datadir <path>                Data <path> for block chan data and keystore (default: "Database")
  --log-level <level>             Default log output <level>(0~5). (0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace) (default: 3)
  --dis-color                     Disable terminal ANSI color. (default = false)
  
ACCOUNT OPTIONS:
  --passwd <path>                 Password file <path> for non-interactive input.
  --account <address>             <address> or index of account
  
TEST NET OPTIONS:
  --testnet                       Test net is a single node network with solo consensus for test purpose
  --block-inval value             Generate block interval, unit(s) (default: 5)
  
CONSENSUS OPTIONS:
  --consensus                     Start consensus module to participate in generate block
  --consensus-log-level <level>   Consensus log output <level>(0~5). (0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace) (default: 3)
  --max-tx-num <number>           Max transaction <number> in block (default: 10000)
  
P2P SERVER OPTIONS:
  --peer-port <port>              peer server listening <port> (default: 30088)
  
API OPTIONS:
  --rpc                           Enable rpc connection
  --rpc-addr <address>            HTTP-RPC server listening <address> (default: "localhost")
  --rpc-port <port>               HTTP-RPC server listening <port> (default: 43026)
  --rpc-log-level <level>         Rpc log output <level>(0~5). (0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace) (default: 3)
  --ws                            Enable websocket connection
  --ws-addr <address>             websocket server listening <address> (default: "localhost")
  --ws-port <port>                websocket server listening <port> (default: 43027)
  --ws-log-level <level>          websocket log output <level>(0~5). (0:Crit 1:Error 2:Warn 3:Info 4:Debug 5:Trace) (default: 3)
  
DEBUGGING OPTIONS:
  --pprof                         Start pprof HTTP server
  --pprof-port value              pprof HTTP server port (default: 0)
  
MISC OPTIONS:
  --help, -h                      show help
```

- 网络编号不同的节点之间无法直接互相连接形成共同的区块链网络
- 同一个网络所有节点的创世区块配置必须一致，否则会因为区块数据不兼容导致无法启动节点或同步区块数据
- 在命令行中输入的账户密码会被保存在系统的日志中，容易造成密码泄露，因此在生产环境中不建议使用 `--passwd` 参数