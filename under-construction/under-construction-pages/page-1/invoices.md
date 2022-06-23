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

### `Supported Operators`

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

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#create) - Generate a new object of supported type.\
[`get`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#get) - Get object of supported type.\
[`list`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#list) - List all objects owned by given user.\
`pay` - Issue funds from supported type.\
`cancel` - To cancel the created invoice\
[`history`](https://github.com/Nexusoft/LLL-TAO/blob/merging-sessions/docs/API/COMMANDS/FINANCE.MD#history) - Generate the history of all last states.\
[`transactions`](../../../getting-started/tritium++-api/broken-reference/) - List all transactions that modified specified object.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`invoice`] - An object register containing a token-id and balance.\
\[`outstanding`] - Generate the history of all last states.\
\[`paid`] - Claim a specified object register.\
\[`cancelled`] - Claim a specified object register.\


**Example:**

```
invocies/list/any
```

The above command will create a debit contract withdrawing from a random sample of your accounts, for all tokens you own.

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

***

## `create` <a href="#user-content-create" id="user-content-create"></a>

Create a new invoice object.

```
invoices/create/noun
```

This command only supports the `invoice` noun.

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the invoice should be created with. For single-user API mode the session should not be supplied.

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

## `pay` <a href="#user-content-create" id="user-content-create"></a>

Make a payment (debit) to settle an invoice.

```
invoices/pay/noun
```

This command only supports the `invoice` noun.

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the invoice to pay. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to pay. This is optional if the name is provided.

`name_from` : The name identifying the account to debit (pay the invoice from). This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address_from` : The register address of the account to debit (pay the invoice from). This is optional if the name is provided.

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

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the invoice to cancel. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to cancel. This is optional if the name is provided.

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

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice. This is optional if the name is provided.

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

## ``
