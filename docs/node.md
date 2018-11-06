# Node Config

## Example

``` javascript
let fibos = require("fibos");

fibos.load("chain");
fibos.load("chain_api");
fibos.load("history_api");
fibos.load("producer", {
	"producer-name": "eosio",
	"enable-stale-production": true
});
fibos.load("http", {
	"http-server-address": "0.0.0.0:8888"
});
fibos.load("net", {
	"p2p-listen-endpoint": "0.0.0.0:9876"
});
fibos.config_dir = "fibos_config_dir/";
fibos.data_dir = "fibos_data_dir/";
fibos.enableJSContract = true;
fibos.start();
```

## Description

### Methods

#### load

``` javascript tab="Load Plugin"
static fibos.load(String name, Object cfg = {});
```

``` javascript tab="Load Attribute"
static fibos.load(Object cfgs);
```

#### start

```javascript
static fibos.start();
```

#### stop

```javascript
static fibos.stop();
```

### Plugins

#### http

| key 					   	  	   | type    | describe 		  	 | default value     |
| -------------------------------- | ------- | --------------------- | ----------------- |
| http-server-address      	  	   | String  | 本地 Http 服务地址		 | 127.0.0.1:8888    |
| https-server-address     	  	   | String  | 本地 Https 服务地址     | - 			     |
| max-body-size     	   	  	   | Number  | RPC 请求允许最大字节 	 | 1024 * 1024 bytes |
| verbose-http-errors	   	  	   | Boolean | 显示 Http 返回的错误日志 | false 			 |
| http-validate-host		  	   | Boolean | 验证 Http 请求 Host	 | true 			 |
| [access-control-allow-origin][1] | String  | - 					 | -	      		 |

[1]: #access-control-allow-origin

##### access-control-allow-origin

!!! tip "access-control-allow-origin"

	```javascript
	// 对每个请求返回特殊的 Access-Control-Allow-Origin
	fibos.load("http", {
		"http-server-address": "0.0.0.0:8870",
		"access-control-allow-origin": "*"
	});
	```

#### chain

| key 					 	   | type    | describe 								     | default value   |
| ---------------------------- | ------- | --------------------------------------------- | --------------- |
| [genesis-json][2] 	 	   | String  | 指定创世块数据路径 							     | - 			   |
| genesis-timestamp		 	   | Number  | 覆盖创世块中的初试时间戳 					     | - 			   |
| print-genesis-json	 	   | Boolean | 是否打印创世数据 							     | false   		   |
| hard-replay-blockchain 	   | Boolean | 是否清除状态数据，然后从区块日志中回滚尽可能多的数据   | false 		   |
| delete-all-blocks		 	   | Boolean | 是否删除所有的状态数据和区块数据	 			     | false 		   |
| fix-reversible-blocks  	   | Boolean | 是否将数据恢复到不可逆高度	 				     | false（无法使用） |
| replay-blockchain 	 	   | Boolean | 是否清除状态数据然后回滚所有数据 				     | false（无法使用） |
| truncate-at-block 	 	   | Boolean | 停止出块，并在该区块高度回滚		 			     | 0（无法使用） 	   |
| blocks-dir 				   | String  | 区块数据存放地址 							   	 | blocks 		   |
| abi-serializer-max-time-ms   | Number  | 覆盖默认 ABI 序列化允许的最大时间 			   	 | 15 * 1000 ms    |
| chain-state-db-size-mb	   | Number  | 区块数据存放地址							   	 | 1024 MB 		   |
| chain-state-db-guard-size-mb | Number  | 当区块数据库中剩余的数据小于此大小时可以安全地关闭节点 | 128 MB 		   |
| reversible-blocks-db-size-mb | Number  | 最大能够回滚的数据量	 							 | 340 MB	 	   |
| chain-state-db-size-mb	   | String  | 区块数据存放地址 								 | 1024 MB	 	   |
| contracts-console		 	   | Boolean | 是否打印合约输出	 							 | false	 	   |
| read-mode		 			   | String  | -	 										 | speculative	   |

[2]: #genesis-json

##### genesis-json

!!! tip "genesis-json"

	```javascript
	fibos.load("chain", {
		"chain-state-db-size-mb": 8 * 1024,
		"genesis-json": "genesis.json"
	});
	```

#### chain_api

``` bash tab="get_info"
// 获取与节点相关的最新信息
curl --request POST \
  --url http://127.0.0.1:8888/v1/chain/get_info
```

``` bash tab="get_block"
// 获取一个块的信息
curl --request POST \
  --url http://127.0.0.1:8888/v1/chain/get_block \
  -d '{"block_num_or_id": 1}'
```

``` bash tab="get_account"
// 获取账户的信息
curl --request POST \
  --url http://127.0.0.1:8888/v1/chain/get_account \
  -d '{"account_name": "eosio"}'
```

``` bash tab="get_code"
// 获取智能合约代码
curl --request POST \
  --url http://127.0.0.1:8888/v1/chain/get_code \
  -d '{"account_name": "fuckfuckfuck"}'
```

#### net

| key 					    | type    | describe 				  	| default value       		  |
| ------------------------- | ------- | --------------------------- | --------------------------- |
| p2p-listen-endpoint 		| String  | 监听 p2p 连接的地址和端口		| 0.0.0.0:9876	      		  |
| p2p-server-addrsss     	| String  | 提供给其它节点 p2p 服务地址		| p2p-listen-endpoint 		  |
| [p2p-peer-address][3]   	| Array   | 公共的p2p 对等节点地址 		| -					  		  |
| p2p-max-nodes-per-host	| Number  | 单个 IP 能够连接的最大客户端数量 | 1 				  		  |
| [allowed-connection][4]	| String  | 允许连接 						| any 			  			  |
| peer-private-key	 		| Array   | 一个 公钥、私钥组成的数组	 	| - 			      	  	  |
| max-clients		 		| Array   | 允许连接客户端的最大数量	 	| 25（0 无限制）	      	  	  |
| connection-cleanup-period | Number  | 清除不可用链接周期 			| 30 s 			      	  	  |
| network-version-match		| Boolean | 是否需要相同版本的网络 			| false			      	  	  |
| peer-log-format			| String  | 节点日志格式化	 				| `[${name} ${_ip}:${_port}]` |

[3]: #p2p-peer-address

##### p2p-peer-address

!!! tip "p2p-peer-address"

	```json
	[
		"se-p2p.fibos.io:9870",
		"sl-p2p.fibos.io:9870",
		"to-p2p.fibos.io:9870",
		"ca-p2p.fibos.io:9870",
		"ln-p2p.fibos.io:9870",
		"va-p2p.fibos.io:9870"
	]
	```

[4]: #allowed-connection

##### allowed-connection

!!! tip "allowed-connection"

	```json
	["any", "producers", "specified", "none"]
	```

	+ specified: 则必须至少指定一次对等密钥。
	+ producers: 则不需要对等密钥。

#### producer

| key 					   	 | type    | describe 		    	  | default value |
| -------------------------- | ------- | ------------------------ | ------------- |
| max-transaction-time	     | Number  | 事务最大超时时间			  | 30 s		  |
| greylist-account	     	 | -  	   | 无法使用 CPU 和 NET 的账号  | - 			  |
| enable-stale-production	 | Boolean | 启用产生区块,即使区块是静止的 | - 			  |
| max-irreversible-block-age | Number  | 最大的不可逆块时间	 	  | -1 (> 1) 	  |
| producer-name			  	 | String  | 控制节点出块的账户名		  | eosio (可多参) |
| [private-key][5] 			 | -       | 签名程序的公钥、私钥 		  | (可多参) 	  |

[5]: #private-key

##### private-key

!!! tip "private-key"

	```javascript
	JSON.stringify(["active_publickey", "active_privateKey"]);
	```

#### history

#### history_api

#### mongo_db

| key 	     	   | type    | describe    | default value |
| ---------------- | ------- | ----------- | ------------- |
| [mongodb-uri][6] | String  | mongodb URL | - 			   |

[6]: #mongodb-uri

##### mongodb-uri

!!! tip "mongodb-uri"

	```javascript
	fibos.load("mongo_db", {
	    "mongodb-uri": "mongodb://localhost:27017/eosmain"
	});
	```

#### bnet

| key 					   | type    | describe 		    	  								   | default value |
| ------------------------ | ------- | ----------------------------------------------------------- | ------------- |
| bnet-endpoint		       | String  | 所监听的传入链接的端点				  						   | 0.0.0.0:4321  |
| bnet-follow-irreversible | Boolean | 是否只接受从其他端点的不可逆的块	 							   | false 		   |
| bnet-threads		 	   | Number  | 用于处理网络消息的线程数	 									   | - 			   |
| bnet-connect	 		   | -  	 | 其他节点的远程端点连接，根据需要使用多个 bnet-connect 选项来组成网络 | - 			   |
| bnet-no-trx			   | Boolean | 这个 peer 请求其他节点没有 pending 的 transactions 			   | false 		   |

### Attributes

| key 			   | type    | describe   | default value |
| ---------------- | ------- | ---------- | ------------- |
| config_dir	   | String  | 配置存放目录 | - 	  		  |
| data_dir	   	   | String  | 数据存放目录 | - 	  		  |
| pubkey_prefix    | String  | 公钥前缀    | FO    		  |
| enableJSContract | Boolean | 智能合约状态 | false  		  |

## genesis.json

```json
{
    "initial_timestamp": "2018-08-28T00:00:00.000",
    "initial_key": "FO6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
    "initial_configuration": {
        "max_block_net_usage": 1048576,
        "target_block_net_usage_pct": 1000,
        "max_transaction_net_usage": 524288,
        "base_per_transaction_net_usage": 12,
        "net_usage_leeway": 500,
        "context_free_discount_net_usage_num": 20,
        "context_free_discount_net_usage_den": 100,
        "max_block_cpu_usage": 200000,
        "target_block_cpu_usage_pct": 1000,
        "max_transaction_cpu_usage": 150000,
        "min_transaction_cpu_usage": 100,
        "max_transaction_lifetime": 3600,
        "deferred_trx_expiration_window": 600,
        "max_transaction_delay": 3888000,
        "max_inline_action_size": 4096,
        "max_inline_action_depth": 4,
        "max_authority_depth": 6
    },
    "initial_chain_id": "6aa7bd33b6b45192465afa3553dedb531acaaff8928cf64b70bd4c5e49b7ec6a"
}
```
