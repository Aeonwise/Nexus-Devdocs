---
description: SYSTEM API
---

# SYSTEM

The System API provides public access to information about this node. This includes data such as the version of software the node is running, ledger and mempool state, node IP address, and connected peers.

### `Direct Endpoints`

The following commands are direct endpoints and thus do not support the `verb` and `noun` structure available above.

[`stop`](system.md#stop)\
[`get/info`](system.md#get-info)\
[`get/metrics`](system.md#get-metrics)\
[`list/peers`](system.md#list-peers)\
[`list/lisp-eids`](system.md#list-lisp-eids)\
[`validate/address`](system.md#validate-address)

### `stop`

Initiate node shutdown sequence.

#### Endpoint:

`/system/stop`

{% swagger method="get" path="/system/stop" baseUrl="http://api.nexus-interactions.io:8080" summary="stop" %}
{% swagger-description %}
Initiate node shutdown sequence.
{% endswagger-description %}

{% swagger-parameter in="body" name="password" %}
A password which should match the one supplied in the config file, to make sure that the node is not stopped accidentally with this method.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="server stopped" %}
```javascript
Nexus server stopping
```
{% endswagger-response %}
{% endswagger %}

#### Parameters:&#x20;

`password` : A password to protect the node from accidental shutdown, this stop password is not the same used to login to the signature chain, but supplied in the config file. This is optional if a stop password is not supplied in the config.&#x20;

#### Return value JSON object:

`Nexus server stopping`

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"
let data = {
    password: "YOUR_SECRET",
}
fetch(`${SERVER_URL}/system/stop`,{
    method: 'GET',
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify(data)
})
.then(resp => resp.json())
.then(json => console.log(json)) // nexus server stopping
.catch(error => console.log(error)) // Some Error Occured
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "password": "YOUR_SECRET"
}
response = requests.post(f"{SERVER_URL}/system/stop", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

### `get/info`

Returns a summary of information about this node

#### Endpoint:

`/system/get/info`

{% swagger method="get" path=":8080/system/get/info" baseUrl="http://api.nexus-interactions.io" summary="get/info" %}
{% swagger-description %}
Returns a summary of information about this node
{% endswagger-description %}

{% swagger-response status="200: OK" description="node information" %}
```json
{
    "version": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
    "protocolversion": 3010000,
    "walletversion": 10001,
    "timestamp": 1654848090,
    "hostname": "localhost",
    "directory": "/home/nexus/.Nexus/",
    "address": "172.105.36.92",
    "private": false,
    "hybrid": false,
    "multiuser": false,
    "litemode": false,
    "blocks": 4479612,
    "synchronizing": false,
    "synccomplete": 100,
    "syncprogress": 100,
    "txtotal": 0,
    "connections": 15
}
[Completed in 0.115028 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

fetch(`${SERVER_URL}/system/get/info`, {
    method: "GET"
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
response = requests.get(f"{SERVER_URL}/system/get/info")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Return value JSON object:

```
{
    "version": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
    "protocolversion": 3010000,
    "walletversion": 10001,
    "timestamp": 1654848090,
    "hostname": "localhost",
    "directory": "/home/nexus/.Nexus/",
    "address": "172.105.36.92",
    "private": false,
    "hybrid": false,
    "multiuser": false,
    "litemode": false,
    "blocks": 4479612,
    "synchronizing": false,
    "synccomplete": 100,
    "syncprogress": 100,
    "txtotal": 0,
    "connections": 15
}
[Completed in 0.115028 ms]
```

#### Return values:

`version` : The daemon software version.

`protocolversion` : The LLP version.

`walletversion` : Legacy wallet version.

`timestamp` : Current unified time as reported by this node.

`hostname` : The local machine host name.

`ipaddress` : The public IP address of the local machine, as seen by peers that you are connected to. This will be blank until you have at least one peer connection.

`testnet` : If this node is running on the testnet then this shows the testnet number.

`private` : Whether this node is running in private mode.

`multiuser` : Whether this node is running in multiuser mode.

`sessions` : Only returned in multiuser mode. This shows the number of user sessions currently logged in to the node.

`litemode` : Flag indicating whether this node is running in client mode.

`legacy_unsupported` : Flag indicating whether this node has been compiled with support for the legacy UTxO wallet. If it has not then all of the legacy wallet functions (available via the legacy RPC) are not available. This flag is currently omitted from the response if it has been compiled with legacy support, hence it will only be included if it is true.

`blocks` : The current block height of this node.

`synchronizing` : Flag indicating whether this node is currently syncrhonizing. Not included if private=true

`synccomplete` : The percentage of the total blocks downloaded

`syncprogress` : The percentage of the current sync completed relative to when the node was started.

`txtotal` : Number of transactions in the node's mempool.

`connections` : Number of peer connections.

`eids` : Array of LISP Endpoint Idendifiers (EID's) for this node. Not included unless this node is using LISP.

***

### `get/metrics`

Returns metrics and statistics for the ledger, registers, etc

#### Endpoint:

`/system/get/metrics`

{% swagger method="get" path="/system/get/metrics" baseUrl="http://api.nexus-interactions.io:8080" summary="get/metrics" %}
{% swagger-description %}
Returns metrics and statistics for the ledger, registers, etc
{% endswagger-description %}

{% swagger-response status="200: OK" description="blockchain metrics" %}
```json
{
    "registers": {
        "total": 182199,
        "account": 90556,
        "append": 0,
        "crypto": 30110,
        "name": 60114,
        "name_global": 37,
        "name_namespaced": 29,
        "namespace": 14,
        "object": 122,
        "object_tokenized": 51,
        "raw": 0,
        "readonly": 651,
        "token": 131
    },
    "sig_chains": 30110,
    "trust": {
        "total": 501,
        "stake": 28120483.0,
        "trust": 8234400574
    },
    "supply": {
        "total": 73584291.341529,
        "target": 70938540.694046,
        "inflationrate": 3.729637826768922,
        "minute": 3.775454,
        "hour": 226.524922,
        "day": 5435.32689,
        "week": 37991.719644,
        "month": 151197.159898
    },
    "reserves": {
        "ambassador": 195.666785,
        "developer": 46.470805,
        "fee": 105885.167,
        "hash": 89.305775,
        "prime": 46.2076
    }
}
[Completed in 5127.594432 ms]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

fetch(`${SERVER_URL}/system/get/metrics`)
.then(resp => resp.json())
.then(json => console.log(json))
.catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
response = requests.get(f"{SERVER_URL}/system/get/metrics")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Return value JSON object:

```
{
    "registers": {
        "total": 182199,
        "account": 90556,
        "append": 0,
        "crypto": 30110,
        "name": 60114,
        "name_global": 37,
        "name_namespaced": 29,
        "namespace": 14,
        "object": 122,
        "object_tokenized": 51,
        "raw": 0,
        "readonly": 651,
        "token": 131
    },
    "sig_chains": 30110,
    "trust": {
        "total": 501,
        "stake": 28120483.0,
        "trust": 8234400574
    },
    "supply": {
        "total": 73584291.341529,
        "target": 70938540.694046,
        "inflationrate": 3.729637826768922,
        "minute": 3.775454,
        "hour": 226.524922,
        "day": 5435.32689,
        "week": 37991.719644,
        "month": 151197.159898
    },
    "reserves": {
        "ambassador": 195.666785,
        "developer": 46.470805,
        "fee": 105885.167,
        "hash": 89.305775,
        "prime": 46.2076
    }
}
[Completed in 5127.594432 ms]
```

#### Return values:

`registers` : Statistics on the register database.\
{\
`total` : Total number of registers in the register database.

`account` : Number of account registers. This includes both NXS and other token accounts.

`append` : Number of append registers.

`crypto` : Number of crypto registers.

`name` : Total number of name registers, including user local, global, and namespaced.

`name_global` : Number of names that have been created in the global namespace.

`name_namespaced` : Number of names that have been created in a namespace.

`object` : Number of non-standard object registers. These include assets, items, and any other derived object register

`object_tokenized` : Number of object registers that have been tokenized (where the object owner is a token rather than a signature chain).

`raw` : Number of raw registers.

`readonly` : Number of read only registers.

`token` : Number of token registers.\
}

`sig_chains` : Total number of signature chains in the ledger.

`trust` : Statistics on active trust accounts.\
{\
`total` : Total number trust accounts that are active (have a non-zero stake balance).

`stake` : Total amount of NXS currently being staked network-wide).

`trust` : Total trust score of all trust accounts being staked.

&#x20;}

`supply` : Metrics on NXS supply rates.\
{\
`total` : Total amount of NXS in existence.

`target` : The target supply rate for this point in time.

`inflationrate` : The current inflation rate percentage based on the last year of data.

`minute` : The increase in supply rate within the last minute.

`hour` : The increase in supply rate within the last hour.

`day` : The increase in supply rate within the last day.

`week` : The increase in supply rate within the last week.

`month` : The increase in supply rate within the last 30 days.\
}

`reserves` : Information on the amount of NXS currently in the various reserves.\
{\
`ambassador` : Amount of NXS in the ambassador reserves, waiting to be paid to the ambassador sig chains.

`developer` : Amount of NXS in the developer reserves, waiting to be paid to the developer sig chains.

`fee` : Amount of NXS in the fee reserves.

`hash` : Amount of NXS in the hash channel reserves.

`ambassador` : Amount of NXS in the prime channel reserves.\
}

### `list/peers`

Returns a summary of information about the peers currently connected to this node. The return array is sorted by the peer `score` value.

#### Endpoint:

`/system/list/peers`

{% swagger method="get" path="/system/list/peers" baseUrl="http://api.nexus-interactions.io:8080" summary="list/peers" %}
{% swagger-description %}
Returns a summary of information about the peers currently connected to this node. The return array is sorted by the peer score value.
{% endswagger-description %}

{% swagger-response status="200: OK" description="peers list" %}
```json
{
  "result": [
    {
      "address": "104.248.2.51:9888",
      "type": "5.0.5 Tritium CLI [LLD][x64]",
      "version": 30000,
      "session": 7046197205965029000,
      "outgoing": true,
      "height": 4178263,
      "best": "64604ef1745cec24eeef",
      "latency": 353,
      "lastseen": 1638986374,
      "connects": 43,
      "drops": 42,
      "fails": 0,
      "score": 10980
    },
    {
      "address": "83.76.253.28:9888",
      "type": "5.0.5 Tritium CLI [LLD][x64]",
      "version": 30000,
      "session": 6452724524911455000,
      "outgoing": true,
      "height": 4178263,
      "best": "64604ef1745cec24eeef",
      "latency": 260,
      "lastseen": 1638986379,
      "connects": 956,
      "drops": 950,
      "fails": 8,
      "score": 21990
    }
  ],
  "info": {
    "method": "system/list/peers",
    "status": "active",
    "address": "103.89.235.214:27157",
    "latency": "1.567808 ms"
  }
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

fetch(`${SERVER_URL}/system/list/peers`)
.then(resp => resp.json())
.then(json => console.log(json))
.catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
response = requests.get(f"{SERVER_URL}/system/list/peers")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters: `NONE`

#### Return value JSON object:

```
{
  "result": [
    {
      "address": "104.248.2.51:9888",
      "type": "5.0.5 Tritium CLI [LLD][x64]",
      "version": 30000,
      "session": 7046197205965029000,
      "outgoing": true,
      "height": 4178263,
      "best": "64604ef1745cec24eeef",
      "latency": 353,
      "lastseen": 1638986374,
      "connects": 43,
      "drops": 42,
      "fails": 0,
      "score": 10980
    },
    {
      "address": "83.76.253.28:9888",
      "type": "5.0.5 Tritium CLI [LLD][x64]",
      "version": 30000,
      "session": 6452724524911455000,
      "outgoing": true,
      "height": 4178263,
      "best": "64604ef1745cec24eeef",
      "latency": 260,
      "lastseen": 1638986379,
      "connects": 956,
      "drops": 950,
      "fails": 8,
      "score": 21990
    }
  ],
[Completed in 0.729357 ms]
```

#### Return values:

`address` : The IP address and port of the peer. This could be a LISP EID using the LISP overlay or an IP address using the native underlay.

`type` : The version string of the connected peer.

`version` : The protocol version being used to communicate.

`session` : Session ID for the current connection

`outgoing` : Flag indicating whether this was an outgoing connection or incoming

`height` : The current height of the peer.

`best` : Block hash of the peer's best chain

`latency` : The calculated network latency between this node and the peer.

`lastseen` : Unix timestamp of the last time this node had any communications with the peer.

`connects` : The number of connections successfully established with this peer since this node started.

`drops` : The number of connections dropped with this peer since this node started.

`fails` : The number of failed connection attempts to this peer since this node started.

`score` : The score value assigned to this peer based on latency and other connection statistics.

***

### `list\lisp-eids`

This will return the LISP Endpoint Identifiers (EID's) currently configured for this node. If the lispers.net API is not running / available then this will return an empty array.

#### Endpoint:

`/system/list/lisp-eids`

{% swagger method="get" path="/system/list/lisp-eids" baseUrl="http://api.nexus-interactions.io:8080" summary="list/lisp-eids" %}
{% swagger-description %}
This will return the LISP Endpoint Identifiers (EID's) currently configured for this node. If the lispers.net API is not running / available then this will return an empty array.
{% endswagger-description %}

{% swagger-response status="200: OK" description="LISP-EID" %}
```json
{
 [
    {
        "instance-id": "200",
        "eid": "240.139.76.244",
        "rlocs": [
            {
                "interface": "wlp59s0",
                "rloc-name": "mymachine",
                "rloc": "102.162.64.46"
            }
        ]
    },
    {
        "instance-id": "200",
        "eid": "fe::139:76:244",
        "rlocs": [
            {
                "interface": "wlp59s0",
                "rloc-name": "mymachine",
                "rloc": "102.162.64.46"
            }
        ]
    }
]
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"
fetch(`${SERVER_URL}/system/list/lisp-eids`)
.then(resp => resp.json())
.then(json => console.log(json))
.catch(error => console.log(error))
```
{% endtab %}

{% tab title="Python" %}
```javascript
import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
response = requests.get(f"{SERVER_URL}/system/list/lisp-eids")
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters: `NONE`

#### Return value JSON object:

```
[
    {
        "instance-id": "200",
        "eid": "240.139.76.244",
        "rlocs": [
            {
                "interface": "wlp59s0",
                "rloc-name": "mymachine",
                "rloc": "102.162.64.46"
            }
        ]
    },
    {
        "instance-id": "200",
        "eid": "fe::139:76:244",
        "rlocs": [
            {
                "interface": "wlp59s0",
                "rloc-name": "mymachine",
                "rloc": "102.162.64.46"
            }
        ]
    }
]
```

#### Return values:

`instance-id` : The LISP instance group that this EID belongs to.

`eid` : The endpoint identifier. This can be either IPv4 or IPv6 format

`rlocs` : The array of RLOC's (routing locators) associtated with the EID

`interface` : Name of the device for the RLOC

`rloc-name` : Hostname associated with the device

`rloc` : The IP address associated with the device.

***

### `validate/address`

Validates a register / legacy address .

#### Endpoint:

`/system/validate/address`

{% swagger method="post" path="/system/validate/address" baseUrl="http://api.nexus-interactions.io" summary="validate/address " %}
{% swagger-description %}
Validates a register / legacy address
{% endswagger-description %}

{% swagger-parameter in="body" name="address" required="true" %}
The base58 encoded register or legacy address to check
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="address validated" %}
```json
{
    "address": "8BuhE1y5oard3cEWqCSYK35bC6LdPRvcPR8N7GXYNatTw6De8eK",
    "is_valid": true,
    "type": "OBJECT",
    "object_type": "ACCOUNT"
} 
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

data = {address: "8BuhE1y5oard3cEWqCSYK35bC6LdPRvcPR8N7GXYNatTw6De8eK"}
fetch(`${SERVER_URL}/system/validate/address`,{
    method: 'POST',
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify(data)
})
.then(resp => resp.json())
.then(json => console.log(json))
.catch(error => console.log(error))
```
{% endtab %}

{% tab title="Pyhton" %}
```python
//import requests
SERVER_URL = "http://api.nexus-interactions.io:8080"
data = {
    "address": "8BuhE1y5oard3cEWqCSYK35bC6LdPRvcPR8N7GXYNatTw6De8eK"
}
response = requests.post(f"{SERVER_URL}/system/validate/address", json=data)
print(response.json())
```
{% endtab %}
{% endtabs %}

#### Parameters:

`address` : The base58 encoded register or legacy address to check.

#### Return value JSON object:

```
{
    "address": "8BuhE1y5oard3cEWqCSYK35bC6LdPRvcPR8N7GXYNatTw6De8eK",
    "is_valid": true,
    "type": "OBJECT",
    "object_type": "ACCOUNT"
}   
```

#### Return values:

`address` : The address that was validated.

`is_valid` : Boolean flag indicating if the address is valid or not. For legacy addresses this indicates that the address if simply formatted correctly. For register addresses this flag checks that a register exists in the blockchain at the specified address.

`type` : For register addresses, the type of register at the address. Values can be `APPEND`, `LEGACY`, `OBJECT`, `RAW`, `READONLY`, `RESERVED`, `SYSTEM`, or `UNKNOWN`.

`object_type` : If the `type` is `OBJECT` this field contains the standard object type of the object register. Values can be `ACCOUNT`, `CRYPTO`, `NAME`, `NAMESPACE`, `REGISTER`, `TOKEN`, `TRUST`, or `UNKNOWN`.

`is_mine` : If the `type` is `LEGACY` this boolean flag indicates if the private key for the address is held in the local wallet.
