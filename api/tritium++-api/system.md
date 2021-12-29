---
description: SYSTEM API
---

# SYSTEM

The System API provides public access to information about this node. This includes data such as the version of software the node is running, ledger and mempool state, node IP address, and connected peers.

### `Methods`

The following methods are currently supported by this API

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

#### Parameters: &#x20;

`password`: Provide a password in the configuration file which will help prevent accidental remote shutdown (optional). Use the password to authenticate the shutdown.

#### Return value JSON object:&#x20;

`Nexus server stopping`

{% swagger method="get" path="/system/stop" baseUrl="http://api.nexus.io" summary="Node shutdown" %}
{% swagger-description %}
Initiate node shutdown sequence
{% endswagger-description %}

{% swagger-parameter in="body" name="password" %}
password to enable authenticated shutdown
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Nexus server stopping" %}
```javascript
Nexus server stopping
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Javascript" %}
```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

data = {password: "YOUR_PASSWORD"}
fetch(`${SERVER_URL}/system/stop`,{
    method: 'POST',
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify(data)
})
.then(resp => resp.json())
.then(json => console.log(json)) // nexus server stopping
.catch(error => console.log(error)) // Some Error Occured
```
{% endtab %}
{% endtabs %}

### `get/info`

Returns a summary of information about this node

#### Endpoint:

`/system/get/info`

{% swagger method="get" path=":8080/system/get/info" baseUrl="http://api.nexus-interactions.io" summary="Node Information" %}
{% swagger-description %}
Returns a summary of information about this node
{% endswagger-description %}

{% swagger-response status="200: OK" description="Node Information" %}
```javascript
{
  "result": {
    "version": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
    "protocolversion": 3010000,
    "walletversion": 10001,
    "timestamp": 1638985630,
    "hostname": "api",
    "directory": "/home/user/.Nexus/",
    "address": "45.32.123.165",
    "private": false,
    "hybrid": false,
    "multiuser": true,
    "sessions": 6,
    "litemode": false,
    "blocks": 4178248,
    "synchronizing": false,
    "synccomplete": 100,
    "syncprogress": 0,
    "txtotal": 1,
    "connections": 31
  },
  "info": {
    "method": "system/get/info",
    "status": "active",
    "address": "10.10.101.101:27155",
    "latency": "0.494391 ms"
  }
}
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
{% endtabs %}

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

`clientmode` : Flag indicating whether this node is running in client mode.

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

{% swagger method="get" path="/system/get/metrics" baseUrl="http://api.nexus-interactions.io:8080" summary="Blockchain Metrics" %}
{% swagger-description %}
Returns metrics and statistics for the ledger, registers, etc
{% endswagger-description %}

{% swagger-response status="200: OK" description="Blockchain Metrics" %}
```javascript
{
  "result": {
    "registers": {
      "total": 175059,
      "account": 86428,
      "append": 0,
      "crypto": 29388,
      "name": 58219,
      "name_global": 18,
      "name_namespaced": 29,
      "namespace": 13,
      "object": 69,
      "object_tokenized": 23,
      "raw": 0,
      "readonly": 360,
      "token": 91
    },
    "sig_chains": 29388,
    "trust": {
      "total": 491,
      "stake": 25985364,
      "trust": 7050474315
    },
    "supply": {
      "total": 72219002.863206,
      "target": 69892243.017788,
      "inflationrate": 3.3290673541924036,
      "minute": 4.15395,
      "hour": 249.23414,
      "day": 5980.03409,
      "week": 41790.959354,
      "month": 166204.447078
    },
    "reserves": {
      "ambassador": 132.163967,
      "developer": 188.918715,
      "fee": 61688.638,
      "hash": 118.467741,
      "prime": 83.770769
    }
  },
  "info": {
    "method": "system/get/metrics",
    "status": "active",
    "address": "103.89.235.214:27044",
    "latency": "3659.265501 ms"
  }
}
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
{% endtabs %}

#### Return value JSON object:

```
{
    "registers": {
        "total": 54732,
        "account": 14190,
        "append": 0,
        "crypto": 13375,
        "name": 26847,
        "name_global": 2,
        "name_namespaced": 1,
        "namespace": 5,
        "object": 3,
        "object_tokenized": 1,
        "raw": 0,
        "readonly": 0,
        "token": 32
    },
    "sig_chains": 13375,
    "trust": {
        "total": 280,
        "stake": 21939510.0,
        "trust": 3004859583
    },
    "supply": {
        "total": 65177607.726929,
        "target": 64175336.948024,
        "inflationrate": 1.5617694063947738,
        "minute": 6.97545,
        "hour": 418.519742,
        "day": 10040.397562,
        "week": 70104.681744,
        "month": 277954.775646
    },
    "reserves": {
        "ambassador": 671.554894,
        "developer": 510.261963,
        "fee": 10946.79,
        "hash": 156.374712,
        "prime": 100.545744
    }
}

```

#### Return values:

`registers` : Statistics on the register database.\
{\
&#x20;  `total` : Total number of registers in the register database.

&#x20;  `account` : Number of account registers. This includes both NXS and other token accounts.

&#x20;  `append` : Number of append registers.

&#x20;  `crypto` : Number of crypto registers.

&#x20;  `name` : Total number of name registers, including user local, global, and namespaced.

&#x20;  `name_global` : Number of names that have been created in the global namespace.

&#x20;  `name_namespaced` : Number of names that have been created in a namespace.

&#x20;  `object` : Number of non-standard object registers. These include assets, items, and any other derived object register

&#x20;  `object_tokenized` : Number of object registers that have been tokenized (where the object owner is a token rather than a signature chain).

&#x20;  `raw` : Number of raw registers.

&#x20;  `readonly` : Number of read only registers.

&#x20;  `token` : Number of token registers.\
}

`sig_chains` : Total number of signature chains in the ledger.

`trust` : Statistics on active trust accounts.\
{\
&#x20;  `total` : Total number trust accounts that are active (have a non-zero stake balance).

&#x20;  `stake` : Total amount of NXS currently being staked network-wide).

&#x20;  `trust` : Total trust score of all trust accounts being staked. }

`supply` : Metrics on NXS supply rates.\
{\
&#x20;  `total` : Total amount of NXS in existence.

&#x20;  `target` : The target supply rate for this point in time.

&#x20;  `inflationrate` : The current inflation rate percentage based on the last year of data.

&#x20;  `minute` : The increase in supply rate within the last minute.

&#x20;  `hour` : The increase in supply rate within the last hour.

&#x20;  `day` : The increase in supply rate within the last day.

&#x20;  `week` : The increase in supply rate within the last week.

&#x20;  `month` : The increase in supply rate within the last 30 days.\
}

`reserves` : Information on the amount of NXS currently in the various reserves.\
{\
&#x20;  `ambassador` : Amount of NXS in the ambassador reserves, waiting to be paid to the ambassador sig chains.

&#x20;  `developer` : Amount of NXS in the developer reserves, waiting to be paid to the developer sig chains.

&#x20;  `fee` : Amount of NXS in the fee reserves.

&#x20;  `hash` : Amount of NXS in the hash channel reserves.

&#x20;  `ambassador` : Amount of NXS in the prime channel reserves.\
}

***

### `list/peers`

Returns a summary of information about the peers currently connected to this node. The return array is sorted by the peer `score` value.

#### Endpoint:

`/system/list/peers`

{% swagger method="get" path="/system/list/peers" baseUrl="http://api.nexus-interactions.io:8080" summary="List connected peers" %}
{% swagger-description %}
Returns a summary of information about the peers currently connected to this node. The return array is sorted by the peer score value.
{% endswagger-description %}

{% swagger-response status="200: OK" description="List peers" %}
```javascript
[
    {
        "address": "109.187.187.55:8888",
        "type": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
        "version": 3010000,
        "session": 13591814304884184032,
        "outgoing": true,
        "height": 47189,
        "best": "dddbe73c9e86ba92f6ba",
        "latency": 151,
        "lastseen": 1639212275,
        "connects": 11,
        "drops": 3,
        "fails": 1583,
        "score": -22055.0
    },
    {
        "address": "29.85.126.121:8888",
        "type": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
        "version": 3010000,
        "session": 4816396900022677033,
        "outgoing": true,
        "height": 0,
        "best": "00007171ddac6e07b1f1",
        "latency": 158,
        "lastseen": 1639212275,
        "connects": 5,
        "drops": 0,
        "fails": 19,
        "score": 8540.0
    },
    {
        "address": "42.53.52.223:8888",
        "type": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
        "version": 3010000,
        "session": 8708530595238045774,
        "outgoing": true,
        "height": 47189,
        "best": "dddbe73c9e86ba92f6ba",
        "latency": 167,
        "lastseen": 1639212277,
        "connects": 72,
        "drops": 22,
        "fails": 1550,
        "score": -15360.0
    },
    {
        "address": "16.9.45.190:8888",
        "type": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
        "version": 3010000,
        "session": 14221545872908411114,
        "outgoing": true,
        "height": 47189,
        "best": "dddbe73c9e86ba92f6ba",
        "latency": 308,
        "lastseen": 1639212278,
        "connects": 83,
        "drops": 17,
        "fails": 2969,
        "score": -24695.0
    },
    {
        "address": "41.97.107.147:8888",
        "type": "5.1.0-rc2 Tritium++ CLI [LLD][x64]",
        "version": 3010000,
        "session": 5703523918450669428,
        "outgoing": true,
        "height": 47189,
        "best": "dddbe73c9e86ba92f6ba",
        "latency": 36,
        "lastseen": 1639212279,
        "connects": 10,
        "drops": 4,
        "fails": 5555,
        "score": -29340.0
    }
]

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
  "info": {
    "method": "system/list/peers",
    "status": "active",
    "address": "103.89.235.214:27157",
    "latency": "1.567808 ms"
  }
}
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

{% swagger method="get" path="/system/list/lisp-eids" baseUrl="http://api.nexus-interactions.io:8080" summary="Returns LISP Endpoint Identifers (EID's)" %}
{% swagger-description %}
This will return the LISP Endpoint Identifiers (EID's) currently configured for this node. If the lispers.net API is not running / available then this will return an empty array.
{% endswagger-description %}

{% swagger-response status="200: OK" description="List Lisp-eids" %}
```javascript
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

```javascript
const SERVER_URL = "http://api.nexus-interactions.io:8080"

fetch(`${SERVER_URL}/system/list/lisp-eids`)
.then(resp => resp.json())
.then(json => console.log(json))
.catch(error => console.log(error))
```

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

{% swagger method="post" path="/system/validate/address" baseUrl="http://api.nexus-interactions.io" summary="Validates a register / legacy address " %}
{% swagger-description %}
Validates a register / legacy address
{% endswagger-description %}

{% swagger-parameter in="body" name="address" required="true" %}
The base58 encoded register or legacy address to check
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="address valid" %}
```javascript
{
    "address": "8CmjM4n8haU26RHRNeeP9NzXkEboN1bfgY7piEcv5AMd1brrJWd",
    "valid": true,
    "type": "OBJECT",
    "mine": true,
    "standard": "ACCOUNT"
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

***