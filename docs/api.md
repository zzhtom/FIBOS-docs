# API

## chainId

### FIBOS

**6aa7bd33b6b45192465afa3553dedb531acaaff8928cf64b70bd4c5e49b7ec6a**

### EOS

**aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906**

## fibos.js

### FIBOS Client

```javascript
const FIBOS = require("fibos.js");
const fibosClient = FIBOS({
    chainId: "cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f",
    httpEndpoint: "http://127.0.0.1:8888",
    keyProvider: private_key
});
```

### randomKeySync

```javascript
const FIBOS = require("fibos.js");
let prikey = FIBOS.modules.ecc.randomKeySync();
```

### privateToPublic

```javascript
const FIBOS = require("fibos.js");
let pubkey = FIBOS.modules.ecc.privateToPublic(prikey);
```

## FIBOS Client

### newaccountSync

> create account

```javascript
fibosClient.newaccountSync({
    creator: "eosio",
    name: "hello",
    owner: public_key,
    active: public_key
});
```

### setcodeSync

```javascript
let jsCode = fs.readTextFile("./hello/hello.js");
let wasm = fibosClient.compileCode(jsCode);
fibosClient.setcodeSync(contractName, 0, 0, wasm);
```

### getCodeSync

```javascript
fibosClient.getCodeSync(contractName, true);
```

### setabiSync

```javascript
let abi = JSON.parse(fs.readTextFile("./hello/hello.abi"));
fibosClient.setabiSync(contractName, abi);
```

### contractSync

```javascript
let ctx = fibosClient.contractSync(contractName);
let rs = ctx.hiSync("hello", {
    authorization: contractName
});
```

## BP

### regproducerSync

> Register BP

```javascript
let ctx = fibosClient.contractSync("eosio");
ctx.regproducerSync(producer_name, public_key, url, 1);
```

### delegatebwSync

> Delegate FO to get Net and CPU

```javascript
let ctx = fibosClient.contractSync("eosio");
let a = ctx.delegatebwSync({
	from: "buyer_name",
	receiver: "recevier_name",
	stake_net_quantity: "quality",
	stake_cpu_quantity: "quality",
	transfer: 0
});
```

### voteproducerSync

> Vote BP

```javascript
let ctx = fibosClient.contractSync("eosio");
ctx.voteproducerSync(producer_name, ", ["producer_name"]);
```

### claimrewardsSync

> Get reward

```javascript
fibosClient.claimrewardsSync("producer");
```

### transferSync

#### EOS => FIBOS

```javascript
let ctx = eosClient.contractSync("eosio.token");
ctx.transferSync(eosaccount, "fiboscouncil", "1.0000 EOS", fibosaccount, {
    authorization: eosaccount
});
```

#### FIBOS => EOS

```javascript
let ctx = fibosClient.contractSync("eosio.token");
ctx.transferSync(fibosaccount, "fiboscouncil", "1.0000 EOS", eosaccount, {
    authorization: fibosaccount
});
```

### exchangeSync

#### EOS => FO

```javascript
let ctx = fibosClient.contractSync("eosio.token");
ctx.exchangeSync(accountName, "10.0000 EOS@eosio", "0.0000 FO@eosio", "exchange EOS to FO", {
    authorization: accountName
});
```

#### FO => EOS

```javascript
let ctx = fibos_client.contractSync('eosio.token');
ctx.exchangeSync(accountName, '10.0000 FO@eosio', `0.0000 EOS@eosio`, 'exchange FO to EOS', {
    authorization: accountName
});
```

## Query

### getInfoSync

**/v1/chain/get_info**

```javascript
fibosClient.getInfoSync();
fibosClient.getInfo({}).then(result => console.log(result));
```

```json
{
    "server_version": "2ad41277", // 主网服务器版本
    "chain_id": "6aa7bd33b6b45192465afa3553dedb531acaaff8928cf64b70bd4c5e49b7ec6a", // 主网的区块链网络 ID。不同 ID 的节点无法相互连接。
    "head_block_num": 1493811, // 当前区块高度（区块号），第一个区块为 1，依次累加。
    "last_irreversible_block_num": 1493485, // 当前（最近的）不可逆区块高度
    "last_irreversible_block_id": "0016c9edb7eda05beebb9598fdca1357945c861147c24542e50d181d53a49978", // 不可逆区块 ID
    "head_block_id": "0016cb33d1a5668b5ee921d45d448f0726da66f217bfd0751594a149cdee68d1", // 当前（最近的）区块 ID
    "head_block_time": "2018-09-06T07:14:00.500", // 产生时间
    "head_block_producer": "fibos123comm", // 打包此区块的节点
    "virtual_block_cpu_limit": 200000000,
    "virtual_block_net_limit": 1048576000,
    "block_cpu_limit": 199900,
    "block_net_limit": 1048576,
    "server_version_string": "v1.2.2" // 主网服务器版本
}
```

### getAccountSync

**/v1/chain/get_account**

```javascript
fibosClient.getAccountSync(accountName);
```

```json
{
    "account_name": "xiangmafibos", // 账号名
    "head_block_num": 1883258,
    "head_block_time": "2018-09-08T13:57:19.000",
    "privileged": false, // 是否特权用户
    "last_code_update": "1970-01-01T00:00:00.000",
    "created": "2018-09-04T09:02:12.000", // 账号创建时间
    "ram_quota": 5475, // 账号拥有的内存配额，以字节为单位
    "net_weight": 1000,
    "cpu_weight": 1000,
    "net_limit": { // 账号拥有的网络资源，字节为单位
        "used": 0,
        "available": 358592,
        "max": 358592
    },
    "cpu_limit": { // 账号拥有的计算资源，字节为单位
        "used": 0,
        "available": 46683,
        "max": 46683
    },
    "ram_usage": 2996, // 账号内存使用量，以字节为单位
    "permissions": [ // 账号权限
        {
            "perm_name": "active",
            "parent": "owner",
            "required_auth": {
            "threshold": 1,
            "keys": [
                {
                    "key": "FO6K6XHiRqjoM1x3NPMpCeq5DQU2JoRb8QwJVjNdY7Anc52UJgqX",
                    "weight": 1
                }
            ],
            "accounts": [],
            "waits": []
            }
        },
        {
            "perm_name": "owner",
            "parent": "",
            "required_auth": {
            "threshold": 1,
            "keys": [
                {
                    "key": "FO6K6XHiRqjoM1x3NPMpCeq5DQU2JoRb8QwJVjNdY7Anc52UJgqX",
                    "weight": 1
                }
            ],
            "accounts": [],
            "waits": []
            }
        }
    ],
    "total_resources": { // 账号拥有的总资产
        "owner": "xiangmafibos",
        "net_weight": "0.1000 FO",
        "cpu_weight": "0.1000 FO",
        "ram_bytes": 4075
    },
    "self_delegated_bandwidth": null, // 自抵押(网络和计算)带宽信息
    "refund_request": null, // 赎回请求
    "voter_info": null // 投票信息
}
```

### getBlockSync

**/v1/chain/get_block**

```javascript
fibosClient.getBlockSync(1);
```

### getProducersSync

**/v1/chain/get_producers**

```javascript
fibosClient.getProducersSync(true, "", 5);
```

```json
{
    "rows": [
        {
            "owner": "fibosmoziben", // 拥有者
            "total_votes": "367632706444448448.00000000000000000", // 投票数
            "producer_key": "FO72kzUjpkrpRXwq36iMovXbAXNU59buUQfUoTxe8pEDMhp2G8cD", // 区块生产者公钥
            "is_active": 1, // 是否活动的
            "url": "http://www.mocapital.top/", // 网址
            "unpaid_blocks": 20976, // 尚未领取奖励的区块
            "last_claim_time": "1536284750000000", // 最近认领奖励时间
            "location": 1 // 位置
        },
        {
            "owner": "plasmatfibos",
            "total_votes": "346268768760910784.00000000000000000",
            "producer_key": "FO8Tt6fYBFYiyUjTsuinLgL5nxhAAa3sx5vVTkC1FUx3xvas2WSZ",
            "is_active": 1,
            "url": "https://www.fiboso.com",
            "unpaid_blocks": 84,
            "last_claim_time": "1536504215000000",
            "location": 1
        },
        {
            "owner": "fibos123comm",
            "total_votes": "343270297819766144.00000000000000000",
            "producer_key": "FO6MzV92DgYjwDa7K3rtc28dPhGt2Gy8oUoHjESUq4gBx63v8num",
            "is_active": 1,
            "url": "https://www.fibos123.com",
            "unpaid_blocks": 7523,
            "last_claim_time": "1536426172500000",
            "location": 1
        },
        {
            "owner": "shaoshaoshao",
            "total_votes": "336672249083071040.00000000000000000",
            "producer_key": "FO6ruGiSvfspqBiUkx9REjsJKZon6AsUcUPv9kDFfWRHgeUsYXya",
            "is_active": 1,
            "url": "http://shuimeiren.io",
            "unpaid_blocks": 2136,
            "last_claim_time": "1536482668000000",
            "location": 1
        },
        {
            "owner": "chaindoctor1",
            "total_votes": "328642388640282368.00000000000000000",
            "producer_key": "FO5jnYK8XU226Mis57TQni7Et8LEjzjr165f3143pPQdA6bKQgEq",
            "is_active": 1,
            "url": "http://www.fodoctor.com",
            "unpaid_blocks": 9423,
            "last_claim_time": "1536406122500000",
            "location": 1
        }
    ],
    "total_producer_vote_weight": "7896774536012429312.00000000000000000", // 总投票权重值
    "more": "fibscandotio" // 下一个区块生产者，下次请求时可用它作为 lower_bound 参数的值
}
```

### getTableRowsSync

```javascript
fibosClient.getTableRowsSync(true, config.testContract.name, config.testContract.sender, "todos"));
```
