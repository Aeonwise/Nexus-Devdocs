---
description: TOKENS API
---

# FINANCE METHODS

The Finance API provides methods for creating fungible tokens, creating accounts to store them in, and sending and receiving tokens between users.

### `Methods`

The following methods are currently supported by this API

[`create/account`](c-tokens.md#create-account)\
[`debit/account or trust`](c-tokens.md#debit-account-or-trust)\
[`credit/account or trust`](c-tokens.md#credit-account)\
[`get/account or trust`](c-tokens.md#get-account)\
`list/accounts`\
`transactions/accounts or trust`\
`create/token`\
`debit/token`\
[`credit/token`](c-tokens.md#credit-token)\
[`get/token`](c-tokens.md#get-token)\
[`burn/token`](c-tokens.md#burn-token)\
[`list/token/transactions`](c-tokens.md#list-token-transactions)\
[`list/token/accounts`](c-tokens.md#list-token-accounts)\
[`get/stakeinfo`](c-tokens.md#get-stakeinfo)\
[`set/stake`](c-tokens.md#set-stake)\
[`migrate/accounts`](c-tokens.md#migrate-accounts)\
[`get/balances`](c-tokens.md#get-balances)\
[`list/balances`](c-tokens.md#list-balances)\
[`list/trustaccounts`](c-tokens.md#list-trustaccounts)

***

### `create/account`

Create a new account for receiving NXS or tokens.&#x20;

#### Endpoint:

`/finance/create/account`

{% swagger method="post" path="/finance/create/account" baseUrl="http://api.nexus-interactions.io:8080" summary="create/account" %}
{% swagger-description %}
Create a new account for receiving NXS.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
The PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token_name" required="false" %}
Optional name of a token to create the account for. 

`token`

 can be supplied as an alternative to 

`token_name`

. Defaults to 

`NXS`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" required="false" %}
Optional token address to create the account for. 

`token_name`

 can be supplied as an alternative to 

`token`

. Defaults to 

`0`

 (

`NXS`

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="false" %}
An optional name to identify the account. If provided a Name object will also be created in the users local namespace, allowing the account to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" required="false" %}
This optional field allows callers to add arbitrary data to an account register
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Account created" %}
```json
{
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/account
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    token_name: "NXS", // optional if token is passed
    // token: "TOKEN ADDRESS TO CREATE THE ACCOUNT FOR",//optional if token_name is passed, Defaults to 0 (NXS)
    // name: "NAME TO IDENTIFY THE ACCOUNT", //optional
    // data: "OPTIONAL FIELD ALLOWS CALLERS TO ADD ARBITRARY DATA TO AN ACCOUNT REGISTER.",//optional
}
fetch(`${SERVER_URL}/finance/create/account`, {
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
    "token_name": "NXS",  # optional if token is passed
    # "token": "TOKEN ADDRESS TO CREATE THE ACCOUNT FOR",#optional if token_name is passed, Defaults to 0 (NXS)
    # "name": "NAME TO IDENTIFY THE ACCOUNT", #optional
    # "data": "OPTIONAL FIELD ALLOWS CALLERS TO ADD ARBITRARY DATA TO AN ACCOUNT REGISTER.",#optional
}
response = requests.post(f"{SERVER_URL}/finance/create/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account should be created with. For single-user API mode the session should not be supplied.

`token_name` : Optional name of a token to create the account for. `token` can be supplied as an alternative to `token_name`. Defaults to `NXS`.

`token` : Optional token address to create the account for. `token_name` can be supplied as an alternative to `token`. Defaults to `0` (`NXS`)

`name` : An optional name to identify the account. If provided a Name object will also be created in the users local namespace, allowing the account to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address.

`data` : This optional field allows callers to add arbitrary data to an account register.

#### Example:

This example creates a NXS account called "main".

```
{
    "pin": "1234",
    "name": "main"
}
```

#### Return value JSON object:

```
{
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the account creation.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

***

## ``

#### Example:

This example creates a token account called "main" for a token identified by the token name. NOTE the name has been qualified with a namespace which is the username of the signature chain that created the token

```
{
    "pin": "1234",
    "name": "main",
    "token_name": "bob:ABC"
}
```

#### Example:

This example creates a token account called "savings" for a token identified by its register address.

## `debit/account or trust`

### `debit/account`

Deduct an amount of NXS from an account and send it to another account or legacy UTXO address or deduct a token amount from a token account and send it to another token account.&#x20;

Any fee that is applicable for this transaction is deducted from the `default` account.

The method supports the ability to send to multiple recipients in one transaction by supplying a `recipients` array instead of the single `name_to`/`address_to`/`amount`. The method allows up to 99 recipients to be supplied.

#### Endpoint:

`/finance/debit/account`

#### `debit/trus`

Deduct an amount of NXS from the Trust account and send it to another account. Trust is a different object compared to account.

#### Endpoint:

`/finance/debit/trust`

{% swagger method="post" path="/finance/debit/account" baseUrl="http://api.nexus-interactions.io:8080" summary="debit/account" %}
{% swagger-description %}
Deduct a token amount from a token account and send it to another token account.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) that owns the token account. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="from" %}
The name identifying the token account to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token account to debit. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" required="true" %}
The amount of tokens to debit
{% endswagger-parameter %}

{% swagger-parameter in="body" name="to" %}
The name identifying the token account to send to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address_to" %}
The register address of the token account to send to. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="reference" %}
This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615
{% endswagger-parameter %}

{% swagger-parameter in="body" name="expires" %}
This optional field allows callers to specify an expiration for the debit transaction. The expires value is the 

`number of seconds`

 from the transaction creation time after which the transaction can no longer be credited by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="receipient" %}
This optional array can be provided as an alternative. Each object in the array can have 

`to`

/

`address_to`

, 

`amount`

, 

`reference`

, and 

`expires`

 fields, as described above. Up to 99 recipients can be included in the array.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Debit token account" %}
```json
{
    "success": true,
    "txid": "014b0807a12f492ee455a7d8c3eeb703bb9197f4f7784bb0b8414a3401eb2cc11044691127821a2ebe6968b28cccc0c80d17fe00c692071db1658981765a79cb"
}
[Completed in 4976.634701 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// debit/account
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  pin: "YOUR_PIN",
  //  session: "YOUR_SESSION_ID" //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  amount: 10,
  name_to: "TO_TOKEN_NAME", //optional if address_to is passed
  // address_to: "TO_TOKEN_ADDRESS", //optional if name_to is passed
  // reference: "64-bit unsigned integer", optional
  // expires: 604800, //optional (in seconds)
  // recipients: [
  //   { name: "NAME", amount: 10, name_to: "NAME_TO" },
  //   { name: "NAME", amount: 10, name_to: "NAME_TO" }
  // ] //optional
}
fetch(`${SERVER_URL}/finance/debit/account`, {
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
    #  "session": "YOUR_SESSION_ID" #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    "amount": 10,
    "name_to": "TO_TOKEN_NAME",  # optional if address_to is passed
    # "address_to": "TO_TOKEN_ADDRESS", #optional if name_to is passed
    # "reference": "64-bit unsigned integer", optional
    # "expires": 604800, #optional (in seconds)
    # "recipients": [
    #   { "name": "NAME", amount: 10, name_to: "NAME_TO"
}
response = requests.post(f"{SERVER_URL}/finance/debit/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`from` : The name identifying the account to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the account to debit. This is optional if the name is provided.

`amount` : The amount of NXS to debit.

`to` : The name identifying the account to send to. This is optional if address\_to is provided. The name should be in username:name format

`address_to` : The register address of the account to send to. This is optional if `to` is provided. The address\_to can also contain a legacy UTXO address if sending from a signature chain account to a legacy address.

`reference` : This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a `64-bit unsigned integer` in the range of 0 to 18446744073709551615

`expires` : This optional field allows callers to specify an expiration for the debit transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be credited by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

`recipients` : This optional array can be provided as an alternative. Each object in the array can have `to`/`address_to`, `amount`, `reference`, and `expires` fields, as described above. Up to 99 recipients can be included in the array.

#### Example:

The following example shows the json payload for a debit of 10.5 tokens from an account called "main" (that itself was created by the sig chain logged into session "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8") being sent to a token account identified by the name "bob:savings". In the name\_to field, "bob" refers to the username of the account holder and "savings" is their token account. If Bob is concerned about privacy he could have provided the register address of his token account instead

```
{
    "pin": "1234",
    "session": "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8",
    "from": "main",
    "amount": 10.5,
    "to": "bob:savings"
}
```

#### Example Multiple Recipients:

The following example shows the json payload for a debit from an account called "main" made to 3 separate recipients, each receiving 5 tokens, and each with a different reference

```
{
    "pin": "1234",
    "name": "main"
    "recipients" :
    [
        {
            "name_to": "bob:abc",
            "amount": 5,
            "reference": 1234
        },
        {
            "address_to": "8CHjS5Qe7Mghgrqb6NeEaVxjexbdp9p2QVdRTt8W4rzbWhu3fL8",
            "amount": 5,
            "reference": 5678
        },
        {
            "address_to": "8CM4SYrd6ohcBB8tfqyZG3MCtDStMUXN37gwC2RtkNSUayM2FSN",
            "amount": 5
        }
    ]
}
```

#### Return value JSON object:

```
{
    "success": true,
    "txid": "014b0807a12f492ee455a7d8c3eeb703bb9197f4f7784bb0b8414a3401eb2cc11044691127821a2ebe6968b28cccc0c80d17fe00c692071db1658981765a79cb"
}
[Completed in 4976.634701 ms]
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the debit.

## `credit/account or trust`

Increment an amount received from another NXS account to an account owned by your signature chain or increment an amount received from another token account to an account owned by your signature chain.&#x20;

#### Endpoint:

`/finance/credit/account`

{% swagger method="post" path="/finance/credit/account" baseUrl="http://api.nexus-interactions.io:8080" summary="credit/account" %}
{% swagger-description %}
Increment an amount received from another token account to an account owned by your signature chain.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the token account. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token account to credit. This is only required for split payments (where the receiving account is not included in the credit transaction) and is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the account to credit. This is only required for split payments (where the receiving account is not included in the credit transaction) and is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" %}
The transaction ID (hash) of the corresponding debit transaction for which you are creating this credit for.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name_proof" %}
he name identifying the account that proves your ability to credit the debit transaction. This is only required for split payments, where your right to receive a payment is determined by the number of tokens you hold in the account proof account. The 

`name_proof`

 parameter can be used as alternative to 

`proof`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address_proof" %}
The register address the account that proves your ability to credit the debit transaction. This is only required for split payments, where your right to receive a payment is determined by the number of tokens you hold in the account proof account. The 

`address_proof`

 parameter can be used as alternative to 

`name_proof`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="credited token" %}
```json
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// // credit/account
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  pin: "YOUR_PIN",
  // session: "YOUR_SESSION_ID" //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  txid: "256 char hash", // (hash) of the corresponding debit transaction for which you are creating this credit for.
  amount: 10,
  name_to: "TO_TOKEN_NAME", //optional if address_to is passed
  // name_proof: "NAME_PROOF", //used only for split payments (optional if address_proof is passed)
  // address_proof: "REGISTER_ADDRESS" //optional if name_proof is passed 
}
fetch(`${SERVER_URL}/finance/credit/account`, {
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
    # "session": "YOUR_SESSION_ID" #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # (hash) of the corresponding debit transaction for which you are creating this credit for.
    "txid": "256 char hash",
    "amount": 10,
    "name_to": "TO_TOKEN_NAME",  # optional if address_to is passed
    # "name_proof": "NAME_PROOF", #used only for split payments (optional if address_proof is passed)
    # "address_proof": "REGISTER_ADDRESS" #optional if name_proof is passed
}
response = requests.post(f"{SERVER_URL}/finance/credit/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the token account. For single-user API mode the session should not be supplied.

`name` : The name identifying the token account to credit. This is only required for split payments (where the receiving account is not included in the credit transaction) and is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the account to credit. This is only required for split payments (where the receiving account is not included in the credit transaction) and is optional if the name is provided.

`txid` : The transaction ID (hash) of the corresponding debit transaction for which you are creating this credit for.

`name_proof` : The name identifying the account that proves your ability to credit the debit transaction. This is only required for split payments, where your right to receive a payment is determined by the number of tokens you hold in the account proof account. The `name_proof` parameter can be used as alternative to `proof`

`address_proof` : The register address the account that proves your ability to credit the debit transaction. This is only required for split payments, where your right to receive a payment is determined by the number of tokens you hold in the account proof account. The `address_proof` parameter can be used as alternative to `name_proof`

#### Example:

The following example shows the json payload for a credit to an account called "savings" (that itself was created by the sig chain logged into session "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8") identified by the name "bob:savings". In the name field, "bob" refers to the username of the account holder and "savings" is their token account.

```
{
    "pin": "1234",
    "session": "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8",
    "name": "bob:savings",
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d"
}
```

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the token credit.

***

## `get/account or trust`

Retrieves information about a NXS account or a token account.&#x20;

#### Endpoint:

`/finance/get/account`

{% swagger method="post" path="/finance/get/account" baseUrl="http://api.nexus-interactions.io:8080" summary="get/account" %}
{% swagger-description %}
Retrieves information about a token account .
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token account to retrieve. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the account. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token account to retrieve. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="count" %}
Optional boolean field that determines whether the response includes the transaction 

`count`

 field. This defaults to false, as including the transaction count can slow the response time of the method considerably
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fieldname" %}
This optional field can be used to filter the response to return only a single field from the token account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="token account details" %}
```json
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1566534164,
    "modified": 1566616211,
    "name": "myaccount",
    "address": "8CbkwEQ9S8owmX74joU6XmiwxJq1aoiqUoXc9fLCKzw15HscM99",
    "token_name": "mytoken",
    "token": "8EHRNnxn5qX9gMF1Jbqvo1jkAeP7XY1PETnMtegv9rdr6QG3ZJy",
    "balance": 990,
    "pending": 0.0,
    "unconfirmed": 10
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/account
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  name: "TOKEN_NAME", //optional if address is passed
  // session: "YOUR_SESSION_ID", //optional
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  // count: false, //optional
  // fieldname: "FILTER_NAME", //optional
}
fetch(`${SERVER_URL}/finance/get/account`, {
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
    "name": "TOKEN_NAME",  # optional if address is passed
    # "session": "YOUR_SESSION_ID", #optional
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # "count": False, #optional
    # "fieldname": "FILTER_NAME", #optional
}
response = requests.post(f"{SERVER_URL}/finance/get/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the token account to retrieve. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the account. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the token account to retrieve. This is optional if the name is provided.

`count` : Optional boolean field that determines whether the response includes the transaction `count` field. This defaults to false, as including the transaction count can slow the response time of the method considerably.

`fieldname`: This optional field can be used to filter the response to return only a single field from the token account.

#### Return value JSON object:

```
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1566534164,
    "modified": 1566616211,
    "name": "myaccount",
    "address": "8CbkwEQ9S8owmX74joU6XmiwxJq1aoiqUoXc9fLCKzw15HscM99",
    "token_name": "mytoken",
    "token": "8EHRNnxn5qX9gMF1Jbqvo1jkAeP7XY1PETnMtegv9rdr6QG3ZJy",
    "balance": 990,
    "pending": 0.0,
    "unconfirmed": 10
}
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this account.

`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`name` : The name identifying the token account. For privacy purposes, this is only included in the response if the caller is the owner of the token account

`address` : The register address of the account.

`token_name` : The name of the token that this account can be used with.

`token` : This is register address of the token that this account can be used with.

`balance` : The available balance of this account. This is the last confirmed balance less any new debits that you have made since the last block

`pending` : This is the sum of all confirmed debit transactions that have been made to this account, that have not yet been credited. To move coins from pending into the available balance you must create a corresponding credit transaction. NOTE: if configured to run, the events processor does this for you.

`unconfirmed` : This is the sum of all unconfirmed debit transactions that have been made to this account PLUS the sum of all unconfirmed credits that you have for confirmed debit transactions. When someone makes a debit to your account it will immediately appear in the unconfirmed balance until that transaction is included in a block, at which point it moves into `pending`. When you (or the events processor) creates the corresponding credit transaction for that debit, the amount will move from `pending` back into `unconfirmed` until the credit transaction is included in a block, at which point the amount moves to `balance`.

`count` : Only returned if the caller requested `count` : true. This is the number of transactions made to/from the account.

***

## `transactions/account or trust`

This will list off all of the transactions related to a given account. You DO NOT need to be logged in to use this command. If you are logged in, then neither username or genesis are required as it will default to the logged in user.

{% hint style="info" %}
**NOTE** : If you use the username parameter it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/finance/transactions/account`

{% swagger method="post" path="/finance/transactions/account" baseUrl="http://api.nexus-interactions.io:8080" summary="transactions/account" %}
{% swagger-description %}
This will list off all of the transactions related to a given account. You DO NOT need to be logged in to use this command. If you are logged in, then neither username or genesis are required as it will default to the logged in user.
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" required="false" %}
The genesis hash identifying the signature chain to scan for transactions (optional if username is supplied or already logged in)
{% endswagger-parameter %}

{% swagger-parameter in="body" required="false" name="username" %}
The username identifying the signature chain to scan for transactions(optional if genesis is supplied or already logged in
{% endswagger-parameter %}

{% swagger-parameter in="body" required="false" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the account name in order to deduce the register address of the account. The

`session`

parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="false" name="name" %}
The name identifying the token account to list transactions for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the

`session`

parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token account to list transactions for. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" %}


Optional, determines how much transaction data to include in the response. Supported values are :\


`default` : hash, contracts

`summary` : type, version, sequence, timestamp, operation, and confirmations.

`detail` : genesis, nexthash, prevhash, pubkey and signature
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

{% swagger-response status="200: OK" description="account transaction list" %}
```json
[
    {
        "txid": "01034b39cb3635d370f97339e6f87b8751d4c0d62676da7d6ec20416966f298f47dea99603d03a74e638b0d50b31b1e721790e5b103abfe3353a709ccf5d1e7c",
        "contracts": [
            {
                "OP": "CREDIT",
                "txid": "01e73b498dbabbf4629ad674b9ae3824b96cca83199c25a67901db53b271d19acf1411b0c4f9a3d8ded80860ffe2dcf683d2d227a675d453303b31f86f868f9e",
                "contract": 0,
                "proof": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
                "to": "8CbkwEQ9S8owmX74joU6XmiwxJq1aoiqUoXc9fLCKzw15HscM99",
                "to_name": "test",
                "amount": 10,
                "token": "8EHRNnxn5qX9gMF1Jbqvo1jkAeP7XY1PETnMtegv9rdr6QG3ZJy",
                "token_name": "TST"
            }
        ]
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
let data = {
  // genesis: "GENESIS_HASH", //optional 
  username: "YOUR_USERNAME",//optional
  // session: "YOUR_SESSION_ID", //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  // verbose: "detail", //optional or "summary","none"
  // limit: 50, //optional
  // page: 1, //optional
  // offset: 10, //optional
  // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/finance/transactions/account`, {
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
    # "genesis": "GENESIS_HASH", #optional
    "username": "YOUR_USERNAME",  # optional
    # "session": "YOUR_SESSION_ID", #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # "verbose": "detail", #optional or "summary","none"
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(
    f"{SERVER_URL}/finance/transactions/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain to scan for transactions (optional if username is supplied or already logged in).

`username` : The username identifying the signature chain to scan for transactions(optional if genesis is supplied or already logged in).

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the account name in order to deduce the register address of the account. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`name` : The name identifying the token account to list transactions for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the token account to list transactions for. This is optional if the name is provided.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash, contracts
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "txid": "01034b39cb3635d370f97339e6f87b8751d4c0d62676da7d6ec20416966f298f47dea99603d03a74e638b0d50b31b1e721790e5b103abfe3353a709ccf5d1e7c",
        "contracts": [
            {
                "OP": "CREDIT",
                "txid": "01e73b498dbabbf4629ad674b9ae3824b96cca83199c25a67901db53b271d19acf1411b0c4f9a3d8ded80860ffe2dcf683d2d227a675d453303b31f86f868f9e",
                "contract": 0,
                "proof": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
                "to": "8CbkwEQ9S8owmX74joU6XmiwxJq1aoiqUoXc9fLCKzw15HscM99",
                "to_name": "test",
                "amount": 10,
                "token": "8EHRNnxn5qX9gMF1Jbqvo1jkAeP7XY1PETnMtegv9rdr6QG3ZJy",
                "token_name": "TST"
            }
        ]
    }
]
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

`for` : For `CREDIT` transactions, the contract that this credit was created for . Always `DEBIT` for token transactions.

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

## `create/token`

Create a new token object register.&#x20;

#### Endpoint:

`/finance/create/token`

{% swagger method="post" path="/finance/create/token" baseUrl="http://api.nexus-interactions.io:8080" summary="create/token" %}
{% swagger-description %}
Create a new token object register
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="true" %}
An optional name to identify the token. If provided a Name object will also be created in the users local namespace, allowing the token to be accessed/retrieved by name. If no name is provided the token will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="supply" required="true" %}
The initial token supply amount. Must be a whole number amount
{% endswagger-parameter %}

{% swagger-parameter in="body" name="decimals" required="true" %}
The maximum number of decimal places that can be applied to token amounts. For example decimals=2 will allow a token amount to be given to 2 decimal places
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="created token" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/token
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  pin: "YOUR_PIN",
  //  session: "YOUR_SESSION_ID", //optional
  // name: "TOKEN_NAME", //optional
  supply: 1000000,
  decimals: 2,
}
fetch(`${SERVER_URL}/finance/create/token`, {
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
    #  "session": "YOUR_SESSION_ID", #optional
    # "name": "TOKEN_NAME", #optional
    "supply": 1000000,
    "decimals": 2,
}
response = requests.post(f"{SERVER_URL}/finance/create/token", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token should be created with. For single-user API mode the session should not be supplied.

`name` : An optional name to identify the token. If provided a Name object will also be created in the users local namespace, allowing the token to be accessed/retrieved by name. If no name is provided the token will need to be accessed/retrieved by its 256-bit register address.

`supply` : The initial token supply amount. Must be a whole number amount.

`decimals` : The maximum number of decimal places that can be applied to token amounts. For example decimals=2 will allow a token amount to be given to 2 decimal places.

#### Example:

```
{
    "pin": "1234",
    "name": "ABC",
    "supply": 1000000,
    "decimals": 4
}
```

#### Return value JSON object:

```
{
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the token creation.

`address` : The register address for this token. The address (or name that hashes to this address) is needed when creating token accounts, so that we know which token the account can be used with.

***

## `debit/token`

Deduct a token amount from a token's initial supply and send it to a token account.&#x20;

The method supports the ability to send to multiple recipients in one transaction by supplying a `recipients` array instead of the single `name_to`/`address_to`/`amount`. The method allows up to 99 recipients to be supplied.

#### Endpoint:

`/finance/debit/token`

{% swagger method="post" path="/finance/debit/token" baseUrl="http://api.nexus-interactions.io:8080" summary="debit/token" %}
{% swagger-description %}
Deduct a token amount from a token's initial supply and send it to a token account
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token owner. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token to debit. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" %}
The amount of tokens to debit
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name_to" %}
The name identifying the token account to send to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address_to" %}
The register address of the token account to send to. This is optional if the name is provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="reference" %}
This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615
{% endswagger-parameter %}

{% swagger-parameter in="body" name="expires" %}
This optional field allows callers to specify an expiration for the debit transaction. The expires value is the 

`number of seconds`

 from the transaction creation time after which the transaction can no longer be credited by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recipients" %}
This optional array can be provided as an alternative to the single 

`name_to`

/

`address_to`

, 

`amount`

, 

`reference`

, and 

`expires`

 fields. Each object in the array can have 

`name_to`

/

`address_to`

, 

`amount`

, 

`refer`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="token debited" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// debit/token
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  pin: "YOUR_PIN",
  //  session: "YOUR_SESSION_ID" //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  amount: 10,
  name_to: "TO_TOKEN_NAME", //optional if address_to is passed
  // address_to: "TO_TOKEN_ADDRESS", //optional if name_to is passed
  // reference: "64-bit unsigned integer", optional
  // expires: 604800, //optional (in seconds)
  // recipients: [
  //   { name: "NAME", amount: 10, name_to: "NAME_TO" },
  //   { name: "NAME", amount: 10, name_to: "NAME_TO" }
  // ] //optional
}
fetch(`${SERVER_URL}/finance/debit/token`, {
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
    #  "session": "YOUR_SESSION_ID" #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    "amount": 10,
    "name_to": "TO_TOKEN_NAME",  # optional if address_to is passed
    # "address_to": "TO_TOKEN_ADDRESS", #optional if name_to is passed
    # "reference": "64-bit unsigned integer", optional
    # "expires": 604800, #optional (in seconds)
    # "recipients": [
    #   { "name": "NAME", amount: 10, name_to: "NAME_TO"
}
response = requests.post(f"{SERVER_URL}/finance/debit/token", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the token to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the token to debit. This is optional if the name is provided.

`amount` : The amount of tokens to debit.

`name_to` : The name identifying the token account to send to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address_to` : The register address of the token account to send to. This is optional if the name is provided.

`reference` : This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615

`expires` : This optional field allows callers to specify an expiration for the debit transaction. The expires value is the `number of seconds` from the transaction creation time after which the transaction can no longer be credited by the recipient. Conversely, when you apply an expiration to a transaction, you are unable to void the transaction until after the expiration time. If expires is set to 0, the transaction will never expire, making the sender unable to ever void the transaction. If omitted, a default expiration of 7 days (604800 seconds) is applied.

`recipients` : This optional array can be provided as an alternative to the single `name_to`/`address_to`, `amount`, `reference`, and `expires` fields. Each object in the array can have `name_to`/`address_to`, `amount`, `reference`, and `expires` fields, as described above. Up to 99 recipients can be included in the array.

#### Example:

The following example shows the json payload for a debit of 10.5 ABC tokens (that itself was created by the sig chain logged into session "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8") being sent to a token account identified by the name "jack:savings". In the name\_to field, "jack" refers to the username of the account holder and "savings" is their token account. If jack is concerned about privacy he could have provided the register address of his token account instead

```
{
    "pin": "1234",
    "session": "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8",
    "name": "bob:ABC",
    "amount": 10.5,
    "name_to": "jack:savings"
}
```

#### Example Multiple Recipients:

The following example shows the json payload for a debit from a token called "ABC" made to 3 separate recipients, each receiving 5 ABC tokens, and each with a different reference

```
{
    "pin": "1234",
    "name": "ABC"
    "recipients" :
    [
        {
            "name_to": "bob:abc",
            "amount": 5,
            "reference": 1234
        },
        {
            "address_to": "8CHjS5Qe7Mghgrqb6NeEaVxjexbdp9p2QVdRTt8W4rzbWhu3fL8",
            "amount": 5,
            "reference": 5678
        },
        {
            "address_to": "8CM4SYrd6ohcBB8tfqyZG3MCtDStMUXN37gwC2RtkNSUayM2FSN",
            "amount": 5
        }
    ]
}
```

#### Return value JSON object:

```
{
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the token debit.

***

## `credit/token`

Increment the token balance by an amount received from a token account. This method allows fungible tokens to be recycled back to the token creator in circumstances where they have reached the end of their life-cycle, for example a loyalty token that has been used by the customer to pay for a product can be debited from the customer's account and back to the token for reuse.&#x20;

#### Endpoint:

`/finance/credit/token`

{% swagger method="post" path="/finance/credit/token" baseUrl="http://api.nexus-interactions.io:8080" summary="credit/token" %}
{% swagger-description %}
Increment the token balance by an amount received from a token account
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the token account. For single-user API mode the session should not be supplie
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token to credit. This is optional if the address is provided. 

`name`

 : The name identifying the token to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token to credit. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txid" required="true" %}
The transaction ID (hash) of the corresponding debit transaction for which you are creating this credit for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="credited tokens" %}
```json
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// credit/token
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  pin: "YOUR_PIN",
  //  session: "YOUR_SESSION_ID", //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  txid: "of the debit transaction",
}
fetch(`${SERVER_URL}/finance/credit/token`, {
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
    #  "session": "YOUR_SESSION_ID", #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    "txid": "of the debit transaction",
}
response = requests.post(f"{SERVER_URL}/finance/credit/token", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the token account. For single-user API mode the session should not be supplied.

`name` : The name identifying the token to credit. This is optional if the address is provided. `name` : The name identifying the token to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the token to credit. This is optional if the name is provided.

`txid` : The transaction ID (hash) of the corresponding debit transaction for which you are creating this credit for.

#### Example:

The following example shows the json payload for a credit to a token called "mytoken" (that itself was created by the sig chain logged into session "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8").

```
{
    "pin": "1234",
    "session": "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8",
    "name": "mytoken",
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d"
}
```

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the token credit.

***

## `get/token`

Retrieves information about a token and its supply.&#x20;

#### Endpoint:

`/finance/get/token`

{% swagger method="post" path="/finance/get/token" baseUrl="http://api.nexus-interactions.io:8080" summary="get/token" %}
{% swagger-description %}
Retrieves information about a token and its supply
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token to debit. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the token. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token to retrieve. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="count" %}
Optional boolean field that determines whether the response includes the transaction 

`count`

 field. This defaults to false, as including the transaction count can slow the response time of the method considerably
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fieldname" %}
This optional field can be used to filter the response to return only a single field from the toke
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="token details" %}
```json
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1566534164,
    "modified": 1566616211,
    "name": "mytoken",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
    "balance": 990,
    "pending": 0,
    "unconfirmed": 0,
    "maxsupply": 1000,
    "currentsupply": 10,
    "decimals": 0
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/token
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  name: "TOKEN_NAME", //optional if address is passed
  //  session: "YOUR_SESSION_ID", //optional
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  // count: false, //optional
  // fieldname: "FILTER_NAME", //optional
}
fetch(`${SERVER_URL}/finance/get/token`, {
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
    "name": "TOKEN_NAME",  # optional if address is passed
    #  "session": "YOUR_SESSION_ID", #optional
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # "count": False, #optional
    # "fieldname": "FILTER_NAME", #optional
}
response = requests.post(f"{SERVER_URL}/finance/get/token", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the token to retrieve information. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the token. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the token to retrieve. This is optional if the name is provided.

`count` : Optional boolean field that determines whether the response includes the transaction `count` field. This defaults to false, as including the transaction count can slow the response time of the method considerably.

`fieldname`: This optional field can be used to filter the response to return only a single field from the token.

#### Return value JSON object:

```
{
    "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "created": 1566534164,
    "modified": 1566616211,
    "name": "mytoken",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
    "balance": 990,
    "pending": 0,
    "unconfirmed": 0,
    "maxsupply": 1000,
    "currentsupply": 10,
    "decimals": 0
}
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this token.

`created` : The UNIX timestamp when the token was created.

`modified` : The UNIX timestamp when the token was last modified.

`name` : The name identifying the token. For privacy purposes, this is only included in the response if the caller is the owner of the token

`address` : The register address of the token.

`balance` : The available balance of this token. Initially the balance of the token is equal to the max supply, until the tokens have been distributed to accounts. This is the last confirmed balance less any new debits that you have made since the last block.

`pending` : This is the sum of all confirmed debit transactions that have been made to this token, that have not yet been credited. To move tokens from pending into the available balance you must create a corresponding credit transaction. NOTE: if configured to run, the events processor does this for you.

`unconfirmed` : This is the sum of all unconfirmed debit transactions that have been made to this token PLUS the sum of all unconfirmed credits that you have for confirmed debit transactions. When someone makes a debit to the token it will immediately appear in the unconfirmed balance until that transaction is included in a block, at which point it moves into `pending`. When you (or the events processor) creates the corresponding credit transaction for that debit, the amount will move from `pending` back into `unconfirmed` until the credit transaction is included in a block, at which point the amount moves to `balance`.

`maxsupply` : The maximum token supply amount.

`currentsupply` : The current amount of tokens that have been distributed to token accounts.

`decimals` : The maximum number of decimal places that can be applied to token amounts. For example decimals=2 will allow a token amount to be given to 2 decimal places.

`count` : Only returned if the caller requested `count` : true. This is the number of transactions made to/from the token.

***

## `burn/account`

This method can be used to take tokens permanently out of the current supply, a process commonly known as burning. The method will debit a token account and send the tokens back to the token address. However the transaction contains a condition that will always evaluate to false, guaranteeing that the debit can not be credited by the token issuer nor the sender. The result is that the amount burned will always appear in the "pending" balance of the token.

#### Endpoint:

`/finance/burn/account`

{% swagger method="post" path="/finance/burn/account" baseUrl="http://api.nexus-interactions.io:8080" summary="burn/token" %}
{% swagger-description %}
This method can be used to take tokens permanently out of the current supply, a process commonly known as burning
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token owner. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the account to debit the tokens from to be burnt. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the account to debit the tokens from the be burnt. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="amount" %}
The amount of tokens to burn
{% endswagger-parameter %}

{% swagger-parameter in="body" name="reference" %}
This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="tokens burnt" %}
```json
{
    "success": true,
    "address": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
    "txid": "0130b5696b59d0fc0764ed9f5848bd8bfca7c0b98a33e4325edc4c3633205866162396e33d9b3e8b0bf8f0af997b7559832f4aa862e86db6965341fff7791ee3"
}
[Completed in 4986.262276 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// burn/token
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  name: "TOKEN_NAME", //optional if address is passed
  // session: "YOUR_SESSION_ID", //optional
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  amount: 10,
  // reference: "64-bit unsigned integer", optional
}
fetch(`${SERVER_URL}/finance/burn/account`, {
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
    "name": "TOKEN_NAME",  # optional if address is passed
    # "session": "YOUR_SESSION_ID", #optional
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    "amount": 10,
    # "reference": "64-bit unsigned integer", optional
}
response = requests.post(f"{SERVER_URL}/finance/burn/account", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the token owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the account to debit the tokens from to be burnt. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the token was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the account to debit the tokens from the be burnt. This is optional if the name is provided.

`amount` : The amount of tokens to burn.

`reference` : This optional field allows callers to provide a reference, which the recipient can then use to relate the transaction to an order number, invoice number etc. The reference is be a 64-bit unsigned integer in the range of 0 to 18446744073709551615

#### Example:

The following example shows the json payload for a burn of 100 tokens from an account called "main" (that itself was created by the sig chain logged into session "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8")

```
{
    "pin": "1234",
    "session": "5e9d8aa625a1838f60f30e12058089169e32c968389f365428f7b0c878bb47f8",
    "name": "main",
    "amount": 100
}
```

#### Return value JSON object:

```
{
    "success": true,
    "address": "8Bk5PxsecfXWpbHsDXeZ47MCgDF7qDLsU4Y4MJw2VB29LsTR98z",
    "txid": "0130b5696b59d0fc0764ed9f5848bd8bfca7c0b98a33e4325edc4c3633205866162396e33d9b3e8b0bf8f0af997b7559832f4aa862e86db6965341fff7791ee3"
}
[Completed in 4986.262276 ms]
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the token burn.

***

## `transactions/token`

This will list off all of the transactions related to a given token. You DO NOT need to be logged in to use this command. If you are logged in, then neither username or genesis are required as it will default to the logged in user.

{% hint style="info" %}
**NOTE** : The returned transaction data will only include contracts that related to the requested token. Any other contracts are omitted from the transaction result.

**NOTE** : If you use the username parameter it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/finance/transactions/token`

{% swagger method="post" path="/finance/transactions/token" baseUrl="http://api.nexus-interactions.io:8080" summary="/transactions/token" %}
{% swagger-description %}
This will list off all of the transactions related to a given token. You DO NOT need to be logged in to use this command. If you are logged in, then neither username or genesis are required as it will default to the logged in user.
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain to scan for transactions (optional if username is supplied or already logged in)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain to scan for transactions(optional if genesis is supplied or already logged in)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the token name in order to deduce the register address of the token. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token to list transactions for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token to list transactions for. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" %}


Optional, determines how much transaction data to include in the response. Supported values are : \


`default` : hash, contracts

``

`summary` : type, version, sequence, timestamp, operation, and confirmations.

``

`detail` : genesis, nexthash, prevhash, pubkey and signature.
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

{% swagger-response status="200: OK" description="token transactions list" %}
```json
[
    {
        "txid": "0191c4091b6226efe47b1fe44efa0d1b7176e4ef4118b5b6451605e484e84af64e1d7eef856906b114fde681c8096177ae63737abfd3917b0a4e5c5981b5b889",
        "type": "tritium user",
        "version": 4,
        "sequence": 1,
        "timestamp": 1654808816,
        "blockhash": "e1dea907a7f80ac820caa5bfea44f64a5b8983af64faa0de83066a47105fb72ad3e4cc3b562ec87058baf331e2c1eff0be15c16bda577ce14a788a084812ec021d3976bfc6e64b1fbf4d31cdc3f3700fb9639dfc104d380d7f8c4e48f2aa595863e078a7ec0b600a641539e774ab18781213f3dbdbb8f3e245e7405927fbf1df",
        "confirmations": 33,
        "contracts": [
            {
                "id": 0,
                "OP": "CREATE",
                "address": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "type": "OBJECT",
                "standard": "TOKEN",
                "object": {
                    "balance": 10000.0,
                    "decimals": 2,
                    "currentsupply": 0.0,
                    "maxsupply": 10000.0,
                    "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                    "ticker": "XYZ"
                }
            }
        ]
    }
]
[Completed in 24.260713 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/token/transactions
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
  // genesis: "GENESIS_HASH", //optional 
  username: "YOUR_USERNAME",//optional
  // session: "YOUR_SESSION_ID", //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  // verbose: "detail", //optional or "summary","none"
  // limit: 50, //optional
  // page: 1, //optional
  // offset: 10, //optional
  // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/finance/transactions/token`, {
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
    # "genesis": "GENESIS_HASH", #optional
    "username": "YOUR_USERNAME",  # optional
    # "session": "YOUR_SESSION_ID", #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # "verbose": "detail", #optional or "summary","none"
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(
    f"{SERVER_URL}/finance/transactions/token", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain to scan for transactions (optional if username is supplied or already logged in).

`username` : The username identifying the signature chain to scan for transactions(optional if genesis is supplied or already logged in).

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the token name in order to deduce the register address of the token. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`name` : The name identifying the token to list transactions for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the token to list transactions for. This is optional if the name is provided.

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash, contracts
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "txid": "0191c4091b6226efe47b1fe44efa0d1b7176e4ef4118b5b6451605e484e84af64e1d7eef856906b114fde681c8096177ae63737abfd3917b0a4e5c5981b5b889",
        "type": "tritium user",
        "version": 4,
        "sequence": 1,
        "timestamp": 1654808816,
        "blockhash": "e1dea907a7f80ac820caa5bfea44f64a5b8983af64faa0de83066a47105fb72ad3e4cc3b562ec87058baf331e2c1eff0be15c16bda577ce14a788a084812ec021d3976bfc6e64b1fbf4d31cdc3f3700fb9639dfc104d380d7f8c4e48f2aa595863e078a7ec0b600a641539e774ab18781213f3dbdbb8f3e245e7405927fbf1df",
        "confirmations": 33,
        "contracts": [
            {
                "id": 0,
                "OP": "CREATE",
                "address": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                "type": "OBJECT",
                "standard": "TOKEN",
                "object": {
                    "balance": 10000.0,
                    "decimals": 2,
                    "currentsupply": 0.0,
                    "maxsupply": 10000.0,
                    "token": "8DXmAmkTtysSZUxM3ePA8wRmbSUofuHKSoCyDpN28aLuSrm1nDG",
                    "ticker": "XYZ"
                }
            }
        ]
    }
]
[Completed in 24.260713 ms]
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

`for` : For `CREDIT` transactions, the contract that this credit was created for . Always `DEBIT` for token transactions.

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

}

## `list/token/accounts`

This will list all accounts (globally) that have been created for a particular token.

#### Endpoint:

`/tokens/list/token/accounts`

{% swagger method="post" path="/tokens/list/token/accounts" baseUrl="http://api.nexus-interactions.io:8080" summary="list/token/accounts" %}
{% swagger-description %}
This will list all accounts (globally) that have been created for a particular token.
{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the token name in order to deduce the register address of the token. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the token to list accounts for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the token to list transactions for. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="token account list" %}
```json
[
    {
        "owner": "a1f94d55f41aba2f75b020d0582c8b607b72f4f2742e4cde6135558adc44583d",
        "created": 1576709402,
        "modified": 1576718254,
        "address": "8CsmdG4rP5J2RRY5M9ibTxKEpnZiJoGu5RzmGyxxy726sv6Nc76",
        "balance": 32930.0
    },
    {
        "owner": "a1fc3f7264e53dba1e1043e29e4087f57f5a30a2008fdc8483be7e6cd3d7ff97",
        "created": 1576655433,
        "modified": 1576673358,
        "address": "8BjpdTPJthrBDQan2mABjbYhs2FPKAih6Vq5oxELiXqXGVq1Mqt",
        "balance": 6586.0
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/token/accounts
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
  // session: "YOUR_SESSION_ID", //optional
  name: "TOKEN_NAME", //optional if address is passed
  // address: "TOKEN_ADDRESS", // optional if name is passed 
  // verbose: "detail", //optional or "summary","none"
  // limit: 50, //optional
  // page: 1, //optional
  // offset: 10, //optional
  // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/tokens/list/token/accounts`, {
  method: 'POST',
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
  .then(resp => resp.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
```
{% endtab %}

{% tab title="Pyhton" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    # "session": "YOUR_SESSION_ID", #optional
    "name": "TOKEN_NAME",  # optional if address is passed
    # "address": "TOKEN_ADDRESS", # optional if name is passed
    # "verbose": "detail", #optional or "summary","none"
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/tokens/list/token/accounts", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the token name in order to deduce the register address of the token. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`name` : The name identifying the token to list accounts for. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the token to list transactions for. This is optional if the name is provided.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "owner": "a1f94d55f41aba2f75b020d0582c8b607b72f4f2742e4cde6135558adc44583d",
        "created": 1576709402,
        "modified": 1576718254,
        "address": "8CsmdG4rP5J2RRY5M9ibTxKEpnZiJoGu5RzmGyxxy726sv6Nc76",
        "balance": 32930.0
    },
    {
        "owner": "a1fc3f7264e53dba1e1043e29e4087f57f5a30a2008fdc8483be7e6cd3d7ff97",
        "created": 1576655433,
        "modified": 1576673358,
        "address": "8BjpdTPJthrBDQan2mABjbYhs2FPKAih6Vq5oxELiXqXGVq1Mqt",
        "balance": 6586.0
    }
]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this account.

`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`address` : The register address of the account.

`balance` : The available balance of this account. This is the last confirmed balance for the account, and does not reflect and any new debits that have been made since the last block.



### `get/stakeinfo`

This will retrieve account values and staking metrics for the trust account belonging to the currently logged in signature chain. If called when the stake minter is not running, this method only returns trust account values. Staking metrics will return 0.

#### Endpoint:

`/finance/get/stakeinfo`

{% swagger method="post" path="/finance/get/stakeinfo" baseUrl="http://api.nexus-interactions.io:8080" summary="get/stakeinfo" %}
{% swagger-description %}
This will retrieve account values and staking metrics for the trust account belonging to the currently logged in signature chain. If called when the stake minter is not running, this method only returns trust account values, Staking metrics will return 0
{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the trust account. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fieldname" %}
This optional field can be used to filter the response to return only a single field from the account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "balance": 150,
    "stake": 5000,
    "trust": 54322,
    "new": false,
    "staking": true,
    "pooled": false,
    "onhold": false,
    "stakerate": 1.97,
    "trustweight": 58.97,
    "blockweight": 47.62,
    "stakeweight": 54.03,
    "change": false
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/stakeinfo
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // session: "YOUR_SESSION_ID", //optional
    fieldname: "FILTERING FIELD NAME" //optional
}
fetch(`${SERVER_URL}/finance/get/stakeinfo`, {
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
    # "session": "YOUR_SESSION_ID", #optional
    "fieldname": "FILTERING FIELD NAME"  # optional
}
response = requests.post(f"{SERVER_URL}/finance/get/stakeinfo", json=data)
print(response.json())

```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the trust account. For single-user API mode the session should not be supplied.

`fieldname`: This optional field can be used to filter the response to return only a single field from the account.

#### Return value JSON object:

```
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
    "balance": 150,
    "stake": 5000,
    "trust": 54322,
    "new": false,
    "staking": true,
    "pooled": false,
    "onhold": false,
    "stakerate": 1.97,
    "trustweight": 58.97,
    "blockweight": 47.62,
    "stakeweight": 54.03,
    "change": false
}
```

#### Return values:

`address` : The register address of the trust account.

`balance` : The current NXS balance of the trust account. This is general account balance that is not staked.

`stake` : The amount of NXS currently staked in the trust account.

`trust` : The current raw trust score of the trust account.

`new` : Indicates whether trust account is new (true, staking Genesis) or established (false, staking Trust).

`staking` : Indicates whether staking is actively running for user account (when false, weight metrics will be 0).

`pooled` : Flag indicating whether pooled staking is enabled or not

`onhold` : When trust account is new, any change to balance requires a minimum wait period before staking begins. During this period, staking is on hold and this field returns true. "staking" field will still be true when on hold.

`holdtime` : Conditional field. Only appears when onhold=true and will contain number of seconds remaining in hold period.

`stakerate` : The current annual reward rate earned for staking as an annual percent.

`trustweight` : The current trust weight applied to staking as a percent of maximum.

`blockweight` : The current block weight applied to staking as a percent of maximum.

`stakeweight` : The current stake weight (trust weight and block weight combined) as a percent of maximum.

`change` : Indicates whether or not there is a pending request to change stake. The remaining fields only appear when this one is true.

`amount` : Amount of stake change. Positive will be added to stake (moved from balance) and negative will unstake (move to balance).

`requested` : Timestamp when the stake change request was created.

`expires`: Timestamp when the stake change request expires, if assigned. Zero indicates does not expire.

***

### `set/stake`

Creates a stake change request for a signature chain's trust account. This request will add or remove stake to set the stake value to the requested amount. If the new value is more than the current stake amount, it adds stake from the account balance. If the new value is less, it removes stake to the account balance (with appropriate trust penalty, if applicable).

Requests are saved locally and take effect with the next stake block found by staking the signature chain's trust account. Because they are saved locally, you must continue to stake on the machine where it was created until the next stake block is found, or the request will not be processed.

Until implemented, you can update the request by calling set/stake again.

To remove a stake change request, you can either set an expiration time, or set the amount equal to the current trust account stake.

#### Endpoint:

`/finance/set/stake`

{% swagger method="post" path="" baseUrl="http://api.nexus-interactions.io:8080" summary="set/stake" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// set/stake
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    amount: 1, // the new amount of NXS to stake 
    expires: 0, // the new expiration time of the stake in seconds. 0 means no expiration
}
fetch(`${SERVER_URL}/finance/set/stake`, {
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
    "amount": 1,  # the new amount of NXS to stake
    "expires": 0,  # the new expiration time of the stake in seconds. 0 means no expiration
}
response = requests.post(f"{SERVER_URL}/finance/set/stake", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for the signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the trust account. For single-user API mode the session should not be supplied.

`amount` : The new amount of NXS to stake.

`expires` : Optional field to assign the number of seconds until the stake change request expires. A value of zero indicates it does not expire. Default is zero if not passed.

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the stake change.

***

### `migrate/accounts`

{% hint style="danger" %}
Depreciated
{% endhint %}

This method will migrate your legacy accounts to signature chain accounts, sending the balance across in the process. A new account will be created in your signature chain for each legacy account, with a corresponding matching name (unless flagged not to create names). The balance of each legacy account is sent to the newly created signature chain account in individual transactions. As such, each transaction incurs the default legacy fee of 0.01 NXS, which is deducted from the amount being migrated.

The method uses the arbitrary `data` field in the account object register to track which legacy account it was created from. As a result, it is possible to invoke this method multiple times, and each time it will sweep any NXS from legacy accounts to existing signature chain accounts (as well as creating any necessary new accounts).

#### Endpoint:

`/finance/migrate/accounts`

#### Parameters:

`pin` : The PIN for the signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) owns the trust account. For single-user API mode the session should not be supplied.

`walletpassphrase` : Optional field to provide the wallet passphrase. This value is required if the wallet is not currently unlocked.

`createname` : Optional boolean field indicating whether to create a Name record for the newly created signature chain accounts. If omitted, the default behaviour is to create a Name record, which incurs a fee of 1 NXS per account.

#### Return value JSON object:

```
[
    {
        "account": "default",
        "address": "8BzPhYgcHAP26CYnCvHTQmPiBH7FBC2PAB4rR9YgWxcK7L4c46X",
        "amount": 44.9,
        "txid": "022fed45add52c82110411c703c070519ef1136f3fe401def06a9fe29b0fe378935f17c3846c8824410e197f1caea2aaae5e994530cf6ee3d06207dd46126171"
    },
    {
        "account": "test1",
        "address": "8CEwJUPAkQQuBbAoQtiiswwk38ASuUZ63JQ2yLakV2cEXPbsmsd",
        "amount": 1.0,
        "txid": "02b88a4a9972aafdf09461bd976617162b8f8df853f46a5e0d98d608b987d5d52a3cb67c3861e6b47b65721d83b4b47c851af32b01df3292d1e814242966b658"
    }
]
```

#### Return values:

`account` : The legacy account name (and tritium account name, unless `createname=false` was explicitly set in the request)

`address` : The register address of the signature chain account that the legacy funds have been migrated to.

`amount` : The NXS amount transferred from the legacy account to the signature chain account.

`txid` : If the the legacy send was successful, the ID (hash) of the legacy transaction.

`error` : If the legacy send failed, this field includes the legacy send error message

***

### `get/balances`

This will retrieve a summary of balance information across all accounts belonging to the currently logged in signature chain for a particular token type.

#### Endpoint:

`/finance/get/balances`

{% swagger method="post" path="/finance/get/balances" baseUrl="http://api.nexus-interactions.io:8080" summary="get/balances" %}
{% swagger-description %}
This will retrieve a summary of balance information across all accounts belonging to the currently logged in signature chain for a particular token type
{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) to return data for. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token_name" %}
Optional name of a token to return the balances for. 

`token`

 can be supplied as an alternative to 

`token_name`

. Defaults to 

`NXS`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" %}
Optional token address to return balances for. 

`token_name`

 can be supplied as an alternative to 

`token`

. Defaults to 

`0`

 (

`NXS`

)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="account balance" %}
```json
{
    "token_name": "NXS",
    "token": "0000000000000000000000000000000000000000000000000000000000000000",
    "available": 1000,
    "pending": 50,
    "unconfirmed": 5,
    "stake": 5000,
    "immature": 100
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/balances
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // session: "YOUR_SESSION_ID", //optional
    token_name: "NXS", // optional if token is passed
    // token: "TOKEN ADDRESS TO CREATE THE ACCOUNT FOR",//optional if token_name is passed, Defaults to 0 (NXS)
}
fetch(`${SERVER_URL}/finance/get/balances`, {
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
    # "session": "YOUR_SESSION_ID", #optional
    "token_name": "NXS",  # optional if token is passed
    # "token": "TOKEN ADDRESS TO CREATE THE ACCOUNT FOR",#optional if token_name is passed, Defaults to 0 (NXS)
}
response = requests.post(f"{SERVER_URL}/finance/get/balances", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) to return data for. For single-user API mode the session should not be supplied.

`token_name` : Optional name of a token to return the balances for. `token` can be supplied as an alternative to `token_name`. Defaults to `NXS`.

`token` : Optional token address to return balances for. `token_name` can be supplied as an alternative to `token`. Defaults to `0` (`NXS`)

#### Return value JSON object:

```
{
    "token_name": "NXS",
    "token": "0000000000000000000000000000000000000000000000000000000000000000",
    "available": 1000,
    "pending": 50,
    "unconfirmed": 5,
    "stake": 5000,
    "immature": 100
}
```

#### Return values:

`token_name` : The name of the token that these balances are for, if known.

`token` : The register address of the token that these balances are for.

`available` : The current balance across all accounts that is available to be spent.

`pending` : The sum of all debit and coinbase transactions made to your accounts that are confirmed but have not yet been credited. This does NOT include immature and unconfirmed amounts.

`unconfirmed` : The sum of all debit transactions made to your accounts that are not confirmed, or credits you have made to your accounts that are not yet confirmed (not yet included in a block).

`immature` The sum of all coinbase transactions that have not yet reached maturity. Only included when returning NXS balances.

`stake` : The amount of NXS currently staked in the trust account. Only included when returning NXS balances.

***

### `list/balances`

This will retrieve a summary of balance information across all accounts for each coin/token type owned, belonging to the currently logged in signature chain.

#### Endpoint:

`/finance/list/balances`

{% swagger method="post" path="/finance/list/balances" baseUrl="http://api.nexus-interactions.io:8080" summary="list/balances" %}
{% swagger-description %}
This will retrieve a summary of balance information across all accounts for each coin/token type owned, belonging to the currently logged in signature chain
{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) to return data for. For single-user API mode the session should not be supplied
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

{% swagger-response status="200: OK" description="list balances" %}
```json
[
     {
        "token_name": "NXS",
        "token": "0000000000000000000000000000000000000000000000000000000000000000",
        "available": 17002.095753,
        "pending": 1.0,
        "unconfirmed": 0.0,
        "stake": 9000.0,
        "immature": 0.0
    },
    {
        "token_name": "ABC",
        "token": "d7885b8ce210375377ee23d6b54f922ddb64c9c8e70fabd392753337331bf29a",
        "available": 250.0,
        "pending": 0.0,
        "unconfirmed": 0.0
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// list/balances
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // session: "YOUR_SESSION_ID", //optional
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/finance/list/balances`, {
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
    # "session": "YOUR_SESSION_ID", #optional
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/finance/list/balances", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) to return data for. For single-user API mode the session should not be supplied.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
     {
        "token_name": "NXS",
        "token": "0000000000000000000000000000000000000000000000000000000000000000",
        "available": 17002.095753,
        "pending": 1.0,
        "unconfirmed": 0.0,
        "stake": 9000.0,
        "immature": 0.0
    },
    {
        "token_name": "ABC",
        "token": "d7885b8ce210375377ee23d6b54f922ddb64c9c8e70fabd392753337331bf29a",
        "available": 250.0,
        "pending": 0.0,
        "unconfirmed": 0.0
    }
]
```

#### Return values:

`token_name` : The name of the token that these balances are for, if known.

`token` : The register address of the token that these balances are for.

`available` : The current balance across all accounts that is available to be spent.

`pending` : The sum of all debit and coinbase transactions made to your accounts that are confirmed but have not yet been credited. This does NOT include immature and unconfirmed amounts.

`unconfirmed` : The sum of all debit transactions made to your accounts that are not confirmed, or credits you have made to your accounts that are not yet confirmed (not yet included in a block).

`immature` The sum of all coinbase transactions that have not yet reached maturity. Only included when returning NXS balances.

`stake` : The amount of NXS currently staked in the trust account. Only included when returning NXS balances.

***

### `list/trustaccounts`

This will list all known trust accounts

#### Endpoint:

`/finance/list/trustaccounts`

{% swagger method="post" path="/finance/list/trustaccounts" baseUrl="http://api.nexus-interactions.io:8080" summary="list/trustaccounts" %}
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
fetch(`${SERVER_URL}/finance/list/trustaccounts`, {
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
response = requests.post(f"{SERVER_URL}/finance/list/trustaccounts", json=data)
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

#### Return values:

`address` : The register address of the trust account

`owner` : The genesis hash of the trust account owner

`created` : The UNIX timestamp when the account was created.

`modified` : The UNIX timestamp when the account was last modified.

`balance` : The current NXS balance of the trust account. This is general account balance that is not staked.

`stake` : The amount of NXS currently staked in the trust account.

`trust` : The current raw trust score of the trust account.

`stakerate` : The current annual reward rate earned for staking as an annual percent.
