# REGISTER METHODS

### `Methods`

The following methods are currently supported by this API

`list/accounts`\
`list/trust`\
`list/tokens`\
`list/names`\
`list/namespaces`\


### list/accounts

This will list all known NXS and token accounts.

#### Endpoint:

`/register/list/accounts`

{% swagger method="post" path="/register/list/accounts" baseUrl="http://api.nexus-interactions.io:8080" summary="list/accounts" %}
{% swagger-description %}
This will list all known trust accounts
{% endswagger-description %}

{% swagger-parameter in="body" name="sort" %}
Determines which field the results should be sorted on. Values can be 

`balance`

, 

`stake`

, or 

`trust`

. Default is 

`trust`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="order" %}
Determines the order of the sort. Values can be 

`desc`

 for descending (the default) or 

`asc`

 for ascending
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

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="trustaccounts list" %}
```json
[
     {
        "owner": "b1221f91b67502d908e1d835d0b455297a20e8bedaf93cc9870a69c89d9d5311",
        "version": 1,
        "created": 1643642075,
        "modified": 1644544090,
        "type": "OBJECT",
        "balance": 934.0,
        "token": "0",
        "ticker": "NXS",
        "total": 934.0
     },
     {
        "owner": "b1b5b4f4197548886016586f95735f0cb8235183a9185b8720bd27502a2e2850",
        "version": 1,
        "created": 1644350882,
        "modified": 1644350882,
        "type": "OBJECT",
        "balance": 0.0,
        "token": "8DGvmgAzEAmYkeUTN5SpgVjDH5zLEirTKNS9gbwqb559Z5Q7xeT",
        "ticker": "Chalet Token",
        "total": 0.0
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/trustaccounts
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // sort: "trust", //determines which field the results should be sorted on , values can be balance, stake, or trust
    // order: "desc", //determines the order of the sort, values can be asc or desc. Default is desc
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/register/list/accounts`, {
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
    # "sort": "trust", #determines which field the results should be sorted on , values can be balance, stake, or trust
    # "order": "desc", #determines the order of the sort, values can be asc or desc. Default is desc
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/register/list/accounts", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`sort` : Determines which field the results should be sorted on.

`order` : Determines the order of the sort. Values can be `desc` for descending (the default) or `asc` for ascending.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "owner": "b1221f91b67502d908e1d835d0b455297a20e8bedaf93cc9870a69c89d9d5311",
        "version": 1,
        "created": 1643642075,
        "modified": 1644544090,
        "type": "OBJECT",
        "balance": 934.0,
        "token": "0",
        "ticker": "NXS",
        "total": 934.0
    },
    {
        "owner": "b1b5b4f4197548886016586f95735f0cb8235183a9185b8720bd27502a2e2850",
        "version": 1,
        "created": 1644350882,
        "modified": 1644350882,
        "type": "OBJECT",
        "balance": 0.0,
        "token": "8DGvmgAzEAmYkeUTN5SpgVjDH5zLEirTKNS9gbwqb559Z5Q7xeT",
        "ticker": "Chalet Token",
        "total": 0.0
    }
]
```

#### Return values:

`address` : The register address of the trust account

`owner` : The genesis hash of the trust account owner

`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`type` : Type of register.

`balance` : The current NXS balance of the trust account. This is general account balance that is not staked.

`token` : The type of token or token account.

`ticker` : The name of the account.

`total` : The total amount of NXS or token held by that account.

### `list/trust`

This will list all known trust accounts.

#### Endpoint:

`/register/list/trust`

{% swagger method="post" path="/register/list/trust" baseUrl="http://api.nexus-interactions.io:8080" summary="list/trust" %}
{% swagger-description %}
This will list all known trust accounts
{% endswagger-description %}

{% swagger-parameter in="body" name="sort" %}
Determines which field the results should be sorted on. Values can be 

`balance`

, 

`stake`

, or 

`trust`

. Default is 

`trust`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="order" %}
Determines the order of the sort. Values can be 

`desc`

 for descending (the default) or 

`asc`

 for ascending
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

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="trustaccounts list" %}
```json
[
    {
        "owner": "b1b5b4f4197548886016586f95735f0cb8235183a9185b8720bd27502a2e2850",
        "version": 1,
        "created": 1638020495,
        "modified": 1647607162,
        "type": "OBJECT",
        "balance": 193.059063,
        "stake": 15000.0,
        "token": "0",
        "ticker": "NXS",
        "trust": 7853245,
        "age": "90 days, 21 hours, 27 minutes",
        "rate": 3.0,
        "address": "8EunQ82qVdnuQkX2gXKZr5P55kQRz4KbpaLdCVBjBNu8jeys4C4",
        "total": 15193.059063
    },
    {
        "owner": "b1cc162a96b1b5869f439101c0b135d9219799a64d1db340ae80435e4fc12207",
        "version": 1,
        "created": 1637569568,
        "modified": 1647607029,
        "type": "OBJECT",
        "balance": 107937.088058,
        "stake": 1000000.0,
        "token": "0",
        "ticker": "NXS",
        "trust": 8573310,
        "age": "99 days, 5 hours, 28 minutes",
        "rate": 3.0,
        "address": "8Fuv6SQdYr9YNd3PUuV1E2LPsNUfgcrstmcPr7CMYze5yiEwfV9",
        "total": 1107937.088058
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/trustaccounts
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // sort: "trust", //determines which field the results should be sorted on , values can be balance, stake, or trust
    // order: "desc", //determines the order of the sort, values can be asc or desc. Default is desc
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/register/list/trust`, {
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
    # "sort": "trust", #determines which field the results should be sorted on , values can be balance, stake, or trust
    # "order": "desc", #determines the order of the sort, values can be asc or desc. Default is desc
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/register/list/trust", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`sort` : Determines which field the results should be sorted on.

`order` : Determines the order of the sort. Values can be `desc` for descending (the default) or `asc` for ascending.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "owner": "b1b5b4f4197548886016586f95735f0cb8235183a9185b8720bd27502a2e2850",
        "version": 1,
        "created": 1638020495,
        "modified": 1647607162,
        "type": "OBJECT",
        "balance": 193.059063,
        "stake": 15000.0,
        "token": "0",
        "ticker": "NXS",
        "trust": 7853245,
        "age": "90 days, 21 hours, 27 minutes",
        "rate": 3.0,
        "address": "8EunQ82qVdnuQkX2gXKZr5P55kQRz4KbpaLdCVBjBNu8jeys4C4",
        "total": 15193.059063
    },
    {
        "owner": "b1cc162a96b1b5869f439101c0b135d9219799a64d1db340ae80435e4fc12207",
        "version": 1,
        "created": 1637569568,
        "modified": 1647607029,
        "type": "OBJECT",
        "balance": 107937.088058,
        "stake": 1000000.0,
        "token": "0",
        "ticker": "NXS",
        "trust": 8573310,
        "age": "99 days, 5 hours, 28 minutes",
        "rate": 3.0,
        "address": "8Fuv6SQdYr9YNd3PUuV1E2LPsNUfgcrstmcPr7CMYze5yiEwfV9",
        "total": 1107937.088058
    }
]
```

#### Return values:

`address` : The register address of the trust account

`owner` : The genesis hash of the trust account owner

`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`balance` : The current NXS balance of the trust account. This is general account balance that is not staked.

`stake` : The amount of NXS currently staked in the trust account.

`trust` : The current raw trust score of the trust account.

`stakerate` : The current annual reward rate earned for staking as an annual percent.

### `list/tokens`

This will list off all of the tokens that were created by a particular signature chain.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/register/list/tokens`

{% swagger method="post" path="/register/list/tokens" baseUrl="http://api.nexus-interactions.io:8080" summary="list/tokens" %}
{% swagger-description %}
This will list off all of the tokens that were created by a particular signature chain.
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain (optional if username is supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="count" %}
Optional boolean field that determines whether the response includes the transaction 

`count`

 field. This defaults to false, as including the transaction count can slow the response time of the method considerably
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

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="tokens list" %}
```json
[
    {
        "name": "mytoken1",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "balance": 0,
        "maxsupply": 26738388,
        "currentsupply": 26738388,
        "decimals": 2
    },
    {
        "name": "mytoken2",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "balance": 990,
        "maxsupply": 1000,
        "currentsupply": 10,
        "decimals": 8
    }


]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/tokens  
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME", // optional 
    // count: true, //optional (defaults to false)
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/register/list/tokens`, {
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
    # "count": True, #optional (defaults to False)
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/register/list/tokens", json=data)
print(response.json())

```
{% endtab %}
{% endtabs %}

#### Parameters:

`sort` : Determines which field the results should be sorted on. Values can be `balance`, `stake`, or `trust`. Default is `trust`

`order` : Determines the order of the sort. Values can be `desc` for descending (the default) or `asc` for ascending.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "name": "mytoken1",
        "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "balance": 0,
        "maxsupply": 26738388,
        "currentsupply": 26738388,
        "decimals": 2
    },
    {
        "name": "mytoken2",
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "balance": 990,
        "maxsupply": 1000,
        "currentsupply": 10,
        "decimals": 8
    }


]
```

#### Return values:

`created` : The UNIX timestamp when the token was created.

`modified` : The UNIX timestamp when the token was last modified.

`name` : The name identifying the token. For privacy purposes, this is only included in the response if the caller is the owner of the token

`address` : The register address for the account

`balance` : The available balance of this token. Initially the balance of the token is equal to the max supply, until the tokens have been distributed to accounts. This is the last confirmed balance less any new debits that you have made since the last block.

`pending` : This is the sum of all confirmed debit transactions that have been made to this token, that have not yet been credited. To move tokens from pending into the available balance you must create a corresponding credit transaction. NOTE: if configured to run, the events processor does this for you.

`unconfirmed` : This is the sum of all unconfirmed debit transactions that have been made to this token PLUS the sum of all unconfirmed credits that you have for confirmed debit transactions. When someone makes a debit to the token it will immediately appear in the unconfirmed balance until that transaction is included in a block, at which point it moves into `pending`. When you (or the events processor) creates the corresponding credit transaction for that debit, the amount will move from `pending` back into `unconfirmed` until the credit transaction is included in a block, at which point the amount moves to `balance`.

`maxsupply` : The maximum available supply for this token.

`currentsupply` : The current number of tokens that have been distributed from this token register.

`decimals` : The maximum number of decimal places that can be applied to token amounts. For example decimals=2 will allow a token amount to be given to 2 decimal places.

`count` : Only returned if the caller requested `count` : true. This is the number of transactions made to/from the token.

## `list/names`

This will list off all of the names created on the network.&#x20;

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/register/list/names`

{% swagger method="post" path="/register/list/names" baseUrl="http://api.nexus-interactions.io:8080" summary="list/names" %}
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
fetch(`${SERVER_URL}/register/list/names`, {
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
response = requests.post(f"{SERVER_URL}/register/list/names", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`sort` : Determines which field the results should be sorted on.&#x20;

`order` : Determines the order of the sort. Values can be `desc` for descending (the default) or `asc` for ascending.

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

### `list/namespaces`

This will list off all of the namespaces created on the network.

#### Endpoint:

`/register/list/namespaces`

{% swagger method="post" path="/users/list/namespaces" baseUrl="http://api.nexus-interactions.io:8080" summary="list/namespaces" %}
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
fetch(`${SERVER_URL}/register/list/namespaces`, {
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
response = requests.post(f"{SERVER_URL}/register/list/namespaces", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`sort` : Determines which field the results should be sorted on.&#x20;

`order` : Determines the order of the sort. Values can be `desc` for descending (the default) or `asc` for ascending.

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

### ``

***
