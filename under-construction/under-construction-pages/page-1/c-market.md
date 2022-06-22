# C-MARKET

The full supported endpoint of the finance URI is as follows:

```
markets/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
markets/verb/noun
```

### `Supported Operators`

The following operators are supported for this API command-set:

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
markets/list/accounts/balance/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "balance": 333.376
}
```

### `Supported Verbs`

The following verbs are currently supported by this API command-set:

`create` - Generate a new object of supported type.\
`user` - Claim funds issued to account from debit.\
`list` - List all objects owned by given user.\
`execute` - \
`cancel` -&#x20;

### `Supported Nouns`

The following nouns are supported for this API command-set:

\[`bid`] - An object register containing a token-id and balance.\
\[`ask`] - An object register containing a token-id, balance, and trust.\
\[`order`] - An object register containing a token-id, balance, supply, and decimals.\
\[`executed`] - An object selection noun allowing mixed accounts of different tokens.\
\[`all`] - An object selection noun to collect all accounts for given token type.

**Example:**

```
marekts/debit/any
```

The above command will create a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

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

#### `create/bid`

#### `create/ask`

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`market` : token1/token2 - The token pair for which the order is created. (This implies you want to buy/bid  token 1 while ask/give token 2 in exchange&#x20;

`price` : The price of the token

`amount` : The amount of tokens to be exchanged

`to` : This is the receiving account name or register address to credit token1.

`from` : This is the sending account name or register address to debit the token2.&#x20;



#### Return value JSON object:

```
{
    "success": true,
    "address": "8CupQ2dym1CZGZZ7U3F8UQ2xR2fr2PZmtEqB5Ds2EJsG1jA3JZw",
    "txid": "012bc5f80460a9605706e643f9364722e97ffe3dd4e94bce8e41ea6ae70b3336fe6274cfe74583bf72cf77d8bbdc951ae1b25c706253590e0c6d74fa4f78f4df"
}
[Completed in 4991.918611 ms]
```

#### Return Values:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

## `list` <a href="#user-content-credit" id="user-content-credit"></a>

Create a new object register specified by given noun.

```
market/list/noun
```

This command supports the `any` wildcard noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`market` : The hash in **hexadecimal** encoding of the transaction that we are crediting.

#### Return value JSON object:

```
{
    "bids": [
        {
            "txid": "012bc5f80460a9605706e643f9364722e97ffe3dd4e94bce8e41ea6ae70b3336fe6274cfe74583bf72cf77d8bbdc951ae1b25c706253590e0c6d74fa4f78f4df",
            "timestamp": 1655815697,
            "owner": "b7392196b83aca438567558462cd0c5d982569c7cefa668500c4bf3e61a03b7a",
            "market": "XYZ/BIT",
            "price": 1.0,
            "type": "bid",
            "contract": {
                "OP": "DEBIT",
                "from": "8CupQ2dym1CZGZZ7U3F8UQ2xR2fr2PZmtEqB5Ds2EJsG1jA3JZw",
                "amount": 10.0,
                "token": "8EdBM41i1MAgb7w2tjEsdd7DeKntUcsw4JWK1LswW457e2jDa8a",
                "ticker": "BIT"
            },
            "order": {
                "OP": "DEBIT",
                "to": "8Cu9QHELK6ofHzbAwtWGnUcfkDz3zvGoSf7eDGZfh81AeMuqcYY",
                "amount": 10.0,
                "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "ticker": "XYZ"
            }
        }
    ],
    "asks": [
        {
            "txid": "016d62c69f05a82eefb463604325dd0331aeee787748a06619d6ca665c5a0576705c62de29f20c3eccfb8cbb25c4df169a53baa1e55493f5293fbbf864af7389",
            "timestamp": 1655817006,
            "owner": "b7a57ddfb001d5d83ab5b25c0eaa0521e6b367784a30025114d07c444aa455c0",
            "market": "XYZ/BIT",
            "price": 1.0,
            "type": "ask",
            "contract": {
                "OP": "DEBIT",
                "from": "8B9adrgaFX8hZvH1vQGjibHVYhsLuDTDu2Kn1FM25RZQtuUFyiE",
                "amount": 10.0,
                "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "ticker": "XYZ"
            },
            "order": {
                "OP": "DEBIT",
                "to": "8C3AiDrbDmbkeza1eBeeEJ2r5wqrnWGr9D1kewKqHfPzdY7jRkF",
                "amount": 10.0,
                "token": "8EdBM41i1MAgb7w2tjEsdd7DeKntUcsw4JWK1LswW457e2jDa8a",
                "ticker": "BIT"
            }
        }
    ]
}

```

#### Return Values:

`order` : `bid` or `ask`

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

## `execute` <a href="#user-content-credit" id="user-content-credit"></a>

Create a new object register specified by given noun.

```
market/list/noun
```

This command supports the `any` wildcard noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`market` : The hash in **hexadecimal** encoding of the transaction that we are crediting.

#### Return value JSON object:

```
{
    "success": true,
    "address": "8CupQ2dym1CZGZZ7U3F8UQ2xR2fr2PZmtEqB5Ds2EJsG1jA3JZw",
    "txid": "012bc5f80460a9605706e643f9364722e97ffe3dd4e94bce8e41ea6ae70b3336fe6274cfe74583bf72cf77d8bbdc951ae1b25c706253590e0c6d74fa4f78f4df"
}
[Completed in 4991.918611 ms]
```

#### Return Values:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

## `cancel` <a href="#user-content-credit" id="user-content-credit"></a>

Create a new object register specified by given noun.

```
market/list/noun
```

This command supports the `any` wildcard noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`market` : The hash in **hexadecimal** encoding of the transaction that we are crediting.

#### Return value JSON object:

```
{
    "success": true,
    "address": "8CupQ2dym1CZGZZ7U3F8UQ2xR2fr2PZmtEqB5Ds2EJsG1jA3JZw",
    "txid": "012bc5f80460a9605706e643f9364722e97ffe3dd4e94bce8e41ea6ae70b3336fe6274cfe74583bf72cf77d8bbdc951ae1b25c706253590e0c6d74fa4f78f4df"
}
[Completed in 4991.918611 ms]
```

#### Return Values:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

## `user` <a href="#user-content-credit" id="user-content-credit"></a>

Create a new object register specified by given noun.

```
market/list/noun
```

This command supports the `any` wildcard noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`market` : The hash in **hexadecimal** encoding of the transaction that we are crediting.

#### Return value JSON object:

```
{
    "success": true,
    "address": "8CupQ2dym1CZGZZ7U3F8UQ2xR2fr2PZmtEqB5Ds2EJsG1jA3JZw",
    "txid": "012bc5f80460a9605706e643f9364722e97ffe3dd4e94bce8e41ea6ae70b3336fe6274cfe74583bf72cf77d8bbdc951ae1b25c706253590e0c6d74fa4f78f4df"
}
[Completed in 4991.918611 ms]
```

#### Return Values:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.
