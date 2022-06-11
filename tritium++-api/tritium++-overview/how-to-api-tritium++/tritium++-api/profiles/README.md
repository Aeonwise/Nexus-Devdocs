---
description: PROFILES API
---

# PROFILES

The Profiles API provides methods for creating and managing profiles. A profile is synonymous with a signature chain. The full supported endpoint of the profiles URI is as follows:

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
profile/transactions/master/contracts.amount/sum
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
profiles/notifications/master/amount,ticker
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

#### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
profiles/transactions/master/contracts.OP
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

[`create`](./#create) - Generate a new object of supported type.\
[`update`](./#update) - Updates the specified object registers.\
[`recover`](./#recover) - Recovers a profile account.\
[`status`](./#status) - Status information of a profile.\
[`notifications`](./#notifications) - List all notifications that modified specified object.\
[`transactions`](./#notifications-1) - List all transactions that modified specified object.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`master`] - The default profile that controls all sub-profiles.\
\[`auth`] - A crypto object register for login auth.\
\[`credentials`] -  Credentials used to secure profiles.\
\[`recovery`] - An object which represents recovery for the profile.

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

This will create a new object register specified by the noun.

```
profiles/create/noun
```

This command does not support the `credentials` or `recovery` nouns.

#### create/master

This will create a new profile (signature chain) specified by given noun, for use on the network. The user account is secured by a combination of username, password, and PIN.

#### Endpoint:

`/profiles/create/master`

#### create/auth

This method will create a crypto object register for login auth for signature chains created with Tritium, for use with Tritium++. Existing users need to first convert their Tritium accounts to be compatible and login with Tritium++ profiles. The interface wallet will do this automatically.

#### Endpoint:

`/profiles/create/auth`

### Parameters:

`username` : The username to be associated with this profile. The signature chain genesis (used to uniquely identify profiles) is a hash of this username, therefore the username must be unique on the blockchain.

`password` : The password to be associated with this profile.

`pin` : The PIN to be associated with this profile

{% hint style="danger" %}
If user forgets the username, he looses access to his nexus assets. There is no option to change the username. Be careful when you choose a username as it is case-sensitive and make a point to back it up with the password and pin.
{% endhint %}

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction that includes the created object.

## `update`

This method provides the user with the ability to change the password, pin, or recovery seed for this signature chain specified by the noun.

```
profiles/update/noun
```

This command does not support the `master` or `auth` nouns.

#### update/credentials

This method provides the user with the ability to change the password, pin for this signature chain. Updating the credentials will also result in each of the keys in the sig chain's Crypto object being regenerated based on the new password / pin.

#### Endpoint:

`/profiles/update/credentials`

#### update/recovery

This method provides the user with the ability to set or change the restore seed for this signature chain.

#### Endpoint:

`/profiles/update/credentials`

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`password` : The existing password for this signature chain.

`pin` : The existing pin for this signature chain.

#### update/credentials

`new_password` : The new password to set for for this signature chain. This is optional if new\_pin is provided

`new_pin` : The new pin to set for this signature chain. This is optional if new\_password is provided.

#### update/recovery

`recovery` : The existing recovery seed for this signature chain. This is only required if an existing recovery seed is being updated via `new_recovery.`

`new_recovery` : The new recovery seed to set on this sig chain. The recovery seed must be a minimum of 40 characters.

{% hint style="danger" %}
The recovery phrase is case-sensitive
{% endhint %}

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction for the updated object.

## `recover`

This recovers the profile (signature chain) specified by the noun, in case of lost password and pin . The user has to be logged out to recover his profile.&#x20;

```
profiles/recover/noun
```

This command only supports the `master` noun.

### Parameters:

`username` : The username identifying the profile.

`password` : The new password for this signature chain.

`pin` : The new pin for this signature chain.

`recovery` : The existing recovery seed for this signature chain.&#x20;

### Results:

#### Return value JSON object:

```
{
    "success": true,
    "txid": "017fbb86583c0e15c0fb994a1f4c70d97f2c084533748ccfd25cd36e5aef9c2e7f89f15f2ec9f2d73769fef9d7a8a28cd018c9907ebf1bf74e4f89837c900091"
}
[Completed in 15242.801822 ms]
```

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully.

`txid` : The ID (hash) of the transaction for the recovered object.

## `status`

Return status information for the requested signature chain.

```
profiles/status/noun
```

This command only supports the `master` noun.

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

### Results:

#### Return value JSON object:

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "confirmed": true,
    "recovery": true,
    "crypto": true,
    "transactions": 10
}
[Completed in 0.108500 ms]
```

#### Return values:

`genesis` : The signature chain genesis hash for the currently logged in signature chain.

`confirmed` : Boolean flag indicating whether the genesis transaction for this signature chain has at least one confirmation.

`recovery` : Flag indicating whether the recovery seed has been set for this signature chain.

`crypto` : Flag indicating whether the crypto object register has been set for this signature chain.

`transactions` : The total transaction count in this signature chain

## `notifications`

This will list off all of the transactions sent to a particular signature chain. It is useful for identifying transactions that you need to accept such as credits.

```
profiles/notifications/noun
```

This command only supports the `master` noun.

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

#### [Sorting / Filtering](./#sorting-filtering) Parameters

### Results:

#### Return value JSON object:

```
[
    {
        "id": 0,
        "OP": "DEBIT",
        "from": "8EW4ALiZxtiRHFGqND5Yah8oYiRaXBLEHBcrovshCDLT1ZE5ENk",
        "to": "8C96zrYqeyYYiRDQ9pWZkxgzMhmV7nxDUEeE8fBCufgWfJ4cnJ1",
        "amount": 150.0,
        "token": "8EW4ALiZxtiRHFGqND5Yah8oYiRaXBLEHBcrovshCDLT1ZE5ENk",
        "ticker": "NEX",
        "reference": 0,
        "timestamp": 1654620671,
        "txid": "012e6dc669cad475b0974308ba719bf5d69c0d7623e31399a3d83ca8e095b63ed7ac98709c08a5a15c543dd91c85c7cbb91082c355fa548435545cfb1b912a8f"
    }
]
[Completed in 0.256792 ms]
```

#### Return values:

`id` : The sequential ID of this transaction.

`OP` : The contract operation. Can be `COINBASE`, `DEBIT`, `FEE`, `TRANSFER`.

`txid` : The transaction that was credited / claimed.

`suppressed`: If the caller included the optional `suppressed` flag, then the response will include this field to indicate whether this notification is currently being suppressed or not.

`from` : For `DEBIT` transactions, the register address of the account that the debit is being made from.

`from_name` : For `DEBIT` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

`to` : For `DEBIT` transactions, the register address of the account that the debit is being made to.

`to_name` : For `DEBIT` transactions, the name of the account that the debit is being made to. Only included if the name can be resolved.

`amount` : the token amount of the transaction.

`token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

`ticker` : The name of the token that the transaction relates to.

`reference` : For `DEBIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

`proof` : The register address of the token account proving the the split dividend debit.

`dividend_payment` : Flag indicating that this debit is a split dividend payment to a tokenized asset

## `transactions`

This will list off all of the transactions for the requested signature chain.

```
profiles/transactions/noun
```

This command only supports the `master` noun.

### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

#### [Sorting / Filtering](./#sorting-filtering) Parameters

### Results:

#### Return value JSON object:

```
[
    {
        "txid": "01eb59a0c4e6c47dd2a8d3a4aa7d83514aab6f5c976207889f786e5947c0eb971065fe6e0f8fc6b7d75157f9dac0cf0c178c15e441ff360c3046c84e1fe799ca",
        "type": "tritium first",
        "version": 4,
        "sequence": 0,
        "timestamp": 1653674591,
        "blockhash": "dc2712f6598a4a8c60f14c707dae4705f773fb1f60b89efd1e0163ee1d7cf823e8d73e6a3fb9f7eb73e72d7764647eb889abebc2aca34f3090dbe1409af511a301409fb8f04409bf97693dc0d908d05d9b62b949aae423196b508b3840419d1df548e72e006c75a6c69d8075693f2c8f6f7d10213a07c03bf9ff250e55a1eeef",
        "confirmations": 23,
        "contracts": [
            {
                "id": 0,
                "OP": "CREATE",
                "address": "89AVoe5S8gjZpVngYoYtqJbSr9fGsdsoXShxnyNaEGPvMMeDnT6",
                "type": "OBJECT",
                "standard": "CRYPTO",
                "object": {
                    "app1": "0000000000000000000000000000000000000000000000000000000000000000",
                    "app2": "0000000000000000000000000000000000000000000000000000000000000000",
                    "app3": "0000000000000000000000000000000000000000000000000000000000000000",
                    "auth": "02b5dbda57225afab10ca271ff513beab748306cff028798e2b64505db59f252",
                    "cert": "0000000000000000000000000000000000000000000000000000000000000000",
                    "lisp": "0000000000000000000000000000000000000000000000000000000000000000",
                    "network": "025193caf52ce6065951a438d30696f7548e12e0ebadd60359141029c1c4d870",
                    "sign": "02efc8e11c7097520080718b5dd0e92267292401678fcecb317bde79df64311c",
                    "verify": "0000000000000000000000000000000000000000000000000000000000000000"
                }
            }
        ]
    }
]
[Completed in 0.616625 ms]
```

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

}
