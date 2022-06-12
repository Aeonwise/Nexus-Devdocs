---
description: NAMES API
---

# NAMES

Names and Namespaces are special kinds of object registers that are used as locators to other object registers in the blockchain. The full supported endpoint of the Names URI is as follows:

```
names/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
names/verb/noun
```

## `Supported Operators`

The operators only work with the profiles `transactions` and `notifications` verbs. The following operators are supported for this API command-set:&#x20;

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
names/transactions/master/contracts.amount/sum
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
names/notifications/master/amount,ticker
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
names/transactions/master/contracts.OP
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

`name` : The Name of the object that this name will point to. The name can contain any characters, but must not START with a colon `:`

`namespace` : Optional field allows callers to specify the **namespace** that the name should be created in. If the namespace is provided then the caller must also be the owner of the namespace. i.e. you cannot create a name in someone elses namespace. If the namespace is left blank (the default) then the Name will be created in the users local namespace (unless specifically flagged as global).

`global` : Optional, boolean field indicates that the Name should be created in the global namespace, i.e. it will be globally unique. If the caller sets this field to true, the namespace parameter is ignored.

`register_address` : The 256-bit hexadecimal register address of the object that this Name will point to.

#### `create/namespace`

`namespace` : A name to identify the namespace. A hash of the name will determine the register address.

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

This will transfer the specified noun

#### transfer/name

This will transfer ownership of global names or names created in a namespace (with a name in the format of mynamespace::myname) can be transferred.&#x20;

#### Endpoint:

`/names/transfer/name`

#### transfer/namespace

This will transfer ownership of an namespace&#x20;

#### Endpoint:

`/names/transfer/namespace`

### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.

`name` : The name identifying the name to be transferred. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided.

`address` : The register address of the name to be transferred. This is optional if the name is provided.

`username` : The username identifying the user account (sig-chain) to transfer the name to. This is optional if the destination is provided.

`destination` : The genesis hash of the signature chain to transfer the the name to. This is optional if the username is provided.

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

## `Methods`

The following methods are currently supported by this API

[`create/namespace`](c-names.md#create-namespace)\
[`get/namespace`](c-names.md#get-namespace)\
[`transfer/namespace`](c-names.md#transfer-namespace)\
[`claim/namespace`](c-names.md#claim-namespace)\
[`list/namespace/history`](c-names.md#list-namespace-history)\
[`create/name`](c-names.md#create-name)\
[`get/name`](c-names.md#get-name)\
[`update/name`](c-names.md#update-name)\
[`transfer/name`](c-names.md#transfer-name)\
[`claim/name`](c-names.md#claim-name)\
[`list/name/history`](c-names.md#list-name-history)

## `create/namespace`

This will create a new namespace. The API supports an alternative endpoint that can include the new namespace name in the URL. For example `/names/create/namespace/mynamespace` will resolve to `names/create/namespace?name=mynamespace`.

**NOTE** : Namespace names can only contain **lowercase letters, numbers, and periods (.)**.

#### Endpoint:

`/names/create/namespace`

{% swagger method="post" path="/names/create/namespace" baseUrl="http://api.nexus-interactions.io:8080" summary="create/namespace" %}
{% swagger-description %}
This will create a new namespace
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
The pin for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the namespace should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="true" %}
A name to identify the namespace. A hash of the name will determine the register address
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Created namespace" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/namespace
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_TO_IDENTIFY_NAMESPACE", //optional if address is provided
}
fetch(`${SERVER_URL}/names/create/namespace`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "name": "NAME_TO_IDENTIFY_NAMESPACE",  # optional if address is provided
}
response = requests.post(f"{SERVER_URL}/names/create/namespace", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the namespace should be created with. For single-user API mode the session should not be supplied.

`name` : A name to identify the namespace. A hash of the name will determine the register address.

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the namespace creation.

`address` : The register address for this namespace.

***

### `get/namespace`

Retrieves a namespace object. The API supports an alternative endpoint that can include the new namespace name in the URL. For example `/names/get/namespace/mynamespace` will resolve to `names/get/namespace?name=mynamespace`.

#### Endpoint:

`/names/get/namespace`

{% swagger method="post" path="/names/get/namespace" baseUrl="http://api.nexus-interactions.io:8080" summary="get/namespace" %}
{% swagger-description %}
Retrieves a namespace object
{% endswagger-description %}

{% swagger-parameter in="body" name="name" required="false" %}
The name identifying the namespace. This is optional if the address is provided
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="get namespace" %}
```json
{
    "name": "mynamespace",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
    "owner": "bf501d4f3d81c31f62038984e923ad01546ff678e305a7cc11b1931742524ce1"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
//// get/namespace
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME_TO_IDENTIFY_NAMESPACE", //optional if address is provided
}
fetch(`${SERVER_URL}/names/get/namespace`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "name": "NAME_TO_IDENTIFY_NAMESPACE",  # optional if address is provided
}
response = requests.post(f"{SERVER_URL}/names/get/namespace", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the namespace. This is optional if the address is provided.

#### Return value JSON object:

```
{
    "name": "mynamespace",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
    "owner": "bf501d4f3d81c31f62038984e923ad01546ff678e305a7cc11b1931742524ce1"
}
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this Name.

`created` : The UNIX timestamp when the Name was created.

`name` : The name identifying the namespace.

`address` : The register address of the namespace.



### `list/namespaces`

This will list off all of the namespaces owned by the signature chain.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/names/list/namespaces`

{% swagger method="post" path="/names/list/namespaces" baseUrl="http://api.nexus-interactions.io:8080" summary="list/namespaces" %}
{% swagger-description %}
This will list off all of the namespaces owned by the signature chain
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json

    {
        "name": "mynamespace1",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
    },
    {
        "name": "mynamespace2",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    }


]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/namespaces
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME", // optional 
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/names/list/namespaces`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # "genesis": "GENESIS_ID", #optional
    "username": "YOUR_USERNAME",  # optional
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/names/list/namespaces", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain (optional if username is supplied).

`username` : The username identifying the signature chain (optional if genesis is supplied).

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "name": "mynamespace1",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
    },
    {
        "name": "mynamespace2",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    }


]
```

#### Return values:

`created` : The UNIX timestamp when the namespace was created.

`modified` : The UNIX timestamp when the namespace was last modified.

`name` : The name identifying the namespace.

`address` : The register address of the namespace.

***

### `transfer/namespace`

This will transfer ownership of an namespace . This is a generic endpoint requiring the namespace name or address to be passed as parameters. The API supports an alternative endpoint that can include the namespace name or register address in the URL. For example `/names/transfer/namespace/mynamespace` will resolve to `names/transfer/namespace?name=mynamespace`.

#### Endpoint:

`/names/transfer/namespace`

{% swagger method="post" path="/names/transfer/namespace" baseUrl="http://api.nexus-interactions.io:8080" summary="transfer/namespace" %}
{% swagger-description %}
This will transfer ownership of an namespace
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the namespace. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="false" %}
The name identifying the namespace to be transferred. This is optional if the address is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" required="false" %}
The register address of the namespace to be transferred. This is optional if the name is provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" required="false" %}
The username identifying the user account (sig-chain) to transfer the namespace to. This is optional if the destination is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" required="false" %}
The genesis hash of the signature chain to transfer the the namespace to. This is optional if the username is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="expires" required="false" %}
This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the

`number of seconds`

from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="transferred namespace" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// transfer/namespace
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_TO_IDENTIFY_NAMESPACE", //optional if address is provided
    // address: "REGISTER_ADDRESS_OF_NAMESPACE", //optional if name is provided
    username: "TO_USERNAME_OF_NAMESPACE", //optional if destination is provided
    // destination: "TO_GENESIS_HASH_OF_NAMESPACE", //optional if username is provided
    expires: 604800, //optional (in seconds)
}
fetch(`${SERVER_URL}/names/transfer/namespace`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "name": "NAME_TO_IDENTIFY_NAMESPACE",  # optional if address is provided
    # "address": "REGISTER_ADDRESS_OF_NAMESPACE", #optional if name is provided
    "username": "TO_USERNAME_OF_NAMESPACE",  # optional if destination is provided
    # "destination": "TO_GENESIS_HASH_OF_NAMESPACE", #optional if username is provided
    "expires": 604800,  # optional (in seconds)
}
response = requests.post(f"{SERVER_URL}/names/transfer/namespace", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the namespace. For single-user API mode the session should not be supplied.

`name` : The name identifying the namespace to be transferred. This is optional if the address is provided.

`address` : The register address of the namespace to be transferred. This is optional if the name is provided.

`username` : The username identifying the user account (sig-chain) to transfer the namespace to. This is optional if the destination is provided.

`destination` : The genesis hash of the signature chain to transfer the the namespace to. This is optional if the username is provided.

`expires` : This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the namespace transfer.

`address` : The register address for this namespace.

***

### `claim/namespace`

Namespaces that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction . This is a generic endpoint requiring the transaction ID (hash) of the corresponding transfer transaction to be passed as a parameter. The API supports an alternative endpoint that can include the transaction ID in the URI. For example `/names/claim/namespace/27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1` will resolve to `names/claim/namespace?txid=27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1`.

#### Endpoint:

`/names/claim/namespace`

{% swagger method="post" path="/names/claim/namespace" baseUrl="http://api.nexus-interactions.io:8080" summary="claim/namespace" %}
{% swagger-description %}
Namespaces that have been transferred need to be claimed by the recipient before the transfer is complete
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the namespace. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="true" %}
The transaction ID (hash) of the corresponding namespace transfer transaction for which you are claiming
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="claimed namespace" %}
```json
{
    "claimed":
    [
        "25428293b6631d2ff55b3a931926fec920e407a56f7759495e36089914718d68",
        "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2"
    ],
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// claim/namespace
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    txid: "HASH_OF_NAMESPACE_TRANSFER_TRANSACTON",
}
fetch(`${SERVER_URL}/names/claim/namespace`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "txid": "HASH_OF_NAMESPACE_TRANSFER_TRANSACTON",
}
response = requests.post(f"{SERVER_URL}/names/claim/namespace", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the namespace. For single-user API mode the session should not be supplied.

`txid` : The transaction ID (hash) of the corresponding namespace transfer transaction for which you are claiming.

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

`claimed`: Array of addresses for each namespace that was claimed by the transaction

`txid` : The ID (hash) of the transaction that includes the namespace transfer.

***

### `list/namespace/history`

This will get the history of a namespace as well as it's ownership. The API supports an alternative endpoint that can include the namespace name (if known) or register address in the URL. For example `/names/list/namespace/history/mynamespace` will resolve to `names/list/namespace/history?name=mynamespace`.

#### Endpoint:

`/names/list/namespace/history`

{% swagger method="post" path="/names/list/namespace/history" baseUrl="http://api.nexus-interactions.io:8080" summary="list/namespace/history" %}
{% swagger-description %}
This will get the history of a namespace as well as it's ownership
{% endswagger-description %}

{% swagger-parameter in="body" name="name" required="false" %}
The name identifying the namespace. This is optional if the address is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" required="false" %}
The register address of the namespace. This is optional if the name is pro
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="namespace history details" %}
```json
[
    {
        "type": "TRANSFER",
        "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1560492117,
        "checksum": 13703027408063695802,
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "name": "test"
    },
    {
        "type": "CREATE",
        "owner": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
        "modified": 1560492117,
        "checksum": 13703027408063695802,
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "name": "test"
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/namespace/history
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME_TO_IDENTIFY_NAMESPACE", //optional if address is provided
    // address: "REGISTER_ADDRESS_OF_NAMESPACE", //optional if name is provided
}
fetch(`${SERVER_URL}/names/list/namespace/history`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "name": "NAME_TO_IDENTIFY_NAMESPACE",  # optional if address is provided
    # "address": "REGISTER_ADDRESS_OF_NAMESPACE", #optional if name is provided
}
response = requests.post(
    f"{SERVER_URL}/names/list/namespace/history", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the namespace. This is optional if the address is provided.

`address` : The register address of the namespace. This is optional if the name is provided.

#### Return value JSON object:

```
[
    {
        "type": "TRANSFER",
        "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1560492117,
        "checksum": 13703027408063695802,
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "name": "test"
    },
    {
        "type": "CREATE",
        "owner": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
        "modified": 1560492117,
        "checksum": 13703027408063695802,
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "name": "test"
    }
]
```

#### Return values:

The return value is a JSON array of objects for each entry in the namespaces history:

`type` : The action that occurred - CREATE | MODIFY | TRANSFER | CLAIM

`owner` : The genesis hash of the signature chain that owned the namespace.

`modified` : The Unix timestamp of when the namespace was updated.

`checksum` : A checksum of the state register used for pruning.

`address` : The register address of the namespace

`name` : The name of the namespace

***

### `create/name`

This will create a new name. The API supports an alternative endpoint that can include the new name in the URL. For example `/names/create/name/myname` will resolve to `names/create/name?name=myname`.

**NOTE** : Names must not contain either colon `:` characters.

#### Endpoint:

`/names/create/name`

{% swagger method="post" path="/names/create/name" baseUrl="http://api.nexus-interactions.io:8080" summary="create/name" %}
{% swagger-description %}
This will create a new name
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the name should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="true" %}
The name of the object that this name will point to. The name can contain any characters, but must not START with a colon '

`:'`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="namespace" required="false" %}
This optional field allows callers to specify the namespace that the name should be created in. If the namespace is provided then the caller must also be the owner of the namespace. i.e. you cannot create a name in someone elses namespace. If the namespace is left blank (the default) then the Name will be created in the users local namespace (unless specifically flagged as global)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="global" type="true" required="false" %}
This optional, boolean field indicates that the Name should be created in the global namespace, i.e. it will be globally unique. If the caller sets this field to true, the namespace parameter is ignored
{% endswagger-parameter %}

{% swagger-parameter in="body" name="register_address" required="true" %}
The 256-bit hexadecimal register address of the the object that this Name will point to
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="created name" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/name
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_OF_THE_OBJECT_THAT_THIS_NAME_WILL_POINT_TO",
    // address: "REGISTER_ADDRESS_OF_NAMESPACE", //optional if name is provided
    // namespace: "YOUR_NAMESPACE",//optional
    // global: false, //optional
    register_address: "256 BIT REGISTER ADDRESS OF THE OBJECT THAT NAME WILL POINT TO"
}
fetch(`${SERVER_URL}/names/create/name`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "name": "NAME_OF_THE_OBJECT_THAT_THIS_NAME_WILL_POINT_TO",
    # "address": "REGISTER_ADDRESS_OF_NAMESPACE", #optional if name is provided
    # "namespace": "YOUR_NAMESPACE",#optional
    # "global": False, #optional
    "register_address": "256 BIT REGISTER ADDRESS OF THE OBJECT THAT NAME WILL POINT TO"
}
response = requests.post(f"{SERVER_URL}/names/create/name", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the name should be created with. For single-user API mode the session should not be supplied.

`name` : The name of the object that this name will point to. The name can contain any characters, but must not START with a colon `:`

`namespace` : This optional field allows callers to specify the namespace that the name should be created in. If the namespace is provided then the caller must also be the owner of the namespace. i.e. you cannot create a name in someone elses namespace. If the namespace is left blank (the default) then the Name will be created in the users local namespace (unless specifically flagged as global).

`global` : This optional, boolean field indicates that the Name should be created in the global namespace, i.e. it will be globally unique. If the caller sets this field to true, the namespace parameter is ignored.

`register_address` : The 256-bit hexadecimal register address of the the object that this Name will point to.

#### Example

The following example creates a name for "asset1" in the namespace "ourcompany", which points to an asset at the register address beginning with bf501... The caller must own the namespace "ourcompany" but the asset asset1 can be owned by someone else. Once created, the asset can be retrieved by the name "ourcompany::asset1"

```
{
    "name": "asset1",
    "namespace": "ourcompany",
    "register_address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1",
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the name creation.

`address` : The register address for this name.

***

### `get/name`

Retrieves a name object. There are two ways that this method can be used to retrieve a name object. 1) by using the `name` parameter the Name object can be retrieved in order to determine the register address that the Name "points" to. 2) by using the `register_address` parameter (in conjunction with `session` for multiuser mode) the Names owned by the caller will be searched to find the corresponding name. NOTE: It is not possible (for privacy reasons) to look up a Name from a register address in signature chains other than the callers.

The API supports an alternative endpoint that can include the name in the URL. For example `/names/get/name/myname` will resolve to `names/get/name?name=myname`.

#### Endpoint:

`/names/get/name`

{% swagger method="post" path="/names/get/name" baseUrl="http://api.nexus-interactions.io:8080" summary="get/name" %}
{% swagger-description %}
Retrieves a name object
{% endswagger-description %}

{% swagger-parameter in="body" name="name" required="false" %}
The name identifying the name object. The name should be in the format username:name (for local names) or name.namespace (for names in a global namespace). If the

`name`

parameter is provided then all other parameters are ignored
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which signature chain should be searched. For single-user API mode the session should not be supplied, and the logged in signature chain will be used. This parameter is ignored if

`name`

is provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="register_address" required="false" %}
The register address to search for. If provided then the Names owned by the callers signature chain are searched to find a match. This parameter is ignored if

`name`

is provided.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="get name details" %}
```json
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1565934825,
    "modified": 1565934825,
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "name": "asset1",
    "namespace": "somenamespace",
    "register_address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/name
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME_IDENTIFYING_THE_NAME_OBJECT", // in format username:name (for local names) or name.namespace (for names in a global namespace)
    // session: "YOUR_SESSION_ID", //optional
    // register_address: "REGISTER_ADDRESS_TO_SEARCH_FOR" //optional if name is provided
}
fetch(`${SERVER_URL}/names/get/name`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # in format username:name (for local names) or name.namespace (for names in a global namespace)
    "name": "NAME_IDENTIFYING_THE_NAME_OBJECT",
    # "session": "YOUR_SESSION_ID", #optional
    # "register_address": "REGISTER_ADDRESS_TO_SEARCH_FOR" #optional if name is provided
}
response = requests.post(f"{SERVER_URL}/names/get/name", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the name object. The name should be in the format username:name (for local names) or name.namespace (for names in a global namespace). If the `name` parameter is provided then all other parameters are ignored.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which signature chain should be searched. For single-user API mode the session should not be supplied, and the logged in signature chain will be used. This parameter is ignored if `name` is provided.

`register_address` : The register address to search for. If provided then the Names owned by the callers signature chain are searched to find a match. This parameter is ignored if `name` is provided.

#### Return value JSON object:

```
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1565934825,
    "modified": 1565934825,
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "name": "asset1",
    "namespace": "somenamespace",
    "register_address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this Name.

`created` : The UNIX timestamp when the Name was created.

`modified` : The UNIX timestamp when the Name was last modified.

`name` : The name identifying the object register.

`namespace` : The namespace that the name was created in. For global names, this will be set to `~GLOBAL~`.

`address` : The register address of the Name.

`register_address` : The register address of the the object that this Name points to.





### `list/names`

This will list off all of the names owned by the signature chain. For privacy reasons Names are only returned for the currently logged in user (multiuser=0) or for the logged in session (multiuser=1)

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/names/list/names`

{% swagger method="post" path="/names/list/names" baseUrl="http://api.nexus-interactions.io:8080" summary="list/names" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1), the session is required to identify which signature chain should be used to return the Names for. For single-user API mode the session should not be supplied and the currently logged in users signature chain is used
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="names list" %}
```json
[

    {
        "name": "default",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "register_address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    }


]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/names
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    session: "YOUR_SESSION_ID", //optional
    // username: "YOUR_USERNAME", // optional but slower to calculate
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/names/list/names`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "session": "YOUR_SESSION_ID",  # optional
    # "username": "YOUR_USERNAME", # optional but slower to calculate
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/names/list/names", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode (configured with multiuser=1), the session is required to identify which signature chain should be used to return the Names for. For single-user API mode the session should not be supplied and the currently logged in users signature chain is used.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[

    {
        "name": "default",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "register_address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    }


]
```

#### Return values:

`created` : The UNIX timestamp when the Name was created.

`modified` : The UNIX timestamp when the Name was last modified.

`name` : The name identifying the object register.

`address` : The register address of the Name.

`register_address` : The register address of the the object that this Name points to.



***

### `update/name`

This method allows the register\_address within a Name object to be changed. The API supports an alternative endpoint that can include the name or register address in the URI. For example `/names/update/name/myname` will resolve to `names/update/name?name=myname`.

#### Endpoint:

`/names/update/name`

{% swagger method="post" path="/names/update/name" baseUrl="http://api.nexus-interactions.io:8080" summary="update/name" %}
{% swagger-description %}
This method allows the register_address within a Name object to be changed
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="false" %}
The PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="false" %}
The name identifying the Name object to update. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the asset was created in the callers namespace (their username), then the username can be omitted from the name if the

`session`

parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" required="false" %}
The register address of the name to update. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="register_address" required="false" %}
The new register address that this Name should point to
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="updated name" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// update/name
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_IDENTIFYING_THE_NAME_OBJECT", // in format username:name (for local names) or name.namespace (for names in a global namespace)
    // address: "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", // optional if name is provided
    // register_address: "NEW_REGISTER_ADDRESS" //optional if name is provided
}
fetch(`${SERVER_URL}/names/udpate/name`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    # in format username:name (for local names) or name.namespace (for names in a global namespace)
    "name": "NAME_IDENTIFYING_THE_NAME_OBJECT",
    # "address": "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", # optional if name is provided
    # "register_address": "NEW_REGISTER_ADDRESS" #optional if name is provided
}
response = requests.post(f"{SERVER_URL}/names/udpate/name", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.

`name` : The name identifying the Name object to update. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the asset was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the name to update. This is optional if the name is provided.

`register_address` : The new register address that this Name should point to.

#### Example:

The following example updates the register\_address field for the name "asset1.ourcompany"

```
{
    "pin": "1234",
    "name": "asset1.ourcompany",
    "register_address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the name update.

`address` : The register address for the name that was updated.

***

### `transfer/name`

This will transfer ownership of a name . Only global names or names created in a namespace (with a name in the format of mynamespace::myname) can be transferred. This is a generic endpoint requiring the name or address to be passed as parameters. The API supports an alternative endpoint that can include the name or register address in the URL. For example `/names/transfer/name/myname` will resolve to `names/transfer/name?name=myname`.

#### Endpoint:

`/names/transfer/name`

{% swagger method="post" path="/names/transfer/name" baseUrl="http://api.nexus-interactions.io:8080" summary="transfer/name" %}
{% swagger-description %}
This will transfer ownership of a name . Only global names or names created in a namespace (with a name in the format of mynamespace::myname) can be transferred.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the name to be transferred. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the name to be transferred. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the user account (sig-chain) to transfer the name to. This is optional if the destination is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" %}
The genesis hash of the signature chain to transfer the the name to. This is optional if the username is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="expires" %}
This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the 

`number of seconds`

 from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// transfer/name
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_IDENTIFYING_THE_NAME_OBJECT", // in format username:name (for local names) or name.namespace (for names in a global namespace)
    // address: "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", // optional if name is provided
    username: "TO_USERNAME", //optional if destination is provided
    // destination: "GENESIS_HASH_OF_THE_SIGCHAIN_TO_TRANSFER_THE_NAME_TO", //optional if username is provided
    // expires: 604800, //optional (in seconds)
}
fetch(`${SERVER_URL}/names/transfer/name`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    # in format username:name (for local names) or name.namespace (for names in a global namespace)
    "name": "NAME_IDENTIFYING_THE_NAME_OBJECT",
    # "address": "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", # optional if name is provided
    "username": "TO_USERNAME",  # optional if destination is provided
    # "destination": "GENESIS_HASH_OF_THE_SIGCHAIN_TO_TRANSFER_THE_NAME_TO", #optional if username is provided
    # "expires": 604800, #optional (in seconds)
}
response = requests.post(f"{SERVER_URL}/names/transfer/name", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied.

`name` : The name identifying the name to be transferred. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided.

`address` : The register address of the name to be transferred. This is optional if the name is provided.

`username` : The username identifying the user account (sig-chain) to transfer the name to. This is optional if the destination is provided.

`destination` : The genesis hash of the signature chain to transfer the the name to. This is optional if the username is provided.

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

***

### `claim/name`

Names that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction . This is a generic endpoint requiring the transaction ID (hash) of the corresponding transfer transaction to be passed as a parameter. The API supports an alternative endpoint that can include the transaction ID in the URI. For example `/names/claim/name/27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1` will resolve to `names/claim/name?txid=27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1`.

#### Endpoint:

`/names/claim/name`

{% swagger method="post" path="/names/claim/name" baseUrl="http://api.nexus-interactions.io:8080" summary="claim/name" %}
{% swagger-description %}
Names that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction .
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the name. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="true" %}
The transaction ID (hash) of the corresponding name transfer transaction for which you are claiming
{% endswagger-parameter %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// claim/name
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  pin: "YOUR_PIN",
  // session: "YOUR_SESSION_ID", //optional
  txid: "HASH_OF_NAME_TRANSFER_TRANSACTON"
}
fetch(`${SERVER_URL}/names/claim/name`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session": "YOUR_SESSION_ID", #optional
    "txid": "HASH_OF_NAME_TRANSFER_TRANSACTON"
}
response = requests.post(f"{SERVER_URL}/names/claim/name", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

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

### `list/name/history`

This will get the history of a name as well as it's ownership. The API supports an alternative endpoint that can include the object name (if known) or register address in the URL. For example `/names/list/name/history/myname` will resolve to `names/list/name/history?name=myname`.

#### Endpoint:

`/names/list/name/history`

{% swagger method="post" path="names/list/name/history" baseUrl="http://api.nexus-interactions.io:8080" summary="list/name/history" %}
{% swagger-description %}
This will get the history of a name as well as it's ownership
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the name. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). This is optional if the address is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the name. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
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
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/name/history
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  name: "NAME_IDENTIFYING_THE_NAME", // in format username:name (for local names) or name.namespace (for names in a global namespace)
  // address: "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", // optional if name is provided
}
fetch(`${SERVER_URL}/list/name/history`, {
        method: 'POST',
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
    })
    .then(resp => resp.json())
    .then(json => console.log(json))
    .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # in format username:name (for local names) or name.namespace (for names in a global namespace)
    "name": "NAME_IDENTIFYING_THE_NAME",
    # "address": "REGISTER_ADDRESS_OF_THE_NAME_TO_UPDATE", # optional if name is provided
}
response = requests.post(f"{SERVER_URL}/list/name/history", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

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
