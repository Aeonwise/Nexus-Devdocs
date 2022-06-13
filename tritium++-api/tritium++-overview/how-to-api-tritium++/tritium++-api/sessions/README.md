---
description: SESSIONS API
---

# SESSIONS

The Sessions API provides methods for creating and managing sessions. A session is synonymous with a user login into the signature chain.

The full supported endpoint of the `sessions` URI is as follows:

```
sessions/verb/noun
```

The minimum required components of the URI are:

```
sessions/verb/noun
```

## `Supported Verbs`

The following verbs are currently supported by this API command-set:

[`create`](./#create) - Generate a new session type specified by the noun.\
[`unlock/lock`](./#unlock-lock) - Unlock/lock the session to carry out specified operations.\
[`save/load`](./#save-load) - Save/load the session to the local database.\
[`terminate`](./#terminate) - Terminates a session specified by the noun.\
[`status`](./#status) - Returns status information for the session type specified by the noun.

## `Supported Nouns`

The following nouns are supported for this API command-set:

\[`local`] - The location of the session.\


## `create`

Create a new session specified by given noun.

```
sessions/create/noun
```

This command only supports the `local` noun.

### Parameters:

`username` : Required to **identify.** The username to create the session for.

`password` : Required to **authenticate.** The password for this profile.

`pin` : Required if **authenticate**. The PIN for this profile.

### Results:

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "session": "0aad63e028dd9e0f31f0b566831fea9dfc7db68fc2ba482a8ce975656971a67e"
}
[Completed in 1659.509829 ms]
```

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the `profile`.

`session` : When using multi-user API mode, an additional session value is returned and must be supplied in subsequent API calls, to allow the managing of multiple login sessions.

## `unlock/lock`

This will unlock/lock the session specified by given noun and cache/purge the PIN in encrypted memory to be used for all subsequent API calls for the specified operations.

```
sessions/unlock/noun
```

```
sessions/lock/noun
```

These commands only supports the `local` noun.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

`mining` : Required for **unlocking**. This boolean value determines whether the logged in users profile can be used for mining.

`notifications` : Required for **unlocking**. This boolean value determines whether the logged in users profile can be used for processing notifications.

`staking` : Required for **unlocking**. This boolean value determines whether the logged in users profile can be used for staking.

`transactions` : Required for **unlocking**. This boolean value determines whether the logged in users profile can be used for creating or claiming transactions.

### Results:

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

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).

## `save/load`

This will save/load the users session to/from the local database, allowing the session to be resumed at a later time without the need to login or unlock. The users PIN is required as this is used (in conjunction with the genesis) to encrypt the session data before persisting it to the local database.

```
sessions/save/noun
```

```
sessions/load/noun
```

These commands only supports the `local` noun.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

#### `save/local`

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "success": true
}
[Completed in 2.962459 ms]
```

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the `profile`.

`success` : Boolean flag indicating that the session was saved successfully .

#### `load/local`

```
{
    "genesis": "b7fa11647c02a3a65a72970d8e703d8804eb127c7e7c41d565c3514a4d3fdf13",
    "session": "2ef9de11b19af82984ddf93275e7ba22c11fe9659d0667f79311c46732bbb7a4"
}
[Completed in 0.072625 ms]
```

`genesis` : The signature chain genesis hash. This is a hash of the username used to create the `profile`.

`session` : When using multi-user API mode, an additional session value is returned to identify the sessions.

## `terminate`

This will terminate an active session specified by given noun.

```
sessions/create/noun
```

This command only supports the `local` noun.

### Parameters:

`pin` : Required if **authenticate**. The PIN for this profile.

`session` : Required by **argument** `-multiuser=1` to be supplied to identify the user session that is creating the transaction.

### Results:

```
{
    "success": true
}
[Completed in 1656.089478 ms]
```

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

`genesis` : The profile genesis hash for the currently logged in user.

`accessed` : The unix timestamp at which the profile was last accessed.

`location` : The location of session, `local` or `remote`.

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users profile is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the profile is unlocked for staking.

`transactions` : Boolean flag indicating whether the profile is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).
