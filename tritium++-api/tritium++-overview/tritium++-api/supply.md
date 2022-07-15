---
description: SUPPLY API
---

# SUPPLY

The Supply API provides functionality to support the ownership transfer requirements typical of a supply chain process. Items in the supply chain can be given a value and this value can be updated over time. The supply API supports the `readonly`, `raw`, `basic` and `JSON` formats and the user is required to specify the format.&#x20;

The `readonly` and `raw` formats are useful when developers wish to store arbitrary data, without incurring the overhead of defining an object. The value will be provided with the `data` parameter. The `readonly` format cannot be updated and is stored in a readonly register. The `raw` format is stored in a raw register and allows the data to be updated.

The `basic` format allows callers to define an asset in terms of simple key=value pairs. It assumes all values are stored using the string data type. The key=value pairs can be updated.

The `JSON` format allows callers to provide a detailed definition of the supply data structure, with each field defined with a specific data type and mutability. Items defined with one of the complex formats can be updated after their initial creation. &#x20;

The supply API also provides a history of changes to the values, as well as the history of ownership of the item.

The full supported endpoint of the supply URI is as follows:

```
supply/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
supply/verb/noun
```

## `Supported Filters`

This command-set supports single or csv field-name filters.&#x20;

**Example:**

```
supply/transactions/item/
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
supply/tranasctions/item/contracts.OP
```

When using recursive filtering, the nested hierarchy is retained.

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
                "OP": "WRITE"
            }
        ]
    }
]
[Completed in 0.722042 ms]
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](supply.md#create) - Generate a new object of supported type.\
[`get`](supply.md#get) - Get object of supported type.\
[`list`](supply.md#list) - List all objects owned by given user.\
[`update`](supply.md#update) - Update a specified object.\
[`transfer`](supply.md#transfer) - Transfer a specified object register.\
[`claim`](supply.md#claim) - Claim ownership of an object register from a transfer.\
[`history`](supply.md#history) - Generate the history of all last states.\
[`transactions`](supply.md#transactions) - List all transactions that modified specified object.\


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

This command only supports the `item`, raw and `readonly` nouns..

#### create/item

Creates a new item on an object register.

#### create/raw

Creates a new item on a raw register.

#### create/readonly

Creates a new item on a readonly register.

#### create/any

Creates a new item specified by the format parameter. &#x20;

### Parameters:

`pin` : Required if **locked**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional **name** to identify the asset. If provided a Name object will also be created in the users local namespace, allowing the asset to be accessed/retrieved by name. If no name is provided the asset will need to be accessed/retrieved by its 256-bit register address.

`format` : Required to **identify** the format used to define the asset. Values can be `readonly`, `raw`, `basic` and`JSON`.

`data` : Required if format is `readonly` or `raw`, then this field contains the hex-encoded data to be stored in this asset. Raw assets are always read-only. All other preceding fields are ignored.

`<fieldname>=<value>` : If format is `basic`, then the caller can provide additional = pairs for each piece of data to store in the asset.

`json` : If format is `JSON`, then this field will hold the json definition of the asset as a JSON array of objects representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `value` : The default value of the field.
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).
* `maxlength`: Only applicable to `string` or `bytes` fields where `mutable`=true, this is the maximum number of characters (bytes) that can be stored in the field. If no maxlength parameter is provided then we will default the field size to the length of the default value rounded up to the nearest 64 bytes.

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "address": "87PUYqzvL73Ta81VfNMWuNyVjAqy3ZzE2iiX6FawibeNY56XC1u",
    "txid": "012d0190164584f6174c0c5b1cfa7c155a7faeb453b050ee4cc520681b47e52402d9aab5cdf327ee614752c4f40647d4bd3b32364db19da2bacb83c5f71a9144"
}
[Completed in 4970.165011 ms]
```

#### Return values:

`success` : Boolean flag indicating that the item was saved successfully.

`address` : The register address for this new created item. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

`txid` : The ID (hash) of the transaction that includes the created object.

## `update`

Update an object register specified by given noun.

```
supply/update/noun
```

This command only supports the `item` and `raw` nouns.

#### update/item

This updates the values of basic and JSON formats with the item noun.

#### update/raw

This updates the data value of the raw item.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Required to  **identify** the item to update by name.  This is optional if the `address` is provided.

`address` : Required to **identify** the item to update by register address. This is optional if the `name` is provided.

`data` : The new value of the data field in this item

### Results:

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

This command supports the `item`,  `raw`, `readonly` and `any` nouns.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the item. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
[
    {
        "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
        "version": 1,
        "created": 1656664801,
        "modified": 1656665317,
        "type": "OBJECT",
        "ABW": "DH0001222145565",
        "Address": "Marks street, PO Box:7887",
        "Item": "Samsung Watch",
        "address": "87wQAJ6nTWqVhGB423mKqtAJyF4B6S5WUXMqzYQWRcH9rJZxZup",
        "name": "local:Item0002"
    }
]
[Completed in 34.175112 ms]
```

#### Return Values:

`owner` : The username hash of the profile that owns this asset.

`created` : The UNIX timestamp when the asset was created.

`modified` : The UNIX timestamp when the asset was last modified.

`type` : Asset register type. Can be `OBJECT`, `RAW` or `READONLY`

`<fieldname>=<value>` : The key-value pair for each piece of data stored in the asset.`name` : The name identifying the item. For privacy purposes, this is only included in the response if the caller is the owner of the item

`data` : The data stored in the item.

`address` : The register address of the item.

`name` : The name identifying the asset. For privacy purposes, this is only included in the response if the caller is the owner of the asset

## list

Retrieves information for a single object for a type specified by the noun

```
supply/list/noun
```

This command supports the `item`,  `raw`, `readonly` and `any` nouns.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

#### Return value JSON object:

```
[
    {
        "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
        "version": 1,
        "created": 1656664801,
        "modified": 1656665317,
        "type": "OBJECT",
        "ABW": "DH0001222145565",
        "Address": "Marks street, PO Box:7887",
        "Item": "Samsung Watch",
        "address": "87wQAJ6nTWqVhGB423mKqtAJyF4B6S5WUXMqzYQWRcH9rJZxZup",
        "name": "local:Item0002"
    },
    {
        "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
        "version": 1,
        "created": 1656662631,
        "modified": 1656663298,
        "type": "OBJECT",
        "ABW": "FED456315463135",
        "Address": "Avenue street, PO Box:4587",
        "Item": "Mobile Holder",
        "address": "87nyrW2TiKX9gZwRi61q3JNoVQK3GGTpZxBJBwR634au1A8Arc3",
        "name": "local:Item0001"
    }
]
[Completed in 34.175112 ms]
```

#### Return Values:

`owner` : The username hash of the profile that owns this asset.

`created` : The UNIX timestamp when the asset was created.

`modified` : The UNIX timestamp when the asset was last modified.

`type` : Asset register type. Can be `OBJECT`, `RAW` or `READONLY`

`<fieldname>=<value>` : The key-value pair for each piece of data stored in the asset.`name` : The name identifying the item. For privacy purposes, this is only included in the response if the caller is the owner of the item

`data` : The data stored in the item.

`address` : The register address of the item.

`name` : The name identifying the asset. For privacy purposes, this is only included in the response if the caller is the owner of the asset

## `transfer`

This will initiate ownership transfer of the specified noun.

```
supply/transfer/noun
```

This command supports the `item`,  `raw` and `readonly` nouns.

#### transfer/item

This initiates transfer for an item to a recipient.

#### transfer/raw

This initiates transfer for a raw item to a recipient.

#### transfer/readonly

This initiates transfer for a readonly item to a recipient.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional name **identifying** the item to be transferred. This is optional if the address is provided.

`address` : Optional register address to **identify** the item to be transferred. This is optional if the name is provided.

`recipient` : Required to **identify** the profile to transfer the item to. This is optional if the username is provided.

`expires` : This optional field allows callers to specify an **expiration** for the transfer transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

### Results:

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

This command supports the `item`,  `raw` and `readonly` nouns.

#### claim/item

This claims ownership of an item which has been transferred.

#### claim/raw

This claims ownership of a raw item which has been transferred.

#### claim/readonly

This claims ownership of a readonly item which has been transferred.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : Required the **transaction ID** (hash) of the item transfer transaction for which is being claimed.

`name` : Optional field allows the user to **rename** an item when it is claimed. By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.

### Results:

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

This command supports the `item`,  `raw`, `readonly` and `any` nouns.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name **identifying** the `item`.  This is optional if the address is provided.

`address` : The **register address** of the item. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
[
    {
        "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
        "version": 1,
        "created": 1656662631,
        "modified": 1656663298,
        "type": "OBJECT",
        "ABW": "FED456315463135",
        "Address": "Avenue street, PO Box:4587",
        "Item": "Mobile Holder",
        "address": "87nyrW2TiKX9gZwRi61q3JNoVQK3GGTpZxBJBwR634au1A8Arc3",
        "name": "local:Item0001",
        "action": "CLAIM"
    },
    {
        "owner": "b7392196b83aca438567558462cd0c5d982569c7cefa668500c4bf3e61a03b7a",
        "version": 1,
        "created": 1656662631,
        "modified": 1656662631,
        "type": "OBJECT",
        "ABW": "FED456315463135",
        "Address": "Avenue street, PO Box:4587",
        "Item": "Mobile Holder",
        "address": "87nyrW2TiKX9gZwRi61q3JNoVQK3GGTpZxBJBwR634au1A8Arc3",
        "name": "local:Item0001",
        "action": "CREATE"
    }
]
[Completed in 9.872044 ms]
```

#### Return values:

## `transactions`

This will list off all of the transactions for the specified noun.

```
supply/transactions/noun
```

This command supports the `item`,  `raw`, `readonly` and `any` nouns.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the [Sorting / Filtering](supply.md#sorting-filtering) parameters.

### Results:

#### Return value JSON object:

```
[
    {
        "txid": "017bf6e36b6986f268a5bb7e7cca6dec5fdbe4ab2b5000d758dc7220bd2e455df487b3b6703fee449b96f5305205df3f93fb31a65261906a64698a9e0702e5f6",
        "type": "tritium user",
        "version": 4,
        "sequence": 5,
        "timestamp": 1656663298,
        "blockhash": "3a4405e03279480c34ec6b8319591bb0974e238f207f3a18576d0c0182dab7dcae3736d322354be9796d80071151358a521f5289b8d859a99bb474431922d93a8ab9ac1b59aa03a21568fdb9b6c7921d908a2b4ff1622023b443425cc983be96da0d93be22623258ca87d537e1cc3cb055e76c19aaed3c81de9c028355564e9d",
        "confirmations": 39,
        "contracts": [
            {
                "id": 0,
                "OP": "CLAIM",
                "txid": "0127324285cf5c57751dd20b5d8c7d17f204101dc1625230a8199d53d0059c3e3ad1a36330d56b2442b80bed1bb65aef9b049d671d800308d807a29174db4663",
                "contract": 0,
                "address": "87nyrW2TiKX9gZwRi61q3JNoVQK3GGTpZxBJBwR634au1A8Arc3"
            }
        ]
    },
    {
        "txid": "01f5ab29bd8df00da8354d67ca337beb5091406c5d9356787ee9a1197662587ce46622fb3b3173b8d9071de4240073d87a5e2fcb4f5637abd6bad9deb2fb0072",
        "type": "tritium user",
        "version": 4,
        "sequence": 2,
        "timestamp": 1656662631,
        "blockhash": "fa0b6c7b3d58b38a5fd3d3ad5d1fd3e03d225e69cf8a3b1ef72c6a30caf9f46056b5094482c4e8b1966b313954a367e60a804451f72895dd6fd169efe4d6e6db981941e4f3203890d3e07cc1d111cf5c77136095959ee15ca552109949f88026f38f838ac30841d8491cd071493761de19248b7ff7001f4b43cd2585f5b08570",
        "confirmations": 41,
        "contracts": [
            {
                "id": 0,
                "OP": "CREATE",
                "address": "87nyrW2TiKX9gZwRi61q3JNoVQK3GGTpZxBJBwR634au1A8Arc3",
                "type": "OBJECT",
                "standard": "NONSTANDARD",
                "object": {
                    "ABW": "FED456315463135",
                    "Address": "Avenue street, PO Box:4587",
                    "Item": "Mobile Holder"
                }
            }
        ]
    }
]
[Completed in 8.149978 ms]
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
