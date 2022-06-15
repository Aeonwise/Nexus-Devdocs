---
description: ASSETS API
---

# ASSETS

An asset is a user-defined data structure that is stored in an object register, owned by a particular signature chain. Assets can hold one or more pieces of data and users can define the fields (name, data type, mutability) that data is stored in. The assets API supports several formats for defining the object data structures giving users and developers varying levels of functionality, including per-field type definition and mutability.

The `basic` format allows callers to define an asset in terms of simple key-value pairs. It assumes that all fields are read-only and all values are stored using the string data type. `basic` assets are always immutable and cannot be updated once created.

The `JSON`, `ANSI`, and `XML` \~formats allow callers to provide a detailed definition of the asset data structure, with each field defined with a specific data type and mutability. Assets defined with one of the complex formats can be updated after their initial creation.

The `raw` format differs from the other formats in that the asset data is not stored in object register, but instead is stored in a read-only state register. The raw format is useful when developers wish to store arbitrary binary data on the Nexus blockchain, without incurring the overhead of defining an object.

The full supported endpoint of the assets URI is as follows:

```
assets/verb/noun/filter
```

The minimum required components of the URI are:

```
assets/verb/noun
```

### `Supported Operators`

The following operators are supported for this API command-set:

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
assets/list/accounts/balance/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "balance": 333.376
}
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](assets.md#create) - Generate a new object of supported type.\
`get` - Get object of supported type.\
`list` - List all objects owned by given user.\
`update` - Update a specified object.\
`transfer` - Transfer a specified object register.\
`claim` - Claim ownership of an object register from a transfer.\
`tokenize` - To represent ownership of an asset object with a token object.\
`transactions` - List all transactions that modified specified object.\
`history` - Generate the history of all last states.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`asset`] - An object register containing an asset object.\
\[`raw`] - An object register containing a raw object.\
\[`readonly`] - An object register containing a read-only object.\
\[`any`] - An object selection noun allowing mixed assets.\
\[`schema`] - An object register containing a user-defined asset object.\
\
**Example:**

```
assets/list/asset
```

The above command will list all the assets for a user account.

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

## `create` <a href="#user-content-create" id="user-content-create"></a>

Create a new object register specified by given noun.

```
assets/create/noun
```

This command supports the `asset` noun.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional for **noun** `name` as a _UTF-8_ encoded string that will generate a name object register that points to new object. If noun is `token` this will be created as a global name.

`format` : The format the caller is using to define the asset. Values can be `basic` (the default), `readonly`, `raw`, `JSON.` This is an optional field and the value `basic` is assumed if omitted.

`data` : Optional for **any** noun, allows caller to add arbitrary data to object.

`<fieldname>=<value>` : If format is `basic`, then the caller can provide additional = pairs for each piece of data to store in the asset.

`json` : If format is `JSON`, then this field will hold the json definition of the asset as a JSON array of objects representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `value` : The default value of the field.
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).
* `maxlength`: Only applicable to `string` or `bytes` fields where `mutable`=true, this is the maximum number of characters (bytes) that can be stored in the field. If no maxlength parameter is provided then we will default the field size to the length of the default value rounded up to the nearest 64 bytes.

{% hint style="warning" %}
There is a limit of 1KB for asset data to be saved in the register, excluding the Asset Name
{% endhint %}

#### Return value JSON object:

```
{
    "success": true,
    "address": "87VmNhitFJv3WA3Yrovt9A3hts2MoXcfExyy9LiXyhK1sdThwYM",
    "txid": "01230bbc8f0d72aaaff13471e34520d17902290de9a1e5ce1f320f0883024e2d96e21b59a3e48b8d2b5ba2874e93d5d3b4f412e06dc92440c2e12682c958fe34"
}
[Completed in 4998.584942 ms]
```

#### `Return Values:`

`success` : Boolean flag indicating that the asset was saved successfully.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

## `get`

Retrieves information for a single object for a type specified by the noun

```
assets/get/noun
```

This command only supports the `asset` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the item. This is optional if the name is provided.

#### Return value JSON object:

## list

Retrieves information for a single object for a type specified by the noun

```
assets/list/noun
```

This command only supports the `asset` noun.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

#### Return value JSON object:

```
[
    {
        "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
        "version": 1,
        "created": 1655272802,
        "modified": 1655278668,
        "type": "OBJECT",
        "data": "{Name: Asset1, Owner: John, Registration No: BC87456123, Location: Mangalore, Address: 1021, Avenue Street}",
        "address": "87VmNhitFJv3WA3Yrovt9A3hts2MoXcfExyy9LiXyhK1sdThwYM",
        "name": "local:Asset1"
    }
]
[Completed in 0.285125 ms]
```

## `transfer`

This will initiate ownership transfer of the specified noun.

```
assets/transfer/noun
```

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
    "success": true,
    "address": "87VmNhitFJv3WA3Yrovt9A3hts2MoXcfExyy9LiXyhK1sdThwYM",
    "txid": "01bc48f80792fd4b97d43555d5993f0edfb0998ab14bcf159404f52e34a64abd6769f61014f006703100aa040a27aca65227133b2b5a71617efac3bc5640e361"
}
[Completed in 4998.999748 ms]
```

#### Return values:

`success` : Boolean flag indicating that the asset transfer was successful.

`address` : The register address for this asset.

`txid` : The ID (hash) of the transaction that includes the asset transfer.

## `claim`

This method will claim ownership of the specified noun by the recipient to complete the transaction.

```
assets/claim/noun
```

This command only supports the `asset` noun.

#### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`txid` : Required the **transaction ID** (hash) of the item transfer transaction for which is being claimed.

`name` : Optional field allows the user to **rename** an item when it is claimed. By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01f35304d41d00b002ca02d3bd9cf6cfeb134f5c454d4b6f5a355e35ffc557cfb3756834f3cf5cbeaf98da6773adcaaeca80154d15e449d4876ddf35c0b895cf"
}
[Completed in 4978.949420 ms]

```

#### Return values:

`success` : Boolean flag indicating that the asset claim was successful.

`txid` : The ID (hash) of the transaction that includes the claimed asset.

## `history`

This will get the history of changes to an item, including both the data and it's ownership.

```
supply/history/noun
```

This command only supports the `asset` noun.

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

####
