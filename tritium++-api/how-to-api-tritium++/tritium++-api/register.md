---
description: REGISTER API
---

# REGISTER

The Register API gives direct access to the register data and this allows network wide information requested to be presented and does not need the user to be logged in.

## Query DSL

The Query DSL allows you to sort and filter recursively to any logical depth. This DSL can be used in conjunction with operators to sort, filter, and compute data in real-time.

### Selector Keys

The following operators are supported for this API command-set:

\[`object`] - This selector key will evaluate the comparison on the raw binary object.\
\[`results`] - This selector key will evaluate the comparison on the results json.

### Supported Objects

The following object classes are supported:

* object - operates on a raw object register
* results - operates and filters based on the JSON results. This can be about 10x slower, so use sparingly.

### Using wildcards

If you are searching by a string parameter, you can includeM' as an any character wildcard match, so that you can search values based on a partial match.

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

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`list`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#list) - List off all the object registers for the type requested.\


## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`accounts`] - An object register containing a token-id and balance.\
\[`trust`] - An object register containing a token-id, balance, and trust.\
\[`token`] - An object register containing a token-id, balance, supply, and decimals.\
\[`names`] - An object register containing Names and Global Names.\
\[`namespaces`] - An object register containing namespaces.\


**Example:**

```
register/list/tokens
```

The above command will list all the token information created on the Nexus blockchain.

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

## `list`

This method provides the user with the ability to directly access the object register data specified by the noun and does not need the user to be logged in.

```
register/list/noun
```

### Parameters:

The parameters used are [Query DSL](register.md#query-dsl), along with the [sorting / filtering](register.md#user-content-create)

### Results:

The results depends on the specified noun.&#x20;

### Power of Query DSL

A few API calls which will showcase the power of registers, the LLD which is the database and the Query-DSL. This is going to be a powerful tool for developers.\


{% hint style="info" %}
All these calls will require the latest build of the core and will not work with the current stable version.
{% endhint %}

#### &#x20;To calculate the sum of all NXS on the tritium chain.

```
register/list/accounts,trust/total/sum sort=total order=desc limit=none where='object. token=0'
```

#### List all the namespaces created in ascending order (First to recent):

```
register/list/namespaces sort=created order=asc
```

#### List all the Global Names on the Network:

```
register/list/names where=object.namespace=~GLOBAL~
```

#### Create a rich list:

```
register/list/accounts,trust sort=total order=desc page=0 where=(object.token=0)
```
