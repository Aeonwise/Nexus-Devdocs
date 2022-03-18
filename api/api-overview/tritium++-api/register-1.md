---
description: Register API
---

# REGISTER

### `Methods`

The following methods are currently supported by this API

[`list/accounts`](register-1.md#list-accounts)\
[`list/names`](register-1.md#list-names)``

### list/accounts



### list/names

```
register/list/accounts WHERE 'object.name=d*'
```

The above will return all accounts that start with a letter 'd'.

#### Following wildcard

The following demonstrates how to check with wildcards.

```
register/list/accounts WHERE 'object.name=*d'
```

This above will return all accounts that have a name ending with letter 'd'.

### Examples

To filter, you can use where='statements' or follow the command with WHERE string:

#### Filtering objects with WHERE clause

The below clause will filter all name object registers, that are Global names that start with letter 'P', or any objects that start with letter 'S'.

```
register/list/names WHERE '(object.namespace=*GLOBAL* AND object.name=P*) OR object.name=S*'
```

Using the object class i.e. 'object.namespace' will invoke the filter on the binary object.

#### Filtering objects with where=

The below clause will filter all name object registers, that are Global names that start with letter 'P', or any objects that start with letter 'S'.

```
register/list/names where='(object.namespace=*GLOBAL* AND object.name=P*) OR object.name=S*'
```

#### Filtering with multiple operators

The below will return all NXS accounts that have a balance greater than 10 NXS.

```
register/list/accounts WHERE 'object.token=0 AND object.balance>10'
```

#### Creating logical grouping

The following demonstrates how to query using multiple recursive levels.

```
register/list/accounts WHERE '(object.token=0 AND object.balance>10) OR (object.token=8Ed7Gzybwy3Zf6X7hzD4imJwmA2v1EYjH2MNGoVRdEVCMTCdhdK AND object.balance>1)'
```

This will give all NXS accounts with balance greater than 10, or all accounts for token '8Ed7Gzybwy3Zf6X7hzD4imJwmA2v1EYjH2MNGoVRdEVCMTCdhdK' with balance greater than 1.

#### More complex queries

There is no current limit to the number of levels of recursion, such as:

```
register/list/names WHERE '((object.name=d* AND object.namespace=~GLOBAL~) OR (object.name=e* AND object.namespace=send.to)) OR object.namespace=*s'
```

The above command will return all objects starting with letter 'd' that are global names, or all objects starting with letter 'e' in the 'send.to' namespace, or finally all objects that are in a namespace that ends with the letter 's'.

### `list/trust`

This will list all known trust accounts

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
        "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "owner": "a2cfad9f505f8166203df2685ee7cb8582be9ae017dcafa4ca4530cd5b4f1dca",
        "created": 1569298627,
        "modified": 1569359986,
        "balance": 9.897773,
        "stake": 1000000.0,
        "stakerate": 0.5179354594112684
    },
    {
        "address": "8FBtvmLSLqhVM9PsJ5Zzy1XzcZyP9XaEErjSoGPaoN5fBJaQbp8",
        "owner": "a2e7b433a4dcb54c3b4dc1111d7945394dc3e140c9a400dc623b3f6d53ec758b",
        "created": 1569306341,
        "modified": 1569312960,
        "balance": 0.004081,
        "stake": 5000.0,
        "stakerate": 0.5013274626265375
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

***
