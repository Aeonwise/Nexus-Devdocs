---
description: PROFILES API
---

# PROFILES

The profiles API provides methods for creating and managing profiles. A profile is synonymous with a signature chain.

{% hint style="info" %}
Profiles API will provide sub-profiles, which can be managed under the master profiles. The sub-profiles will be available in the next update.
{% endhint %}

The full supported endpoint of the `profiles` URI is as follows:

```
profiles/verb/noun/filter
```

The minimum required components of the URI are:

```
profiles/verb/noun
```

## `Supported Filters`

This command-set supports single or csv field-name filters.

**Example:**

```
profiles/list/accounts/balance,ticker
```

The above command will return an array of objects with only the `balance` and `ticker` JSON keys.

### `Recursive Filtering`

Nested JSON objects and arrays can be filtered recursively using the `.` operator.

```
finance/transactions/account/contracts.OP
```

When using recursive filtering, the nested hiearchy is retained.

```
[
    {
        "contracts": [
            {
                "OP": "CREATE"
            }
        ]
    },
    {
        "contracts": [
            {
                "OP": "CREDIT"
            }
        ]
    }
]
```

***

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

`create` - Generate a new object of supported type.\
`update` - Claim funds issued to account from debit.\
`recover` - Issue funds from supported type.\
`status` - Get object of supported type.\
`notifications` - List all objects owned by given user.\
`transactions` - List all transactions that modified specified object.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`master`] - The default profile that controls all sub-profiles.\
\[`auth`] - A crypto object register for login auth.\
\[`credentials`] -  Profile credentials used to secure profiles.\
\[`recovery`] - An object selection noun allowing mixed accounts of different tokens.\


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

```
finance/get/balances/balance/sumreate
```

Create a new object register specified by given noun.

```
profiles/create/noun
```

This command does not support the `credentials` or `recovery` nouns.

### Parameters:

`username` : The username to be associated with this profile. The signature chain genesis (used to uniquely identify profiles) is a hash of this username, therefore the username must be unique on the blockchain.

`password` : The password to be associated with this user.

`pin` : The PIN can be a combination of letters/numbers/symbols or could be tied into an external digital fingerprint. The PIN is required for all API calls that modify the profile (such as sending or claiming transactions).

{% hint style="danger" %}
If user forgets the username, he looses access to his nexus assets. There is no option to change the username.  Be careful when you choose a username (case sensitive) and make a point to back it up.&#x20;
{% endhint %}

### Results:

`txid` : The hash of the transaction that was generated for this tx. If using `-autotx` this field will be ommitted.

`address` : The register address for this account. The address (or name that hashes to this address) is needed when creating crediting or debiting the account.

```
{
    "success": true,
    "address": "8ESvYizqdApiuKEBjZMF1hnB8asDqECaDwAstcH3UtJ4Z6ceCn2",
    "txid": "0131e17af8029b414814283a3d90813d12c238db6ddab213440249b795090a9cd77079d5804ec38303a59414d87108d4e44bf31f54a6c176285281a88ab5d737"
}
```

## `Methods`

The following methods are currently supported by this API&#x20;

[`create/master`](users.md#create-master-1)\
[`update/credentials`](users.md#update-credentials)\
[`update/recovery`](users.md#update-credentials-1)\
[`recover/master`](users.md#recover-master)\
[`status/master`](users.md#status-master)\
[`notifications/master`](users.md#notifications-master)\
[`transactions/master`](users.md#transactions-master)\
[`create/auth`](users.md#create-auth)\


## `create/master`

This will create a new master profile (signature chain) for use on the network. The master profile is secured by a combination of username, password, and PIN

{% hint style="info" %}
**NOTE:**&#x20;

&#x20;_username_ must be a minimum of 3 characters\
&#x20;_password_ must be a minimum of 8 characters\
&#x20;_pin_ must be a minimum of 4 characters
{% endhint %}

#### Endpoint:

`/profiles/create/master`

{% swagger method="post" path="/profiles/create/master" baseUrl="http://api.nexus-interactions.io:8080" summary="create/master" %}
{% swagger-description %}
This will create a new master profile (signature chain) for use on the network. The master profile is secured by a combination of username, password, and PIN.

**NOTE** :&#x20;

_username_ must be a minimum of 3 characters\
_password_ must be a minimum of 8 characters\
pin must be a minimum of 4 characters
{% endswagger-description %}

{% swagger-parameter in="header" name="username" required="true" %}
The username to be associated with this profile. The signature chain genesis (used to uniquely identify profiles) is a hash of this username, therefore the username must be unique on the blockchain.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="password" required="true" %}
Password for the profile
{% endswagger-parameter %}

{% swagger-parameter in="header" name="pin" required="true" %}
The PIN can be a combination of letters/numbers/symbols or could be tied into an external digital fingerprint. The PIN is required for all API calls that modify the profile (such as sending or claiming transactions)
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="user account created" %}
```json
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
//  /users/create/user
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    username: "YOUR_USERNAME",
    password: "YOUR_SECRET",
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/profiles/create/master`, {
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
    "username": "YOUR_USERNAME",
    "password": "YOUR_SECRET",
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/profiles/create/master", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`username` : The username to be associated with this profile. The signature chain genesis (used to uniquely identify profiles) is a hash of this username, therefore the username must be unique on the blockchain.

{% hint style="danger" %}
If user forgets the username, he looses access to his nexus assets. There is no option to change the username.  Be careful when you choose a username (case sensitive) and make a point to back it up.&#x20;
{% endhint %}

`password` : The password to be associated with this user.

`pin` : The PIN can be a combination of letters/numbers/symbols or could be tied into an external digital fingerprint. The PIN is required for all API calls that modify the profile (such as sending or claiming transactions).

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```

#### Return values:

`success` : Boolean flag indicating that the `profile` was saved successfully .

`txid` : The ID (hash) of the transaction that includes the master profile creation.

## `update/credentials`

This method provides the user with the ability to change the password and pin, for this signature chain.

Updating the credentials will also result in each of the keys in the sig chain's Crypto object being regenerated based on the new password / pin.

This method requires the user to already be logged in.

#### Endpoint:

`/profiles/update/credentials`

{% swagger method="post" path="/profiles/update/credentials" baseUrl="http://api.nexus-interactions.io:8080" summary="update/credentials" %}
{% swagger-description %}
This method provides the user with the ability to change the password and pin, for this signature chain.
{% endswagger-description %}

{% swagger-parameter in="body" name="session" required="false" %}
When using multi-user API mode the session parameter must be supplied to identify which user to update
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" required="true" %}
The current password for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pin" required="true" %}
The current pin for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="new_password" required="true" %}
The new password for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="new_pin" required="true" %}
The new pin for this user account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user credentials updated" %}
```json
{
    "success": true,
    "txid": "01417fb5d5cde01677790b0c1f4b72e04f8eab2eece8ea9f0f122b891a916794c82210cb2613375e46783d376a84763c4e9e4d763ed092f908eaf1b3cd7641ec"
}
[Completed in 9943.302462 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/update/user
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    session: "YOUR_SESSION_ID",
    password: "YOUR_SECRET",
    pin: "YOUR_PIN",
    new_password: "NEW_PASSWORD",
    new_pin: "NEW_PIN",
}
fetch(`${SERVER_URL}/profiles/update/credentials`, {
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
    "session": "YOUR_SESSION_ID",
    "password": "YOUR_SECRET",
    "pin": "YOUR_PIN",
    "new_password": "NEW_PASSWORD",
    "new_pin": "NEW_PIN",
}
response = requests.post(f"{SERVER_URL}/profiles/update/credentials", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to update.

`password` : The existing password for this signature chain.

`pin` : The existing pin for this signature chain.

`new_password` : The new password to set for for this signature chain. This is optional if new\_pin is provided

`new_pin` : The new pin to set for this signature chain. This is optional if new\_password is provided.

#### Example 1:

The following example changes the existing password from `password1` to `password1234` and existing pin from `1234` to `12345`

```
{
    "password": "password1",
    "pin": "1234",
    "new_password": "password12345",
    "new_pin": "12345"
}
```

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01417fb5d5cde01677790b0c1f4b72e04f8eab2eece8ea9f0f122b891a916794c82210cb2613375e46783d376a84763c4e9e4d763ed092f908eaf1b3cd7641ec"
}
[Completed in 9943.302462 ms]
```

#### Return values:

`success` : Boolean flag indicating that the credentials were saved successfully.

`txid` : The ID (hash) of the transaction that includes the update to the signature chain credentials.

## `update/recovery`

This method provides the user with the ability to set or change the recovery seed for this signature chain.

This method requires the user to be logged in.

#### Endpoint:

`/profiles/update/recovery`

{% swagger method="post" path="/profiles/update/recovery" baseUrl="http://api.nexus-interactions.io:8080" summary="update/recovery" %}
{% swagger-description %}
This method provides the user with the ability to set or change the recovery seed for this signature chain.
{% endswagger-description %}

{% swagger-parameter in="body" name="session" required="false" %}
When using multi-user API mode the session parameter must be supplied to identify which user to update
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" required="true" %}
The current password for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pin" required="true" %}
The current pin for this user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recovery" required="false" %}
The existing recovery seed for this user account. This is only required if an existing recovery seed is being updated via 

`new_recovery`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="new_recovery" required="true" %}
new recovery seed to set on this sig chain. This is optional if new\_pin or new\_password is provided. The recovery seed must be a minimum of 40 characters.

**NOTE**: the recovery seed is case sensitive
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user credentials updated" %}
```json
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/update/user
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    session: "YOUR_SESSION_ID",
    password: "YOUR_SECRET",
    pin: "YOUR_PIN",
    recovery: "your recovery seed phrase", //Not required when setting the recovery phrase for the first time.
    new_recovery: "your new recovery seed phrase",
}
fetch(`${SERVER_URL}/profiles/update/recovery`, {
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
    "session": "YOUR_SESSION_ID",
    "password": "YOUR_SECRET",
    "pin": "YOUR_PIN",
    "recovery": "your recovery seed phrase", #Not required optional when setting the recovery phrase for the first time.
    "new_recovery": "your new recovery seed phrase",
}
response = requests.post(f"{SERVER_URL}/profiles/update/recovery", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

`session` : When using multi-user API mode the session parameter must be supplied to identify which user to update.

`password` : The existing password for this signature chain.

`pin` : The existing pin for this signature chain.

`recovery` : The existing recovery seed for this signature chain. This is only required if an existing recovery seed is being updated via `new_recovery.`

`new_recovery` : The new recovery seed to set on this sig chain. The recovery seed must be a minimum of 40 characters.

{% hint style="danger" %}
The recovery seed is case sensitive.
{% endhint %}

#### Example 1:

The following example sets the initial recovery seed on the sig chain

```
{
    "password": "password1",
    "pin": "1234",
    "new_recovery": "this is the recovery seed that I wish to use"
}
```

#### Example 2:

The following example updates the recovery seed on the sig chain

```
{
    "password": "password1",
    "pin": "1234",
    "recovery": "this is the recovery seed that I wish to use"
    "new_recovery": "this is the changed recovery seed I wish to use"
}
```

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01947f824e9b117d618ed49a7dd84f0e7c4bb0896e40d0a95e04e27917e6ecb6b9a5ccfba7d0d5c308b684b95e98ada4f39bbac84db75e7300a09befd1ac0999"
}
[Completed in 18533.182336 ms]
```

#### Return values:

`success` : Boolean flag indicating that the recovery seed was saved or changed successfully.

`txid` : The ID (hash) of the transaction that includes the update to the signature chain credentials.

## `recover/master`

This method provides the user with the ability to recover the profile in case of a lost password and pin.

This method requires that the user is not logged in.

#### Endpoint:

`/profiles/recover/master`

{% swagger method="post" path="/profiles/recover/master" baseUrl="http://api.nexus-interactions.io:8080" summary="recover/master" %}
{% swagger-description %}
This method provides the user with the ability to recover the profile in case of a lost password and pin..
{% endswagger-description %}

{% swagger-parameter in="body" name="username" required="true" %}
The username identifying the profile.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" required="true" %}
The new password for this sig chain.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pin" required="true" %}
The new pin for this sig chain
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recovery" required="true" %}
The existing recovery seed for this sig chain.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user credentials updated" %}
```json
{
    "success": true,
    "txid": "017fbb86583c0e15c0fb994a1f4c70d97f2c084533748ccfd25cd36e5aef9c2e7f89f15f2ec9f2d73769fef9d7a8a28cd018c9907ebf1bf74e4f89837c900091"
}
[Completed in 15242.801822 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/update/user
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    username: "YOUR_USERNAME",
    password: "YOUR_SECRET",
    pin: "YOUR_PIN",
    recovery: "your recovery seed phrase",
}
fetch(`${SERVER_URL}/profiles/update/recovery`, {
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
    "username": "YOUR_USERNAME",
    "password": "YOUR_SECRET",
    "pin": "YOUR_PIN",
    "recovery": "your recovery seed phrase",
}
response = requests.post(f"{SERVER_URL}/profiles/update/recovery", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

`username` : The username identifying the profile.

`password` : The new password for this signature chain.

`pin` : The new pin for this signature chain.

`recovery` : The existing recovery seed for this signature chain.&#x20;

{% hint style="danger" %}
The recovery phrase is case sensitive
{% endhint %}

#### Example 1:

The following example recovers the signature chain

```
{
    "username": "John Doe"
    "password": "password1",
    "pin": "1234",
    "recovery": "this is the recovery seed that I wish to use"
}
```

#### Return value JSON object:

```
{
    "success": true,
    "txid": "017fbb86583c0e15c0fb994a1f4c70d97f2c084533748ccfd25cd36e5aef9c2e7f89f15f2ec9f2d73769fef9d7a8a28cd018c9907ebf1bf74e4f89837c900091"
}
[Completed in 15242.801822 ms]
```

#### Return values:

`success` : Boolean flag indicating that the recovery was completed successfully.

`txid` : The ID (hash) of the transaction that includes the update to the signature chain credentials.

## `status/master`

Return status information for the currently logged in user or session

#### Endpoint:

`/profiles/status/master`

{% swagger method="post" path="/profiles/status/master" baseUrl="http://api.nexus-interactions.io:8080" summary="status/master" %}
{% swagger-description %}
Return status information for the currently logged in user
{% endswagger-description %}

{% swagger-parameter in="body" name="session" required="false" %}
When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="User status" %}
```json
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "confirmed": true,
    "recovery": true,
    "crypto": true,
    "transactions": 10
}
[Completed in 1.609584 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/get/status
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    session: "YOUR_SESSION_ID",
}
fetch(`${SERVER_URL}/profiles/status/master`, {
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
    "session": "YOUR_SESSION_ID",
}
response = requests.post(f"{SERVER_URL}/profiles/status/master", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

#### Return value JSON object:

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "confirmed": true,
    "recovery": true,
    "crypto": true,
    "transactions": 10
}
[Completed in 1.609584 ms]
```

#### Return values:

`genesis` : The signature chain genesis hash for the currently logged in signature chain.

`confirmed` : Boolean flag indicating whether the genesis transaction for this signature chain has at least one confirmation.

`recovery` : Flag indicating whether the recovery seed has been set for this signature chain.

`crypto` : Flag indicating whether the crypto object register has been set for this signature chain.

`transactions` : The total transaction count in this signature chain

## `notifications/master`

This will list of all of the transactions sent to a particular genesis or username. It is useful for identifying transactions that you need to accept such as credits.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/profiles/notifications/master`

{% swagger method="post" path="/profiles/notifications/master" baseUrl="http://api.nexus-interactions.io:8080" summary="notifications/master" %}
{% swagger-description %}
This will list off all of the transactions sent to a particular genesis or username. It is useful for identifying transactions that you need to accept such as credits.
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="suppressed" %}
Optional boolean flag indicating that suppressed notifications should be included in the list
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

{% swagger-response status="200: OK" description="" %}
```json
{
[
    {
        "OP": "DEBIT",
        "from": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "from_name": "abc",
        "to": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "to_name": "myasset",
        "amount": 10.0,
        "reference": 0,
        "token": "8GHxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nAbD",
        "token_name": "abc",
        "txid": "01fa33c49901f3a2622b724d33e6bae238a8a0c3615facb4a00842b1b3a8545e275aff1015f1f2d04bffb3f47f54f64aa917702e3a8904c1be874ce5e969adb4",
        "time": 1566479032,
        "proof": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "dividend_payment": true
    },
    {
        "OP": "TRANSFER",
        "address": "8HJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8n8Dc",
        "destination": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "force": false,
        "txid": "0123200b85d720d85211d245fa4a38671622accd544f049262506eff242241733c5acfb396499e88a9156534b7d7943d554b59cbb38070fa323b16d52e439995",
        "time": 1566479610
    }

]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/notifications
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    session: "YOUR_SESSION_ID",
    suppressed: true, // includes suppresed notifcations
    limit: 50, //optional
    page: 1, //optional
    offset: 10, //optional
    where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/profiles/notifications/master`, {
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
    # "suppressed": True, # includes suppresed notifcations
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/users/list/notifications", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the notifications should be processed for. For single-user API mode the session should not be supplied.

`suppressed`: Optional boolean flag indicating that suppressed notifications should be included in the list.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "OP": "DEBIT",
        "from": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "from_name": "abc",
        "to": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P",
        "to_name": "myasset",
        "amount": 10.0,
        "reference": 0,
        "token": "8GHxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nAbD",
        "token_name": "abc",
        "txid": "01fa33c49901f3a2622b724d33e6bae238a8a0c3615facb4a00842b1b3a8545e275aff1015f1f2d04bffb3f47f54f64aa917702e3a8904c1be874ce5e969adb4",
        "time": 1566479032,
        "proof": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse",
        "dividend_payment": true
    },
    {
        "OP": "TRANSFER",
        "address": "8HJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8n8Dc",
        "destination": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "force": false,
        "txid": "0123200b85d720d85211d245fa4a38671622accd544f049262506eff242241733c5acfb396499e88a9156534b7d7943d554b59cbb38070fa323b16d52e439995",
        "time": 1566479610
    }

]
```

#### Return values:

An array of contracts deifned as:

`OP` : The contract operation. Can be `COINBASE`, `DEBIT`, `FEE`, `TRANSFER`.

`txid` : The transaction that was credited / claimed.

`suppressed`: If the caller included the optional `suppressed` flag, then the response will include this field to indicate whether this notification is currently being suppressed or not.

`from` : For `DEBIT` transactions, the register address of the account that the debit is being made from.

`from_name` : For `DEBIT` transactions, the name of the account that the debit is being made from. Only included if the name can be resolved.

`to` : For `DEBIT` transactions, the register address of the account that the debit is being made to.

`to_name` : For `DEBIT` transactions, the name of the account that the debit is being made to. Only included if the name can be resolved.

`amount` : the token amount of the transaction.

`token` : the register address of the token that the transaction relates to. Set to 0 for NXS transactions

`token_name` : The name of the token that the transaction relates to.

`reference` : For `DEBIT` transactions this is the user supplied reference used by the recipient to relate the transaction to an order or invoice number.

`proof` : The register address of the token account proving the the split dividend debit.

`dividend_payment` : Flag indicating that this debit is a split dividend payment to a tokenized asset

## `transactions/master`

This will list off all of the transactions for the requested signature chain genesis or username (either can be used). If you are using single-user API mode and are logged in, then neither username or genesis are required. It will return transactions for the currently logged in user.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`/profiles/transactions/master`

{% swagger method="post" path="/profiles/transactions/master" baseUrl="http://api.nexus-interactions.io:8080" summary="transactions/master" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="verbose" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="order" %}
The transaction order, based on signature chain sequence. 'asc' for oldest first, 'desc' for most recent first. The default is 'desc'
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="transactions list" %}
```json
{
    "txid": "222fafd56cf1fa5dc8d274d83dc49d1fcd4ecee958c62338fa865c7d43e5ed9f0abb843fdbf5b161dd1d1b9e2e64a9a1cb3a32657b8559b03ef60210257f8206",
    "contracts": [
        {
            "OP": "COINBASE",
            "genesis": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
            "nonce": 3,
            "amount": 76499504
        }
    ],
    "type": "tritium base",
    "version": 1,
    "sequence": 10,
    "timestamp": 1560196174,
    "confirmations": 6,
    "genesis": "1ff463e036cbde3595fbe2de9dff15721a89e99ef3e2e9bfa7ce48ed825e9ec2",
    "nexthash": "117ac949f88a65f6466e2b332e88abb1b7548cc7a09fdf6512ef4d96b44c1b2e",
    "prevhash": "cdfd0721fd860f52046a4752c6e67a30b11541662f89815091800f54c994455ecf20994651c38d32c0d8a3c3affc32d88977987c25af30c4c907b9666802c229",
    "pubkey": "095afa24f8dcdf15ac8c7701d7a0429ca866b2881ad5a2e59af77d8c6ebeb04a150b5ecc23d4c60593d583022cefea32cdca51f8dd476653208c72a14150ae85567e90f90e649a67fd1a32e501ea135ee88859e2606aa8ec22f524e784c8e9e9722401512028878fa1a56606e4481641b58295d6879e665f1e180777d011e709d120b1214f647bf68713be98c0b968d86815c22a1d9a7128e780e910c8ad82c595b20504772b8c842507d4a26a9345500829a1d4b28665507e1789a526eb6be60662fb4da11e0cd50bd78e698ba0f4220b1798bc64492910dc84876a6c95ace3563e7a23da96627d7b04a1284606d029567bb26a0a412a4a8a82a09dc7062968894b11c22410d8711b90e33f4d20a5385a969d69b05118d2d17585c999e1ef35a97444c8b2da5b54c1ce556da90477c2020bb780e402daa0f4bdee92b5745433f71e4a30a42816f518e2a0da4d38fa49ce136e18aadfcef9463ad641e05b4220f54139605c5328c27fd0d5cf1329f8928094aeebbd70f116e8271e7bb280e7f4aac58115bb0064920a36fcb028f50b0b376553a6af00334899abe5be0cad14b435b377c6514680a58072691947181fa0a85b70ad15e585265e7f8bc6c22954a5bf95065815cc18771eb1a486ad6810b80c4a38f92716500220804324ff01728e3b4e87b9a34984a1456f7ab14459e4859e62961eedbb788e820b4580d12fe0266aec3a82da88c190b94d715a01793c69bf16d94bb89abc5e116f03792aa2e8306604ce559415f74948ebb3e63b8371d0e58a0d5b2798aadeb4a5dafbd44a4ca27e0a523e57a49600c826208fe4932848eb82a7c96b32bb19bb5a2e1c64ffa31756d484eddd521eae75bcf633227783d7ad0150c6198c2754c2e166e34ac00cc488573d40b82ad650e25221e414b419c66450ec266516605759e985ec1cd3f7ebc89525cf67aa2401a3cc2469e686a97f00e47484499554fa2917616e9d9c5d6f42f9d32438910fd1c00bc10a2a9234856ec6021aa69b76da10299e73540ce3175328ccb68c0b1d1878e584bd40b60af0c323c49aad7efef783d5049dae18a53794a5432f30b6c4ec6e0a43dc11f5d869f04dbe5eb963240e716ce220c75a165e6a8b5b33cb318c985e50e5d26aa9c25c2b746b4f3edcc318cae0b060a8dcc628878070b60c24f9e35a9596a0c4e1278d1a9d5b33c8e76984340cf7ec991e767c071b47d47e2ba89cb99145bcae716f402fcdb5416ab7d3b0d50b1ec4399a076faa",
    "signature": "29381098b52dd52a0f4d2f82ec4fcab2c5044ad4f9bb299c713be43345b4c215c8125936ab3445a6bde280e5f22bbf35d588458ed5169f25f0711efc2299bc6d7885c96c4cd978149dd5a1bc3b554e9bc3b8c5cbaf1d3185202ca3778c42d1d8161eeab49c05b3733e90959e7d626e96b75adc5ba18e514f36cac68ad111e8f1a8e9a2710cd5252cc54121d0b39459ec49662531a250e64e1d2571c863c8697dc91f04551dfb3517555e91014f4a36a2763452ac1698d32cadd727b1dca320c6c0efc0a6cc63f8d8e850ec5592b0d6358b25fa0246f50a7ba89161cab487f4d6925f43a3978367300c59f1f64bac24cdb1ea6ce3d7457620c9b2c508a6619f29a91a19f816393b57503ff7059954dac507fd5ab756ee1c5b861352b9e5086fb6fe388c317e59778c79a7a1ccd2152543bf56ed58c8cfefb788ecef56c9ae84ab26f71545926d1c26b91a6bc20896b99087c3eb36720c9998cb7018c2247d321c7eadf4c6d1def93b451c4ddb94123ef8691fa83f81eddc426a5cc1afbeb1f615331a97eda571e57b8cf2b46eb25cdd19691d3b8723786e9b4476afdb7e99adc56e5d237d33311f09162c74fc24c5386458a4290a7d1b8b870fdeae68e689675b5290c9704d7f1323d1657bdc2c6be2914a7de447d7c475f8ce74487b6225958831cdbbc1887af2c8016141568bd51e039c648c6541578067b2c626fe40455791ae345d0d5702356567fcda5c24365ed6b039cd9b0286dffad2c982044ae2f0b38e76deac4b19e5bac5773e45eaa78125b13df9db726cffe8c6e266cb68520ce3febf4a66fd66225f639e095ea6bdb833529a5f2bb1c9b1c544c17dee717256b471ab703d11541d55f0f78bd60d7e671f9a22a2328b70fb034b8f6459f155a17c21ad2750ce7282257980ba571c6296"
},
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/transactions
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME", // optional 
    // verbose: "summary" //or "detail"
    // order: 'asc', //or 'desc'
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/profiles/transactions/master`, {
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
    # "verbose": "summary" #or "detail"
    # "order": 'asc', #or 'desc'
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/profiles/transactions/master", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`verbose` : Optional, determines how much transaction data to include in the response. Supported values are :

* `default` : hash
* `summary` : type, version, sequence, timestamp, operation, and confirmations.
* `detail` : genesis, nexthash, prevhash, pubkey and signature.

`order` : The transaction order, based on signature chain sequence. 'asc' for oldest first, 'desc' for most recent first. The default is 'desc'.

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
{
    "txid": "01285bbd668df2bfc67515c7e9d4818b23095c10ae0585e2aa6626c018c6253fed7852dd8598ddd0668574c05a7b02eb4c89a6cf4804fcfa2bf5e7e0ffab4e77",
    "type": "tritium first",
    "version": 4,
    "sequence": 0,
    "timestamp": 1654261755,
    "blockhash": "aae806d080e8ce32d14aded37987e6969d791414328fdde96a510630727c94b0588c0ba479f8f6afe0f90a6fdf295b0a61815774b237d70c51adfbf1ac640d662802541f6125c64d43566d1347792b5f45c0ac868ac988742b9a98fb0adbc4ba76540a3538b9acce2e8ee987079ff05bdbc57d6e4c3ada9e87d41bf6e01001ab",
    "confirmations": 8,
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "nexthash": "da9562e126398dc3e5a7c70448dffb4404425132dc8f8249c910d1c7c8103932",
    "prevhash": "2446c05789a16d5a0709a65f74c469277fc5691f895766819de6ddcf78286e52e5a07682375bcf14e3e343f98cb5047a3349f967fd573892e08422f03180d53e",
    "pubkey": "032b69f4bb5c812d9f6b01e055941b2ecb2ba52e86a054847b7e2925e50540a3e8a4e477bba33d6bbd7e7da51d85a9b4f9604327f0f616030712f0762539ac4cda",
    "signature": "308184024001653b3b52c04ceb250abdb861ed24e7d2c797d377576dcc7d09b2d0aa3b0aa1afba1488872e4c6b04ef559c79c237f4b0a15519a8ed6e3bb27fbef50bf45246024035ea2174418e41e9c2ecf0b4fdd2d5e4d18665124c7a87da8b1eb72714dae8825ce8c9c8223057368e1e2eecd2703287ac73e8b003e67a39d3fd763d41d39c86",
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

## `create/auth`

This will create a crypto object register for login auth for signature chains created with Tritium, for use with Tritium++. Existing users need to run this API to convert their tritium accounts to be compatible with profiles. The interface wallet will do this automatically.

#### Endpoint:

`/profiles/create/auth`

{% swagger method="post" path="/profiles/create/auth" baseUrl="http://api.nexus-interactions.io:8080" summary="create/auth" %}
{% swagger-description %}
This will create a new master profile (signature chain) for use on the network. The master profile is secured by a combination of username, password, and PIN.

**NOTE** :&#x20;

_username_ must be a minimum of 3 characters\
_password_ must be a minimum of 8 characters\
pin must be a minimum of 4 characters
{% endswagger-description %}

{% swagger-parameter in="header" name="username" required="true" %}
The username to be associated with this profile. The signature chain genesis (used to uniquely identify profiles) is a hash of this username, therefore the username must be unique on the blockchain.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="password" required="true" %}
Password for the profile
{% endswagger-parameter %}

{% swagger-parameter in="header" name="pin" required="true" %}
The PIN can be a combination of letters/numbers/symbols or could be tied into an external digital fingerprint. The PIN is required for all API calls that modify the profile (such as sending or claiming transactions)
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="user account created" %}
```json
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
//  /users/create/user
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    username: "YOUR_USERNAME",
    password: "YOUR_SECRET",
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/profiles/create/auth`, {
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
    "username": "YOUR_USERNAME",
    "password": "YOUR_SECRET",
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/profiles/create/master", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`username` : The username identifying the signature chain.

`password` : The password for the signature chain.

`pin` : Pin for the signature chain.

#### Return value JSON object:

```
{
    "success": true,
    "txid": "01d872456b966a14796d80f1687fe59a107fe6c3b6edd3558dce146d08f3093837136634022734d9d9b5e877311fd68f847f226ae276a12b8bc3f246513ccd08"
}
```

#### Return values:

`success` : Boolean flag indicating that the `profile` was saved successfully .

`txid` : The ID (hash) of the transaction that includes the master profile creation.

***
