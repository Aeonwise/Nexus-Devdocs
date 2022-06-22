# Market Methods

## `Direct Endpoints`

The following commands are direct endpoints and thus do not support the above `verb` and `noun` structure available above.

`create/bid`\
`list/block`\
`execute/order`\
`cancel/order`\
`user/order`\
[`void/transaction`](market-methods.md#void-transaction)\
[`get/info`](market-methods.md#get-info)``

Direct endpoints support filters and operators.

{% hint style="info" %}
Retrieving block data by height is only possible if the daemon has been configured with --`indexheight=1`.
{% endhint %}

## `create/bid`

Retrieves the hash of the block for the given height.

```
ledger/get/blockhash
```

#### Parameters:

`height` : The block height to retrieve the hash for.

#### Return value JSON object:

```
{
    "hash": "289b76fbe40ca5e0d0df68016677a8c9be6389f3225cc9faf65cb92bbc98a3580b57cb7ba3d7f7ab36ce890d47a4be90254d407270ad4c2e69c99c998ef8547ca45f054b081959d4d59feb871360bce888f82b4cdc1476502bd1b8e95dffe811e55592d3d4768b56e0510e02b5e32171858de097cd811e7c8d09920c24960ae8"
}
[Completed in 0.083791 ms]  
```

#### Return values:

`hash` : The hash of the block.

## `` <a href="#user-content-create" id="user-content-create"></a>
