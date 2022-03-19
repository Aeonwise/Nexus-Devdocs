# c-SESSION

The Users API provides methods for creating and managing user sessions. A user is synonymous with a signature chain.

### `Named Shortcuts`

For each API method we support an alternative endpoint that includes the username at the end of the the URI. This shortcut removes the need to include the username or address as an additional parameter.

For example `/session/login/user/myusername` is a shortcut to `users/login/user?username=myusername`.

Similarly `/users/list/accounts/myusername` is a shortcut to `users/list/accounts?username=myusername`.

### `Methods`

The following methods are currently supported by this API

`create/local`\
`create/remote`\
`load/session`\
`load/session`\
[`save/session`](c-session-1.md#save-session)\
[`load/session`](c-session-1.md#load-session)\
[`has/session`](c-session-1.md#has-session)





### `create/local`

This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

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
    "pin": "YOUR_PIN"
}
response = requests.post(f"{SERVER_URL}/session/create/local", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

#### Return value JSON object:

#### Return values:

`success` : Boolean flag indicating that the session was saved successfully .





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

### `save/session`

This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

#### Endpoint:

`/users/save/session`

{% swagger method="post" path="/users/save/session" baseUrl="http://api.nexus-interactions.io:8080" summary="save/session" %}
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
fetch(`${SERVER_URL}/users/save/session`, {
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
response = requests.post(f"{SERVER_URL}/users/save/session", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

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
