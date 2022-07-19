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
[`void/transaction`](ledger.md#void-transaction)\
[`get/info`](ledger.md#get-info)\
[`sync/headers`](ledger.md#sync-headers)``

Direct endpoints support filters and operators.

{% hint style="info" %}
Retrieving block data by height is only possible if the daemon has been configured with --`indexheight=1`.
{% endhint %}

## `get/blockhash`

Retrieves the hash of the block for the given height.

```
ledger/get/blockhash
```

#### Parameters:

`height` : The block height to retrieve the hash for.

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

#### Parameters:

`hash` : Required to **identify** the block to retrieve the block data for. This is optional if the `height` is provided

`height` : Required to **identify** the block to retrieve the block data for. This is optional if the **hash** is provided

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

#### Return value JSON object:

```
{
    "hash": "289b76fbe40ca5e0d0df68016677a8c9be6389f3225cc9faf65cb92bbc98a3580b57cb7ba3d7f7ab36ce890d47a4be90254d407270ad4c2e69c99c998ef8547ca45f054b081959d4d59feb871360bce888f82b4cdc1476502bd1b8e95dffe811e55592d3d4768b56e0510e02b5e32171858de097cd811e7c8d09920c24960ae8",
    "proofhash": "1aca4fc53b82bb057606dba3b928dbc2219bd23f9acae138581fa4468184974b4176cd78ba54101e4d121275f04b7e5562bc787dad818bfb4416135b7a2bf397af46e561033e9189250e42acd74e705edcda6d293133b76f7edc312d062b9ac450367457641def7e6a22cf1029bf079787d697c567b08c2649f72e685b40f209",
    "size": 862,
    "height": 10,
    "channel": 3,
    "version": 8,
    "merkleroot": "6e037db150580c317f606bfca13fdae21bbc5b268ad8d961ffc0227b7e7d1754e9d7e59590d836d17f9fae78b7350ddf8340a1880b264f2d0435a515da48484e",
    "timestamp": 1654810515,
    "date": "2022-06-09 21:35:15 UTC",
    "nonce": 1,
    "bits": "7e7fffff",
    "difficulty": 31.999515,
    "mint": 0.0,
    "previousblockhash": "9b943b1c05a4f6ddd7bac066d70e2ab0c0f21f2f88008d9a1b94faddb2817328c4c6197db64fba78bd7a08bec84b5c6669cf074855d89bd98d651111df8e2b718b20b9e3b2ac6beb6c5809273231db024a0c2e92fcf85690628ce3a7ec8c75c8955f1571e1671725e7ff4cc8d9bbb9e0aa8d246f38621ce73e6677b658985662",
    "nextblockhash": "e772e768e8dc64906844c412ee4b65f25b1c987ba77094adf8ad09dfc8651364f29e8d9aa2dacb8e4307f3cecb6f71e76f7365a16b33e10dcbe12f1d2dbe4adf8570962581f4e18b0bfc9faf1a141515e22a61b7b8a5672ccf351c47d859c68579061810ddd49fbaf04fa23f0e87000db6eb65ce8a4ad3d761461e7365ff97ff",
    "tx": [
        {
            "txid": "01227fe04554476229db2717a4dbe5bdd0299a51ee75bba2a531dc8fc8cfa713e6199dd5d6972068968620209d89684672bd119ccbe27af65a00736c62a8fdba"
        },
        {
            "txid": "016e95de55daa8ba9113718fa0204cfe9a1ab97ce712c064ccee26d36cd42f25ebcde4398156eee6866d61a850b2e613a87b52e8524bd5c6e5dd63ff825c889a"
        }
    ]
}
[Completed in 3.194669 ms]
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
&#x20;  `id` : The sequential ID of this contract within the transaction.

&#x20;  `OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

&#x20;  `for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

&#x20;  `txid` : The transaction that was credited / claimed.

&#x20;  `contract` : The ID of the contract within the transaction that was credited / claimed.

&#x20;  `proof` : The register address proving the credit.

&#x20;  `from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

&#x20;  `from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

&#x20;  `to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

&#x20;  `to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

&#x20;  `amount` : the token amount of the transaction.

&#x20;  `token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

&#x20;  `token_name` : The name of the token that the transaction relates to.

&#x20;  `reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

}

## `list/blocks`

Retrieves an array of block data for a sequential range of blocks from a given hash or height.

```
ledger/list/blocks
```

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

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here [Filtering Results](https://github.com/Nexusoft/LLL-TAO/blob/merging/docs/API/filtering-results.md)

**NOTE** : Either the hash or the height needs to be supplied, but not both.\
Retrieving block data by height is only allowed if the daemon has been configured with `indexheight=1`.

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
&#x20;  `idcontractid` : The sequential ID of this contract within the transaction.

&#x20;  `OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

&#x20;  `for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

&#x20;  `txid` : The transaction that was credited / claimed.

&#x20;  `contract` : The ID of this contract within the transaction that was credited / claimed.

&#x20;  `proof` : The register address proving the credit.

&#x20;  `from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

&#x20;  `from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

&#x20;  `to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

&#x20;  `to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

&#x20;  `amount` : the token amount of the transaction.

&#x20;  `token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

&#x20;  `token_name` : The name of the token that the transaction relates to.

&#x20;  `reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

}

***

## `get/transaction`

Retrieves transaction data for a given transaction hash.

```
ledger/get/transaction
```

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
&#x20;  `id` : The sequential ID of this contract within the transaction.

&#x20;  `OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

&#x20;  `for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

&#x20;  `txid` : The transaction that was credited / claimed.

&#x20;  `contract` : The ID of the contract within the transaction that was credited / claimed.

&#x20;  `proof` : The register address proving the credit.

&#x20;  `from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

&#x20;  `from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

&#x20;  `to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

&#x20;  `to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

&#x20;  `amount` : the token amount of the transaction.

&#x20;  `token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

&#x20;  `token_name` : The name of the token that the transaction relates to.

&#x20;  `reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

}

## `list/transactions`

Retrieves transaction data for a given profile.

```
ledger/get/transaction
```

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

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
&#x20;  `id` : The sequential ID of this contract within the transaction.

&#x20;  `OP` : The contract operation. Can be `APPEND`, `CLAIM`, `COINBASE`, `CREATE`, `CREDIT`, `DEBIT`, `FEE`, `GENESIS`, `LEGACY`, `TRANSFER`, `TRUST`, `STAKE`, `UNSTAKE`, `WRITE`.

&#x20;  `for` : For `CREDIT` transactions, the contract that this credit was created for . Can be `COINBASE`, `DEBIT`, or`LEGACY`.

&#x20;  `txid` : The transaction that was credited / claimed.

&#x20;  `contract` : The ID of the contract within the transaction that was credited / claimed.

&#x20;  `proof` : The register address proving the credit.

&#x20;  `from` : For `DEBIT`, `CREDIT`, `FEE` transactions, the register address of the account that the debit is being made from.

&#x20;  `from_name` : For `DEBIT`, `CREDIT`, `FEE` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

&#x20;  `to` : For `DEBIT` and `CREDIT` transactions, the register address of the recipient account.

&#x20;  `to_name` : For `DEBIT` and `CREDIT` transactions, the name of the recipient account. Only included if the name can be resolved.

&#x20;  `amount` : the token amount of the transaction.

&#x20;  `token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

&#x20;  `token_name` : The name of the token that the transaction relates to.

&#x20;  `reference` : For `DEBIT` and `CREDIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

}

## `submit/transaction`

Submits a transaction to be included in the mempool and broadcast to the network.

```
ledger/submit/transaction
```

#### Parameters:

`data` : The serialized, hex-encoded transaction data to be submitted.

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```

#### Return values:

`hash` : The tranaction hash, if successfully committed to the mempool / broadcast.

## `void/transaction`

{% hint style="info" %}
`Depreciated. Check finance/void/transaction`
{% endhint %}

Voids (reverses) a debit or transfer transaction that you have previously made, that has not yet been credited or claimed by the recipient. The method creates a corresponding credit or claim transaction but back to the originating account/signature chain. This means that any applicable fees will apply, as will conditions on the debit/transfer transaction (such as expiration conditions).

For debits that were made to a tokenized asset as part of a split payment transaction, the reversing credit will be made for the debit amount minus any partial amounts that have already been credited by the token holders.

```
ledger/void/transaction
```

Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : The transaction ID (hash) of the debit or transfer transaction that you wish to void.

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```

#### Return values:

`hash` : The transaction hash of the credit transaction, if successfully committed to the mempool / broadcast.

## `get/info`

Retrieves mining related data for the ledger.

```
ledger/get/info
```

#### Parameters:

\-none-

#### Return value JSON object:

```
{
    "blocks": 20267,
    "timestamp": 1554269575,
    "stakeDifficulty": 0.0,
    "primeDifficulty": 3.8210223,
    "hashDifficulty": 0.0,
    "stakeHeight"   : 1,
    "primeHeight"   : 1,
    "hashHeight"    : 2,
    "primeReserve": 21852246.988376,
    "hashReserve": 0.0,
    "primeValue": 74.893038,
    "hashValue": 1.0,
    "pooledtx": 0,
    "primesPerSecond": 6764,
    "hashPerSecond": 0,
    "totalConnections": 0
}
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

## `sync/headers`

Sync headers for lite mode.

```
ledger/sync/headers
```

#### Parameters:

\-None-

#### Return value JSON object:

```
```

#### Return values: 
