# SESSIONS

The Sessions API provides methods for creating and managing profile sessions. A profile is synonymous with a signature chain.

### `Named Shortcuts`

For each API method we support an alternative endpoint that includes the username at the end of the the URI. This shortcut removes the need to include the username or address as an additional parameter.

For example `/sessions/create/local/myusername` is a shortcut to `sessions/create/local?username=myusername`.

### `Methods`

The following methods are currently supported by this API

__[_`create/local`_](sessions.md#create-local)\
`unlock/local`\
`lock/local`\
`save/local`\
_`load/local`_\
_`terminate/local`_\
_`has/session`_





### `create/local`

This will create a local session for your profile with this specific API instance. Username, password, and pin fields are mandatory for creating the session.

#### Endpoint:

`/session/create/local`

{% swagger method="post" path="/session/create/local" baseUrl="http://api.nexus-interactions.io:8080" summary="create/local" %}
{% swagger-description %}
This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="session saved" %}
```javascript
{
    success:true
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/save/session
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    username: "YOUR_USERNAME",
    password: "YOUR_SECRET"
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/session/create/local`, {
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
    "password": "YOUR_SECRET"
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/session/create/local", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`username` : The username associated with this signature chain.

`password` : The password to be associated with this signature chain.

`pin` : The PIN for this signature chain.

#### Return value JSON object:

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully .



### `unlock/local`

This will unlock your signature chain and cache the PIN in encrypted memory to be used for all subsequent API calls. This method is only available when using single-user API mode (multiuser=0).

The `pin` field is mandatory for unlock. The mining/staking/transactions/notifications parameters are optional and, if supplied, allow the signature chain to be used for either mining,staking, creating transactions, or processing notifications (or all three ) when in single-user API mode.

**NOTE** : If the mining/staking/transactions/notifications parameters are not supplied then it is assumed that the signature chain can be used for all. **NOTE** : This method can be called multiple times to unlock the options not previously unlocked.

#### Endpoint:

`/users/unlock/user`

{% swagger method="post" path="/sessions/unlock/local" baseUrl="http://api.nexus-interactions.io:8080" summary="unlock/local" %}
{% swagger-description %}
This will unlock your signature chain and cache the PIN in encrypted memory to be used for all subsequent API calls. This method is only available when using single-user API mode (multiuser=0).
{% endswagger-description %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode (configured with multiuser=1), the session is required to identify which session has to be unlocked. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pin" required="true" type="string" %}
The PIN for the particular user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="mining" type="true" required="false" %}
This boolean value determines whether the logged in users account can be used for mining.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="notifications" type="true" required="false" %}
This boolean value determines whether the logged in users account can be used for processing notifications
{% endswagger-parameter %}

{% swagger-parameter in="body" name="staking" type="false" required="false" %}
This boolean value determines whether the logged in users account can be used for staking
{% endswagger-parameter %}

{% swagger-parameter in="body" name="transactions" type="false" required="false" %}
This boolean value determines whether the logged in users account can be used for creating or claiming transactions
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user account unlocked" %}
```json
{
    "unlocked": {
        "mining": false,
        "notifications": true,
        "staking": true,
        "transactions": false
    }
}
[Completed in 1664.238652 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

data = {
    session: "YOUR_SESSION_ID"
    pin: "YOUR_PIN",
    mining: true,
    notifications: true,
    staking: false,
    transactions: false,
}
fetch(`${SERVER_URL}/users/unlock/user`, {
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
    "session": "YOUR_SESSION_ID"
    "pin": "YOUR_PIN",
    "mining": True,
    "notifications": True,
    "staking": False,
    "transactions": False,
}
response = requests.post(f"{SERVER_URL}/sessions/unlock/local", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode (configured with multiuser=1), the session is required to identify which session has to be unlocked. For single-user API mode the session should not be supplied.

`pin` : The PIN for this signature chain.

`mining` : This boolean value determines whether the logged in users signature chain can be used for mining.

`notifications` : This boolean value determines whether the logged in users signature chain can be used for processing notifications.

`staking` : This boolean value determines whether the logged in users signature chain can be used for staking.

`transactions` : This boolean value determines whether the logged in users signature chain can be used for creating or claiming transactions.

#### Return value JSON object:

```
{{
    "unlocked": {
        "mining": false,
        "notifications": true,
        "staking": true,
        "transactions": false
    }
}
[Completed in 1664.238652 ms]
```

#### Return values:

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).

***

### `lock/user`

This will lock your signature chain, making it unavailable for use unless it is either unlocked or the PIN is passed in to all API requests. Only available in single-user API mode (multiuser=0).

The mining/staking/transactions/notifications parameters are optional and, if supplied, allow the signature chain to be locked for either mining, staking, creating transactions, or processing notifications. This allows, for example, a signature chain to be continually unlocked for mining, but only temporarily unlocked for sending a transaction.

**NOTE** : This method can be called multiple times to lock the options not previously locked.

#### Endpoint:

`/users/lock/user`

{% swagger method="post" path="/users/lock/user" baseUrl="http://api.nexus-interactions.io:8080" summary="lock/user" %}
{% swagger-description %}
This will lock your signature chain, making it unavailable for use unless it is either unlocked or the PIN is passed in to all API requests. Only available in single-user API mode (multiuser=0)
{% endswagger-description %}

{% swagger-parameter in="body" name="mining" type="false" required="false" %}
This boolean value locks the user account for mining
{% endswagger-parameter %}

{% swagger-parameter in="body" name="notifications" type="true" required="false" %}
This boolean value locks the users account from processing notifications
{% endswagger-parameter %}

{% swagger-parameter in="body" name="staking" type="true" required="false" %}
This boolean value locks the users account for staking
{% endswagger-parameter %}

{% swagger-parameter in="body" name="transactions" type="false" required="false" %}
This boolean value locks the users account from claiming transactions
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user account unlocked" %}
```json
{
    "unlocked": {
        "mining": false,
        "notifications": true,
        "staking": true,
        "transactions": false
    }
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

data = {
    pin: "YOUR_PIN",
    mining: true,
    notifications: true,
    staking: false,
    transactions: false,
}
fetch(`${SERVER_URL}/users/lock/user`, {
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
    "mining": True,
    "notifications": True,
    "staking": False,
    "transactions": False,
}
response = requests.post(f"{SERVER_URL}/users/lock/user", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`mining` : If set this will lock the users sig chain for mining.

`notifications` : If set this will lock the users sig chain from processing notifications.

`staking` : If set this will lock the users sig chain for staking.

`transactions` : If set this will lock the users sig chain from creating any transactions, except those required for mining or processing notifications which are controlled separately.

#### Return value JSON object:

```
{
    "unlocked": {
        "mining": true,
        "notifications": true,
        "staking": false,
        "transactions": false
    }
}
```

#### Return values:

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).

***

### `create/remote`

This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

#### Endpoint:

`/session/create/remote`

{% swagger method="post" path="/session/create/remote" baseUrl="http://api.nexus-interactions.io:8080" summary="create/remote" %}
{% swagger-description %}
This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="session saved" %}
```javascript
{
    success:true
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/save/session
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/session/create/remote`, {
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
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/session/create/remote", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

#### Return value JSON object:

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully .

### `save/local`

This will save the users session to the local database, allowing the session to be resumed at a later time without the need to create or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

#### Endpoint:

`/sessions/save/local`

{% swagger method="post" path="/sessions/save/local" baseUrl="http://api.nexus-interactions.io:8080" summary="save/local" %}
{% swagger-description %}
This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="session saved" %}
```javascript
{
    success:true
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/save/session
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    session: "YOUR_SESSION_ID",
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/sessions/save/local`, {
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
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/sessions/save/local", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : When using multi-user API mode the session parameter must be supplied to identify which user to update.

#### Return value JSON object:

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully .

***

## `load/session`

Loads a previously saved session from the local database. In addition to restoring the logged in status of the user, the session resumes the unlocked status that was active at the time it was saved. The users PIN is required as this is used (in conjunction with the genesis) to decrypt the session data loaded from the local database.

#### Endpoint:

`/users/load/session`

{% swagger method="post" path="/users/load/session" baseUrl="http://api.nexus-interactions.io:8080" summary="load/session" %}
{% swagger-description %}
Loads a previously saved session from the local database. In addition to restoring the logged in status of the user, the session resumes the unlocked status that was active at the time it was saved. The users PIN is required as this is used (in conjunction with the genesis) to decrypt the session data loaded from the local database.
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain to load the session for (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain to load the session for (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="load session" %}
```json
{
    "genesis": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "session": "0000000000000000000000000000000000000000000000000000000000000000"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/load/session
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME", // optional 
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/users/load/session`, {
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
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/users/load/session", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain to load the session for (optional if username is supplied).

`username` : The username identifying the signature chain to load the session for (optional if genesis is supplied).

`pin` : The PIN for this signature chain.

#### Return value JSON object:

```
{
    "genesis": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "session": "0000000000000000000000000000000000000000000000000000000000000000"
}
```

#### Return values:

`genesis` : The signature chain genesis hash is echoed back to confirm the session has been loaded for the correct user.

`session` : The session ID for the resumed session. NOTE: in multi-user mode this will be a NEW session ID, generated at the point the session is loaded. The previous session ID is not persisted and restored. When not in multi-user mode this will always be 0

***

### `terminate/local`

This will terminate the active session, and delete the profile credentials stored in encrypted memory.

#### Endpoint:

`/sessions/terminate/local`

{% swagger method="post" path="/sessions/terminate/local" baseUrl="http://api.nexus-interactions.io:8080" summary="terminate/local" %}
{% swagger-description %}
This will log you the particular user and delete user credentials stored in encrypted memory
{% endswagger-description %}

{% swagger-parameter in="body" name="session" type="" required="false" %}
session ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="user logged out" %}
```json
{
  success : true
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

data = {
    session: "YOUR_SESSION_ID",
    pin: "YOUR_PIN"
}
fetch(`${SERVER_URL}/sessions/terminate/local`, {
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
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/sessions/terminate/local", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`session` : For multi-user API mode (configured with multiuser=1), the session is required to identify which session you are logging out of. For single-user API mode the session should not be supplied.

#### Return value JSON object:

#### Return values:

`success` : Flag indicating if the call was successful

***

## `has/session`

Determines whether a previously saved session exists in the local database for the specified user.

#### Endpoint:

`/users/has/session`

{% swagger method="post" path="/users/has/session" baseUrl="http://api.nexus-interactions.io:8080" summary="has/session" %}
{% swagger-description %}
Determines whether a previously saved session exists in the local database for the specified user
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" %}
The genesis hash identifying the signature chain to check the session for (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" %}
The username identifying the signature chain to check the session for (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="has session" %}
```json
{
    "genesis": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "has": true
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/has/session
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME", // optional 
}
fetch(`${SERVER_URL}/users/has/session`, {
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
}
response = requests.post(f"{SERVER_URL}/users/has/session", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain to check the session for (optional if username is supplied).

`username` : The username identifying the signature chain to check the session for (optional if genesis is supplied).

#### Return value JSON object:

```
{
    "genesis": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
    "has": true
}
```

#### Return values:

`genesis` : The signature chain genesis hash is echoed back to confirm the session has been checked for the correct user.

`has` : Boolean flag indicating whether the session exists in the local database or not

***
