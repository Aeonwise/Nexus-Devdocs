---
description: LEDGER API
---

# LEDGER

The Ledger API provides users with access to data held by the ledger such as blocks and transactions. This API does not need  the user to be logged in except for the `void` verb. The full supported endpoint of the ledger URI is as follows:

```
ledger/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
ledger/verb/noun
```

### `Supported Operators`

The following operators are supported for this API command-set:

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
ledger/get/block/tx.contracts.amount height=10 verbose=detail where=object.ticker=XYZ
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "tx": [
        {
            "contracts": [
                {
                    "amount": 1000.0
                }
            ]
        }
    ]
}
[Completed in 0.624417 ms]
```

***

### `Supported Filters`

This command-set supports single or csv field-name filters.

**Example:**

```
ledger/list/tranasctions/type,version
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
ledger/list/transactions/contracts.OP
```

When using recursive filtering, the nested hiearchy is retained.

```
[
    {
        "contracts": [
            {
                "OP": "CREATE"
            }
        ]
    },
    {
        "contracts": [
            {
                "OP": "CREDIT"
            }
        ]
    }
]
```

### `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`get`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#get) - Get object of supported type.\
[`list`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#list) - List all objects owned by given session.\
[`submit`](ledger.md#submit) - To submit the modified object state to the mempool.\
[`void`](ledger.md#void) - To void a debit transaction and credit it back to the same account.

### `Supported Nouns`

The following nouns are supported for this API command-set:

\[`block`] - A block is a cryptographic object that contains transactions in a specific sequence of time.\
\[`blockhash`] - A blockhash is a cryptographic hash of a block.\
\[`transactions`] - An operation which modified a specified object.

**Example:**

```
ledger/list/tranactions
```

The above command will list all the transactions a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

## `Sorting / Filtering` <a href="#user-content-create" id="user-content-create"></a>

The following parameters can be used to apply **sorting** and **filtering** to the returned data-set.

`limit`: The number of records to return. _Default: 100_.

`page`: Zero-indexed page number that depends on `limit` for page boundaries.

`offset`: Alternative to `page`, offset can be used to page the results by index.

`order`: Descending **desc** or ascending **asc** as only permitted values.

`sort`: The column or field-name to apply the sorting logic to. This parameter supports moving up levels of JSON keys by using `.`, such as `sort=json.date` would apply a sort to a nested JSON object:

```
{
    "modified": 1621782289,
    "json": {
        "account": "8Cdr874GBd8t6MaQ4BVK8fXVVpzVHrGwZpQquUVzUXZroruYdeR",
        "date": "12-21-2020"
    }
}
```

`where`: Apply a boolean statement to the results of command, following the SQL-DSL syntax.

#### Alternative input

The `limit` and `offset` parameters can be given with the following format:

```
limit=100.10
```

This above will map to the parameters of `limit=100` and `offset=10`.

```
finance/get/balances/balance/sum
```

## `get`

Retrieves information for a single object for a type specified by the noun

```
ledger/get/noun
```

This command  supports the `blockhash`, `block` and `transaction` nouns.

`get/blockhash`

Retrieves information for a specified NXS or token account.

`get/block`

Retrieves information for a specified `trust` account.

`get/transaction`

Retrieves information for a specified token address.

#### Parameters:

#### `get/blockhash`

`height` : The block height to retrieve the hash for.

#### `get/block`

`hash` : The block hash to retrieve the block data for.

`height` : The block height to retrieve the block data for.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

#### `get/transaction`

`format` : Determines the format of the return value. Parameter value can be `JSON` (the default) or `raw`. If `raw` is specified then the method returns a serialized, hex-encoded transaction that can subsequently be broadcast to the network via `ledger/submit/transaction`.

`hash` : The block hash to retrieve the block data for. This is ignored if `raw` format is requested.&#x20;

`txid` : The block hash to retrieve the block data for. This is an alias for `hash`.

`verbose` : Optional, determines how much transaction data to include in the response. This is ignored if `raw` format is requested. Supported values are :

* `summary` : hash, type, version, sequence, timestamp, and contracts.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

#### Return value JSON object:

#### `get/blockhash`

```
{
    "hash": "289b76fbe40ca5e0d0df68016677a8c9be6389f3225cc9faf65cb92bbc98a3580b57cb7ba3d7f7ab36ce890d47a4be90254d407270ad4c2e69c99c998ef8547ca45f054b081959d4d59feb871360bce888f82b4cdc1476502bd1b8e95dffe811e55592d3d4768b56e0510e02b5e32171858de097cd811e7c8d09920c24960ae8"
}
[Completed in 0.083791 ms]
```

#### `get/block`

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

## `list`

This will list off all of the object register details specified by the noun.&#x20;

```
finance/list/noun
```

This command  supports the `blockhash`, `block` and `transaction` nouns.

`list/blockhash`

Retrieves information for a specified NXS or token account.

`list/blocks`

Retrieves information for a specified `trust` account.

`list/transactions`

Retrieves information for a specified token address.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

The parameters used are [Query DSL](ledger.md#query-dsl), along with the [sorting / filtering](ledger.md#user-content-create)

#### `list/blocks`

```
[
    {
        "hash": "048f3b308e8bd8c1aa31ec1ec2e136a9ccc91ec4498283d07fc5d0a00c8576e2c199567a44058222961f474626c6f2c5d7e774eee34c34f98acafaeb50b7abaaade7e9c641fe9727fe62533b1ec6bf2f75ffbf19d17d74671e2458bd73b6407b4bba1951fc84e1af11c2c4fbce1d05d7739e910fdb8a37197c1c422521e2e9f3",
        "proofhash": "73c5705c9689421fd0e62011993b896a1467e25c254a917132bcae114af5a411b21d74e9950e896933753cabc39dc92fa74f392d585c4be072a16cda7a9c5f8c061ee18acbc89cf3ff57968416b945b818603afb766a5b09acb6ab34183ed5bb4ce0b2e2e388e90cedc2a45edf29fb355bde0d5d9d7fa5e0201886fa3442faca",
        "size": 862,
        "height": 41,
        "channel": 3,
        "version": 8,
        "merkleroot": "ff8534c57ad64d914a9c65c08272196a19ae1cf425500860f630a5e71b5f3eddac384e31bbc9f18bf15f50b958a6c191d7c5672047eb81ffdbb9f12c2106616d",
        "timestamp": 1655060221,
        "date": "2022-06-12 18:57:01 UTC",
        "nonce": 1,
        "bits": "7e7fffff",
        "difficulty": 31.999515,
        "mint": 0.0,
        "previousblockhash": "ebddd2263aefc82e599e38a03faba3fa3719c5c5730079475c19d0442ac64eb55be59780abb7fd7b42ab84114f1538d57172758d55122e3aee3c98766f1063e5d7138e07969b7f9f4d9842a1c3e59576a31c63faec6bc392ac5639c971d7b6ef2d3ab61eb7d36d7c9e3d47cfa48cffa23354c2a2dc126dcf6a23b070c6c69e39",
        "nextblockhash": "39628fce35bcb6b90f76524ff4acff96ae6216e92d62499c89220721f4fd53bbe060da1ba2e46053f2b3eff28a5e545f3b9e5f82f570eddf8a88010d40c39843cdc6e0487e3aa47d74e143dc47cec97a72bf36f30d4f1aadebb8746918a99bfdeff46527ca5e3248f780fac861b7f83df2691804f504eea52fac2de1d1c6eddf",
        "tx": [
            {
                "txid": "01f1a3f9227a69382f9811a5b1497a865ace17ad83b03118b24f875f6ade83117887c35d08375c259aa1076b91f42206110314756a11a943760bb5c0dd0523d7"
            },
            {
                "txid": "01e4fb90b11aae48f4e824a8f78fd7ffcc429610450d18a3259f3d92fb5aef10f1e7d87774e2f803c1c272f26e036080ab07887bf0a83e17e4c8209ce590d5e6"
            }
        ]
    }
]
[Completed in 0.488042 ms]
```

```
[
    {
        "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
        "version": 1,
        "created": 1654808816,
        "modified": 1655061950,
        "type": "OBJECT",
        "balance": 5998.0,
        "decimals": 2,
        "currentsupply": 4002.0,
        "maxsupply": 10000.0,
        "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
        "ticker": "XYZ",
        "address": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG"
    }
]
[Completed in 0.473708 ms]
```



`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`name` : The name identifying the account. For privacy purposes, this is only included in the response if the caller is the owner of the account

`address` : The register address of the account.

`ticker` : This will always be set to `NXS`.

`token` : This will always be set to 0 for NXS accounts.

`data` : The arbitrary data included in the account register. If no data was included during the account creation then this field will be omitted.

`balance` : The available balance of this account. This is the last confirmed balance less any new debits that you have made since the last block

`pending` : This is the sum of all confirmed debit transactions that have been made to this account, that have not yet been credited. To move coins from pending into the available balance you must create a corresponding credit transaction. NOTE: if configured to run, the events processor does this for you.

`unconfirmed` : This is the sum of all unconfirmed debit transactions that have been made to this account PLUS the sum of all unconfirmed credits that you have for confirmed debit transactions. When someone makes a debit to your account it will immediately appear in the unconfirmed balance until that transaction is included in a block, at which point it moves into `pending`. When you (or the events processor) creates the corresponding credit transaction for that debit, the amount will move from `pending` back into `unconfirmed` until the credit transaction is included in a block, at which point the amount moves to `balance`.

`stake` : Only returned for the trust account, this is the amount of NXS currently staked in the trust account.

`count` : Only returned if the caller requested `count` : true. This is the number of transactions made to/from the account.

## `submit`

Submits a transaction to be included in the mempool and broadcast to the network.

```
ledger/submit/noun
```

This command  supports the `transaction` noun.

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

## `void`

Voids (reverses) a debit or transfer transaction that you have previously made, that has not yet been credited or claimed by the recipient. The method creates a corresponding credit or claim transaction but back to the originating account/signature chain. This means that any applicable fees will apply, as will conditions on the debit/transfer transaction (such as expiration conditions).

For debits that were made to a tokenized asset as part of a split payment transaction, the reversing credit will be made for the debit amount minus any partial amounts that have already been credited by the token holders.

```
ledger/void/noun
```

This command  supports the `transaction` noun.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : The **transaction ID** (hash) of the debit or transfer transaction that you wish to void.

#### Return value JSON object:

```
{
    "hash": "47959e245f45aab773c0ce5320a5454f49ac15f63e15acb36855ac654d54d6314fe36b61dd64ec7a9a546bcc439a628e9badcdccb6e5f8072d04a0a3b67f8679"
}
```

#### Return values:

`hash` : The transaction hash of the credit/claim transaction, if successfully committed to the mempool / broadcast.

## `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

[`get/info`](ledger.md#get-info)\
[`sync/headers`](ledger.md#sync-headers)\
Direct endpoints support filters and operators.

### `get/info`

Retrieves mining related data for the ledger.

```
ledger/get/info
```

#### Parameters:

\-None-

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



### `sync/headers`

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
