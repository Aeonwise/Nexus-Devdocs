---
description: NAMES API
---

# NAMES

Names and Namespaces are special kinds of object registers that are used as locators to other object registers in the blockchain. The full supported endpoint of the Names URI is as follows:

```
names/verb/noun/filter
```

The minimum required components of the URI are:

```
names/verb/noun
```

## `Supported Filters`

The filters only work with the profiles `transactions` and `notifications` verbs. This command-set supports single or csv field-name filters.&#x20;

**Example:**

```
names/list/name/created,address
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
names/list/name/contracts.OP
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

[`create`](c-names.md#create) - Generate a new object of supported type.\
[`get`](../../../getting-started/tritium++-api/broken-reference/) - Get object of supported type.\
[`list`](../../../getting-started/tritium++-api/broken-reference/) - List all objects owned by given user.\
`transfer` - Transfer ownership of a specific object register.\
`claim` - Claim ownership of a specific object registers.\
`history` - Generate the history of all last states.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`name`] - The default profile that controls all sub-profiles.\
\[`namespace`] - A crypto object register for login auth.\


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

```
finance/get/balances/balance/sumreate
```

## `create`

This will create a new object register specified by given noun.

```
names/create/noun
```

This command supports the  `name` and `namespace` nouns.

`create/name`

This will create a new local name or namespaced or global name

`create/namespace`

This will create a new namespace.

{% hint style="info" %}
**NOTE** : Namespaces can only contain **lowercase letters, numbers, and periods (.)**.
{% endhint %}

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

#### create/name

`name` : The name of the object that this name will point to. The name can contain any characters, but must not START with a colon `:`

`namespace` : Optional field allows callers to specify the **namespace** that the name should be created in. If the namespace is provided then the caller must also be the owner of the namespace. i.e. you cannot create a name in someone elses namespace. If the namespace is left blank (the default) then the Name will be created in the users local namespace (unless specifically flagged as global).

`global` : Optional, boolean field indicates that the Name should be created in the global namespace, i.e. it will be globally unique. If the caller sets this field to true, the namespace parameter is ignored.

`register_address` : The 256-bit hexadecimal register address of the object that this Name will point to.

#### `create/namespace`

`namespace` : A name to **identify** the namespace. A hash of the name will determine the register address.

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "address": "8LBEGF1Yo3UR2HPtVVokZMpmAespLfDdPdt99cpKiFJ7VSufsJ5",
    "txid": "01dc4d11a9b3418832796ad6c9ca90ba18897b0ec24be44667f0fb0172481775925fa5fc568d54d8cac1e18a81beb552cbbb8cf242210a70233a79355e5a056f"
}
[Completed in 4981.591417 ms]
```

#### Return values:

`success` : Boolean flag indicating that the namespace was created successfully .

`address` : The register address for this namespace.

`txid` : The ID (hash) of the transaction that includes the namespace creation.

## `get`

Retrieves the object information specified by given noun.

```
names/get/noun
```

`get/name`

This will create a new name.

```
names/get/name
```

`create/namespace`

This will create a new name.

```
```

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

#### get/namespace

`name` : The name identifying the namespace. This is optional if the address is provided.

`address` : The register address of the namespace to be transferred. This is optional if the name is provided.

#### `get/name`

`name` : The name identifying the name object. The name should be in the format username:name (for local names) or name.namespace (for names in a global namespace). If the `name` parameter is provided then all other parameters are ignored.

`register_address` : The register address to search for. If provided then the Names owned by the callers signature chain are searched to find a match. This parameter is ignored if `name` is provided.

#### Results:

#### `get/namespace`

#### Return value JSON object:

```
{
    "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "version": 1,
    "created": 1654698239,
    "modified": 1654698239,
    "type": "OBJECT",
    "namespace": "nexenture",
    "address": "8LBEGF1Yo3UR2HPtVVokZMpmAespLfDdPdt99cpKiFJ7VSufsJ5"
}
[Completed in 0.174417 ms]

```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this namespace.

`version` : The serialization version of the namespace.

`created` : The UNIX timestamp when the namespace was created.

`modified` : The UNIX timestamp when the namespace was last modified.

`type` : The description of the register `OBJECT`

`namespace` : The name identifying the object register.&#x20;

`address` : The register address of the namespace.

#### `get/name`

#### Return value JSON object:

```
[
    {
        "owner": "b743770768128f7ca2c15cb859b01583c2d5c5c772dc5564b3e7e48c4d0d54f4",
        "version": 1,
        "created": 1654620478,
        "modified": 1654620478,
        "type": "OBJECT",
        "register": "8C96zrYqeyYYiRDQ9pWZkxgzMhmV7nxDUEeE8fBCufgWfJ4cnJ1",
        "name": "Nex_Token",
        "namespace": "",
        "address": "8HXCoGDXsYBxQeGJUTvTX3HpjtNZYu5h75yetyjradHRwczw3FV"
    }
]
[Completed in 0.291792 ms]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this Name.

`version` : The serialization version of the Name.

`created` : The UNIX timestamp when the Name was created.

`modified` : The UNIX timestamp when the Name was last modified.

`type` : The description of the register `OBJECT`

`register` : The register address of the the object that this Name points to.

`name` : The name identifying the object register.

`namespace` : The namespace that the name was created in. For global names, this will be set to `~GLOBAL~`.

`address` : The register address of the Name.

## `transfer`

This will initiate transfer ownership of the specified noun

```
names/transfer/noun
```

#### transfer/name

This will transfer ownership of global names or names created in a namespace (a name in the format of mynamespace::myname) can be transferred.&#x20;

#### transfer/namespace

This will transfer ownership of an namespace&#x20;

### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.

`name` : The name identifying the name to be transferred. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided.

`address` : The register address of the name to be transferred. This is optional if the name is provided.

`recipient` : The username identifying the user account (sig-chain) to transfer the name to. This is optional if the destination is provided.

`recipient` : The username of the profile to transfer the the name to. This is optional if the username is provided.

`expires` : This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

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

#### claim/name

Names that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction .&#x20;

#### Endpoint:

`/names/claim/name`

#### claim/namespace

Namespaces that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction .&#x20;

#### Endpoint:

`/names/claim/namespace`

### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.

`txid` : The transaction ID (hash) of the corresponding name transfer transaction for which you are claiming.

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

This will get the history and ownership of the specified noun.

#### history/name

This will get the history of a name as well as it's ownership.&#x20;

#### Endpoint:

`/names/history/name`

#### history/namespace

This will get the history of a namespace as well as it's ownership.&#x20;

#### Endpoint:

`/names/history/namespace`

### Parameters:

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name.

`name` : The name identifying the name. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided.

`address` : The register address of the name. This is optional if the name is provided.

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

The return value is a JSON array of objects for each entry in the names history:

`type` : The action that occurred - CREATE | MODIFY | TRANSFER | CLAIM

`owner` : The genesis hash of the signature chain that owned the name.

`modified` : The Unix timestamp of when the name was updated.

`checksum` : A checksum of the state register used for pruning.

`address` : The register address of the name

`name` : The name

`namespace` : The namespace name

`register_address` : The register address that this name points to at this point in history

***
