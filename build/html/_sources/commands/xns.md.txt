# 域名管理

当你在使用区块链的时候肯定被很长的一长串地址所困惑，不可能记得住。为了方面记忆，Bithumb Chain域名管理帮你注册方便记忆的域名并绑定到地址，这样在做任何操作的时候只要记住域名即可。

域名管理提供了注册、设置主域名、修改状态和查询服务。你可以使用 `./bithumbchain-cli vdns help` 查看所有的域名管理命令。

```shell
xtar-cli xns help
```

## [域名注册](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=域名注册)

要注册域名，可以使用 `vdns register`.

**Note**: 域名仅能包含 `a~z1~9` 并且其长度仅能为12位，此种类型的域名免费。

```bash
$ xtar-cli xns register --xns-name=testtest8888 0
Address account
Please input password
Password:
Register XnsName
Address:XfgKWGv5LdYgJW6mg6qVDNeoFcPtdFRQG7
XnsName:testtest8888
TxHash:037873533a61cd4613f0459db347751a06ea79b4f2d7219364e42a209f13d80f
Waiting for eventLog...
EventLog:=>
{
   "height": 92,
   "tx_hash": "037873533a61cd4613f0459db347751a06ea79b4f2d7219364e42a209f13d80f",
   "error_code": 0,
   "net_usage": 213,
   "cpu_usage": 40,
   "ram_usage": [],
   "event_logs": [
      {
         "contract": "XagqqFetxiDb9wbartKDrXgnqLahx9xZhB",
         "data": [
            "registerName",
            "testtest8888",
            "XfgKWGv5LdYgJW6mg6qVDNeoFcPtdFRQG7",
            "1"
         ]
      }
   ]
}
```

## [查询域名](https://dev-docs.xtar.io/#/docs-cn/xtar-cli/10-wallet-manager?id=查询域名)	

查询域名对应的地址、状态可以使用 `vdns addressInfo` :

```shell
$ xtar-cli xns addressInfo testtest8888
XnsName:testtest8888=>
Address:XfgKWGv5LdYgJW6mg6qVDNeoFcPtdFRQG7
Status:non-transferable
```

