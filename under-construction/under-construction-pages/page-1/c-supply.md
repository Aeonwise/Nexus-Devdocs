---
description: SUPPLY API
---

# SUPPLY

The Supply API provides functionality to support the ownership transfer requirements typical of a supply chain process. Items in the supply chain can be given a `data` value and this value can be updated over time. Items are stored in an APPEND register, meaning that changes to the item are recorded in sequence in the register. This in turn means that a history of changes to the `data` field, as well as the history of ownership of the item, can be obtained.

The full supported endpoint of the profiles URI is as follows:

```
profiles/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
profiles/verb/noun
```

## `Supported Operators`

The operators only work with the profiles `transactions` and `notifications` verbs. The following operators are supported for this API command-set:&#x20;

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
supply/transactions/master/contracts.amount/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "amount": 5150.0
}
[Completed in 2.440583 ms]
```

## `Supported Filters`

The filters only work with the profiles `transactions` and `notifications` verbs. This command-set supports single or csv field-name filters.&#x20;

**Example:**

```
supply/list/items/address,name
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
`get` - Get object of supported type.\
`list` - List all objects owned by given user.\
`update` - Update a specified object.\
`transfer` - Transfer a specified object register.\
`claim` - Claim a specified object register.\
`history` - Generate the history of all last states.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`item`] - The default profile that controls all sub-profiles.\


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

`format` : The format the caller is using to **define** the asset. Values can be `basic` (the default), `raw`, `JSON`, `ANSI` (not currently supported), or `XML` (not currently supported). This is an optional field and the value `basic` is assumed if omitted.

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



***
