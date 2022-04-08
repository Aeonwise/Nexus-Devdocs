# C-PROFILE

he Users API provides methods for creating and managing users. A user is synonymous with a signature chain.

{% hint style="info" %}
The Tritium++ Users API differs a little from Tritium
{% endhint %}

### `Named Shortcuts`

For each API method we support an alternative endpoint that includes the username at the end of the the URI. This shortcut removes the need to include the username or address as an additional parameter.

For example `/users/login/user/myusername` is a shortcut to `users/login/user?username=myusername`.

Similarly `/users/list/accounts/myusername` is a shortcut to `users/list/accounts?username=myusername`.

### `Methods`

The following methods are currently supported by this API

`create/master`\
`create/myprofile`\
[`logout/user`](c-profile.md#logout-user)\
[`unlock/user`](c-profile.md#unlock-user)\
[`lock/user`](c-profile.md#lock-user)\
[`update/user`](c-profile.md#update-user)\
[`get/status`](c-profile.md#get-status)\
[`list/notifications`](c-profile.md#list-notifications)\
[`process/notifications`](c-profile.md#process-notifications)\
[`list/transactions`](c-profile.md#list-transactions)\
[`save/session`](c-profile.md#save-session)\
[`load/session`](c-profile.md#load-session)\
[`has/session`](c-profile.md#has-session)
