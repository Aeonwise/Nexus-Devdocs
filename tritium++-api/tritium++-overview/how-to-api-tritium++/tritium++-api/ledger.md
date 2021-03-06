# LEDGER

The Ledger API provides users with access to data held by the ledger such as blocks and transactions.

## `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

[`get/blockhash`](ledger.md#get-blockhash)\
[`get/block`](ledger.md#get-block)\
[`list/blocks`](ledger.md#list-blocks)\
[`get/transaction`](ledger.md#get-transaction)\
[`list/transactions`](ledger.md#get-transaction-1)\
[`submit/transaction`](ledger.md#submit-transaction)\
[`get/info`](ledger.md#get-info)

Direct endpoints support filters and operators.

{% hint style="info" %}
Retrieving block data by height is only possible if the core has been configured with --`indexheight=1`.
{% endhint %}

## `get/blockhash`

Retrieves the hash of the block for the given height.

```
ledger/get/blockhash
```

### Parameters:

`height` : The block height to retrieve the hash for.

### Results:

#### Return value JSON object:

```
{
    "hash": "289b76fbe40ca5e0d0df68016677a8c9be6389f3225cc9faf65cb92bbc98a3580b57cb7ba3d7f7ab36ce890d47a4be90254d407270ad4c2e69c99c998ef8547ca45f054b081959d4d59feb871360bce888f82b4cdc1476502bd1b8e95dffe811e55592d3d4768b56e0510e02b5e32171858de097cd811e7c8d09920c24960ae8"
}
[Completed in 0.083791 ms]  
```

#### Return values:

`hash` : The hash of the block.

## `get/block`

Retrieves block data for the given block hash or height.

```
ledger/get/block
```

### Parameters:

`hash` : Required to **identify** the block to retrieve the block data for. This is optional if the `height` is provided

`height` : Required to **identify** the block to retrieve the block data for. This is optional if the **hash** is provided

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the Sorting / Filtering parameters.

### Results:

#### Return value JSON object:

```
{
    "hash": "69336bc69ac2b9d06d7b9f9ede253cbeadba227751125cbb220654499c59a7b5138ea06ff62670fae614e5fed26aec8d3a0f346e03f10ea6c83dcb20ba8d93d17d92fffd6604666034e7002f421e39e64cef43325dfdf2c0334e51e018a31710c8fac17214d96d309b8dddb8f9bf9b3a27d28c71016b93a0e89f588c3604420a",
    "proofhash": "0000000000036b5c62766b5781f78734cf336f6fc6bce0363b93092551390fa7bd4d942d20316758def777b770db0500c3910f43b147c006ad11246ae62ed28284a4af309aebdc31ed6df23ebc7218b5a638b4da42cb496661fb12cace6890b920233e7db70a3daa8b439e0973a0dd01be67db652eaf8e7281027228f47a89cf",
    "size": 797,
    "height": 4555100,
    "channel": 2,
    "version": 8,
    "merkleroot": "017ff70158049886f8bad93cfe15285bae34d0e643991088e00769228161868a60230551724ac0ad5c7a98fabfdbebca88dffedf1a9e5ff095524b7565bccee1",
    "timestamp": 1658817542,
    "date": "2022-07-26 06:39:02 UTC",
    "nonce": 396216179366,
    "bits": "7b06e198",
    "difficulty": 2380.934877,
    "mint": 1.921131,
    "previousblockhash": "7dff690671fb7af3413f59315757a2fad8fb8acb7ea323269cc94152e9cbc696ed5064ec581c0709972a584f159ec75f8597559f93ec5f13d1bc608c0e7799ec891a5f26b455c97f0ac9dd1d826cb396aae1bcef8c0b74639e034161bb1d22a7cd4b29580d73ecba882a91273cc10121233096a5bb12c45ad774975c1bef925e",
    "nextblockhash": "5c985f5402f0bcb339ebef0e2edcad30427bf79803994772cd3a1458012748cd1d9d2ee8d6f1466f0c8ca867e1d8a2d9cfe6bc8cfdd885421fb56c55bfc11328f19c61bdc87193831851526a835e457fb46fcd0088879d564e88bb39524aa40489329ecc2cbc6a0453b8dd69a90b82232df6ee6d39ddec80e16149c99b1ddeab",
    "tx": [
        {
            "txid": "017ff70158049886f8bad93cfe15285bae34d0e643991088e00769228161868a60230551724ac0ad5c7a98fabfdbebca88dffedf1a9e5ff095524b7565bccee1",
            "type": "tritium base",
            "version": 4,
            "sequence": 11340,
            "timestamp": 1658817487,
            "blockhash": "69336bc69ac2b9d06d7b9f9ede253cbeadba227751125cbb220654499c59a7b5138ea06ff62670fae614e5fed26aec8d3a0f346e03f10ea6c83dcb20ba8d93d17d92fffd6604666034e7002f421e39e64cef43325dfdf2c0334e51e018a31710c8fac17214d96d309b8dddb8f9bf9b3a27d28c71016b93a0e89f588c3604420a",
            "confirmations": 262,
            "genesis": "a1e91c3b5d2856427508823fea815a37a11f5745d78e79091022d55e5bdb224d",
            "nexthash": "6adfd58123aa4ea622d808bbcf5b11d80d41921358c50dea71d25cf5791df106",
            "prevhash": "018f90d5ed4c2744e1d319c3b77e16f3a06187ee79e4a2bc982bc53dedfb2298aa0df30622df9cf080324aa103623db21648043cb1bb8f5a186764151fa9b242",
            "pubkey": "025d8d82339653aa9256bec27119cee55a9535e07b55aedcf7ed44a1f775a6afde6db2f76df2f337dd941ec337a98ad196c6840c040130ddc033cf779680735cd4",
            "signature": "30818402404da5e71d74b981fc7027cf30d0e7e56ea79367e6f97c76d3380bd3b56ea6aa608f6c370553c17c66fa022571412ab6cf2ebd41d9cc80295e37d6b59ed3a82fe602407a09ed3861823b1ec0ddd0d45a327b7615408b7d116032f6a806d2f286a420ac52275b1540085048491667173429c72113654255da72b12189a22eea89b8fa90",
            "contracts": [
                {
                    "id": 0,
                    "OP": "COINBASE",
                    "genesis": "a1e91c3b5d2856427508823fea815a37a11f5745d78e79091022d55e5bdb224d",
                    "nonce": 180,
                    "amount": 1.921131,
                    "token": "0000000000000000000000000000000000000000000000000000000000000000",
                    "ticker": "NXS"
                }
            ]
        }
    ]
}
[Completed in 0.422600 ms]
```

#### Return values:

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

}

## `list/blocks`

Retrieves an array of block data for a sequential range of blocks from a given hash or height.

```
ledger/list/blocks
```

### Parameters:

`hash` : Required to **identify** the block to retrieve the block data for. This is optional if the `height` is provided

`height` : Required to **identify** the block to retrieve the block data for. This is optional if the **hash** is provided

`verbose` : This is optional, determines how much **transaction data** to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, and operation.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

`where` : An array of clauses to **filter** the JSON results. More information on filtering the results from /list/xxx API methods can be found here [**`Queries`**](../../../how-to-api-tritium++/register.md)**``**

This method supports the Sorting / Filtering parameters.

**NOTE** : Either the hash or the height needs to be supplied, but not both.\
Retrieving block data by height is only allowed if the daemon has been configured with `indexheight=1`.

### Results:

#### Return value JSON object:

```
[
    {
        "hash": "5c985f5402f0bcb339ebef0e2edcad30427bf79803994772cd3a1458012748cd1d9d2ee8d6f1466f0c8ca867e1d8a2d9cfe6bc8cfdd885421fb56c55bfc11328f19c61bdc87193831851526a835e457fb46fcd0088879d564e88bb39524aa40489329ecc2cbc6a0453b8dd69a90b82232df6ee6d39ddec80e16149c99b1ddeab",
        "proofhash": "3f10d762af5952f7fdda2f84a27f9afe121e54be25eb0a41ee5074219919b9a2832d535d2c7c995a91e21ee6000017b2e7cc2099537cda035c09ebcaa94112384e88cdeec9d1ba6eb662b1acdf4f41d9b964f3da5a578d757bd2653acd8ed7364e8bd807777fbaabb4cda19cad6506b84431940e6a1e43a32d92eff9d9ace635",
        "size": 807,
        "height": 4555101,
        "channel": 1,
        "version": 8,
        "merkleroot": "0161d4629a2f385be81bda414359aaafb6baec7522f017c6fd06183c975c6c4a9baf4b89e5890327ddd79853eb415bfb054f84da719ac653520097bbc01b6e6c",
        "timestamp": 1658817563,
        "date": "2022-07-26 06:39:23 UTC",
        "nonce": 15731843013639865474,
        "bits": "04c23531",
        "difficulty": 7.9836465,
        "mint": 1.92113,
        "previousblockhash": "69336bc69ac2b9d06d7b9f9ede253cbeadba227751125cbb220654499c59a7b5138ea06ff62670fae614e5fed26aec8d3a0f346e03f10ea6c83dcb20ba8d93d17d92fffd6604666034e7002f421e39e64cef43325dfdf2c0334e51e018a31710c8fac17214d96d309b8dddb8f9bf9b3a27d28c71016b93a0e89f588c3604420a",
        "nextblockhash": "6e4b01acefa38b2f591ba7060c165daafe4cdaac8f147f7b8a02986b8f8b3c82d051601746ac53ff57167a650d75d501eab859b4a52d48444f036b9df1571648a22d777dd71896860b6faa7236bcd54d2f2de2445dcdf2e5a34f64b5c8918d3a8a8fce357024d74ca3c8b76c5a1457d4182ce18e015a782316390fc0dfb8eb34",
        "tx": [
            {
                "txid": "0161d4629a2f385be81bda414359aaafb6baec7522f017c6fd06183c975c6c4a9baf4b89e5890327ddd79853eb415bfb054f84da719ac653520097bbc01b6e6c",
                "type": "tritium base",
                "version": 4,
                "sequence": 176177,
                "timestamp": 1658817543,
                "blockhash": "5c985f5402f0bcb339ebef0e2edcad30427bf79803994772cd3a1458012748cd1d9d2ee8d6f1466f0c8ca867e1d8a2d9cfe6bc8cfdd885421fb56c55bfc11328f19c61bdc87193831851526a835e457fb46fcd0088879d564e88bb39524aa40489329ecc2cbc6a0453b8dd69a90b82232df6ee6d39ddec80e16149c99b1ddeab",
                "confirmations": 249,
                "contracts": [
                    {
                        "id": 0,
                        "OP": "COINBASE",
                        "genesis": "a165c6bc1b1e0dd9444f3ec01f07686991a8b4bdc1ceb4143dbb3eed16339a74",
                        "nonce": 308,
                        "amount": 1.92113,
                        "token": "0000000000000000000000000000000000000000000000000000000000000000",
                        "ticker": "NXS"
                    }
                ]
            }
        ]
    }
]
[Completed in 8.883924 ms]
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

}

## `get/transaction`

Retrieves transaction data for a given transaction hash.

```
ledger/get/transaction
```

### Parameters:

`format` : Determines the format of the return value. Parameter value can be `JSON` (the default) or `raw`. If `raw` is specified then the method returns a serialized, hex-encoded transaction that can subsequently be broadcast to the network via `/ledger/submit/transaction`.

`txid` : Required to **identify** the txid to retrieve the transaction data for.&#x20;

`verbose` : Optional, determines how much transaction data to include in the response. This is ignored if `raw` format is requested. Supported values are :

* `summary` : hash, type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the Sorting / Filtering parameters.

### Results:

#### Return value JSON object:

```
{
    "txid": "017ff70158049886f8bad93cfe15285bae34d0e643991088e00769228161868a60230551724ac0ad5c7a98fabfdbebca88dffedf1a9e5ff095524b7565bccee1",
    "type": "tritium base",
    "version": 4,
    "sequence": 11340,
    "timestamp": 1658817487,
    "blockhash": "69336bc69ac2b9d06d7b9f9ede253cbeadba227751125cbb220654499c59a7b5138ea06ff62670fae614e5fed26aec8d3a0f346e03f10ea6c83dcb20ba8d93d17d92fffd6604666034e7002f421e39e64cef43325dfdf2c0334e51e018a31710c8fac17214d96d309b8dddb8f9bf9b3a27d28c71016b93a0e89f588c3604420a",
    "confirmations": 267,
    "genesis": "a1e91c3b5d2856427508823fea815a37a11f5745d78e79091022d55e5bdb224d",
    "nexthash": "6adfd58123aa4ea622d808bbcf5b11d80d41921358c50dea71d25cf5791df106",
    "prevhash": "018f90d5ed4c2744e1d319c3b77e16f3a06187ee79e4a2bc982bc53dedfb2298aa0df30622df9cf080324aa103623db21648043cb1bb8f5a186764151fa9b242",
    "pubkey": "025d8d82339653aa9256bec27119cee55a9535e07b55aedcf7ed44a1f775a6afde6db2f76df2f337dd941ec337a98ad196c6840c040130ddc033cf779680735cd4",
    "signature": "30818402404da5e71d74b981fc7027cf30d0e7e56ea79367e6f97c76d3380bd3b56ea6aa608f6c370553c17c66fa022571412ab6cf2ebd41d9cc80295e37d6b59ed3a82fe602407a09ed3861823b1ec0ddd0d45a327b7615408b7d116032f6a806d2f286a420ac52275b1540085048491667173429c72113654255da72b12189a22eea89b8fa90",
    "contracts": [
        {
            "id": 0,
            "OP": "COINBASE",
            "genesis": "a1e91c3b5d2856427508823fea815a37a11f5745d78e79091022d55e5bdb224d",
            "nonce": 180,
            "amount": 1.921131,
            "token": "0000000000000000000000000000000000000000000000000000000000000000",
            "ticker": "NXS"
        }
    ]
}
[Completed in 0.290130 ms]
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

}

## `list/transactions`

Retrieves transaction data for a given profile.

```
ledger/get/transaction
```

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `summary` : hash, type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the Sorting / Filtering parameters.

### Results:

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

}

## `submit/transaction`

Submits a transaction to be included in the mempool and broadcast to the network.

```
ledger/submit/transaction
```

### Parameters:

`data` : The serialized, hex-encoded transaction data to be submitted.

### Results:

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
[Completed in 0.290130 ms]
```

#### Return values:

`hash` : The tranaction hash, if successfully committed to the mempool / broadcast.

## `get/info`

Retrieves mining related data for the ledger.

```
ledger/get/info
```

### Parameters:

\-none-

### Results:

#### Return value JSON object:

```
{
    "stake": {
        "height": 1465766,
        "weight": "0000000000000000000136338813e58f",
        "timespan": 162,
        "fees": 17781.438,
        "difficulty": 485.016406
    },
    "prime": {
        "height": 1593771,
        "weight": "0000000000000000000ab53e370e49ee",
        "timespan": 149,
        "fees": 40180.29,
        "difficulty": 7.9014281,
        "reserve": 444.478007,
        "reward": 1.920952,
        "primes": 3400159600
    },
    "hash": {
        "height": 1495822,
        "weight": "00000000000000000151e6c3955062c7",
        "timespan": 160,
        "fees": 48058.759,
        "difficulty": 2521.068135,
        "reserve": 312.411867,
        "reward": 1.920952,
        "hashes": 4360790031072
    },
    "height": 4555356,
    "timestamp": 1658832043,
    "checkpoint": "26e8ba97ae9545d878d00bf515da21dd7865fe3d4156d0bdd80f1e7a4abcf3181fbd394d998e511a6634ea39435d8bdc52bdae1d79b2ca371bac9a35444bbd4043a92907a0502fe01cccce5cba8ab196ec0ba6e6e88e027de82e267032fdd85d8feb05d1e207798bee4816ae6664cb091836a370ff4c508e3ecd86688d50d280"
}
[Completed in 275.128995 ms]
```

#### Return values:

`Stake` : This is the stake channel details.

{

`height` : The current number of blocks for the stake channel.

`weight` : The total work completed for the stake channel.

`timespan` : It is the average block time for the stake channel.

`fees` : It is the total NXS accumulated on the stake channel.

`difficulty` : The current difficulty of the stake channel.

}

`prime` : This is the prime channel details.

{

`height` : The current number of blocks for the prime channel.

`weight` : The total work completed for prime channel.

`timespan` : It is the average block time for the prime channel.

`fees` : It is the total NXS accumulated on the prime channel.

`difficulty` : The current difficulty of the prime channel.

`reserve` : The amount of NXS in the reserve balance for the prime channel.

`reward` : The block reward for the next prime block to be found.

`hashes` : It is total network hash rate for prime channel.

}

`hash` : This is the prime channel details.

{

`height` : The current number of blocks for the hash channel.

`weight` : The total work completed for hash channel.

`timespan` : It is the average block time for the hash channel.

`fees` : It is the total NXS accumulated on the hash channel.

`difficulty` : The current difficulty of the hash channel.

`reserve` : The amount of NXS in the reserve balance for the hash channel.

`reward` : The block reward for the next hash block to be found.

`hashes` : It is total network hash rate for the hash channel.

}

`height` : The block height when the information is retrieved.

`timestamp` :  The Unix timestamp of the most recent block in the blockchain.

`checkpoint` : It is the most recent checkpoint that blocks can???t be reorganized before, so everything before that hash is finalized.

####
