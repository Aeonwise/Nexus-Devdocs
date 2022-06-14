---
description: LEDGER API
---

# LEDGER

The Ledger API provides users with access to data held by the ledger such as blocks and transactions. The full supported endpoint of the ledger URI is as follows:

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
ledger/list/accounts/balance/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "balance": 333.376
}
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
`submit` - To submit the modified object state to the mempool.\
`void` - To void a debit transaction and credit it back to the same account.\
`sync`- Claim funds issued to account from debit.\


### `Supported Nouns`

The following nouns are supported for this API command-set:

\[`block`] - A block is a cryptographic object that contains transactions in a specific sequence of time.\
\[`blockhash`] - A blockhash is a cryptographic hash of a block.\
\[`info`] - An object register containing specific information.\
\[`transactions`] - An operation which modified a specified object.\
\[`headers`] - A cryptographic hash identifier for each block.\
\[`any`] - An object selection noun allowing mixed accounts of different tokens.\
\[`all`] - An object selection noun to collect all accounts for given token type.

**Example:**

```
ledger/list/tranactions
```

The above command will list all the transactions a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

***

### ``

### `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

`get/info`\
`sync/headers`\
Direct endpoints support filters and operators.

***

### `Sorting / Filtering` <a href="#user-content-create" id="user-content-create"></a>

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
names/get/noun
```

This command  supports the `account`, `trust` and `token` nouns.

`get/account`

Retrieves information for a specified NXS or token account.

`get/trust`

Retrieves information for a specified `trust` account.

`get/token`

Retrieves information for a specified token address.

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`name` : The name identifying the account/trust/token. This is optional if the address is provided.

`address` : The register address of the account/trust//token to be transferred. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
{
    "owner": "b1b5b4f4197548886016586f95735f0cb8235183a9185b8720bd27502a2e2850",
    "version": 1,
    "created": 1638020495,
    "modified": 1655118914,
    "type": "OBJECT",
    "balance": 300.536104,
    "stake": 15000.0,
    "token": "0",
    "ticker": "NXS",
    "trust": 3399813,
    "age": "39 days, 8 hours, 23 minutes",
    "rate": 3.0,
    "address": "8EunQ82qVdnuQkX2gXKZr5P55kQRz4KbpaLdCVBjBNu8jeys4C4"
}
[Completed in 0.301107 ms]
```

#### Return values:

## `list`

This will list off all of the object register details specified by the noun.&#x20;

```
finance/list/noun
```

This command supports the `account, trust and token` nouns.

#### list/account

This lists all the NXS except '`trust` and token accounts for the logged in user.

#### list/trust

This lists the '`trust` account for the logged in user.

#### list/token

This lists all the token addresses for the logged in user.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

The parameters used are [Query DSL](c-ledger.md#query-dsl), along with the [sorting / filtering](c-ledger.md#user-content-create)

### Results:

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

The results depends on the specified noun.&#x20;

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

## `get/info`

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
