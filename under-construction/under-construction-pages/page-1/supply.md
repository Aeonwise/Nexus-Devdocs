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

[`create`](supply.md#create) - Generate a new object of supported type.\
`get` - Get object of supported type.\
`list` - List all objects owned by given user.\
`update` - Update a specified object.\
`transfer` - Transfer a specified object register.\
`claim` - Claim a specified object register.\
`history` - Generate the history of all last states.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`items`] - The default profile that controls all sub-profiles.\


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

This command does not support the This command only supports the `item` noun.

### Parameters:

``

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the asset should be created with. For single-user API mode the session should not be supplied.

`name` : An optional name to identify the asset. If provided a Name object will also be created in the users local namespace, allowing the asset to be accessed/retrieved by name. If no name is provided the asset will need to be accessed/retrieved by its 256-bit register address.

`format` : The format the caller is using to define the asset. Values can be `basic` (the default), `raw`, `JSON`, `ANSI` (not currently supported), or `XML` (not currently supported). This is an optional field and the value `basic` is assumed if omitted.

`data` : If format is `raw`, then this field contains the hex-encoded data to be stored in this asset. Raw assets are always read-only. All other preceding fields are ignored.

`<fieldname>=<value>` : If format is `basic`, then the caller can provide additional = pairs for each piece of data to store in the asset.

`json` : If format is `JSON`, then this field will hold the json definition of the asset as a JSON array of objects representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `value` : The default value of the field.
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).
* `maxlength`: Only applicable to `string` or `bytes` fields where `mutable`=true, this is the maximum number of characters (bytes) that can be stored in the field. If no maxlength parameter is provided then we will default the field size to the length of the default value rounded up to the nearest 64 bytes.

### Results:

```
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction that includes the created object.

## `update`

Update an object register specified by given noun.

```
profiles/update/noun
```

This command only supports the `item` noun.

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`password` : The existing password for this signature chain.

`pin` : The existing pin for this signature chain.

### update/credentials

`new_password` : The new password to set for for this signature chain. This is optional if new\_pin is provided

`new_pin` : The new pin to set for this signature chain. This is optional if new\_password is provided.

### update/recovery

`recovery` : The existing recovery seed for this signature chain. This is only required if an existing recovery seed is being updated via `new_recovery.`

`new_recovery` : The new recovery seed to set on this sig chain. The recovery seed must be a minimum of 40 characters.

### Results:

```
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction for the updated object.

### `Methods`

The following methods are currently supported by this API

[`create/item`](supply.md#create-item)\
[`get/item`](supply.md#get-item)\
[`update/item`](supply.md#update-item)\
[`transfer/item`](supply.md#transfer-item)\
[`claim/item`](supply.md#claim-item)\
[`list/item/history`](supply.md#list-item-history)

***

## `create/item`

This will create a new item, assigning ownership to the user logged in to the provided session. The API supports an alternative endpoint that can include the new asset name in the URI. For example `supply/create/item/myitem` will resolve to `supply/create/item?name=myitem`.

#### Endpoint:

`/supply/create/item`

{% swagger method="post" path="/supply/create/item" baseUrl="http://api.nexus-interactions.io:8080" summary="create/item" %}
{% swagger-description %}
This will create a new item, assigning ownership to the user logged in to the provided session
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the asset should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
An optional name to identify the item. If provided a Name object will also be created in the users local namespace, allowing the item to be accessed/retrieved by name. If no name is provided the item will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" required="true" %}
The data to store in the item
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item created" %}
```json
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "txid": "70e5d8b6227d209fafe4c90ed0ed7e63b23b72dc1349c60b37a00ed06e18215d5fa384da1b6522e24cb1467b11b0b0e8ac4e9db8374f09718ab1218e8da33a11"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/item
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    // name: "NAME TO IDENTIFY THE ITEM", //optional, If provided a Name object will also be created in the users local namespace, allowing the item to be accessed/retrieved by name. If no name is provided the item will need to be accessed/retrieved by its 256-bit register address.
    data: "DATA TO STORE IN THE ITEM"
}
fetch(`${SERVER_URL}/supply/create/item`, {
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
    # "name": "NAME TO IDENTIFY THE ITEM", #optional, If provided a Name object will also be created in the users local namespace, allowing the item to be accessed/retrieved by name. If no name is provided the item will need to be accessed/retrieved by its 256-bit register address.
    "data": "DATA TO STORE IN THE ITEM"
}
response = requests.post(f"{SERVER_URL}/supply/create/item", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the asset should be created with. For single-user API mode the session should not be supplied.

`name` : An optional name to identify the item. If provided a Name object will also be created in the users local namespace, allowing the item to be accessed/retrieved by name. If no name is provided the item will need to be accessed/retrieved by its 256-bit register address.

`data` : The data to store in the item

#### Return value JSON object:

```
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "txid": "70e5d8b6227d209fafe4c90ed0ed7e63b23b72dc1349c60b37a00ed06e18215d5fa384da1b6522e24cb1467b11b0b0e8ac4e9db8374f09718ab1218e8da33a11"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the item creation.

`address` : The register address for this item. The address (or name that hashes to this address) is needed for downstream transactions / api methods for this item.

***

### `get/item`

This is the generic endpoint for retrieving an item from the object register. The Supply API supports an alternative endpoint that can include the item name (if known) in the URI. For example `/supply/get/item/myitem` will resolve to `supply/get/item?name=myitem`.

Additionally the API supports passing a field name in the URL after the asset name, which will populate the `fieldname` parameter in the request. For example `/supply/get/item/myitem/owner` will resolve to `supply/get/item?name=myitem&fieldname=owner`

#### Endpoint:

`/supply/get/item`

{% swagger method="post" path="/supply/get/item" baseUrl="http://api.nexus-interactions.io:8080" summary="get/item" %}
{% swagger-description %}
This is the generic endpoint for retrieving an item from the object register.
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the item. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the item. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fieldname" %}
This optional field can be used to filter the response to return only a single field from the item{
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item details" %}
```json
{
    "name": "myitem",
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "created": 1560982537,
    "modified": 1560982615,
    "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "data": "xxxxxx"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/item
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME IDENTIFYING THE ITEM", //optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    // session: "YOUR_SESSION_ID", //optional
    // address: "REGISTER ADDRESS OF THE ITEM", //optional if the name is provided.
    // fieldname: "FILTER_FIELD", //optional field can be used to filter the response to return only a single field from the item.
}
fetch(`${SERVER_URL}/supply/get/item`, {
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
    # optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    "name": "NAME IDENTIFYING THE ITEM",
    # "session": "YOUR_SESSION_ID", #optional
    # "address": "REGISTER ADDRESS OF THE ITEM", #optional if the name is provided.
    # "fieldname": "FILTER_FIELD", #optional field can be used to filter the response to return only a single field from the item.
}
response = requests.post(f"{SERVER_URL}/supply/get/item", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the item. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the item. This is optional if the name is provided.

`fieldname`: This optional field can be used to filter the response to return only a single field from the item.

#### Return value JSON object:

```
{
    "name": "myitem",
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "created": 1560982537,
    "modified": 1560982615,
    "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "data": "xxxxxx"
}
```

#### Return values:

`created` : The UNIX timestamp when the item was created.

`modified` : The UNIX timestamp when the item was last modified.

`name` : The name identifying the item. For privacy purposes, this is only included in the response if the caller is the owner of the item

`address` : The register address of the item.

`owner` : The genesis hash of the owner of the item.

`data` : The data stored in the item.



### `list/items`

This will list off all of the supply chain items (append registers) owned by a signature chain.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/supply/list/items`

{% swagger method="post" path="/supply/list/items" baseUrl="http://api.nexus-interactions.io:8080" summary="list/items" %}
{% swagger-description %}
This will list off all of the supply chain items (append registers) owned by a signature chain
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

{% swagger-response status="200: OK" description="item list" %}
```json
[
    {
        "created": 1560982537,
        "modified": 1560982615,
        "name": "myitem",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "data": "xxxxxx"
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/items
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME",
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/supply/list/items`, {
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
    "username": "YOUR_USERNAME",
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/supply/list/items", json=data)
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
        "created": 1560982537,
        "modified": 1560982615,
        "name": "myitem",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "owner": "2be51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "data": "xxxxxx"
    }
]
```

#### Return values:

`created` : The UNIX timestamp when the item was created.

`modified` : The UNIX timestamp when the item was last modified.

`name` : The name identifying the item. For privacy purposes, this is only included in the response if the caller is the owner of the item

`address` : The register address of the item.

`owner` : The genesis hash of the owner of the item.

`data` : The data stored in the item.

***

### `update/item`

This is the generic endpoint for updating the data value in an item. The API supports an alternative endpoint that can include the item name (if known) or register address in the URI. For example `/supply/update/item/myitem` will resolve to `supply/update/item?name=myitem`.

#### Endpoint:

`/supply/update/item`

{% swagger method="post" path="/supply/update/item" baseUrl="http://api.nexus-interactions.io:8080" summary="update/item" %}
{% swagger-description %}
This is the generic endpoint for updating the data value in an item
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the supply should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the supply to update. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the supply to update. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" %}
The new value of the data field in this item
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item updated" %}
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
// update/item
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session : "YOUR_SESSION_ID", //optional
    // name : "NAME IDENTIFYING THE SUPPLY TO UPDATE", //optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    // address : "REGISTER ADDRESS OF THE SUPPLY TO UPDATE", //optional if the name is provided.
    data: "NEW VALUE OF THE DATA FIELD IN THIS ITEM"
}
fetch(`${SERVER_URL}/supply/update/item`, {
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
    # "session" : "YOUR_SESSION_ID", #optional
    # "name" : "NAME IDENTIFYING THE SUPPLY TO UPDATE", #optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    # "address" : "REGISTER ADDRESS OF THE SUPPLY TO UPDATE", #optional if the name is provided.
    "data": "NEW VALUE OF THE DATA FIELD IN THIS ITEM"
}
response = requests.post(f"{SERVER_URL}/supply/update/item", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the supply should be created with. For single-user API mode the session should not be supplied.

`name` : The name identifying the supply to update. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the supply to update. This is optional if the name is provided.

`data` : The new value of the data field in this item

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the item update.

`address` : The register address for this item.

***

### `transfer/item`

This will transfer ownership of an item. This is a generic endpoint requiring the item name or address to be passed as parameters. The API supports an alternative endpoint that can include the item name (if known) or register address in the URI. For example `/supply/transfer/item/myitem` will resolve to `supply/transfer/item?name=myitem`.

#### Endpoint:

`/supply/transfer/item`

{% swagger method="post" path="/supply/transfer/item" baseUrl="http://api.nexus-interactions.io:8080" summary="transfer/item" %}
{% swagger-description %}
This will transfer ownership of an item
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the item. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the item to be transferred. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the item to be transferred. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the user account (sig-chain) to transfer the item to. This is optional if the destination is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="destination" %}
The genesis hash of the signature chain to transfer the the item to. This is optional if the username is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="expires" %}
This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the 

`number of seconds`

 from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item transfer details" %}
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
// transfer/item
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session : "YOUR_SESSION_ID", //optional
    // name : "NAME IDENTIFYING THE ITEM TO BE TRANSFERRED", //optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    // address : "REGISTER ADDRESS OF THE ITEM TO BE TRANSFERRED", //optional if the name is provided.
    username: "THE USERNAME IDENTIFYING THE USER ACCOUNT (SIG-CHAIN) TO TRANSFER THE ITEM TO", //optional if the destination is provided.
    destination: "THE GENESIS HASH OF THE SIGNATURE CHAIN TO TRANSFER THE THE ITEM TO", //optional if the username is provided.
    // expires : 604800 //optional (in seconds)
}
fetch(`${SERVER_URL}/supply/transfer/item`, {
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
    # "session" : "YOUR_SESSION_ID", #optional
    # "name" : "NAME IDENTIFYING THE ITEM TO BE TRANSFERRED", #optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    # "address" : "REGISTER ADDRESS OF THE ITEM TO BE TRANSFERRED", #optional if the name is provided.
    # optional if the destination is provided.
    "username": "THE USERNAME IDENTIFYING THE USER ACCOUNT (SIG-CHAIN) TO TRANSFER THE ITEM TO",
    # optional if the username is provided.
    "destination": "THE GENESIS HASH OF THE SIGNATURE CHAIN TO TRANSFER THE THE ITEM TO",
    # "expires" : 604800 #optional (in seconds)
}
response = requests.post(f"{SERVER_URL}/supply/transfer/item", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the item. For single-user API mode the session should not be supplied.

`name` : The name identifying the item to be transferred. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the item to be transferred. This is optional if the name is provided.

`username` : The username identifying the user account (sig-chain) to transfer the item to. This is optional if the destination is provided.

`destination` : The genesis hash of the signature chain to transfer the the item to. This is optional if the username is provided.

`expires` : This optional field allows callers to specify an expiration for the transfer transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be claimed by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the item transfer.

`address` : The new register address for this item.

***

### `claim/item`

Items that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction . This is a generic endpoint requiring the transcation ID (hash) of the corresponding transfer transaction to be passed as a parameter. The API supports an alternative endpoint that can include the transaction ID in the URI. For example `/supply/transfer/item/27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1` will resolve to `supply/transfer/item?txid=27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1`.

#### Endpoint:

`/supply/claim/item`

{% swagger method="post" path="/supply/claim/item" baseUrl="http://api.nexus-interactions.io:8080" summary="claim/item" %}
{% swagger-description %}
Items that have been transferred need to be claimed by the recipient before the transfer is complete. This method creates the claim transaction
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the item. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="true" %}
The transaction ID (hash) of the corresponding item transfer transaction for which you are claiming
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
This optional field allows the user to rename an item when it is claimed. By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item claimed" %}
```json
{
    "claimed" : 
    [
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    ],
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// claim/item
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session : "YOUR_SESSION_ID", //optional
    txid: "TRANSACTION ID (HASH)" //of the corresponding item transfer transaction for which you are claiming.
        // name : "NEW_NAME", // By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.
}
fetch(`${SERVER_URL}/supply/claim/item`, {
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
// Some cimport requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "pin": "YOUR_PIN",
    # "session" : "YOUR_SESSION_ID", #optional
    # of the corresponding item transfer transaction for which you are claiming.
    "txid": "TRANSACTION ID (HASH)"
    # "name" : "NEW_NAME", # By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.
}
response = requests.post(f"{SERVER_URL}/supply/claim/item", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the item. For single-user API mode the session should not be supplied.

`txid` : The transaction ID (hash) of the corresponding item transfer transaction for which you are claiming.

`name` : This optional field allows the user to rename an item when it is claimed. By default the name is copied from the previous owner and a Name record is created for the item in your user namespace. If you already have an object for this name then you will need to provide a new name in order for the claim to succeed.

#### Return value JSON object:

```
{
    "claimed" : 
    [
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    ],
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
}
```

#### Return values:

`claimed` : Array of addresses for each item that was claimed by this transaction

`txid` : The ID (hash) of the transaction that includes the item transfer.

***

### `list/item/history`

This will get the history of changes to an item, including both the data and it's ownership. The API supports an alternative endpoint that can include the item name (if known) or register address in the URI. For example `/supply/list/item/history/myitem` will resolve to `/supply/list/item/history?name=myitem`.

#### Endpoint:

`/supply/list/item/history`

{% swagger method="post" path="/list/item/history" baseUrl="http://api.nexus-interactions.io:8080" summary="list/item/history" %}
{% swagger-description %}
This will get the history of changes to an item, including both the data and it's ownership.
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the item. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the item. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="item history details" %}
```json
[
    {
        "type": "CREATE",
        "name": "mysupplyitem",
        "checksum": 9070277379865698600,
        "owner": "bf501d4f3d81c31f62038984e923ad01546ff678e305a7cc11b1931742524ce1",
        "data": "0b6e657764617461696e746f",
        "modifed": 1546888868
    },
    {
        "type": "MODIFY",
        "name": "mysupplyitem",
        "checksum": 9060775569865698900,
        "owner": "5efc8a9437d93e894defd50f8ba73a0b2b5529cc593d5e4a7ea413022a9c35e9",
        "data": "0b6e657764617461696e746f",
        "modifed": 1546888999
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/item/history
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "THE NAME IDENTIFYING THE ITEM", //optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    // session: "YOUR_SESSION_ID", //optional
    address: "REGISTER ADDRESS OF THE ITEM" //optional if the name is provided.
}
fetch(`${SERVER_URL}/supply/list/item/history`, {
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
    # optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    "name": "THE NAME IDENTIFYING THE ITEM",
    # "session": "YOUR_SESSION_ID", #optional
    # optional if the name is provided.
    "address": "REGISTER ADDRESS OF THE ITEM"
}
response = requests.post(f"{SERVER_URL}/supply/list/item/history", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the item. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the item was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the item. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the item. This is optional if the name is provided.

#### Return value JSON object:

```
[
    {
        "type": "CREATE",
        "name": "mysupplyitem",
        "checksum": 9070277379865698600,
        "owner": "bf501d4f3d81c31f62038984e923ad01546ff678e305a7cc11b1931742524ce1",
        "data": "0b6e657764617461696e746f",
        "modifed": 1546888868
    },
    {
        "type": "MODIFY",
        "name": "mysupplyitem",
        "checksum": 9060775569865698900,
        "owner": "5efc8a9437d93e894defd50f8ba73a0b2b5529cc593d5e4a7ea413022a9c35e9",
        "data": "0b6e657764617461696e746f",
        "modifed": 1546888999
    }
]
```

#### Return values:

The return value is a JSON array of objects for each entry in the item's history:

`type` : The action that occurred - CREATE | MODIFY | TRANSFER | CLAIM

`owner` : The genesis hash of the signature chain that owned the item.

`modified` : The Unix timestamp of when the item was updated.

`checksum` : A checksum of the state register used for pruning.

`address` : The register address of the item

`name` : The name of the item

`data` : The data at this point in the history

***
