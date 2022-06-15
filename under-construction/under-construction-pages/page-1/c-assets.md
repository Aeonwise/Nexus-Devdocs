---
description: ASSETS API
---

# c-ASSETS

An asset is a user-defined data structure that is stored in an object register, owned by a particular signature chain. Assets can hold one or more pieces of data and users can define the fields (name, data type, mutability) that data is stored in. The assets API supports several formats for defining the object data structures giving users and developers varying levels of functionality, including per-field type definition and mutability.

The `basic` format allows callers to define an asset in terms of simple key-value pairs. It assumes that all fields are read-only and all values are stored using the string data type. `basic` assets are always immutable and cannot be updated once created.

The `JSON`, `ANSI`, and `XML` \~formats allow callers to provide a detailed definition of the asset data structure, with each field defined with a specific data type and mutability. Assets defined with one of the complex formats can be updated after their initial creation.

The `raw` format differs from the other formats in that the asset data is not stored in object register, but instead is stored in a read-only state register. The raw format is useful when developers wish to store arbitrary binary data on the Nexus blockchain, without incurring the overhead of defining an object.

The full supported endpoint of the finance URI is as follows:

```
assets/verb/noun/filter/operator
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

[`create`](c-assets.md#create) - Generate a new object of supported type.\
`get` - Get object of supported type.\
`list` - List all objects owned by given user.\
`update` - Update a specified object.\
`transfer` - Transfer a specified object register.\
`claim` - Claim a specified object register.\
`history` - Generate the history of all last states.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`items`] - The default profile that controls all sub-profiles.\


**Example:**

```
assets/list/asset
```

The above command will create a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

***

### ``

### `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

[`get/balances`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#create)\
[`get/stakeinfo`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#credit)\
[`migrate/accounts`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#debit)\
[`set/stake`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#get)

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

***

## `create` <a href="#user-content-create" id="user-content-create"></a>

Create a new object register specified by given noun.

```
finance/create/noun
```

This command does not support the `any` or `all` nouns.

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional for **noun** `name` as a _UTF-8_ encoded string that will generate a name object register that points to new object. If noun is `token` this will be created as a global name.

`format` : The format the caller is using to define the asset. Values can be `basic` (the default), `raw`, `JSON`, `ANSI` (not currently supported), or `XML` (not currently supported). This is an optional field and the value `basic` is assumed if omitted.

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

## ``
