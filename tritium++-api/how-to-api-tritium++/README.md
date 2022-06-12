# How-to-API: Tritium++

### The Nexus API Interface

Accessing the Nexus API is a seamless and simple experience. To access the API, you can use one of three options:

#### Use a web browser with a URL based request:

Make a **GET** request with the parameters in the URL as seen below.

```
http://localhost:8080/<api>/<method>?<key>=<value>&<key1>=<value1>
```

The API uses form encoding, so you are free to use any characters you wish. The only thing to be aware of is when using a '+' character in the encoding, which acts as an escape character for a space as well as '%20'. The API will detect this behavior as long as the form encodes the + sign in form encoding. At times making a **GET** request, the web browser will assume that + is a space and not encode it, the same is true for '%' when making a **GET** request.

#### Use a web browser with a **POST** based request

The POST body contains parameters, and would go to an endpoint such as:

```
http://localhost:8080/<api>/<method>
```

An example web form would look like this:

```
<form action='http://localhost:8080/<api>/<method>' method='post'>
<input type = 'text' name = '<key>'  value = '<value>' >
<input type = 'text' name = '<key1>' value = '<value2>'>
<input type = 'submit'>
</form>
```

This will allow you to integrate the Nexus API into existing web forms. This allows you to harness the power of the Nexus Blockchain in all your existing web services.

#### Use the the nexus command-line :

The Nexus Daemon must already be running for this to work. Make sure you are in the same directory as the Nexus Daemon.

```
./nexus <api>/<method> <key>=<value> <key1>=<value1>
```

This will return all data in JSON format into your console.

#### Results JSON

The response JSON you will receive will contain one of two keys, results or error. When a command is successfully executed, you will receive a JSON string such as:

```
{"result":{"genesis":"69c2479c6780782448f419a0865542002ee85fec39228275b2db44bb6d3aa503","session":4940881975319897416}}
```

You will need to parse out the values of the result object in your corresponding language of choice. If you happen to receive a failure, it will look like this:

```
{"error":{"code":-24,"message":"Missing Password"}}
```

Note that you will be parsing for the error key now, and there will be no results. This allows you the choice as a programmer to check for the existence of the "error" key to check for errors, or check for the existence of the "result" key to find if it executed successfully. The choice is yours how you handle it, just know that it is an either / or situation. You will get only one or the other.

### Profiles API

The Profiles API is responsible for maintaining user account level functions. DO NOT USE this API on a foreign node you are not the manager of. Your username and password are stored in secure allocators, but the remote endpoint can still gather your credentials when you submit an API call. This API is meant for a service node you control only.

The Profiles API can be found in the link below:

{% content-ref url="../tritium++-overview/how-to-api-tritium++/tritium++-api/profiles/" %}
[profiles](../tritium++-overview/how-to-api-tritium++/tritium++-api/profiles/)
{% endcontent-ref %}

{% hint style="info" %}
**NOTE** : Login sessions do not persist if you restart your node. They are stored in secure allocator in memory only, do not cache them on disk otherwise you could open potential security issues.
{% endhint %}

### Sessions API

The Sessions API is responsible for maintaining user account level functions. DO NOT USE this API on a foreign node you are not the manager of. Your username and password are stored in secure allocators, but the remote endpoint can still gather your credentials when you submit an API call. This API is meant for a service node you control only.

The Users API can be found in the following repo path:

{% content-ref url="../tritium++-overview/how-to-api-tritium++/tritium++-api/sessions/" %}
[sessions](../tritium++-overview/how-to-api-tritium++/tritium++-api/sessions/)
{% endcontent-ref %}

### Finance API

The Finance API provides methods for creating tokens, accounts, sending and receiving NXS coins or other tokens between users / accounts and managing staking.

The Finance API can be found in the following repo path:

{% content-ref url="tritium++-api/finance.md" %}
[finance.md](tritium++-api/finance.md)
{% endcontent-ref %}

NOTE: All the commands in this API require LOGIN. Make sure to use the sessions API to create session before using session required commands.

### Assets API

The Assets API is designed for managing of digital assets, by recording meta-data that represents the specific asset. It also provides functionality to tokenize the assets themselves, in relation to having an asset owned by many individuals.

The Assets API can be found in the link below:



NOTE: All of the commands in this API require LOGIN. Make sure to use the users API to login before using LOGIN required commands.

### Tokens API

The tokens API allows you to create tokens and accounts, in order to send and receive tokens with one another. This is also a prerequisite to creating tokens that are meant to be shares in a digital asset, or company (STO).

The Tokens API can be found in the following repo path:

LLL-TAO/docs/API/TOKENS.MD

{% hint style="info" %}
NOTE: some of the commands in this API require LOGIN. Make sure to use the users API to login before using LOGIN required commands.
{% endhint %}

### Ledger API

The ledger API provides access to the ledger, primarily to create and retrieve blocks and transactions, but also to query for ledger state information.



### Supply API

The supply API is responsible for handling supply chain logistics. It's main aim is designed for managing the routes and movements along a supply chain, along with reviewing the history of events associated with changes of custody.



{% hint style="info" %}
NOTE: some of the commands in this API require LOGIN. Make sure to use the users API to login before using LOGIN required commands.
{% endhint %}

### System API

The System API provides public access to information about this node. This includes data such as the version of software the node is running, ledger and mempool state, node IP address, and connected peers.

The System API can be found in the link below:

{% content-ref url="../tritium++-overview/how-to-api-tritium++/tritium++-api/system.md" %}
[system.md](../tritium++-overview/how-to-api-tritium++/tritium++-api/system.md)
{% endcontent-ref %}

### Invoices API

The Invoices API provides users and application developers the ability to issue and pay invoices on the Nexus blockchain. This API is a demonstration of how registers can be used to store arbitrary user data, and conditional contracts can be used to implement use-case rules. Invoices can be created and then sent to a recipient signature chain to be paid. The API supports invoices issued in NXS or any other token on the Nexus blockchain.
