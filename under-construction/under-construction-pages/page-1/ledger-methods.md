# LEDGER METHODS

### `Methods`

The following methods are currently supported by this API



[`get/block`](ledger-methods.md#get-block)\
[`get/blockhash`](ledger-methods.md#get-blockhash)\
`get/info`\
`get/transaction`\
`list/blocks`\
`list/transactions`\
`get/transaction`\
`submit/transaction`\
`void/transaction`\
`sync/headers`

## `get/block`

Retrieves block data for the given block hash or height.

#### Endpoint:

`/ledger/get/block`

{% swagger method="post" path="/ledger/get/block" baseUrl="http://api.nexus-interactions.io:8080" summary="Get blockdata" %}
{% swagger-description %}
Retrieves block data for the given block hash or height.
{% endswagger-description %}

{% swagger-parameter in="body" name="hash" required="false" %}
The block hash to retrieve the block data for
{% endswagger-parameter %}

{% swagger-parameter in="body" name="height" required="false" %}
The block height to retrieve the block data for
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" required="false" %}
Optional, determines how much transaction data to include in the response. Supported values are :

`none` : no transaction data

`default` : hash

`summary` : type, version, sequence, timestamp, and contracts.

`detail` : genesis, nexthash, prevhash, pubkey and sign
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="blockdata" %}
```json
{
"result": {
"hash": "f175d2e4951db8046939807085f8841ef2cf65141bd0ef7c356edb7a9ffae61c95b1c8a4049af9202c3ef7a198c47ac5df7db2ed750c7890e3d240c45eabdb7f10162e8843c44dea582c5042fb9ef6b003ab719912faa6bfe4c822f35aeb7252d02cb0e880eb5234e5f621b8ea917432f907731eb4e478440df981fee2983ac8",
"proofhash": "29c7de12ec3ecb4135b728c9c12113604810d661afdeb0e3ebe127bbafa70d9dbc10afae9366644cec230090c26536ff61aed503ae8497ca652ccfac56f7a427c04ff9d7a19f0d50299defe1a6a662887dad2453bca0c937f775704a808c5546df3eadc590305714e2367ef4f65f29fce05f7d9da8f13114ee508a9d12ee1aa1",
"size": 808,
"height": 4187405,
"channel": 1,
"version": 8,
"merkleroot": "01583bbdfceabc1bbf83a723cb9f2d81090b9098502f059ed0ade6d0da6b25596fd615f2eb69e886f0f32706941b2f4cedd1c255daa3876a5ce90be130b5e16c",
"time": "2021-12-14 07:41:08 UTC",
"nonce": 11329506036253405000,
"bits": "04c28a57",
"difficulty": 7.9858263,
"mint": 2.206912,
"previousblockhash": "e266a0b4013de0f88c4acf73abcbc7b25d3c044919d8360664d2c07c640984e11788d69a112ba4e9a5cca0ce6c3aab706b7df51505090f044b466ec2087bb6c904c8ad1da4f8d02cd73454c3bfa55a62ebb57a66a616aee87044a13edb36b826a556d52dada3899b215e6ef3ae02c506b754660ad3cab4984d7704d1028e3588",
"nextblockhash": "20c45a79a65f8ed40c35455ecc70952591d3f661d1ca577f549ae256482efe6d5225b81cfd1a740ce8bd5d5032cb3ee8e33c3886b9b04ba3ea98631744c9ac155d31ec419b1642965e2e76e3639f16a53ae0c4d710f0edf04eafc2989d593bf8fb28c312f524fa7bb697446bc4db59cc4ee7e8d2abde5b109762043f30f0dcd3",
"tx": [
{
"txid": "01583bbdfceabc1bbf83a723cb9f2d81090b9098502f059ed0ade6d0da6b25596fd615f2eb69e886f0f32706941b2f4cedd1c255daa3876a5ce90be130b5e16c"
}
]
}
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// ledger/get/block
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  // hash: "256 chars hash" // optional 
  height: 4000000,
  // verbose: "detail", //optional or "summary","none"
}
fetch(`${SERVER_URL}/ledger/get/block`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # "hash": "256 chars hash" # optional
    "height": 4000000,
    # "verbose": "detail", #optional or "summary","none"
}
response = requests.post(f"{SERVER_URL}/ledger/get/block", json=data)
print(response.json()
```
{% endtab %}
{% endtabs %}

#### Parameters:

`hash` : The block hash to retrieve the block data for.

`height` : The block height to retrieve the block data for.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `none` : no transaction data
* `default` : hash
* `summary` : type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

{% hint style="info" %}
Either the hash or the height needs to be supplied, but not both. Retrieving block data by height is only allowed if the daemon has been configured with `indexheight=1`
{% endhint %}

#### Return value JSON object:

```
{
    "hash": "7b68f0011baa6e3d3fa77c6941d8dfed92b3263630a44a109555a5d283239f7a7b8463dbdc3b93a7a90e75190bd1df44e375e461c88ff779ab59979fc8b4690d149f08964812b0e059030d01e6f8ffb1c059cd7423ed2d7fc9570e98206ca30313d28a72bab07832584faba5ee861bd9f3f00d25c12690052974757bb0febed0",
    "proofhash": "8efae9f99be3ceb91be886365ba1c38ded9fa8c0afd70223074376bb7164cb808bde38c3976ef324b2ee830fae2af601f6e3235a3e0eb7dc682cb8cc1048da250e068f30ed4d69b417182759c2a11989df04f7df94728de9707ac4b359ba3e99c63e112e0df321089f6c6d98614bc8e1519f7894383a1dfb46eb9a3ab2442be9",
    "size": 861,
    "height": 2,
    "channel": 3,
    "version": 8,
    "merkleroot": "d11d74bc3e82f6e157f8da5a61594def1706f9c15adccd2c3d95843f2d2b1d6478c956805a9f81d50ad503cea90081eb4dc97b77f3ae7e5ecc6cd650518e9882",
    "timestamp": 1654281061,
    "date": "2022-06-03 18:31:01 UTC",
    "nonce": 1,
    "bits": "7e7fffff",
    "difficulty": 31.999515,
    "mint": 0.0,
    "previousblockhash": "aae806d080e8ce32d14aded37987e6969d791414328fdde96a510630727c94b0588c0ba479f8f6afe0f90a6fdf295b0a61815774b237d70c51adfbf1ac640d662802541f6125c64d43566d1347792b5f45c0ac868ac988742b9a98fb0adbc4ba76540a3538b9acce2e8ee987079ff05bdbc57d6e4c3ada9e87d41bf6e01001ab",
    "nextblockhash": "cf6b4c38951b359186c5ec315ce55fa7d891f59d601dd8cca5cb53f9ac89bc2814eaf646c79d9a1ba98d0a50020b6e33b01ee1cd5dd0b1fb9333918ced312e8ebc0faa8e5d088f3c1a563aa176a028e44c5cad71f1a8d82b6e2c571ecfff7385325a652f1940c890265914c166879878f5f3a24ddb07491320e0f90d0ced5137",
    "tx": [
        {
            "txid": "016af51b4a6f67a7663754b1865cc53fd2dae9c682f820437e826c4494087b79583a00a8b37c2ee78e7f00a04bafdc88687cd14da00236ac6d73586fa4855673"
        },
        {
            "txid": "0119082785a2b4308c6fe5b4f0c73cee154f2c580d256f25391805bcb39965d7e7f2359f783807fa2c3217ca09f8002d7a1cf2dbdb79603e6d0c4f9533b48037"
        }
    ]
}
[Completed in 1.475897 ms]Return values:
```

`hash` : The hash of the block.

`proofhash` : The proof hash of the block.

`size` : The block size in bytes.

`height` : The block height.

`channel` : The block channel (0=Stake, 1=Prime, 2=Hash).

`version` : Serialization version of this block

`merkleroot` : The hash merkle root of block transactions.

`time` : The unified time the block was created.

`nonce` : The solution nonce

`bits` : The compact representation for the block difficulty.

`difficulty` : The channel specific difficulty.

`mint` : The value minted in this block.

`previousblockhash` : The hash of the previous block in the chain.

`nextblockhash` : The hash of the next block in the chain, unless this is the last block in the chain.

`tx` : An array of transactions included in this block.

`type` : The description of the transaction (`legacy` | `tritium base` | `trust` | `genesis` | `user`).

`version` : The serialization version of the transaction.

`sequence` : The sequence number of this transaction within the signature chain.

`timestamp` : The Unix timestamp of when the transaction was created.

`blockhash` : The hash of the block that this transaction is included in. Blank if not yet included in a block.

`confirmations` : The number of confirmations that this transaction has obtained by the network.

`genesis` : The signature chain genesis hash.

`nexthash` : The hash of the next transaction in the sequence.

`prevhash` : The hash of the previous transaction in the sequence.

`pubkey` : The public key.

`signature` : The signature hash.

`hash` : The transaction hash.

`contracts` : The array of contracts bound to this transaction and their details with opcodes.\
{\
`id` : The sequential ID of this contract within the transaction.

`OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

`for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

`txid` : The transaction that was credited / claimed.

`contract` : The ID of the contract within the transaction that was credited / claimed.

`proof` : The register address proving the credit.

`from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

`from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

`to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

`to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

`amount` : the token amount of the transaction.

`token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

`token_name` : The name of the token that the transaction relates to.

`reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

## `get/blockhash`

Retrieves the hash of the block for the given height.

#### Endpoint:

`/ledger/get/blockhash`

{% swagger method="post" path="/ledger/get/blockhash" baseUrl="http://api.nexus-interactions.io:8080" summary="Get blockhash" %}
{% swagger-description %}
Retrieves the hash of the block for the given height
{% endswagger-description %}

{% swagger-parameter in="body" name="height" required="true" %}
The block height to retrieve the hash for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="blockhash" %}
```javascript
{
"result": {
"hash": "f175d2e4951db8046939807085f8841ef2cf65141bd0ef7c356edb7a9ffae61c95b1c8a4049af9202c3ef7a198c47ac5df7db2ed750c7890e3d240c45eabdb7f10162e8843c44dea582c5042fb9ef6b003ab719912faa6bfe4c822f35aeb7252d02cb0e880eb5234e5f621b8ea917432f907731eb4e478440df981fee2983ac8"
}
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// ledger/get/blockhash
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let height = 4180579
fetch(`${SERVER_URL}/ledger/get/blockhash?height=${height}`)
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
height = 4000000  #This is the block height or block count
response = requests.get(f"{SERVER_URL}/ledger/get/blockhash?height={height}")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`height` : The block height to retrieve the hash for.

#### Return value JSON object:

#### Return values:

`hash` : The hash of the block.

***

### `get/info`

Retrieves mining related data for the ledger.

#### Endpoint:

`/ledger/get/info`

{% swagger method="get" path="/ledger/get/info" baseUrl="http://api.nexus-interactions.io:8080" summary="get/info" %}
{% swagger-description %}
Retrieves mining related data for the ledger.
{% endswagger-description %}

{% swagger-response status="200: OK" description="Mining information" %}
```javascript
{
"result": {
"blocks": 4187383,
"timestamp": 1639466841,
"stakeDifficulty": 470.34551638219693,
"primeDifficulty": 7.9926008,
"hashDifficulty": 1854.1598715225612,
"stakeHeight": 1347186,
"primeHeight": 1464823,
"hashHeight": 1375377,
"primeReserve": 52.603552,
"hashReserve": 97.449141,
"primeValue": 2.206934,
"hashValue": 2.206926,
"pooledtx": 0,
"primesPerSecond": 5649859746,
"hashPerSecond": 2949078396266
}
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/mininginfo
const SERVER_URL = "http://api.nexus-interactions.io:8080"
fetch(`${SERVER_URL}/ledger/get/info`)
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
response = requests.get(f"{SERVER_URL}/ledger/get/info")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`None.`

#### Return value JSON object:

```
{
    "stake": {
        "height": 0,
        "weight": 0,
        "timespan": 0,
        "fees": 0,
        "difficulty": 0
    },
    "prime": {
        "height": 0,
        "weight": 0,
        "timespan": 0,
        "fees": 0,
        "difficulty": 0
    },
    "hash": {
        "height": 0,
        "weight": 0,
        "timespan": 0,
        "fees": 0,
        "difficulty": 0
    },
    "height": 23,
    "timestamp": 1654341926,
    "checkpoint": "deecfbef95517742861bd3bfa8f1bee77e6ba1b172961756dafe4a35f2e23efbb6eac0858af7b541636f91c41a8955eee98738e4a5779cbe14dcf5c3030dae378694c78c28199265cd05815e354686885d6598cf17226b1158dc5c7ab519ca5599c72a60df7b7ee79904523bc03dad2a2a65acc837dff4ed5399e9398a2b9302"
}
[Completed in 0.384917 ms]
```

#### Return values:

`blocks` : The current block height.

`timestamp` : The Unix timestamp of the last block.

`stakeDifficulty` : The current difficulty of the stake channel.

`primeDifficulty` : The current difficulty of the prime channel.

`hashDifficulty` : The current difficulty of the hash channel.

`stakeHeight` : The current number of blocks for the stake channel.

`primeHeight` : The current number of blocks for the prime channel.

`hashHeight` : The current number of blocks for the hash channel.

`primeReserve` : The amount of NXS in the reserve balance for the prime channel.

`hashReserve` : The amount of NXS in the reserve balance for the hash channel.

`primeValue` : The block reward for the next prime block to be found.

`hashValue` : The block reward for the next hash block to be found.

`pooledtx` : The number of transactions currently in the mempool.

`primesPerSecond` : The average number of primes per second currently being calculated by the whole network.

`hashPerSecond` : The average number of hashes per second currently being calculated by the whole network.

`totalConnections` : The number of connections to the mining LLP of this node.

## `list/blocks`

Retrieves an array of block data for a sequential range of blocks from a given hash or height.

#### Endpoint:

`/ledger/list/blocks`

{% swagger method="post" path="/ledger/list/blocks" baseUrl="http://api.nexus-interactions.io:8080" summary="List blocks" %}
{% swagger-description %}
Retrieves an array of block data for a sequential range of blocks from a given hash or height.
{% endswagger-description %}

{% swagger-parameter in="body" name="hash" required="false" %}
The block hash to retrieve the block data for
{% endswagger-parameter %}

{% swagger-parameter in="body" name="height" required="false" %}
The block height to retrieve the block data for
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" required="false" %}
This is optional, determines how much transaction data to include in the response. Supported values are :

`none` : no transaction data

`default` : hash

`summary` : type, version, sequence, timestamp, and operation.

`detail` : genesis, nexthash, prevhash, pubkey and signature.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" required="false" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" required="false" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" required="false" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" required="false" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Blocks list" %}
```python
[
    {
        "hash": "5534a161f52a2b4904c752fc487fca2ce18f27a25b2b7de9b894ac18a41a3b312a4a9b55204cf18b7b0ce0c55bc1d7a6decb0a4f3bad98f8274ae9e31d19a4862cf40daa0bc8bf567f14bcce43555c4a7ac9a7445647e9cb9f95ef759a7737917ece8d61306da38fd7d9ae45909de7ed96e82caa8af3f7ab6758fe539ac4d12f",
        "proofhash": "500344c3287f8082802cbf21dbd31c4ef7f1f626046ab2b1e5f4db7dbf344cfbf4681c757f9e4c67ce5efde5bfa34d5b58b3d0c9456e9166c733cc37b4a2f5d5764ba122252f5e6efa9a233bb37f7b1528b46c56b7f17e24516a86a378738ea8dbc7d85b41e48705c827538f86dad226fda5257077542f2a9fe343a986b52e1f",
        "size": 727,
        "height": 1,
        "channel": 1,
        "version": 6,
        "merkleroot": "3328b8e25d45904bb26ad1298260eda220417859bca8b7b1e123817518b8aecc8ea89404900c0ac6941139b5403579e705677bf9e2bd2f131891c6348f667d1b",
        "time": "2019-03-22 03:58:09 UTC",
        "nonce": 11636693071151760,
        "bits": "017d7840",
        "difficulty": 2.5,
        "mint": 0.0,
        "previousblockhash": "00002a0ccd35f2e1e9e1c08f5a2109a82834606b121aca891d5862ba12c6987d55d1e789024fcd5b9adaf07f5445d24e78604ea136a0654497ed3db0958d63e72f146fae2794e86323126b8c3d8037b193ce531c909e5222d090099b4d574782d15c135ddd99d183ec14288437563e8a6392f70259e761e62d2ea228977dd2f7",
        "nextblockhash": "c4603451343ea95bfd543074751200bab6b55c7d27495eab25286d441edb112ea7fc6883a86f538218bba8fabf20eb30557fc6a73e9929059d20e7f74676ce1f700420b951832116e7420b4f5b8863b1ffc09810b2b123e465637d136d130033c6bc3b2fee701f25e980e4dee2711a2aca248ad529368dacb220968951cbd2a3",
        "tx": [
            {
                "hash": "3328b8e25d45904bb26ad1298260eda220417859bca8b7b1e123817518b8aecc8ea89404900c0ac6941139b5403579e705677bf9e2bd2f131891c6348f667d1b"
            }
        ]
    },
    {
        "hash": "c4603451343ea95bfd543074751200bab6b55c7d27495eab25286d441edb112ea7fc6883a86f538218bba8fabf20eb30557fc6a73e9929059d20e7f74676ce1f700420b951832116e7420b4f5b8863b1ffc09810b2b123e465637d136d130033c6bc3b2fee701f25e980e4dee2711a2aca248ad529368dacb220968951cbd2a3",
        "proofhash": "61090468e7369af7013a46c4532013d21dc0e80401da19b723ceb7ef2f331c0864fba8c0dff2907d732b9f867af8fead3aabdf8ea8072a512e4a208c63f6b08c5d83a7855d3d6cf3f5ec6aad080f246f1c140710de2ecf614afe8bcd0b973f8e0d89830c4d9d66d4409b6e46819d337a2402d6eed2c664d38475d8a09993773b",
        "size": 727,
        "height": 2,
        "channel": 1,
        "version": 6,
        "merkleroot": "b3e587c3299b8f4b9c65cd221e7ad3afe579a1d01b0acd935c9bbd2e35d102f9f2023bfc7bcba1f840a3b9725e651c26dc516e53c63eff7e37206ed94b8f746f",
        "time": "2019-03-22 03:58:17 UTC",
        "nonce": 143375589399502436,
        "bits": "017d7840",
        "difficulty": 2.5,
        "mint": 0.0,
        "previousblockhash": "5534a161f52a2b4904c752fc487fca2ce18f27a25b2b7de9b894ac18a41a3b312a4a9b55204cf18b7b0ce0c55bc1d7a6decb0a4f3bad98f8274ae9e31d19a4862cf40daa0bc8bf567f14bcce43555c4a7ac9a7445647e9cb9f95ef759a7737917ece8d61306da38fd7d9ae45909de7ed96e82caa8af3f7ab6758fe539ac4d12f",
        "nextblockhash": "2e9176b4a8462244fafca6fa91d7549257d2513207272890b3c6875443abaa24cca19ddb5acaa234b0b70e3d226b37599e1a2a6950236dcb81871e4b685559e7f977e413cb2fbcf8589f62ebcd97471d377e22e4ad301ce7e9fd0049459accebf97813e669eda5e0fc33ad2ec1a77c104cd713112de09fc2f2c19b9b14306ae8",
        "tx": [
            {
                "hash": "b3e587c3299b8f4b9c65cd221e7ad3afe579a1d01b0acd935c9bbd2e35d102f9f2023bfc7bcba1f840a3b9725e651c26dc516e53c63eff7e37206ed94b8f746f"
            }
        ]
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/blocks
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  // hash: "256 chars hash" // optional 
  // height: 4000000, //optional
  // verbose: "detail", //optional or "summary","none"
  // limit: 50, //optional
  // page: 1, //optional
  // offset: 10, //optional
  // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/ledger/list/blocks`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # "hash": "256 chars hash" # optional
    # "height": 4000000, #optional
    # "verbose": "detail", #optional or "summary","none"
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/ledger/list/blocks", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`hash` : The block hash to retrieve the block data for.

`height` : The block height to retrieve the block data for.

`verbose` : This is optional, determines how much transaction data to include in the response. Supported values are :

* `none` : no transaction data
* `default` : hash
* `summary` : type, version, sequence, timestamp, and operation.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

{% hint style="info" %}
Either the hash or the height needs to be supplied, but not both. Retrieving block data by height is only allowed if the daemon has been configured with `indexheight=1`
{% endhint %}

#### Return value JSON object:

```
[
    {
        "hash": "5534a161f52a2b4904c752fc487fca2ce18f27a25b2b7de9b894ac18a41a3b312a4a9b55204cf18b7b0ce0c55bc1d7a6decb0a4f3bad98f8274ae9e31d19a4862cf40daa0bc8bf567f14bcce43555c4a7ac9a7445647e9cb9f95ef759a7737917ece8d61306da38fd7d9ae45909de7ed96e82caa8af3f7ab6758fe539ac4d12f",
        "proofhash": "500344c3287f8082802cbf21dbd31c4ef7f1f626046ab2b1e5f4db7dbf344cfbf4681c757f9e4c67ce5efde5bfa34d5b58b3d0c9456e9166c733cc37b4a2f5d5764ba122252f5e6efa9a233bb37f7b1528b46c56b7f17e24516a86a378738ea8dbc7d85b41e48705c827538f86dad226fda5257077542f2a9fe343a986b52e1f",
        "size": 727,
        "height": 1,
        "channel": 1,
        "version": 6,
        "merkleroot": "3328b8e25d45904bb26ad1298260eda220417859bca8b7b1e123817518b8aecc8ea89404900c0ac6941139b5403579e705677bf9e2bd2f131891c6348f667d1b",
        "time": "2019-03-22 03:58:09 UTC",
        "nonce": 11636693071151760,
        "bits": "017d7840",
        "difficulty": 2.5,
        "mint": 0.0,
        "previousblockhash": "00002a0ccd35f2e1e9e1c08f5a2109a82834606b121aca891d5862ba12c6987d55d1e789024fcd5b9adaf07f5445d24e78604ea136a0654497ed3db0958d63e72f146fae2794e86323126b8c3d8037b193ce531c909e5222d090099b4d574782d15c135ddd99d183ec14288437563e8a6392f70259e761e62d2ea228977dd2f7",
        "nextblockhash": "c4603451343ea95bfd543074751200bab6b55c7d27495eab25286d441edb112ea7fc6883a86f538218bba8fabf20eb30557fc6a73e9929059d20e7f74676ce1f700420b951832116e7420b4f5b8863b1ffc09810b2b123e465637d136d130033c6bc3b2fee701f25e980e4dee2711a2aca248ad529368dacb220968951cbd2a3",
        "tx": [
            {
                "hash": "3328b8e25d45904bb26ad1298260eda220417859bca8b7b1e123817518b8aecc8ea89404900c0ac6941139b5403579e705677bf9e2bd2f131891c6348f667d1b"
            }
        ]
    },
    {
        "hash": "c4603451343ea95bfd543074751200bab6b55c7d27495eab25286d441edb112ea7fc6883a86f538218bba8fabf20eb30557fc6a73e9929059d20e7f74676ce1f700420b951832116e7420b4f5b8863b1ffc09810b2b123e465637d136d130033c6bc3b2fee701f25e980e4dee2711a2aca248ad529368dacb220968951cbd2a3",
        "proofhash": "61090468e7369af7013a46c4532013d21dc0e80401da19b723ceb7ef2f331c0864fba8c0dff2907d732b9f867af8fead3aabdf8ea8072a512e4a208c63f6b08c5d83a7855d3d6cf3f5ec6aad080f246f1c140710de2ecf614afe8bcd0b973f8e0d89830c4d9d66d4409b6e46819d337a2402d6eed2c664d38475d8a09993773b",
        "size": 727,
        "height": 2,
        "channel": 1,
        "version": 6,
        "merkleroot": "b3e587c3299b8f4b9c65cd221e7ad3afe579a1d01b0acd935c9bbd2e35d102f9f2023bfc7bcba1f840a3b9725e651c26dc516e53c63eff7e37206ed94b8f746f",
        "time": "2019-03-22 03:58:17 UTC",
        "nonce": 143375589399502436,
        "bits": "017d7840",
        "difficulty": 2.5,
        "mint": 0.0,
        "previousblockhash": "5534a161f52a2b4904c752fc487fca2ce18f27a25b2b7de9b894ac18a41a3b312a4a9b55204cf18b7b0ce0c55bc1d7a6decb0a4f3bad98f8274ae9e31d19a4862cf40daa0bc8bf567f14bcce43555c4a7ac9a7445647e9cb9f95ef759a7737917ece8d61306da38fd7d9ae45909de7ed96e82caa8af3f7ab6758fe539ac4d12f",
        "nextblockhash": "2e9176b4a8462244fafca6fa91d7549257d2513207272890b3c6875443abaa24cca19ddb5acaa234b0b70e3d226b37599e1a2a6950236dcb81871e4b685559e7f977e413cb2fbcf8589f62ebcd97471d377e22e4ad301ce7e9fd0049459accebf97813e669eda5e0fc33ad2ec1a77c104cd713112de09fc2f2c19b9b14306ae8",
        "tx": [
            {
                "hash": "b3e587c3299b8f4b9c65cd221e7ad3afe579a1d01b0acd935c9bbd2e35d102f9f2023bfc7bcba1f840a3b9725e651c26dc516e53c63eff7e37206ed94b8f746f"
            }
        ]
    }
]
```

#### Return values:

`hash` : The hash of the block.

`proofhash` : The proof hash of the block.

`size` : The block size in bytes.

`height` : The block height.

`channel` : The block channel (0=Stake, 1=Prime, 2=Hash).

`version` : The serialization version of this block.

`merkleroot` : The hash merkle root of the block transactions.

`time` : The unified time the block was created.

`nonce` : The solution nonce.

`bits` : The compact representation for the block difficulty.

`difficulty` : The channel specific difficulty.

`mint` : The value minted in this block.

`previousblockhash` : The hash of the previous block in the chain.

`nextblockhash` : The hash of the next block in the chain, unless this is the last block in the chain.

`tx` : An array of transactions included in this block.

`type` : The description of the transaction (`legacy` | `tritium base` | `trust` | `genesis` | `user`).

`version` : The serialization version of the transaction.

`sequence` : The sequence number of this transaction within the signature chain.

`timestamp` : The Unix timestamp of when the transaction was created.

`blockhash` : The hash of the block that this transaction is included in. Blank if not yet included in a block.

`confirmations` : The number of confirmations that this transaction has obtained by the network.

`genesis` : The signature chain genesis hash.

`nexthash` : The hash of the next transaction in the sequence.

`prevhash` : The hash of the previous transaction in the sequence.

`pubkey` : The public key.

`signature` : The signature hash.

`hash` : The transaction hash.

`contracts` : The array of contracts bound to this transaction and their details with opcodes.\
{\
`idcontractid` : The sequential ID of this contract within the transaction.

`OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

`for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

`txid` : The transaction that was credited / claimed.

`contract` : The ID of this contract within the transaction that was credited / claimed.

`proof` : The register address proving the credit.

`from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

`from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

`to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

`to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

`amount` : the token amount of the transaction.

`token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

`token_name` : The name of the token that the transaction relates to.

`reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

### `get/transaction`

Retrieves transaction data for a given transaction hash.

#### Endpoint:

`/ledger/get/transaction`

{% swagger method="post" path="/ledger/get/transaction" baseUrl="http://api.nexus-interactions.io:8080" summary="Get transaction" %}
{% swagger-description %}
Retrieves transaction data for a given transact
{% endswagger-description %}

{% swagger-parameter in="body" name="format" required="false" %}
Determines the format of the return value. Parameter value can be `JSON` (the default) or `raw` .  If `raw` is specified then the method returns a serialized, hex-encoded transaction that can subsequently be broadcast to the network via

`/ledger/submit/transaction`


{% endswagger-parameter %}

{% swagger-parameter in="body" name="hash" required="false" %}
The block hash to retrieve the block data for. This is ignored if 

`raw`

 format is requested
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="false" %}
The transaction ID (hash) to retrieve the transaction data
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" required="false" %}
Optional, determines how much transaction data to include in the response. This is ignored if `raw` format is requested. Supported values are :

`summary` : hash, type, version, sequence, timestamp, and contracts.

`detail` : genesis, nexthash, prevhash, pubkey and signature.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="transaction list" %}
```json
{
    "txid": "51fd1f61de02f0788cac3c2fcde94012ef12ed34227717f9f5fe2c019ac1aa0a3d3ad9580721eeced8f036c771eb2a8cc6d67a6409c721b68bbe66d1387b97f3",
    "contracts": [
        {
            "OP": "REGISTER",
            "address": "5211647134daf849e94720e65fc5e685361d9329c8709e159c784fbd9dd840ae",
            "type": 4,
            "data": "046e616d65080574727573740761646472657373ff055714f14df2588a47c1a13db683e585bd16de1460a70f89d98769ed3c3701534f"
        },
        {
            "OP": "REGISTER",
            "address": "4f5301373ced6987d9890fa76014de16bd85e583b63da1c1478a58f24df11457",
            "type": 4,
            "data": "0762616c616e6365ff040000000000000000057472757374ff040000000000000000057374616b65ff04000000000000000005746f6b656e050000000000000000000000000000000000000000000000000000000000000000"
        },
    ],
    "type": "tritium first",
    "version": 1,
    "sequence": 0,
    "timestamp": 1560189145,
    "confirmations": 19,
    "genesis": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
    "nexthash": "8e5d67616022c84625326280ebaaa1f0be9e49bf26a1a8f85f62c476bd31e6b0",
    "prevhash": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "pubkey": "091ccca8e8e4082585c84345bfa298427ef887f66ab73ac298eb8be5b3678c0f32688a757380ab72e2a7fc055916d99ca38a2635fe82fe26b138bea72a84466a3d948f5993840d85c57a3025097d05ddf302a3d60a412af91075ec144412daa5ba4cefa40a51c5c616c4034236305eef9d8580ead40654752f261d03b5904f2dcd6cb0649455302c93f734a75681309072d7d0b129f20947aae5050079887aca2e69a2b11b8782a57d1f90e380dd6f5290940e44620e0f7c7d676718057aa0a845b35dec649ca42636d503f036651aabe41bee16f9fa15f65add93885791c2c9a607e86b16470d9b3a3891a5c9eb204796ef57cf9a1a50b40b31180035be308fa7be1aa6b3b02836db6001638cbab7c5ed49172aa7c257ae107b2608e24a510c8aa205d3c2031a39fccebbf4992b93e97d3693809178ba7352159b199a125bb8804c086964a470f0cccd6e0e937311ebd1873d6421c6e61a8408624b776636a25aae239988f42de112f23ace8f38de251bcc269c532914d4d895814f942233cce76d1854e8d84aea1691f2964ba066cab0b827eb11045043edecb3ad52e566d5b70236c750387558144443a20a3d13eaf716a418b4de9843295283d49f45cdc8ddb699808708a85521e924d24b41ef1cfc54984fc449252904a176595e9ebe0301e150f88fc16b87e644850e849c21bf64f57575ef99d308f341de9ea3f4a6509b5a44fa2755b45df6f8e83184ac2c90eb783d90bb919ab4fa3b0d31ad41ea6aa584320cbc6ec9dfdff5249cada2ce20d0b1d8da865161c077c08ad943d260426ee02a549be1320ab7c9419bff5b39c005641f3123f9d3079b15d15dfd5de96768e8b8ba91f26d6a192d41859996952039a81484835d49839108f6465ed246182f1a18cca7af46cefb9927a48a04c4db34509a267973f1a4afb137f9c29860ed3a886492a69acae09c4efbe09c4f7170ac6b1b4a8bf81b452b07c00b72a87492f11bc9d7bf3ef5c7f4635a209145891691b79575aea6c344f993fc2eb95b2ba7a2e947a9ed0c560882901a2c2a40a8158a9448039d71207347eb636408f8bb105baa685dc452fa7f5dbba98676aba9c414360876c606db032205eb574c2f579f092809e65f9c5dd6e1cad30b49a63653552fd00ea9e28e46c1ddf6b69d97446751401f7dcfb27f082d09e1720bfb0c1984f27e2472a4107b6108047e8dec47e084b18d5bc890b4e7c3a9818a3d0c718838a66fa2285799c92819ad64d645ba00de",
    "signature": "297cd35d0f87e78d6a8b7979709bcf932363c4b141ee4893e7aa8f15633b12896c9ee8696aa66f9ee41106cec4bcaf8616bf656498bddd112abb431084309e314dfdb500d320750849abfc76d5bb4761ecdcebf065e5f29cf4b4c4bfa930deba881655d0667bb36cc2db2d35711291040ce983eecbf10a04167871f967f25a4587d309da7f48e3e83118be7ce8c673f696116073c47ffeb229d1844bfd6cde266464c972f8b8dd5c4b68cb645143959c3e77bc8548cbff605c1c0f4d8e6a2fffec396ecad7fd48e8f09ce2147518353caf3ff5c3bb2c5f2d0b5c321dd2c762d365c380c64ccf51369ce9b30c7e2bdb8f157d42f3bb68b70461b66327db6f2b6f9247d3fc0ec9cfa0649006ad4eecb067798a3c6ba33fe473602840ea4d6a44daaf94efdf7047b5ab2d9608509a6dea77bfbe207a58dde3577535b319b78796a59b098ddfba44dd3b27d17f3646ffddce63a16dc69694a2107639d760892952f5ee7ea6c0d469947614c1163974b60ec22146bae72b5622f918ff6aeb3d67f39b18a414cc7c6330632128bfeab3b1c75b05f3e1e81a1e6766a12550cda5960ebed2fa5252cc80115811826a985ab228535f16a5807c7508ea42808f6c24ceeeab193f268f0ca9ca50341fa4fdd38d646e6cc26a94de3dc1b14a7fd934bb4cb8e1b919deb668d94610dcee424885ea12b7e5969e47eff743dcf4ab93422ddaca9544e53e382ad74ec361c8332dcd99388f1b3769c4760a7c6571dca14cea8a8f303e54382449e860ba71212c85e9968815de04f14810ad77236fc2644a3edb0f354c552f410f28ac92610462aa32b3872a2b4df5d67534377fd8948d708962dfa8d54e8923bf61f85c69fd660fd90f7f8bdb7ebafef9112b1b57e10a6a6cb29506250099451f87994dc226bfff7c45ca888a4aed"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/transaction
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  // format: "raw" // optional
  // txid: "YOUR_TXID" // optional if hash is passed
  hash: "256 chars hash" // optional 
  // verbose: "detail", //optional or "summary","none"
}
fetch(`${SERVER_URL}/ledger/get/transaction`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # "format": "raw" # optional
    # "txid": "YOUR_TXID" # optional if hash is passed
    "hash": "256 chars hash"  # optional
    # "verbose": "detail", #optional or "summary","none"
}
response = requests.post(f"{SERVER_URL}/ledger/get/transaction", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`format` : Determines the format of the return value. Parameter value can be `JSON` (the default) or `raw`. If `raw` is specified then the method returns a serialized, hex-encoded transaction that can subsequently be broadcast to the network via `/ledger/submit/transaction`.

`hash` : The block hash to retrieve the block data for. This is ignored if `raw` format is requested. `txid` : The block hash to retrieve the block data for. This is an alias for `hash`.

`verbose` : Optional, determines how much transaction data to include in the response. This is ignored if `raw` format is requested. Supported values are :

* `summary` : hash, type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

#### Return value JSON object:

```
{
    "txid": "51fd1f61de02f0788cac3c2fcde94012ef12ed34227717f9f5fe2c019ac1aa0a3d3ad9580721eeced8f036c771eb2a8cc6d67a6409c721b68bbe66d1387b97f3",
    "contracts": [
        {
            "OP": "REGISTER",
            "address": "5211647134daf849e94720e65fc5e685361d9329c8709e159c784fbd9dd840ae",
            "type": 4,
            "data": "046e616d65080574727573740761646472657373ff055714f14df2588a47c1a13db683e585bd16de1460a70f89d98769ed3c3701534f"
        },
        {
            "OP": "REGISTER",
            "address": "4f5301373ced6987d9890fa76014de16bd85e583b63da1c1478a58f24df11457",
            "type": 4,
            "data": "0762616c616e6365ff040000000000000000057472757374ff040000000000000000057374616b65ff04000000000000000005746f6b656e050000000000000000000000000000000000000000000000000000000000000000"
        },
    ],
    "type": "tritium first",
    "version": 1,
    "sequence": 0,
    "timestamp": 1560189145,
    "confirmations": 19,
    "genesis": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
    "nexthash": "8e5d67616022c84625326280ebaaa1f0be9e49bf26a1a8f85f62c476bd31e6b0",
    "prevhash": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "pubkey": "091ccca8e8e4082585c84345bfa298427ef887f66ab73ac298eb8be5b3678c0f32688a757380ab72e2a7fc055916d99ca38a2635fe82fe26b138bea72a84466a3d948f5993840d85c57a3025097d05ddf302a3d60a412af91075ec144412daa5ba4cefa40a51c5c616c4034236305eef9d8580ead40654752f261d03b5904f2dcd6cb0649455302c93f734a75681309072d7d0b129f20947aae5050079887aca2e69a2b11b8782a57d1f90e380dd6f5290940e44620e0f7c7d676718057aa0a845b35dec649ca42636d503f036651aabe41bee16f9fa15f65add93885791c2c9a607e86b16470d9b3a3891a5c9eb204796ef57cf9a1a50b40b31180035be308fa7be1aa6b3b02836db6001638cbab7c5ed49172aa7c257ae107b2608e24a510c8aa205d3c2031a39fccebbf4992b93e97d3693809178ba7352159b199a125bb8804c086964a470f0cccd6e0e937311ebd1873d6421c6e61a8408624b776636a25aae239988f42de112f23ace8f38de251bcc269c532914d4d895814f942233cce76d1854e8d84aea1691f2964ba066cab0b827eb11045043edecb3ad52e566d5b70236c750387558144443a20a3d13eaf716a418b4de9843295283d49f45cdc8ddb699808708a85521e924d24b41ef1cfc54984fc449252904a176595e9ebe0301e150f88fc16b87e644850e849c21bf64f57575ef99d308f341de9ea3f4a6509b5a44fa2755b45df6f8e83184ac2c90eb783d90bb919ab4fa3b0d31ad41ea6aa584320cbc6ec9dfdff5249cada2ce20d0b1d8da865161c077c08ad943d260426ee02a549be1320ab7c9419bff5b39c005641f3123f9d3079b15d15dfd5de96768e8b8ba91f26d6a192d41859996952039a81484835d49839108f6465ed246182f1a18cca7af46cefb9927a48a04c4db34509a267973f1a4afb137f9c29860ed3a886492a69acae09c4efbe09c4f7170ac6b1b4a8bf81b452b07c00b72a87492f11bc9d7bf3ef5c7f4635a209145891691b79575aea6c344f993fc2eb95b2ba7a2e947a9ed0c560882901a2c2a40a8158a9448039d71207347eb636408f8bb105baa685dc452fa7f5dbba98676aba9c414360876c606db032205eb574c2f579f092809e65f9c5dd6e1cad30b49a63653552fd00ea9e28e46c1ddf6b69d97446751401f7dcfb27f082d09e1720bfb0c1984f27e2472a4107b6108047e8dec47e084b18d5bc890b4e7c3a9818a3d0c718838a66fa2285799c92819ad64d645ba00de",
    "signature": "297cd35d0f87e78d6a8b7979709bcf932363c4b141ee4893e7aa8f15633b12896c9ee8696aa66f9ee41106cec4bcaf8616bf656498bddd112abb431084309e314dfdb500d320750849abfc76d5bb4761ecdcebf065e5f29cf4b4c4bfa930deba881655d0667bb36cc2db2d35711291040ce983eecbf10a04167871f967f25a4587d309da7f48e3e83118be7ce8c673f696116073c47ffeb229d1844bfd6cde266464c972f8b8dd5c4b68cb645143959c3e77bc8548cbff605c1c0f4d8e6a2fffec396ecad7fd48e8f09ce2147518353caf3ff5c3bb2c5f2d0b5c321dd2c762d365c380c64ccf51369ce9b30c7e2bdb8f157d42f3bb68b70461b66327db6f2b6f9247d3fc0ec9cfa0649006ad4eecb067798a3c6ba33fe473602840ea4d6a44daaf94efdf7047b5ab2d9608509a6dea77bfbe207a58dde3577535b319b78796a59b098ddfba44dd3b27d17f3646ffddce63a16dc69694a2107639d760892952f5ee7ea6c0d469947614c1163974b60ec22146bae72b5622f918ff6aeb3d67f39b18a414cc7c6330632128bfeab3b1c75b05f3e1e81a1e6766a12550cda5960ebed2fa5252cc80115811826a985ab228535f16a5807c7508ea42808f6c24ceeeab193f268f0ca9ca50341fa4fdd38d646e6cc26a94de3dc1b14a7fd934bb4cb8e1b919deb668d94610dcee424885ea12b7e5969e47eff743dcf4ab93422ddaca9544e53e382ad74ec361c8332dcd99388f1b3769c4760a7c6571dca14cea8a8f303e54382449e860ba71212c85e9968815de04f14810ad77236fc2644a3edb0f354c552f410f28ac92610462aa32b3872a2b4df5d67534377fd8948d708962dfa8d54e8923bf61f85c69fd660fd90f7f8bdb7ebafef9112b1b57e10a6a6cb29506250099451f87994dc226bfff7c45ca888a4aed"
}
```

#### Return values:

`data` : If `format` was specified as `raw` then the response will contain only the data field containing the raw, hex encoded, transaction data.

`txid` : The transaction hash.

`type` : The description of the transaction (`legacy` | `tritium base` | `trust` | `genesis` | `user`).

`version` : The serialization version of the transaction.

`sequence` : The sequence number of this transaction within the signature chain.

`timestamp` : The Unix timestamp of when the transaction was created.

`blockhash` : The hash of the block that this transaction is included in. Blank if not yet included in a block.

`confirmations` : The number of confirmations that this transaction obtained by the network.

`genesis` : The signature chain genesis hash.

`nexthash` : The hash of the next transaction in the sequence.

`prevhash` : the hash of the previous transaction in the sequence.

`pubkey` : The public key.

`signature` : The signature hash.

`contracts` : The array of contracts bound to this transaction and their details with opcodes.\
{\
`id` : The sequential ID of this contract within the transaction.

`OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

`for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

`txid` : The transaction that was credited / claimed.

`contract` : The ID of the contract within the transaction that was credited / claimed.

`proof` : The register address proving the credit.

`from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

`from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

`to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

`to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

`amount` : the token amount of the transaction.

`token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

`token_name` : The name of the token that the transaction relates to.

`reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.



### &#x20;`submit/transaction`

Submits a transaction to be included in the mempool and broadcast to the network.

#### Endpoint:

`/ledger/submit/transaction`

{% swagger method="post" path="/ledger/submit/transaction" baseUrl="http://api.nexus-interactions.io:8080" summary="Submit/transaction" %}
{% swagger-description %}
Submits a transaction to be included in the mempool and broadcast to the network.
{% endswagger-description %}

{% swagger-parameter in="body" name="data" required="true" %}
The serialized, hex-encoded transaction data to be submitted.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="submit transaction: success" %}
```javascript
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// submit/transaction
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  data: "The serialized, hex-encoded transaction data to be submitted"
}
fetch(`${SERVER_URL}/ledger/submit/transaction`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "data": "The serialized, hex-encoded transaction data to be submitted"
}
response = requests.post(f"{SERVER_URL}/ledger/submit/transaction", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`data` : The serialized, hex-encoded transaction data to be submitted.

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```

#### Return values:

`hash` : The transaction hash, if successfully committed to the mempool / broadcast.

***

### `void/transaction`

Voids (reverses) a debit or transfer transaction that you have previously made, that has not yet been credited or claimed by the recipient. The method creates a corresponding credit or claim transaction but back to the originating account/signature chain. This means that any applicable fees will apply, as will conditions on the debit/transfer transaction (such as expiration conditions).

For debits that were made to a tokenized asset as part of a split payment transaction, the reversing credit will be made for the debit amount minus any partial amounts that have already been credited by the token holders.

#### Endpoint:

`/ledger/void/transaction`

{% swagger method="post" path="/ledger/void/transaction" baseUrl="http://api.nexus-interactions.io:8080" summary="Void transaction" %}
{% swagger-description %}
Voids (reverses) a debit or transfer transaction that you have previously made, that has not yet been credited or claimed by the recipient.
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="pin" %}
The PIN for the signature chain voiding the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) created the transaction being voided. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="true" %}
The transaction ID (hash) of the debit or transfer transaction that you wish to void
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="void transaction:success" %}
```javascript
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// void/transaction
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  pin: "YOUR_PIN",
  // session: "YOUR_SESSION_ID", //optional 
  txid: "256 char hash of your txn"
}
fetch(`${SERVER_URL}/ledger/void/transaction`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "txid": "256 char hash of your txn"
}
response = requests.post(f"{SERVER_URL}/ledger/void/transaction", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for the signature chain voiding the transaction.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) created the transaction being voided. For single-user API mode the session should not be supplied.

`txid` : The transaction ID (hash) of the debit or transfer transaction that you wish to void.

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```

#### Return values:

`hash` : The transaction hash of the credit/claim transaction, if successfully committed to the mempool / broadcast.
