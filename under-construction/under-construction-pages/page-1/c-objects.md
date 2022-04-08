# c-objects

An Object is a user-defined data structure that is stored in an object register, owned by a particular signature chain. Objects can hold one or more pieces of data and users can define the fields (name, data type, mutability) that data is stored in. The Objects API provides functionality for querying, accessing, and manipulating object registers in a generic and programmable way. The API supports several formats for defining the object data structures giving users and developers varying levels of functionality, including per-field type definition and mutability.

The `basic` format allows callers to define an object in terms of simple key-value pairs. It assumes that all fields are read-only and all values are stored using the string data type. `basic` objects are always immutable and cannot be updated once created.

The `JSON`, `ANSI`, and `XML` formats allow callers to provide a detailed definition of the object data structure, with each field defined with a specific data type and mutability. objects defined with one of the complex formats can be updated after their initial creation.

The `raw` format differs from the other formats in that the object data is not stored in object register, but instead is stored in a read-only state register. The raw format is useful when developers wish to store arbitrary binary data on the Nexus blockchain, without incurring the overhead of defining an object.

### `Methods`

The following methods are currently supported by this API

[`create/schema`](c-objects.md#create-schema)\
[`get/schema`](c-objects.md#get-schema)

***

## `create/schema`

This will create a new object schema, a special type of object that can be used to define the format of other objects created on the object register. The API supports an alternative endpoint that can include the new object name in the URI. For example `/objects/create/schema/myschema` will resolve to `objects/create/schema?name=myschema`.

#### Endpoint:

`/objects/create/schema`

{% swagger method="post" path="/objects/create/schema" baseUrl="http://api.nexus-interactions.io:8080" summary="create/schema" %}
{% swagger-description %}
This will create a new object schema, a special type of object that can be used to define the format of other objects created on the object register
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the object should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" %}
An optional name to identify the schema. If provided a Name object will also be created in the users local namespace, allowing the schema to be accessed/retrieved by name. If no name is provided the schema will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="format" %}
The format the caller is using to define the object schema. Values can be 

`JSON`

 (the default), 

`ANSI`

 (not currently supported), or 

`XML`

 (not currently supported). This is an optional field and the value 

`JSON`

 is assumed if omitted
{% endswagger-parameter %}

{% swagger-parameter in="body" name="json" required="true" %}


If format is `JSON`, then this field will hold the json definition of the schema as a JSON array representing each field in the object. It uses the following format:



`name` : The name of the data field.

`type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.

`max_length` : Optional, the maximum number of characters if type=string

`value` : The default value of the field.

`mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// create/schema
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    pin: "YOUR_PIN",
    // session: "YOUR_SESSION_ID", //optional
    name: "NAME TO IDENTIFY THE SCHEMA", // If provided a Name object will also be created in the users local namespace, allowing the schema to be accessed/retrieved by name. If no name is provided the schema will need to be accessed/retrieved by its 256-bit register address.
    format: "JSON", // Values can be JSON (the default), ANSI (not currently supported), or XML (not currently supported). This is an optional field and the value JSON is assumed if omitted.
    //If format is JSON, then this field will hold the json definition of the schema as a JSON array representing each field in the object. It uses the following format:
    json: {
        name: "NAME OF THE DATA FIELD",
        type: "string", // Values can be uint8, uint16, uint32, uint64, uint256, uint512, uint1024, string, or bytes.
        // max_length : 1024,//Optional, the maximum number of characters if type=string
        value: "DEFAULT VALUE OF THE FIELD",
        // mutable : true, //The boolean field to indicate whether the field is writable (true) or read-only (false).
    }
}
fetch(`${SERVER_URL}/objects/create/schema`, {
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
    # If provided a Name object will also be created in the users local namespace, allowing the schema to be accessed/retrieved by name. If no name is provided the schema will need to be accessed/retrieved by its 256-bit register address.
    "name": "NAME TO IDENTIFY THE SCHEMA",
    # Values can be JSON (the default), ANSI (not currently supported), or XML (not currently supported). This is an optional field and the value JSON is assumed if omitted.
    "format": "JSON",
    # If format is JSON, then this field will hold the json definition of the schema as a JSON array representing each field in the object. It uses the following format:
    "json": {
        "name": "NAME OF THE DATA FIELD",
        # Values can be uint8, uint16, uint32, uint64, uint256, uint512, uint1024, string, or bytes.
        "type": "string",
        # "max_length" : 1024,#Optional, the maximum number of characters if type=string
        "value": "DEFAULT VALUE OF THE FIELD",
        # "mutable" : True, #The boolean field to indicate whether the field is writable (True) or read-only (False).
    }
}
response = requests.post(f"{SERVER_URL}/objects/create/schema", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`pin` : The PIN for this signature chain.

`session` : For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the object should be created with. For single-user API mode the session should not be supplied.

`name` : An optional name to identify the schema. If provided a Name object will also be created in the users local namespace, allowing the schema to be accessed/retrieved by name. If no name is provided the schema will need to be accessed/retrieved by its 256-bit register address.

`format` : The format the caller is using to define the object schema. Values can be `JSON` (the default), `ANSI` (not currently supported), or `XML` (not currently supported). This is an optional field and the value `JSON` is assumed if omitted.

`json` : If format is `JSON`, then this field will hold the json definition of the schema as a JSON array representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `max_length` : Optional, the maximum number of characters if type=string
* `value` : The default value of the field.
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).

#### Example:

The following is an example of an object defined using the `JSON` format:\
**NOTE**: In this example the shelf\_location field is defined as mutable and can therefore be updated.

```
{
    "pin": "1234",
    "name": "watch",
    "format": "JSON",
    "json" :
        [
            {
                "name": "serial_number",
                "type": "uint64",
                "value": "0",
                "mutable" : "false"
            },
            {
                "name": "description",
                "type": "string",
                "value": "",
                "max_length": 100,
                "mutable" : "false"
            },
            {
                "name": "shelf_location",
                "type": "string",
                "value": "",
                "max_length": 50
                "mutable" : "true"
            }
        ]
}
```

#### Return value JSON object:

```
{
    "txid": "27ef3f31499b6f55482088ba38b7ec7cb02bd4383645d3fd43745ef7fa3db3d1"
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
}
```

#### Return values:

`txid` : The ID (hash) of the transaction that includes the object creation.

`address` : The register address for this object. The address (or name that hashes to this address) is needed for downstream transactions / api methods for this object.

***

## `get/schema`

Retrieves the schema / object definition of an object.

#### Endpoint:

`/objects/get/schema`

{% swagger method="post" path="/objects/get/schema" baseUrl="http://api.nexus-interactions.io:8080" summary="get/schema" %}
{% swagger-description %}
Retrieves the schema / object definition of an object
{% endswagger-description %}

{% swagger-parameter in="body" name="name" %}
The name identifying the object. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the 

`session`

 parameter is provided (as we can deduce the username from the session)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" %}
For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the object. The 

`session`

 parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" %}
The register address of the object. This is optional if the name is provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="format" %}
The format that the schema should be returned in. Values can be 

`JSON`

 (the default), 

`ANSI`

 (not currently supported), or 

`XML`

 (not currently supported). This is optional field and the value 

`JSON`

 is assumed if omitted
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="schema details" %}
```json
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    "json" :
        [
            {
                "name": "serial_number",
                "type": "uint32",
                "value": "0",
                "mutable" : "false"
            },
            {
                "name": "description",
                "type": "string",
                "value": "",
                "max_length": 100,
                "mutable" : "false"
            },
            {
                "name": "shelf_location",
                "type": "string",
                "value": "",
                "max_length": 100,
                "mutable" : "true"
            },
        ]
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
// get/schema
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    name: "NAME IDENTIFYING THE OBJECT", //optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    // session: "YOUR_SESSION_ID", //optional
    address: "REGISTER ADDRESS OF THE OBJECT", //optional if the name is provided.
    format: "JSON", // Values can be JSON (the default), ANSI (not currently supported), or XML (not currently supported). This is an optional field and the value JSON is assumed if omitted.
}
fetch(`${SERVER_URL}/objects/get/schema`, {
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
    # optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the session parameter is provided (as we can deduce the username from the session)
    "name": "NAME IDENTIFYING THE OBJECT",
    # "session": "YOUR_SESSION_ID", #optional
    # optional if the name is provided.
    "address": "REGISTER ADDRESS OF THE OBJECT",
    # Values can be JSON (the default), ANSI (not currently supported), or XML (not currently supported). This is an optional field and the value JSON is assumed if omitted.
    "format": "JSON",
}
response = requests.post(f"{SERVER_URL}/objects/get/schema", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`name` : The name identifying the object. This is optional if the address is provided. The name should be in the format username:name (for local names) or namespace::name (for names in a namespace). However, if the object was created in the callers namespace (their username), then the username can be omitted from the name if the `session` parameter is provided (as we can deduce the username from the session)

`session` : For multi-user API mode, (configured with multiuser=1) the session can be provided in conjunction with the name in order to deduce the register address of the object. The `session` parameter is only required when a name parameter is also provided without a namespace in the name string. For single-user API mode the session should not be supplied.

`address` : The register address of the object. This is optional if the name is provided.

`format` : The format that the schema should be returned in. Values can be `JSON` (the default), `ANSI` (not currently supported), or `XML` (not currently supported). This is optional field and the value `JSON` is assumed if omitted.

**NOTE**: If the object was initially created using the `raw` format then this API method will error as there is no schema associated with a raw state register.

#### Return value JSON object:

```
{
    "address": "8FJxzexVDUN5YiQYK4QjvfRNrAUym8FNu4B8yvYGXgKFJL8nBse"
    "json" :
        [
            {
                "name": "serial_number",
                "type": "uint32",
                "value": "0",
                "mutable" : "false"
            },
            {
                "name": "description",
                "type": "string",
                "value": "",
                "max_length": 100,
                "mutable" : "false"
            },
            {
                "name": "shelf_location",
                "type": "string",
                "value": "",
                "max_length": 100,
                "mutable" : "true"
            },
        ]
}
```

#### Return values:

`address` : The register address for this object. The address (or name that hashes to this address) is needed for downstream transactions / api methods for this object.

`json` : If format is `JSON`, then this field will hold the json definition of the object as an array of JSON objects representing each field in the object. It uses the following format:

* `name` : The name of the data field.
* `type` : The data type to use for this field. Values can be `uint8`, `uint16`, `uint32`, `uint64`, `uint256`, `uint512`, `uint1024`, `string`, or `bytes`.
* `value` : The default value of the field.
* `max_length` : Optional, the maximum number of characters if type=string
* `mutable` : The boolean field to indicate whether the field is writable (true) or read-only (false).

***
