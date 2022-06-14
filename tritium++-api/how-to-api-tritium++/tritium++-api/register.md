---
description: REGISTER API
---

# REGISTER

The Register API gives direct access to the register data and this allows network wide information requested to be presented and does not need the user to be logged in.&#x20;

The full supported endpoint of the profiles URI is as follows:

```
register/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
register/verb/noun
```

## `Supported Operators`

The operators only work with the profiles `transactions` and `notifications` verbs. The following operators are supported for this API command-set:&#x20;

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
register/list/accounts
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
register/list/names/created,address
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
register/list/tokens
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

#### Parameters:

The parameters used are Filtering -Query DSL, along with the [sorting / filtering](register.md#user-content-create)

#### Return values:

The return values depends on the specified noun, check the return values of the specified API.

## Register API with Query DSL

A few register API calls which will showcase the power of registers, LLD, which is the database and  Query-DSL. This is going to be a powerful tool for developers.\


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
