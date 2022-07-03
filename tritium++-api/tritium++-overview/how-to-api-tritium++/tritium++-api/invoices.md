---
description: INVOICES API
---

# INVOICES

The Invoices API provides users and application developers the ability to issue and pay invoices on the Nexus blockchain. The full supported endpoint of the invoices URI is as follows:

```
invoices/verb/noun/filter/operator
```

The minimum required components of the URI are:

```
invoices/verb/noun
```

## `Supported Operators`

The following operators are supported for this API command-set:

[`array`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#array) - Generate a list of values given from a set of filtered results.\
[`mean`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#mean) - Calculate the mean or average value across a set of filtered results.\
[`sum`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#sum) - Compute a sum of a set of values derived from filtered results.

**Example:**

```
invoices/list/invoice/balance/sum
```

**Result:**

This command will return a sum of the balances for all accounts:

```
{
    "balance": 333.376
}
```

## `Sorting / Filtering`

The following parameters can be used to apply **sorting** and **filtering** to the returned data-set.

`limit`: The number of records to return. _Default: 100_. limit=none is allowed

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
invoices/get/balances/balance/sum
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](invoices.md#user-content-create) - Generate a new object of supported type.\
[`get`](invoices.md#get) - Get object of supported type.\
[`list`](invoices.md#list) - List all objects owned by given user.\
[`pay`](invoices.md#user-content-create-2) - Issue funds from supported type.\
[`cancel`](invoices.md#user-content-create-3) - To cancel the created invoice\
[`history`](invoices.md#user-content-create-4) - Generate the history of all last states.\
[`transactions`](invoices.md#transactions) - List all transactions that modified specified object.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`invoice`] - An object register containing a token-id and balance.\
\[`outstanding`] - Generate the history of all last states.\
\[`paid`] - Claim a specified object register.\
\[`cancelled`] - Claim a specified object register.\


**Example:**

```
invocies/list/paid
```

The above command will list all the invoices that have been paid.

## `create` <a href="#user-content-create" id="user-content-create"></a>

Create a new invoice object.

```
invoices/create/noun
```

This command only supports the `invoice` noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`recipient` : The genesis hash of the signature chain to issue the invoice to. This is optional if the recipient\_username is provided.

`recipient_username` : The username identifying the user account (sig-chain) to issue the invoice to. This is optional if the recipient is provided.

`account_name` : The name identifying the account the invoice should be paid to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`account` : The register address of the account the invoice should be paid to. This is optional if the name is provided.

`name` : An optional name to identify the invoice. If provided a Name object will also be created in the users local namespace, allowing the invoice to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address.

`items` : Array of line items that make up this invoice. At least one item in the items array must be included. The total invoice amount is calculated as the sum of all item amounts, and each item amount is calculated as the `unit_amount` multiplied by the `units`\
{\
`unit_amount` : The unit amount to be invoiced for this line item. This amount should be supplied in the currency of the payment account

`units` : The number of units to be invoiced at the unit amount.\
}

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "address": "8ESvYizqdApiuKEBjZMF1hnB8asDqECaDwAstcH3UtJ4Z6ceCn2",
    "txid": "0131e17af8029b414814283a3d90813d12c238db6ddab213440249b795090a9cd77079d5804ec38303a59414d87108d4e44bf31f54a6c176285281a88ab5d737"
}
```

#### Return values:

`success` : Boolean flag indicating that the invoice was created successfully .

`address` : The register address for the invoice. The address (or name that hashes to this address) is needed when retrieving or paying the invoice.

`txid` : The ID (hash) of the transaction that includes the invoice creation.

## `get`

Retrieves information about an invoice.&#x20;

```
invoices/get/noun
```

This command only supports the `invoice` noun.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : Optional to **identify** the invoice using the name. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : Optional to **identify** the invoice using register address. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
[
    {
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "modified": 1581389107,
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
        "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "number": "0004",
        "PO": "Purch1234",
        "contact": "accounts@mycompany.com",
        "sender_detail": "My Company, 32 Some Street, Some Place",
        "recipient_detail": "Some recipient details such as address or email",
        "items": [
            {
                "description": "First item description",
                "base_price": 1.0,
                "tax": 0.1,
                "unit_amount": "1.1",
                "units": 3
            },
            {
                "description": "Second item description",
                "base_price": 5.0,
                "tax": 0.5,
                "unit_amount": "5.5",
                "units": 1
            }
        ],
        "amount": 8.8,
        "token": "0",
        "status": "PAID"
    }
]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this invoice.

`created` : The UNIX timestamp when the invoice was created.

`modified` : The UNIX timestamp when the invoice was last modified.

`name` : The name identifying the invoice. For privacy purposes, this is only included in the response if the caller is the owner of the invoice

`address` : The register address of the invoice.

`recipient` : The genesis hash of the signature chain to issue the invoice to.

`account` : The register address of the account the invoice should be paid to.

`items` : Array of line items that make up this invoice.\
{\
`unit_amount` : The unit amount to be invoiced for this line item.

`units` : The number of units to be invoiced at the unit amount.\
}

`amount` : The total invoice amount. This is the sum of all line item total amounts (unit\_amount x units).

`token` : The register address of the token that this invoice should be paid in. Set to 0 for NXS transactions

`status` : The current status of this invoice. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment).

## `list`

This will list all invoices issued or received by the signature chain.

```
invoices/list/noun
```

This command only supports the `invoice` noun.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`genesis` : The genesis hash identifying the signature chain (optional if username is supplied).

`username` : The username identifying the signature chain (optional if genesis is supplied).

`status` : Optional filter by invoice status. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment)

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

### Results:

#### Return value JSON object:

```
[
    {
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "modified": 1581389107,
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
        "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "number": "0004",
        "PO": "Purch1234",
        "contact": "accounts@mycompany.com",
        "sender_detail": "My Company, 32 Some Street, Some Place",
        "recipient_detail": "Some recipient details such as address or email",
        "items": [
            {
                "description": "First item description",
                "base_price": 1.0,
                "tax": 0.1,
                "unit_amount": "1.1",
                "units": 3
            },
            {
                "description": "Second item description",
                "base_price": 5.0,
                "tax": 0.5,
                "unit_amount": "5.5",
                "units": 1
            }
        ],
        "amount": 8.8,
        "token": "0",
        "status": "PAID"
    }
]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this invoice.

`created` : The UNIX timestamp when the invoice was created.

`modified` : The UNIX timestamp when the invoice was last modified.

`name` : The name identifying the invoice. For privacy purposes, this is only included in the response if the caller is the owner of the invoice

`address` : The register address of the invoice.

`recipient` : The genesis hash of the signature chain to issue the invoice to.

`account` : The register address of the account the invoice should be paid to.

`items` : Array of line items that make up this invoice.\
{\
`unit_amount` : The unit amount to be invoiced for this line item.

`units` : The number of units to be invoiced at the unit amount.\
}

`amount` : The total invoice amount. This is the sum of all line item total amounts (unit\_amount x units).

`token` : The register address of the token that this invoice should be paid in. Set to 0 for NXS transactions

`status` : The current status of this invoice. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment).

## `` <a href="#user-content-create" id="user-content-create"></a>

## `pay` <a href="#user-content-create" id="user-content-create"></a>

Make a payment (debit) to settle an invoice.

```
invoices/pay/noun
```

This command only supports the `invoice` noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the invoice to pay. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to pay. This is optional if the name is provided.

`name_from` : The name identifying the account to debit (pay the invoice from). This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address_from` : The register address of the account to debit (pay the invoice from). This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the DEBIT to pay the invoice AND the CLAIM to claim ownership of the invoice.

## `cancel` <a href="#user-content-create" id="user-content-create"></a>

Cancels an unpaid invoice so that ownership returns back to the issuers signature chain.

```
invoices/cancel/noun
```

This command only supports the `invoice` noun.

### Parameters:

`pin` : Required if **locked**. The `PIN` to authorize the transaction.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the invoice to cancel. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to cancel. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the DEBIT to pay the invoice AND the CLAIM to claim ownership of the invoice.

## `history` <a href="#user-content-create" id="user-content-create"></a>

This will get the history of the transactions for an invoice showing when it was created & transferred (two contracts that happen at the same point in time when the invoice is created) and claimed (paid or cancelled).

```
invoices/history/noun
```

This command only supports the `invoice` noun.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`name` : The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice. This is optional if the name is provided.

### Results:

#### Return value JSON object:

```
[
    {
        "type": "CLAIM",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 2752588510729237979,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "TRANSFER",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "CREATE",
        "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    }
]
```

#### Return values:

The return value is a JSON array of objects for each entry in the invoice's history:

`type` : The action that occurred - CREATE | TRANSFER | CLAIM

`owner` : The genesis hash of the signature chain that owned the invoice.

`modified` : The Unix timestamp of when the invoice was updated.

`checksum` : A checksum of the state register used for pruning.

`address` : The register address of the invoice

`name` : The name of the invoice

`data` : The JSON invoice data

## `transactions`

This will list off all of the transactions for the specified noun.

```
finance/transactions/noun
```

This command supports the `account, trust and token` nouns.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

This method supports the [Sorting / Filtering](invoices.md#sorting-filtering) parameters.

### Results

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
