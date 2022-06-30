---
description: NAMES API
---

# NAMES

Names and Namespaces are special kinds of object registers that are used as locators to other object registers in the blockchain. When an object register is first created (an asset for example) the caller can provide a name for the register. If a name is provided then a Name object register is also created with its register address based on a hash of the name. The Name object also has a address field, which is populated with the register address of the register (asset, token, account etc) that the Name "points" to. In this way, objects can be retrieved by name by first hashing the name to get the Name object's address, retrieving the Name object, and then using the address stored within it to retrieve the object register. A Name then, is best thought of as a named index to object registers.

The TAO Naming System (TNS) allows Name objects to be created in one of three different contexts, - `local`, `namespaced`, and `global` and will be owned by a profile. Names must be unique within the profile in which it was defined.

Local Names are those created within the context of a user profile. To use a local name you must prefix the name with the owners username separated by a single colon, e.g. `bob:savings`. This is equivalent to saying "look at all the Names registered in the profile `bob` and find one called `savings` and then see what object register it points to". There can only be one Name called `savings` in the sig chain `bob`, but another user `alice` can also create a local name called `savings`

Namespaced Names are those created within the context of a namespace, which itself is a globally unique keyword. Namespaces allow users to provide user-friendly names for their object registers without needing to disclose their username. This is useful for privacy, but also to allow names to be related to a business or some other meaningful context. To use a namespaced name you must prefix the name with the namespace separated by a double colon, e.g. `bobscoffeeshop::payments`. In this example bob would have first registered the namespace `bobscoffeeshop` and created an account to receive payments to (which could be called anything). He then creates a Name with a `name=payments`, `namespace=bobscoffeeshop` and `address=(register address of the account)`. From then on, anyone can use the name `bobscoffeeshop::payments` and it will resolve to the register address of the account. To avoid name-squatting registering a namespace name attracts a high fee (`1000 NXS`). However once registered, creating Names within that namespace costs only `1 NXS`

Global Names require no username or namespace prefix, and are therefore globally unique. These will be likely reserved for use cases where a succinct, unique, name is necessary, for example a token ticker symbol. To avoid needless name-squatting, global names attract a high fee (`2000 NXS`).

The Names API allows callers to access and manage both Names and Namespaces. Names can be created to "point" to any register address you wish, whether the caller owns the register or not. This is useful, for example, if somebody gives you the register address of a NXS account to receive payments and you wish to add a friendly Name for it for future use.

Namespaces can be transferred to other signature chains, opening the possibility for a secondary market to buy and sell namespaces (similar to internet domain names).

Global Names and Names that have been created within a namespace can also be transferred to other signature chains. Local names cannot be transferred.

The full supported endpoint of the Names URI is as follows:

```
names/verb/noun/filter/opertor
```

The minimum required components of the URI are:

```
names/verb/noun
```

## `Supported Filters`

This command-set supports single or csv field-name filters.&#x20;

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
[`transfer`](c-names.md#transfer) - Transfer ownership of an object register to a recipient.\
[`claim`](c-names.md#claim) - Claim ownership of an object register from a transfer.\
[`history`](c-names.md#history) - Generate the history of all last states.\
[`transactions`](c-names.md#transactions) - List all transactions that modified specified object.\
[`update`](c-names.md#update) -  Update an object register\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`name`] -  An object register containing a name object. \
\[`namespace`] - An object register containing a namespace object. \
\[`global`] - An object register which is recognised globally on the network. \
\[`local`] - An object register which is recognised only within the context of a user profile.\
\[`any`] - An object selection noun allowing mixed `names` nouns.

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

#### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

#### create/name

`name` : The name of the object that this name will point to. The name can contain any characters, but must not START with a colon `:`

`namespace` : Optional field allows callers to specify the **namespace** that the name should be created in. If the namespace is provided then the caller must also be the owner of the namespace. i.e. you cannot create a name in someone elses namespace. If the namespace is left blank (the default) then the Name will be created in the users local namespace (unless specifically flagged as global).

`global` : Optional, boolean field indicates that the Name should be created in the global namespace, i.e. it will be globally unique. If the caller sets this field to true, the namespace parameter is ignored.

`register_address` : The 256-bit hexadecimal register address of the object that this Name will point to.

#### `create/namespace`

`namespace` : A name to **identify** the namespace. A hash of the name will determine the register address.

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

`get/namespace`

This will create a new name.

```
names/get/namespace
```

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

#### get/namespace

`name` : The name **identifying** the namespace. This is optional if the address is provided.

`address` : The **register address** of the namespace to be transferred. This is optional if the name is provided.

#### `get/name`

`name` : The name **identifying** the name object. If the `name` parameter is provided then all other parameters are ignored.

`address` : The r**egister address** to search for. If provided then the Names owned by the callers signature chain are searched to find a match. This parameter is ignored if `name` is provided.

### Results:

#### `get/name`

#### Return value JSON object:

```
{
    "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "version": 1,
    "created": 1656571493,
    "modified": 1656571493,
    "type": "OBJECT",
    "register": "8BJ747ASK45oU7UC5e2dePXeMviskmU1t5Kd4iyKLdSiCgKtLcJ",
    "name": "UPS-Token",
    "namespace": "",
    "address": "8HeR7kxrk9zsAcEAqaQaD2juMnZ73m4WDb4sW16pW93qd7i2Q3G"
}
[Completed in 0.418167 ms]
```

#### `get/namespace`

#### Return value JSON object:

```
{
    "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "version": 1,
    "created": 1654698239,
    "modified": 1654698239,
    "type": "OBJECT",
    "namespace": "valkyrie",
    "address": "8LBEGF1Yo3UR2HPtVVokZMpmAespLfDdPdt99cpKiFJ7VSufsJ5"
}
[Completed in 0.174417 ms]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this Name.

`version` : The serialization version of the Name.

`created` : The UNIX timestamp when the Name was created.

`modified` : The UNIX timestamp when the Name was last modified.

`type` : The type of register - OBJECT | READONLY | RAW

`register` : The register address of the the object that this Name or namespace points to.

`name` : The name identifying the object register.

`namespace` : The name identifying the namespace or the namespace that the name was created in. For global names, this will be set to `~GLOBAL~`.

`address` : The register address of the Name.

## list

This will list off all of the object register details specified by the noun.

```
names/list/noun
```

This command supports all the nouns.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

#### Return value JSON object:

```
[
    {
        "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
        "version": 1,
        "created": 1656571493,
        "modified": 1656571493,
        "type": "OBJECT",
        "register": "8BJ747ASK45oU7UC5e2dePXeMviskmU1t5Kd4iyKLdSiCgKtLcJ",
        "name": "UPS-Token",
        "namespace": "",
        "address": "8HeR7kxrk9zsAcEAqaQaD2juMnZ73m4WDb4sW16pW93qd7i2Q3G"
    },
    {
        "owner": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
        "version": 1,
        "created": 1656571181,
        "modified": 1656571181,
        "type": "OBJECT",
        "register": "8DS2qGLhuEC2reKrzxyWaXXwtVq2KmGWGQCWKBQwrCQc4XS2b8V",
        "name": "UPS",
        "namespace": "~GLOBAL~",
        "address": "8H5tcBwU31FBTzokw3gDhz7e1k3mGytk26MBbKeAfjJbFHoXo7Y"
    }
]
[Completed in 0.781230 ms]
```

#### Return Values:

The return value is a JSON array of objects for each entry in the namespaces history:

`owner` : The genesis hash of the signature chain that owned the namespace.

`version` :  The serialization version of the namespace.

`created` : The UNIX timestamp when the object was created.

`modified` : The Unix timestamp when the object was updated.

`type` : The type of register - OBJECT | READONLY | RAW

`name` : The name of the Name object.

`namespace` :  The name of the namespace or the namespace that the name was created in. For global names, this will be set to `~GLOBAL~`.

`address` : The register address of the object.

## `transfer`

This will initiate transfer ownership of the specified noun

```
names/transfer/noun
```

#### transfer/name

This will transfer ownership of global names or names created in a namespace (a name in the format of mynamespace::myname) can be transferred.&#x20;

#### transfer/namespace

This will transfer ownership of an namespace&#x20;

#### Parameters:

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

#### claim/namespace

Namespaces that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction .&#x20;

#### Parameters:

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

```
names/history/noun
```

#### history/name

This will get the history of a name as well as it's ownership.&#x20;

#### history/namespace

This will get the history of a namespace as well as it's ownership.&#x20;

#### Parameters:

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

## `transactions`

This will list off all of the transactions for the specified noun.

```
names/transactions/noun
```

This command supports the `account, trust and token` nouns.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the [Sorting / Filtering](c-names.md#sorting-filtering) parameters.

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

## `update`

This method provides the user with the ability to update fields specified by the noun

```
names/update/noun
```

This command does not support the `master` or `auth` nouns.

#### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```

#### Return values:
