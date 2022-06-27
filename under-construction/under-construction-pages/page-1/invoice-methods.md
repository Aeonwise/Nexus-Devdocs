# INVOICE METHODS

## `Methods`

The following methods are currently supported by this API

[`create/invoice`](invoice-methods.md#create-invoice)\
[`get/invoice`](invoice-methods.md#get-invoice)\
[`pay/invoice`](invoice-methods.md#pay-invoice)\
[`cancel/invoice`](invoice-methods.md#cancel-invoice)\
[`list/invoice/history`](invoice-methods.md#list-invoice-history)

***

### `create/invoice`

Create a new invoice. The API supports an alternative endpoint that can include the invoice name in the URL. For example `/invoices/create/invoice/INV123` will resolve to `invoices/create/invoice?name=INV123`.

In addition to the parameters documented here, callers are free to include any other fields in the JSON that they desire and these will be stored in the invoice register on chain. When later retrieving an invoice, all fields that were initially passed to the create/invoice method are returned in the response.

#### Endpoint:

`/invoices/create/invoice`

{% swagger method="post" path="/invoices/create/invoice" baseUrl="http://api.nexus-interactions.io:8080" summary="create/invoice" %}
{% swagger-description %}
Create a new invoice
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the invoice should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="receipient" %}
The genesis hash of the signature chain to issue the invoice to. This is optional if the recipient_username is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="recipient_username" %}
The username identifying the user account (sig-chain) to issue the invoice to. This is optional if the recipient is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="account_name" %}
The name identifying the account the invoice should be paid to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="account" %}
The register address of the account the invoice should be paid to. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
An optional name to identify the invoice. If provided a Name object will also be created in the users local namespace, allowing the invoice to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="items" %}
Array of line items that make up this invoice. At least one item in the items array must be included. The total invoice amount is calculated as the sum of all item amounts, and each item amount is calculated as the `unit_amount` multiplied by the `units.`\
``{\
`unit_amount` : The unit amount to be invoiced for this line item. This amount should be supplied in the currency of the payment account

`units` : The number of units to be invoiced at the unit amount.\
}
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="invoice created" %}
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
// create/invoice
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    // recipient: "TO_GENEISIS_HASH", //optional if the recipient_username is passed
    recipient_username: "TO_USERNAME", //optional if the recipient is provided.
    account_name: "TO_ACCOUNT_NAME", //This is optional if the address is provided.
    account: "TO_REGISTER_ADDRESS", //optional if name is provided
    // name: "NAME_TO_IDENTIFY_INVOICE", //optional
    items: [{
            description: "First item description",
            base_price: 1.0,
            tax: 0.1,
            unit_amount: 5.5,
            units: 1
        }] //required atleast 1 item 
}
fetch(`${SERVER_URL}/invoices/create/invoice`, {
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
    # "recipient": "TO_GENEISIS_HASH", #optional if the recipient_username is passed
    # optional if the recipient is provided.
    "recipient_username": "TO_USERNAME",
    # This is optional if the address is provided.
    "account_name": "TO_ACCOUNT_NAME",
    "account": "TO_REGISTER_ADDRESS",  # optional if name is provided
    # "name": "NAME_TO_IDENTIFY_INVOICE", #optional
    "items": [{
        "description": "First item description",
        "base_price": 1.0,
        "tax": 0.1,
        "unit_amount": 5.5,
        "units": 1
    }]
}
response = requests.post(f"{SERVER_URL}/invoices/create/invoice", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the invoice should be created with. For single-user API mode the session should not be supplied.

`recipient` : The genesis hash of the signature chain to issue the invoice to. This is optional if the recipient\_username is provided.

`recipient_username` : The username identifying the user account (sig-chain) to issue the invoice to. This is optional if the recipient is provided.

`account_name` : The name identifying the account the invoice should be paid to. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`account` : The register address of the account the invoice should be paid to. This is optional if the name is provided.

`name` : An optional name to identify the invoice. If provided a Name object will also be created in the users local namespace, allowing the invoice to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address.

`items` : Array of line items that make up this invoice. At least one item in the items array must be included. The total invoice amount is calculated as the sum of all item amounts, and each item amount is calculated as the `unit_amount` multiplied by the `units`\
{\
`unit_amount` : The unit amount to be invoiced for this line item. This amount should be supplied in the currency of the payment account

`units` : The number of units to be invoiced at the unit amount.\
}

#### Example:

The following example creates an invoice with two line items. The first item is for a total of 3.3 NXS and the second for 5.5 NXS. **NOTE** that there are several non-mandatory fields included here that the calling system wishes to store in the invoice, such as `number`, `PO`, `contact`, `sender_detail`, `recipient_detail`, and in the items `description`, `base_price`, and `tax`

```
{
    "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
    "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
    "number": "0004",
    "PO": "Purch1234",
    "contact": "accounts@mycompany.com",
    "sender_detail": "My Company, 32 Some Street, Some Place",
    "recipient_detail": "Some recipient details such as address or email",
    "items": [
        {
            "description": "First item description",
            "base_price": 1.0,
            "tax": 0.1,
            "unit_amount": "1.1",
            "units": 3
        },
        {
            "description": "Second item description",
            "base_price": 5.0,
            "tax": 0.5,
            "unit_amount": "5.5",
            "units": 1
        }
    ]
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

`txid` : The ID (hash) of the transaction that includes the invoice creation.

`address` : The register address for the invoice. The address (or name that hashes to this address) is needed when retrieving or paying the invoice.

***

### `get/invoice`

Retrieves information about an invoice. The API supports an alternative endpoint that can include the invoice name or address in the URL. For example `/invoices/get/invoice/inv123` will resolve to `invoices/get/invoice?name=inv123`.

#### Endpoint:

`/invoices/get/invoice`

{% swagger method="post" path="/invoices/get/invoice" baseUrl="http://api.nexus-interactions.io:8080" summary="get/invoice" %}
{% swagger-description %}
Retrieves information about an invoice
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the invoice. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the invoice. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fieldname" %}
This optional field can be used to filter the response to return only a single field from the invoice data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="invoice details" %}
```json
{
    "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
    "created": 1581389015,
    "modified": 1581389107,
    "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
    "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
    "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
    "number": "0004",
    "PO": "Purch1234",
    "contact": "accounts@mycompany.com",
    "sender_detail": "My Company, 32 Some Street, Some Place",
    "recipient_detail": "Some recipient details such as address or email",
    "items": [
        {
            "description": "First item description",
            "base_price": 1.0,
            "tax": 0.1,
            "unit_amount": "1.1",
            "units": 3
        },
        {
            "description": "Second item description",
            "base_price": 5.0,
            "tax": 0.5,
            "unit_amount": "5.5",
            "units": 1
        }
    ],
    "amount": 8.8,
    "token": "0",
    "status": "PAID"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/invoice
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    name: "NAME_TO_IDENTIFY_INVOICE", //optional if address is provided
    // session: "YOUR_SESSION_ID", //optional
    // address: "REGISTER_ADDRESS_OF_INVOICE", //This is optional if the name is provided.
    // fieldname: "FILTER_FIELD", //optional if name is provided
}
fetch(`${SERVER_URL}/invoices/get/invoice`, {
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
    "name": "NAME_TO_IDENTIFY_INVOICE",  # optional if address is provided
    # "session": "YOUR_SESSION_ID", #optional
    # "address": "REGISTER_ADDRESS_OF_INVOICE", #This is optional if the name is provided.
    # "fieldname": "FILTER_FIELD", #optional if name is provided
}
response = requests.post(f"{SERVER_URL}/invoices/get/invoice", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

`name` : The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the invoice. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the invoice. This is optional if the name is provided.

`fieldname`: This optional field can be used to filter the response to return only a single field from the invoice data.

#### Return value JSON object:

```
{
    "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
    "created": 1581389015,
    "modified": 1581389107,
    "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
    "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
    "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
    "number": "0004",
    "PO": "Purch1234",
    "contact": "accounts@mycompany.com",
    "sender_detail": "My Company, 32 Some Street, Some Place",
    "recipient_detail": "Some recipient details such as address or email",
    "items": [
        {
            "description": "First item description",
            "base_price": 1.0,
            "tax": 0.1,
            "unit_amount": "1.1",
            "units": 3
        },
        {
            "description": "Second item description",
            "base_price": 5.0,
            "tax": 0.5,
            "unit_amount": "5.5",
            "units": 1
        }
    ],
    "amount": 8.8,
    "token": "0",
    "status": "PAID"
}
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this invoice.

`created` : The UNIX timestamp when the invoice was created.

`modified` : The UNIX timestamp when the invoice was last modified.

`name` : The name identifying the invoice. For privacy purposes, this is only included in the response if the caller is the owner of the invoice

`address` : The register address of the invoice.

`recipient` : The genesis hash of the signature chain to issue the invoice to.

`account` : The register address of the account the invoice should be paid to.

`items` : Array of line items that make up this invoice.\
{\
`unit_amount` : The unit amount to be invoiced for this line item.

`units` : The number of units to be invoiced at the unit amount.\
}

`amount` : The total invoice amount. This is the sum of all line item total amounts (unit\_amount x units).

`token` : The register address of the token that this invoice should be paid in. Set to 0 for NXS transactions

`status` : The current status of this invoice. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment).



### `list/invoices`

This will list all invoices issued or received by the signature chain.

{% hint style="info" %}
**NOTE** : If you use the username parameter, it will take slightly longer to calculate the username genesis with our brute-force protected hashing algorithm. For higher performance, use the genesis parameter.
{% endhint %}

#### Endpoint:

`invoices/list/invoices`

{% swagger method="post" path="/invoices/list/invoices" baseUrl="http://api.nexus-interactions.io:8080" summary="list/invoices" %}
{% swagger-description %}
This will list all invoices issued or received by the signature chain
{% endswagger-description %}

{% swagger-parameter in="body" name="genesis" required="false" %}
The genesis hash identifying the signature chain (optional if username is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="username" required="false" %}
The username identifying the signature chain (optional if genesis is supplied)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="status" required="false" %}
Optional filter by invoice status. Values can be 

`OUTSTANDING`

 (the invoice has been issued but not paid), 

`PAID`

 (the invoice has been paid by the recipient), or 

`CANCELLED`

 (the invoice was cancelled by the issuer before payment)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="limit" required="false" %}
The number of records to return for the current page. The default is 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" required="false" %}
Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="offset" required="false" %}
An alternative to 

`page`

, offset can be used to return a set of (limit) results from a particular index
{% endswagger-parameter %}

{% swagger-parameter in="body" name="where" required="false" %}
An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="invoice details" %}
```json
   {
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "modified": 1581389107,
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
        "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "number": "0004",
        "PO": "Purch1234",
        "contact": "accounts@mycompany.com",
        "sender_detail": "My Company, 32 Some Street, Some Place",
        "recipient_detail": "Some recipient details such as address or email",
        "items": [
            {
                "description": "First item description",
                "base_price": 1.0,
                "tax": 0.1,
                "unit_amount": "1.1",
                "units": 3
            },
            {
                "description": "Second item description",
                "base_price": 5.0,
                "tax": 0.5,
                "unit_amount": "5.5",
                "units": 1
            }
        ],
        "amount": 8.8,
        "token": "0",
        "status": "PAID"
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// /users/list/invoices
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    // genesis: "GENESIS_ID", //optional
    username: "YOUR_USERNAME",
    // status: "OUTSTANDING" //optional 
    // limit: 50, //optional
    // page: 1, //optional
    // offset: 10, //optional
    // where: "FILTERING SQL QUERY" //optional
}
fetch(`${SERVER_URL}/invoices/list/invoices`, {
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
    "username": "YOUR_USERNAME",
    # "status": "OUTSTANDING" #optional
    # "limit": 50, #optional
    # "page": 1, #optional
    # "offset": 10, #optional
    # "where": "FILTERING SQL QUERY" #optional
}
response = requests.post(f"{SERVER_URL}/invoices/list/invoices", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`genesis` : The genesis hash identifying the signature chain (optional if username is supplied).

`username` : The username identifying the signature chain (optional if genesis is supplied).

`status` : Optional filter by invoice status. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment)

`limit` : The number of records to return for the current page. The default is 100.

`page` : Allows the results to be returned by page (zero based). E.g. passing in page=1 will return the second set of (limit) records. The default value is 0 if not supplied.

`offset` : An alternative to `page`, offset can be used to return a set of (limit) results from a particular index.

`where` : An array of clauses to filter the JSON results. More information on filtering the results from /list/xxx API methods can be found here Filtering Results

#### Return value JSON object:

```
[
    {
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "modified": 1581389107,
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "account": "8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N",
        "recipient": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "number": "0004",
        "PO": "Purch1234",
        "contact": "accounts@mycompany.com",
        "sender_detail": "My Company, 32 Some Street, Some Place",
        "recipient_detail": "Some recipient details such as address or email",
        "items": [
            {
                "description": "First item description",
                "base_price": 1.0,
                "tax": 0.1,
                "unit_amount": "1.1",
                "units": 3
            },
            {
                "description": "Second item description",
                "base_price": 5.0,
                "tax": 0.5,
                "unit_amount": "5.5",
                "units": 1
            }
        ],
        "amount": 8.8,
        "token": "0",
        "status": "PAID"
    }
]
```

#### Return values:

`owner` : The genesis hash of the signature chain that owns this invoice.

`created` : The UNIX timestamp when the invoice was created.

`modified` : The UNIX timestamp when the invoice was last modified.

`name` : The name identifying the invoice. For privacy purposes, this is only included in the response if the caller is the owner of the invoice

`address` : The register address of the invoice.

`recipient` : The genesis hash of the signature chain to issue the invoice to.

`account` : The register address of the account the invoice should be paid to.

`items` : Array of line items that make up this invoice.\
{\
`unit_amount` : The unit amount to be invoiced for this line item.

`units` : The number of units to be invoiced at the unit amount.\
}

`amount` : The total invoice amount. This is the sum of all line item total amounts (unit\_amount x units).

`token` : The register address of the token that this invoice should be paid in. Set to 0 for NXS transactions

`status` : The current status of this invoice. Values can be `OUTSTANDING` (the invoice has been issued but not paid), `PAID` (the invoice has been paid by the recipient), or `CANCELLED` (the invoice was cancelled by the issuer before payment).

***

***

### `pay/invoice`

Make a payment (debit) to settle an invoice. The API supports an alternative endpoint that can include the invoice name or address in the URL. For example `/invoices/pay/invoice/inv123` will resolve to `invoices/pay/invoice?name=inv123`.

The API only supports paying an invoice in full, i.e. the amount to pay is determined by the invoice total.

If payment is successful, ownership of the invoice is claimed by the caller's signature chain and the invoice status will change to `PAID`.

#### Endpoint:

`/invoices/pay/invoice`

{% swagger method="post" path="/invoices/pay/invoice" baseUrl="http://api.nexus-interactions.io:8080" summary="pay/invoice" %}
{% swagger-description %}
Make a payment (debit) to settle an invoice.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the invoice to pay. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the invoice to pay. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name_from" %}
The name identifying the account to debit (pay the invoice from). This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address_from" %}
The register address of the account to debit (pay the invoice from). This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
// pay/invoice
const SERVER_URL = "http://api.nexus-interactions.io:8080"

let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_TO_IDENTIFY_INVOICE", //optional
    // address: "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", //This is optional if the name is provided.
    name_from: "The name identifying the account to debit (pay the invoice from).",
    // address_from: //This is optional if the name is provided.
}
fetch(`${SERVER_URL}/invoices/pay/invoice`, {
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
    "name": "NAME_TO_IDENTIFY_INVOICE",  # optional
    # "address": "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", #This is optional if the name is provided.
    "name_from": "The name identifying the account to debit (pay the invoice from).",
    # "address_from": #This is optional if the name is provided.
}
response = requests.post(f"{SERVER_URL}/invoices/pay/invoice", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the invoice to pay. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to pay. This is optional if the name is provided.

`name_from` : The name identifying the account to debit (pay the invoice from). This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address_from` : The register address of the account to debit (pay the invoice from). This is optional if the name is provided.

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the DEBIT to pay the invoice AND the CLAIM to claim ownership of the invoice.

***

### `cancel/invoice`

Cancels an unpaid invoice so that ownership returns back to the issuers signature chain. The API supports an alternative endpoint that can include the invoice name or address in the URL. For example `/invoices/cancel/invoice/inv123` will resolve to `invoices/cancel/invoice?name=inv123`.

**NOTE** Only the creator of an invoice has the ability to cancel, and only if it is unpaid, i.e. it has an `OUTSTANDING` status.

**NOTE** Cancelled invoices no longer appear in the recipients invoice list, but will continue to appear in the issuers list so that they have a record of the cancellation.

#### Endpoint:

`/invoices/cancel/invoice`

{% swagger method="post" path="" baseUrl="http://api.nexus-interactions.io:8080" summary="cancel/invoice" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
The name identifying the invoice to cancel. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the invoice to cancel. This is optional if the name is provided.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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
// cancel/invoice
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME_TO_IDENTIFY_INVOICE", //optional if address is provided
    // address: "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", //This is optional if the name is provided.
}
fetch(`${SERVER_URL}/invoices/cancel/invoice`, {
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
    "name": "NAME_TO_IDENTIFY_INVOICE",  # optional if address is provided
    # "address": "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", #This is optional if the name is provided.
}
response = requests.post(f"{SERVER_URL}/invoices/cancel/invoice", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account owner. For single-user API mode the session should not be supplied.

`name` : The name identifying the invoice to cancel. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the account was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`address` : The register address of the invoice to cancel. This is optional if the name is provided.

#### Return value JSON object:

```
{
    "txid": "318b86d2c208618aaa13946a3b75f14472ebc0cce9e659f2830b17e854984b55606738f689d886800f21ffee68a3e5fd5a29818e88f8c5b13b9f8ae67739903d"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the claim contract back to the issuers signature chain.

***

### `list/invoice/history`

This will get the history of the transactions for an invoice showing when it was created & transferred (two contracts that happen at the same point in time when the invoice is created) and claimed (paid or cancelled). The API supports an alternative endpoint that can include the invoice name (if known) or register address in the URI. For example `/invoices/list/invoice/history/inv123` will resolve to `/invoices/list/invoice/history?name=inv123`.

#### Endpoint:

`/invoices/list/invoice/history`

{% swagger method="post" path="" baseUrl="http://api.nexus-interactions.io:8080" summary="list/invoice/history" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the invoice. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the invoice. This is optional if the name is provided.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
[
    {
        "type": "CLAIM",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 2752588510729237979,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "TRANSFER",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "CREATE",
        "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
//// list/invoice/history
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME_TO_IDENTIFY_INVOICE", //optional if address is provided
    // session: "YOUR_SESSION_ID", //optional
    // address: "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", //This is optional if the name is provided.
}
fetch(`${SERVER_URL}/invoices/list/invoice/history`, {
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
// Someimport requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "name": "NAME_TO_IDENTIFY_INVOICE",  # optional if address is provided
    # "session": "YOUR_SESSION_ID", #optional
    # "address": "REGISTER_ADDRESS_OF_INVOICE_TO_PAY", #This is optional if the name is provided.
}
response = requests.post(
    f"{SERVER_URL}/invoices/list/invoice/history", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the invoice. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the invoice was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the invoice. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the invoice. This is optional if the name is provided.

#### Return value JSON object:

```
[
    {
        "type": "CLAIM",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 2752588510729237979,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "TRANSFER",
        "owner": "a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    },
    {
        "type": "CREATE",
        "owner": "a2e51edcd41a8152bfedb24e3c22ee5a65d6d7d524146b399145bced269aeff0",
        "modified": 1581389015,
        "checksum": 3200103280975089335,
        "address": "822G7ZSsHh1ncTj4AJt6FnZ6MTWG3Q9NNTZ9KvG3CWA1SP3aq97",
        "created": 1581389015,
        "data": "{\"account\":\"8Bx6ZmCev3DsGjoWuhfQSNmycdZT4cyKKJNc36NWTMik6Zkqh7N\",\"recipient\":\"a2056d518d6e6d65c6c2e05af7fe2d3182a93def20e960fcfa0d35777a082440\",\"number\":\"0004\",\"PO\":\"Purch1234\",\"contact\":\"paul@nexus.io\",\"items\":[{\"description\":\"item1 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":1},{\"description\":\"item2 description\",\"base_price\":1.0,\"tax\":0.1,\"unit_amount\":\"1.1\",\"units\":3}],\"amount\":4.4,\"token\":\"8DC5b4uRPYmfF8DWJy6ja2qPbtN7kNpdonHQeKx1nfauRAWDMUG\"}"
    }
]
```

#### Return values:

The return value is a JSON array of objects for each entry in the invoice's history:

`type` : The action that occurred - CREATE | TRANSFER | CLAIM

`owner` : The genesis hash of the signature chain that owned the invoice.

`modified` : The Unix timestamp of when the invoice was updated.

`checksum` : A checksum of the state register used for pruning.

`address` : The register address of the invoice

`name` : The name of the invoice

`data` : The JSON invoice data