---
description: SESSIONS API
---

# SESSIONS

The Sessions API provides methods for creating and managing sessions. A session is synonymous with a user login into the signature chain. The full supported endpoint of the `sessions` URI is as follows:

```
sessions/verb/noun
```

The minimum required components of the URI are:

```
sessions/verb/noun
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](./#a-namecreatea-create) - Generate a new session type specified by the noun.\
[`unlock`](./#a-nameunlocka-unlock) - Unlock the session to carry out specified operations.\
[`lock`](./#lock) - Lock the session to stop a specified operations.\
[`save`](./#save) - Save a specified session to the local database.\
[`load`](./#load) - Load a specified session from the local database.\
[`terminate`](./#terminate) - Terminates a session specified by the noun.\
[`status`](./#status) - Returns status information for the session specified by the noun.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`local`] - The location of the session.\


## `create` <a href="#a-namecreatea-create" id="a-namecreatea-create"></a>

Create a new session specified by given noun.

```
sessions/create/noun
```

This command only supports the `local` noun.

### Parameters: <a href="#parameters" id="parameters"></a>

`username` : Required to **identify** the profile to create the session for.

`password` : Required to **authenticate** the password for creating the session.

`pin` : Required to **authenticate** the `pin` for creating the session.

### Results: <a href="#results" id="results"></a>

**Return value JSON object:**

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "session": "0aad63e028dd9e0f31f0b566831fea9dfc7db68fc2ba482a8ce975656971a67e"
}
[Completed in 1659.509829 ms]
```

**Return values:**

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the profile.

`session` : When using multi-user API mode, an additional session value is returned and must be supplied in subsequent API calls, to allow the managing of multiple login sessions.

## `unlock` <a href="#a-nameunlocka-unlock" id="a-nameunlocka-unlock"></a>

This will unlock the session specified by given noun and cache the PIN in encrypted memory to be used for all subsequent API calls for the specified operations.

```
sessions/unlock/noun
```

These commands only supports the `local` noun.

### Parameters: <a href="#parameters-1" id="parameters-1"></a>

`pin` : Required to **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`mining` : Required for **unlocking** mining transactions. This boolean value unlocks the session for mining.

`notifications` : Required for **unlocking** notifications. This boolean value unlocks the session to automatically process incoming notifications.

`staking` : Required for **unlocking** staking transactions. This boolean value unlocks the session for staking.

`transactions` : Required for **unlocking** transactions. This boolean value unlocks the session to allow creating or claiming transactions.

### Returns: <a href="#returns" id="returns"></a>

**Return value JSON object:**

```
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

**Return values:**

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).

## `lock` <a href="#lock" id="lock"></a>

This will lock the session specified by given noun and purge the PIN stored in the encrypted memory.

```
sessions/lock/noun
```

These commands only supports the `local` noun.

### Parameters: <a href="#parameters-2" id="parameters-2"></a>

`pin` : Required to **authenticate**. The PIN for this profile.

`session` : Required by **argument** -multiuser=1 to be supplied to identify the user session that is creating the transaction.

`mining` : Required for **locking** mining transactions. This boolean value locks the session for mining.

`notifications` : Required for **locking** notifications. This boolean value determines locks the session from processing incoming notifications.

`staking` : Required for **locking** staking transactions. This boolean value locks the session for staking.

`transactions` : Required for **locking** transactions. This boolean value locks the session for creating or claiming transactions.

### Returns: <a href="#returns-1" id="returns-1"></a>

**Return value JSON object:**

```
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

**Return values:**

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).

## `save` <a href="#save" id="save"></a>

This will save the users session to the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

```
sessions/save/noun
```

These commands only supports the `local` noun.

### Parameters: <a href="#parameters-3" id="parameters-3"></a>

`pin` : Required to **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results: <a href="#results-1" id="results-1"></a>

**Return value JSON object:**

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "success": true
}
[Completed in 2.962459 ms]
```

**Return values:**

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the profile.

`success` : Boolean flag indicating that the session was saved successfully .

## `load` <a href="#load" id="load"></a>

This will load the users session from the local database, allowing a saved session to be resumed without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to decrypt the session data.

```
sessions/load/noun
```

These commands only supports the `local` noun.

### Parameters: <a href="#parameters-4" id="parameters-4"></a>

`pin` : Required to **authenticate**. The PIN for this profile.

`session` : Required by argument `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results: <a href="#results-2" id="results-2"></a>

**Return value JSON object:**

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "session": "2ef9de11b19af82984ddf93275e7ba22c11fe9659d0667f79311c46732bbb7a4"
}
[Completed in 0.072625 ms]
```

**Return values:**

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the profile.

`session` : When using multi-user API mode, an additional session value is returned to identify the sessions.

## `terminate`

This will terminate an active session specified by given noun.

```
sessions/create/noun
```

This command only supports the `local` noun.

### Parameters:

`pin` : Required to **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

#### Return value JSON object:

```
{
    "success": true
}
[Completed in 1656.089478 ms]
```

#### Return values:

`success` : Boolean flag indicating that the session was terminated successfully .

## `status`

Get the profile status specified by given noun.

```
sessions/status/noun
```

This command only supports the `local` noun.

### Parameters:

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

#### Return value JSON object:

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "accessed": 1653680627,
    "location": "local",
    "unlocked": {
        "mining": false,
        "notifications": true,
        "staking": false,
        "transactions": false
    }
}
[Completed in 0.080333 ms]
```

#### Return values:

`genesis` : The profile genesis hash for the currently logged in user.

`accessed` : The unix timestamp at which the profile was last accessed.

`location` : The location of session, `local` or `remote`.

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users profile is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the profile is unlocked for staking.

`transactions` : Boolean flag indicating whether the profile is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).
