---
description: FINANCE API
---

# FINANCE

The Finance API provides methods for sending and receiving NXS or other tokens between users / accounts, creating accounts, and managing staking. The full supported endpoint of the finance URI is as follows:

```
finance/verb/noun/filter/operator
```

The minimum required components of the URI are:

## `Supported Operators`

The following operators are supported for this API command-set:

[`array`](../../../getting-started/tritium++-api/broken-reference/) - Generate a list of values given from a set of filtered results.\
[`mean`](../../../getting-started/tritium++-api/broken-reference/) - Calculate the mean or average value across a set of filtered results.\
[`sum`](../../../getting-started/tritium++-api/broken-reference/) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
finance/list/accounts/balance/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

***

## `Supported Filters`

This command-set supports single or csv field-name filters.

**Example:**

```
finance/list/accounts/balance,ticker
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
finance/transactions/account/contracts.OP
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

***

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](../../../getting-started/tritium++-api/broken-reference/) - Generate a new object of supported type.\
[`debit`](../../../getting-started/tritium++-api/broken-reference/) - Issue funds from supported type.\
[`credit`](../../../getting-started/tritium++-api/broken-reference/) - Claim funds issued to account from debit.\
[`burn`](finance.md#user-content-create-2) - Remove a given token from circulation.\
[`get`](../../../getting-started/tritium++-api/broken-reference/) - Get object of supported type.\
[`list`](../../../getting-started/tritium++-api/broken-reference/) - List all objects owned by given user.\
[`history`](../../../getting-started/tritium++-api/broken-reference/) - Generate the history of all last states.\
[`transactions`](../../../getting-started/tritium++-api/broken-reference/) - List all transactions that modified specified object.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`account`] - An object register containing a token-id and balance.\
\[`trust`] - An object register containing a token-id, balance, and trust.\
\[`token`] - An object register containing a token-id, balance, supply, and decimals.\
\[`any`] - An object selection noun allowing mixed accounts of different tokens.\
\[`all`] - An object selection noun to collect all accounts for given token type.

**Example:**

```
finance/debit/any
```

The above command will create a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

## `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

[`get/balances`](../../../getting-started/tritium++-api/broken-reference/)\
[`get/stakeinfo`](../../../getting-started/tritium++-api/broken-reference/)\
[`migrate/accounts`](../../../getting-started/tritium++-api/broken-reference/)\
[`set/stake`](../../../getting-started/tritium++-api/broken-reference/)

Direct endpoints support filters and operators.

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

This above will map to the parameters of `limit=100` and `offset=10`.

```
finance/get/balances/balance/sum
```

## `create` <a href="#user-content-create" id="user-content-create"></a>

Create a new object register specified by given noun.

```
finance/create/noun
```

This command does not support the  `trust,` `any` or `all` nouns.

#### create/account

Create a new account to hold NXS or tokens

#### create/token

Create a new token object.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`data` : Optional for `any` **noun**, allows caller to add arbitrary data to object.

#### **`create/token`**

`name` : Optional for **noun** `token` as a _UTF-8_ encoded string that will generate a global name object register that points to new object.

`supply` : Required by **noun** `token` that sets the maximum token supply.

`decimals` : Required by **noun** `token` that sets the total number of significant figures. Defaults to    2.

#### **`create/account`**

`name` : Optional for **noun** `account` as a _UTF-8_ encoded string that will generate a name object register that points to new object.&#x20;

`token` : Required by **noun** `account` as a _Base58_ encoded address or ticker name. Defaults to `NXS`.

#### Return value JSON object:

```
{
    "success": true,
    "address": "8BfLgEprhbHs82LxUkJR9jhQufRZf49g73Nt8XTGevfiyy7ijhb",
    "txid": "01854fe4fdf0d59aebb3a880141484f0542af061cbebfd468db3fcecd13f63a986d990cf669ca4a60822a82b2d4fc7e7e474801a01bff86a35fd0a147a5a62da"
}
[Completed in 4973.117685 ms]
```

#### Return values: <a href="#user-content-credit" id="user-content-credit"></a>

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be omitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

## `debit`

Deduct an amount of NXS or token specific by the noun and send it to another account or legacy UTXO address. Only NXS can be sent to the legacy address.

```
finance/debit/noun
```

This command supports the `any` wildcard noun.

#### debit/account

This deducts an amount of NXS or tokens from a token account or token address to be sent to a receiving account.

#### debit/token

This deducts an amount of tokens from a token generation account to send to a token account

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`from` : The name **identifying** the account to debit. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name.

`amount` : The **amount** of NXS to debit.

`to` : The name or register address **identifying** the receiving account. This is optional if address\_to is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name. The address\_to can also contain a legacy UTXO address if sending from a signature chain account to a legacy address.

`address_to` : The **register address** of the account to send to. This is optional if name\_to is provided. The address\_to can also contain a legacy UTXO address if sending from a signature chain account to a legacy address.

`reference` : Optional field allows callers to provide a r**eference**, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a `64-bit unsigned integer` in the range of 0 to 18446744073709551615

`expires` : Optional field allows callers to specify an **expiration** for the debit transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be credited by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01f51d6b23b871fc8da848afa57cf066cb9e3b8fb845a666335e8c678ef5249e98d4f3e477659098918e4bb590472a63d0ed0a17fa87904fcff6316158e9edfd"
}
[Completed in 4979.735275 ms]
```

#### Return values:

`success` : Boolean flag indicating that the debit was successful.&#x20;

`txid` : The ID (hash) of the transaction that includes the debit.

## `credit` <a href="#user-content-credit" id="user-content-credit"></a>

Create a new object register specified by given noun.

```
finance/credit/noun
```

This command supports the `any` wildcard noun.

#### credit/account

Increment an amount received from a NXS account, token account or token address.

#### credit/token

Increment an amount of tokens received from a token account.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : The hash in **hexadecimal** encoding of the transaction that we are crediting.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01f51d6b23b871fc8da848afa57cf066cb9e3b8fb845a666335e8c678ef5249e98d4f3e477659098918e4bb590472a63d0ed0a17fa87904fcff6316158e9edfd"
}
[Completed in 4979.735275 ms]
```

#### Return values:

`success` : Boolean flag indicating that the debit was successful.&#x20;

`txid` : The ID (hash) of the transaction that includes the credit.

## `burn` <a href="#user-content-create" id="user-content-create"></a>

This method can be used to take tokens permanently out of the current supply, a process commonly known as burning. The method will debit a token account and send the tokens back to the token address. However the transaction contains a condition that will always evaluate to false, guaranteeing that the debit can not be credited by the token issuer nor the sender. The result is that the amount burned will always appear in the "pending" balance of the token.

```
finance/burn/noun
```

This command only supports the `account` noun.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional for i**dentifying** the account to debit the tokens from to be burnt. This is optional if the `address` is provided.&#x20;

`address` :  Optional for **identifying** the register address of the account to debit the tokens from the be burnt. This is optional if the `name` is provided.

`amount` : The amount of **tokens** to burn.

`reference` : Optional field allows callers to provide a **reference**, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615

#### Return value JSON object:

```
{
    "success": true,
    "address": "8BfLgEprhbHs82LxUkJR9jhQufRZf49g73Nt8XTGevfiyy7ijhb",
    "txid": "01854fe4fdf0d59aebb3a880141484f0542af061cbebfd468db3fcecd13f63a986d990cf669ca4a60822a82b2d4fc7e7e474801a01bff86a35fd0a147a5a62da"
}
[Completed in 4973.117685 ms]
```

#### Return values:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be omitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

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

#### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`name` : The name identifying the account/trust/token. This is optional if the address is provided.

`address` : The register address of the account/trust//token to be transferred. This is optional if the name is provided.

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

This command  supports the `account`, `trust` and `token` nouns.

#### list/account

This lists all the NXS except '`trust` and token accounts for the logged in user.

#### list/trust

This lists the '`trust` account for the logged in user.

#### list/token

This lists all the token addresses for the logged in user.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

The parameters used are [Query DSL](finance.md#query-dsl), along with the [sorting / filtering](finance.md#user-content-create)

#### Return value JSON object:

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

#### Return values:&#x20;

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

## `history`

This will get the history of the specified noun.

```
finance/history/noun
```

This command supports the `account, trust and token` nouns.

#### history/account

This lists all the NXS except '`trust` and token accounts for the logged in user.

#### history/trust

This lists the '`trust` account for the logged in user.

#### history/token

This lists all the token addresses for the logged in user.

#### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`name` : The name identifying the account/trust/token. This is optional if the address is provided.

`address` : The register address of the account/trust//token to be transferred. This is optional if the name is provided.

#### Return value JSON object:

```
[
    {
        "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
        "version": 1,
        "created": 1654808903,
        "modified": 1654809207,
        "type": "OBJECT",
        "balance": 1000.0,
        "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
        "ticker": "XYZ",
        "address": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
        "name": "local:XYZToken",
        "action": "CREDIT"
    },
    {
        "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
        "version": 1,
        "created": 1654808903,
        "modified": 1654808903,
        "type": "OBJECT",
        "balance": 0.0,
        "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
        "ticker": "XYZ",
        "address": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
        "name": "local:XYZToken",
        "action": "CREATE"
    }
]
[Completed in 12.427076 ms]
```

#### Return value :

## `transactions`

This will list off all of the transactions for the specified noun.

```
finance/transactions/noun
```

This command only supports the `master` noun.

#### transactions/account

Increment an amount received from a NXS account or tokens from a token or token account.

#### tranactions/token

Increment an amount of tokens received from a token account.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the [Sorting / Filtering](finance.md#sorting-filtering) parameters.

#### Return value JSON object:

```
[
    {
        "txid": "0123517ca0f1ca110c7b07de9e3c9b33ccbe717f96911e1449b7c73bb9695fbc9c14a58f01f5fb7e9b64756f658af91daec9f0f579df2fad8df61843defae833",
        "type": "tritium user",
        "version": 4,
        "sequence": 23,
        "timestamp": 1655061950,
        "blockhash": "8b206ab2ee4b46a835f74af0ff5d4e0b395acdb94d66468a24083f2a5fd01a07a93956774001bab1a801d53d7bf6ed60ee84a573650eef1a9feaf6fa9beb308bd20b567663cc7ec4f85796b261164ef3452ebfaa13a60141b42fc49d6d2eb2792440925b1b19248ad9fe65e01d3742f2d3dec2817c56c8e4f6e03a10f4147308",
        "confirmations": 4,
        "contracts": [
            {
                "id": 0,
                "OP": "DEBIT",
                "from": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "to": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
                "amount": 1.0,
                "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "ticker": "XYZ",
                "reference": 57891358795
            }
        ]
    },
    {
        "txid": "01f1a3f9227a69382f9811a5b1497a865ace17ad83b03118b24f875f6ade83117887c35d08375c259aa1076b91f42206110314756a11a943760bb5c0dd0523d7",
        "type": "tritium user",
        "version": 4,
        "sequence": 21,
        "timestamp": 1655060214,
        "blockhash": "048f3b308e8bd8c1aa31ec1ec2e136a9ccc91ec4498283d07fc5d0a00c8576e2c199567a44058222961f474626c6f2c5d7e774eee34c34f98acafaeb50b7abaaade7e9c641fe9727fe62533b1ec6bf2f75ffbf19d17d74671e2458bd73b6407b4bba1951fc84e1af11c2c4fbce1d05d7739e910fdb8a37197c1c422521e2e9f3",
        "confirmations": 6,
        "contracts": [
            {
                "id": 0,
                "OP": "DEBIT",
                "from": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "to": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
                "amount": 1.0,
                "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "ticker": "XYZ",
                "reference": 0
            }
        ]
    }
]
[Completed in 2.187165 ms]
```

#### Return values:

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

`object` : Returns a list of all hashed public keys in the crypto object register for the specified profile. The object result will contain the nine default keys**`(`**`app1,` `app2, app3,` `auth, cert` `lisp,` `network,` `sign`  and `verify).`

***
