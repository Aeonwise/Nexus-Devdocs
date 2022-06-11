# Page 3



{% swagger method="post" path="/finance/create/account" baseUrl="http://api.nexus-interactions.io:8080" summary="create/account" %}
{% swagger-description %}
Create a new account for receiving NXS.
{% endswagger-description %}

{% swagger-parameter in="body" name="pin" required="true" %}
The PIN for the user account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="session" required="false" %}
For multi-user API mode, (configured with multiuser=1) the session is required to identify which session (sig-chain) the account should be created with. For single-user API mode the session should not be supplied
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token_name" required="false" %}
Optional name of a token to create the account for a 

`token,`

 can be supplied as an alternative to 

`token_name`

. Defaults to 

`NXS`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" required="false" %}
Optional token address to create the account for. 

`token_name`

 can be supplied as an alternative to 

`token`

. Defaults to 

`0`

 (

`NXS`

)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" required="false" %}
An optional name to identify the account. If provided a Name object will also be created in the users local namespace, allowing the account to be accessed/retrieved by name. If no name is provided the account will need to be accessed/retrieved by its 256-bit register address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" required="false" %}
This optional field allows callers to add arbitrary data to an account register
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Account created" %}
```json
{
    "txid": "f9dcd28bce2563ab288fab76cf3ee5149ea938c735894ce4833b55e474e08e8a519e8005e09e2fc19623577a8839a280ca72b6430ee0bdf13b3d9f785bc7397d",
    "address": "8CvLySLAWEKDB9SJSUDdRgzAG6ALVcXLzPQREN9Nbf7AzuJkg5P"
}
```
{% endswagger-response %}
{% endswagger %}
