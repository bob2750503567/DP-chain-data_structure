# C++版 DP-chain的数据结构

## 共用的数据结构


### 区块头 `BlockHeaderbase`

| `BlockHeaderBase` | 描述 | 字段类型| 可选值 |
|:----:       |:----:  |:----:|:----:|
| `height`      | 高度    |`int32`|
| `time`        | 时间        |`int32`|
| `zoneIntra`   | 本分区链上前一个区块的hash值 | `uint256`|
| `zoneInter`   | 其它分区的链上的前一个区块的区块头的hash值| `uint256`|
| `prevBlockInter` | 其它分区的链上前一个区块的hash值| `uint256`|
| `prevBlockHash` | 本分区前一个区块的区块头hash值 | `uint256` |
| `publicKeyChar` | 本区块的公钥 | `[]byte` |
| `difficultyLimit` | 区块的难度值 | `uint256`|
| `nonce` | 本区块的nonce值 | `int32` |
| `host` | 节点的地址 | `[]byte` |




### 区块体 `BlockBody`

| `BlockBody` | 描述 | 字段类型| 可选值 |
|:----:       |:----:  |:----:|:----:|
| `signature`  | 区块头公钥对整个区块体hash值的签名 |  `[]byte` |
|`transactionsHash`| 区块体的hash值merkle树 |  `[]uint256` |
| `transactions` | 交易列表 | `[]TransactionBaseData`|
| `blockHeaderHash` | 区块体对应的区块头的hash值 | `uint256` |
| `zone` | 区块头所属分区 | `int32`|




### 交易 `TransactionBase`

| `TransactionBase` | 描述 | 字段类型| 可选值 |
|:----:       |:----:  |:----:|:----:|
| `signature` | 交易签名 |  `[]byte` |
| `transactionBaseData` | 交易内容打包 | `TransactionBaseData`|


交易打包 `TransactionBaseData`

|`TransactionBaseData`| 描述 | 字段类型| 可选值 |
|:----:       |:----:  |:----:|:----:|
|`sendIP` | 发送方ip | `int32` |
| `sendPort` | 发送方端口 | `int32` |
| `receiveIP` | 接收方ip | `int32` |
| `receivePort`| 接收方端口 | `int32` |

###  与其它节点通信类 `ConnectionBase`
|`ConnectionBase`| 描述 | 字段类型|  可选值 |
|:----:  | :----:  |:----:|:----:|
|`host` | 远机地址 | `string ` |
|`port` | 远机端口 | `string` |
| `zone`| 节点的分区号 | `int32` |
|`type` | 远机的类型 | `enum `|

### 通信信令 `Message`

|`Message`| 描述 | 字段类型|  可选值 |
|:----:  | :----:  |:----:|:----:|
|`messageVersion`|消息版本|`string`|
|`messageType`|消息种类|`string`|
|`messageBody`|消息内容|`string`|
## layer1的数据结构

### 配置参数  `configBase`

| `configBase` | 描述 | 字段类型 | 可选值 |
|:----:       |:----:  |:----:|:----:|
|`host` | 本机地址 | `string ` |
|`port` | 本机端口 | `string` |
| `zone`| 节点的分区号 | `int32` |
|`type` | 本机的类型 | `enum `|
|`dagMaintainer` | 本机连接的dag节点 | `ConnectionBase` |
|`blockRate`|区块生成速率|`int32`|
|`timespan` |区块难度调整间隔时间|`int32` |
|` difficultyLimit`|区块难度最低值|`uint256`|
|`transactionNum`|区块包含交易个数|`int32`|
|`publicKeyChar`|本机公钥| `string`|
|`privateKeyChar`|本机私钥|`string`|

### 区块维护 `BlockSercvice`

链的管理维护
| `BlockSercvice` | 描述 | 字段类型 |
|:----:       |:----:  |:----:|
|`headBlock` | 链上最新区块头 | ` BlockHeader` |
|`headHash` | 链上最新区块头的hash值 | ` uint256` |
|`chainBlocks` | 链的全部区块头，hash索引 | ` map` |
|`chainBlockBodys` | 链的全部区块体，hash索引 | ` map` |

### 可接收信令 

通过`messageType`区分信令

|name|描述|包裹字段类型|包裹字段类型描述|
|:----:       |:----:  |:----:|:----:|
|`connection`| 接收其他节点的连接请求| `Connection`|
|`blockHeader`| 接收其他节点的请求上链区块头| `BlockHeader`|
|`blockBody` | 接收其它节点请求上链的区块体| `BlockBody`|
|`getBlockHeader` |向本机请求在链上的区块头| `HashRequest`| 包裹分区号和hash值|
|`getblockBody`|向本机请求在链上的区块体| `HashRequest`| 包裹分区号和hash值|
|`getHeaderChain`|请求本机最新的链上区块头| `null`|
|`transactionData`|接收提交的交易|`TransactionBase`|


## layer2的数据结构

### 配置参数 

| `configBase` | 描述 | 字段类型 | 可选值 |
|:----:       |:----:  |:----:|:----:|
|`host` | 本机地址 | `string ` |
|`port` | 本机端口 | `string` |
| `zone`| 节点的分区号 | `int32` |
|`type` | 本机的类型 | `enum `|
|`neighborList` | 本机连接的dag节点 | `ConnectionBase` |
|`blockRate`|区块生成速率|`int32`|
|`timespan` |区块难度调整间隔时间|`int32` |
|` difficultyLimit`|区块难度最低值|`uint256`|
|`transactionNum`|区块包含交易个数|`int32`|
|`publicKeyChar`|本机公钥| `string`|
|`privateKeyChar`|本机私钥|`string`|

### 多链管理

单链数据结构
| `Chain` | 描述 | 字段类型 | 可选值 |
| :----:       |:----:  |:----:|:----:|
|`headBlock` | 链上最新区块头 | ` BlockHeader` |
|`headHash` | 链上最新区块头的hash值 | ` uint256` |
|`chainBlocks` | 链的全部区块头，hash索引 | ` map` |
|`chainBlockBodys` | 链的全部区块体，hash索引 | ` map` |
|`zone `|本链分区号| `int32`|

多链维护
| `ChainService` | 描述 | 字段类型 | 可选值 |
| :----:       |:----:  |:----:|:----:|
| `chainMap` | 多链分区索引| ` map` |

### 可接收信令
通过`messageType`区分信令

|name|描述|包裹字段类型|包裹字段类型描述|
|:----:       |:----:  |:----:|:----:|
|`connection`| 接收其他节点的连接请求| `Connection`|
|`blockHeader`| 接收其他节点的请求上链区块头| `BlockHeader`|
|`blockHeaders`| 接收其他节点的返回的上链全部区块头| `list<BlockHeader>`|
|`blockBody` | 接收其它节点请求上链的区块体| `BlockBody`|
|`getBlockHeader` |向本机请求在链上的区块头| `HashRequest`| 包裹分区号和hash值|
|`getblockBody`|向本机请求在链上的区块体| `HashRequest`| 包裹分区号和hash值|
|`getHeaderChain`|请求本机最新的链上区块头| `null`|
|`getHeaderChains`|请求节点对应分区整个链的区块头| `null`|

