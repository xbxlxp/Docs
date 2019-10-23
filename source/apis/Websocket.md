Websocket
============
## ws.SubscribeBlock

订阅区块

**Parameters**
N/A

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.SubscribeBlock"
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.UnsubscribeBlock

取消订阅区块

**Parameters**
N/A

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.UnsubscribeBlock"
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.SubscribeTx

订阅交易

**Parameters**

1. `txHash` HexString - 交易的Hash值

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.SubscribeTx",
  "params":["953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca"]
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.UnsubscribeTx

取消订阅交易

**Parameters**

1. `txHash` HexString - 交易的Hash值

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.UnsubscribeTx",
  "params":["953e5d30edbe7ab65b5e6cd38b01fe8a460bbd8ecf961a1ded917ed58f493bca"]
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.SubscribeContract

订阅合约Event

**Parameters**

1. `contractAccount` String - 合约Base58地址/域名
2. `logName` String - EventLog名称

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.SubscribeContract",
  "params":["bithumbchain.system", "transfer"]
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.UnsubscribeContract

取消订阅合约Event

**Parameters**

1. `contractAccount` String - 合约Base58地址/域名
2. `logName` String - EventLog名称

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.UnsubscribeContract",
  "params":["bithumbchain.system", "transfer"]
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```

## ws.Heartbeat

发送心跳

**Parameters**
N/A

**Example**

```json
{
  "jsonrpc": "2.0", 
  "id": "1",
  "method": "ws.Heartbeat"
}
```

**Returns**

1. `result` bool - 成功/失败

**Example**

```json
{
  "jsonrpc":"2.0",
  "id":"1",
  "result":true,
  "error":{
    "code":0,
    "message":"ok"
    }
}
```