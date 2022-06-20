---
description: SUPPLY API
---

# SUPPLY

The Supply API provides functionality to support the ownership transfer requirements typical of a supply chain process. Items in the supply chain can be given a `data` value and this value can be updated over time. Items are stored in an APPEND register, meaning that changes to the item are recorded in sequence in the register. This in turn means that a history of changes to the `data` field, as well as the history of ownership of the item, can be obtained.

The full supported endpoint of the supply URI is as follows:

```
supply/verb/noun/filter
```

The minimum required components of the URI are:

```
supply/verb/noun
```

## `Supported Filters`

This command-set supports single or csv field-name filters.&#x20;

**Example:**

```
supply/transactions/item
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
supply/list/items/contracts.OP
```

When using recursive filtering, the nested hierarchy is retained.

```
[
    {
        "contracts": [
            {
                "OP": "DEBIT"
            }
        ]
    },
    {
        "contracts": [
            {
                "OP": "WRITE"
            }
        ]
    }
]
[Completed in 0.722042 ms]
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](c-supply.md#create) - Generate a new object of supported type.\
[`get`](c-supply.md#get) - Get object of supported type.\
[`list`](c-supply.md#list) - List all objects owned by given user.\
[`update`](c-supply.md#update) - Update a specified object.\
[`transfer`](c-supply.md#transfer) - Transfer a specified object register.\
[`claim`](c-supply.md#claim) - Claim ownership of an object register from a transfer.\
[`history`](c-supply.md#history) - Generate the history of all last states.\
[`transactions`](c-supply.md#transactions) - List all transactions that modified specified object.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`item`] - An object register containing an item object.\
\[`raw`] - An object register containing raw object.\
\[`readonly`] - An object register containing read-only object.\
\[`any`] - An object selection noun allowing mixed items.

## `Sorting / Filtering`

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

## `create`

Create a new object register specified by given noun.

```
supply/create/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional **name** to identify the asset. If provided a Name object will also be created in the users local namespace, allowing the asset to be accessed/retrieved by name. If no name is provided the asset will need to be accessed/retrieved by its 256-bit register address.

`format` : Required to **identify** the format used to define the asset. Values can be `basic` (the default), `raw`, `JSON`, `ANSI` (not currently supported), or `XML` (not currently supported). This is an optional field and the value `basic` is assumed if omitted.

`data` : If format is `raw`, then this field contains the hex-encoded data to be stored in this asset. Raw assets are always read-only. All other preceding fields are ignored.

`<fieldname>=<value>` : If format is `basic`, then the caller can provide additional = pairs for each piece of data to store in the asset.

`json` : If format is `JSON`, then this field will hold the json definition of the asset as a JSON array of objects representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `value` : The default value of the field.
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).
* `maxlength`: Only applicable to `string` or `bytes` fields where `mutable`=true, this is the maximum number of characters (bytes) that can be stored in the field. If no maxlength parameter is provided then we will default the field size to the length of the default value rounded up to the nearest 64 bytes.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```

#### Return values:

`success` : Boolean flag indicating that the item was saved successfully.

`txid` : The ID (hash) of the transaction that includes the created object.

## `update`

Update an object register specified by given noun.

```
supply/update/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional to  **identify** the item to update.  This is optional if the `address` is provided.

`address` : Optional **register address** to identify the `item` to update. This is optional if the `name` is provided.

`data` : The new value of the data field in this item

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction for the updated object.

## `get`

Retrieves information for a single object for a type specified by the noun

```
supply/get/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the item. This is optional if the name is provided.

#### Return value JSON object:

## list

Retrieves information for a single object for a type specified by the noun

```
supply/list/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

#### Return value JSON object:

## `transfer`

This will initiate ownership transfer of the specified noun.

```
supply/transfer/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional name **identifying** the item to be transferred. This is optional if the address is provided.

`address` : Optional register address to **identify** the item to be transferred. This is optional if the name is provided.

`recipient` : Required to **identify** the profile to transfer the item to. This is optional if the username is provided.

`expires` : This optional field allows callers to specify an **expiration** for the transfer transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the name transfer.

`address` : The register address for this name.

## `claim`

This method will claim ownership of the specified noun by the recipient to complete the transaction.

```
supply/claim/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : Required the **transaction ID** (hash) of the item transfer transaction for which is being claimed.

`name` : Optional field allows the user to **rename** an item when it is claimed. By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.

#### Return value JSON object:

```
{
    "claimed":
    [
        "25428293b6631d2ff55b3a931926fec920e407a56f7759495e36089914718d68",
        "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2"
    ],
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
}
```

#### Return values:

`claimed`: Array of addresses for each name that was claimed by the transaction

`txid` : The ID (hash) of the transaction that includes the name transfer.

***

## `history`

This will get the history of changes to an item, including both the data and it's ownership.

```
supply/history/noun
```

This command only supports the `item` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name **identifying** the `item`.  This is optional if the address is provided.

`address` : The **register address** of the item. This is optional if the name is provided.

#### Return value JSON object:

```
[
    {
        "type": "CREATE",
        "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1560492280,
        "checksum": 5612332250743384100,
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "name": "paul",
        "namespace": "test",
        "register_address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
    }

]
```

#### Return values:

## `transactions`

This will list off all of the transactions for the specified noun.

```
supply/transactions/noun
```

This command supports the `account, trust and token` nouns.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the [Sorting / Filtering](c-supply.md#sorting-filtering) parameters.

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

`object` : Returns a list of all hashed public keys in the crypto object register for the specified profile. The object result will contain the nine default keys**`(`**`app1,` `app2, app3,` `auth, cert` `lisp,` `network,` `sign`  and `verify)`

***
