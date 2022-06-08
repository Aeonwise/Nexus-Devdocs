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

[`create`](sessions.md#create) - Generate a new session type specified by the noun.\
[`unlock/lock`](sessions.md#unlock-lock) - Unlock/lock the session to carry out specified operations.\
[`save/load`](sessions.md#save-load) - Save/load the session to the local database.\
[`terminate`](sessions.md#terminate) - Terminates a session specified by the noun.\
[`status`](sessions.md#status) - Returns status information for the session type specified by the noun.

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

`username` : The username associated with this signature chain.

`password` : The password to be associated with this signature chain.

`pin` : The PIN for this signature chain.

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

`pin` : The PIN for this signature chain.

`session` : When using multi-user API mode, an additional session value is returned and must be supplied in subsequent API calls, to allow the managing of multiple login sessions.

`mining` : This boolean value determines whether the logged in users signature chain can be used for mining.

`notifications` : This boolean value determines whether the logged in users signature chain can be used for processing notifications.

`staking` : This boolean value determines whether the logged in users signature chain can be used for staking.

`transactions` : This boolean value determines whether the logged in users signature chain can be used for creating or claiming transactions.

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

`pin` : The PIN for this signature chain.

`session` : When using multi-user API mode the session parameter must be supplied to identify which user to update.

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

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

`pin` : The PIN for this signature chain.

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

`session` : When using multi-user API mode the session parameter must be supplied to identify which profile to return the status for.

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

`genesis` : The signature chain genesis hash for the currently logged in user.

`accessed` : The unix timestamp at which the signature chain was last accessed.

`location` : The location of session, `local` or `remote`.

`unlocked` : This will contain child elements describing which functions the session is currently unlocked for

`mining` : Boolean flag indicating whether the users sig chain is unlocked for mining.

`notifications` : Boolean flag indicating whether the users sig chain is unlocked for processing notifications.

`staking` : Boolean flag indicating whether the users sig chain is unlocked for staking.

`transactions` : Boolean flag indicating whether the users sig chain is unlocked for creating any transactions (except those automatically created through mining/processing notifications if those are unlocked).
