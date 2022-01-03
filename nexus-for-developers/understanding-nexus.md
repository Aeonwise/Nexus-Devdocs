---
description: Things to think about when building an app on Nexus.
---

# Understanding Nexus

Nexus is a community run project and compared to other VC run projects we differ in our thinking and that reflects in the way Nexus is developed and our goal is to provide power to the people. Nexus is like a toolbox and will not provide a finished product to users, we believe the community will to build applications&#x20;



## Application Wallet Connect:

As of today the only way that an application can access the user wallet is when the user provides the user credentials, this is not the preferred method&#x20;

Nexus has designed 2fa which will be released with the Tritium++ v6.0 update where the user&#x20;

## Fee Structure

Nexus simple transactions are free, but most operations and contract execution will have fees, this is done to protect spamming the network and some actors to take undue advantage.

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

### Tokens:

Nexus makes it very simple to generate tokens with the UI wallet and as of today Nexus provides the&#x20;



* Fungible Tokens&#x20;
* Non Fungible Tokens (NFT)
* Protected Assets - <mark style="color:red;">Future</mark>

## Names and Namespaces:

Nexus provides native Names andNamespaces whih are one of the important things a developer has to take advantage to get the best user experience and also

Names and Namespaces are special kinds of object registers that are used as locators to other object registers in the blockchain. When an object register is first created (an asset for example) the caller can provide a name for the register. If a name is provided then a Name object register is also created with its register address based on a hash of the name. The Name object also has a address field, which is populated with the register address of the register (asset, token, account etc) that the Name "points" to. In this way, objects can be retrieved by name by first hashing the name to get the Name object's address, retrieving the Name object, and then using the address stored within it to retrieve the object register. A Name then, is best thought of as a named index to object registers

Names on Nexus are of three different types\\

### Local Names:&#x20;

Local names are names which are local to a user account (sig chain). These can be an account, asset. item name. To use a local name you must prefix the name with the owners username separated by a single colon, e.g. `bob:savings`. This is equivalent to saying "look at all the Names registered in the sig chain `bob` and find one called `savings` and then see what object register it points to". There can only be one Name called `savings` in the sig chain `bob`, but another user `alice` can also create a local name called `savings.`  Creating Names will cost `1 NXS`

### Namespace:&#x20;

Namespace are globally unique keyword and Namespaces allow users to provide user-friendly names for their object registers without needing to disclose their username. This is useful for privacy, but also to allow names to be related to a business or some other meaningful context. To avoid name-squatting registering a namespace name attracts a high fee (`1000 NXS`).

{% hint style="info" %}
**Good to Know:** Namespace names can only contain **lowercase letters, numbers, and periods (.)**
{% endhint %}

#### Namespaced:&#x20;

These are created within the context of a namespace, which itself is a globally unique keyword.  To use a namespaced name you must prefix the name with the namespace separated by a double colon, e.g. `bobscoffeeshop::payments`. In this example bob would have first registered the namespace `bobscoffeeshop` and created an account to receive payments to (which could be called anything). He then creates a Name with a `name=payments`, `namespace=bobscoffeeshop` and `address=(register address of the account)`. From then on, anyone can use the name `bobscoffeeshop::payments` and it will resolve to the register address of the account. Creating Names within that namespace costs only `1 NXS`

### Global Names:

Global names require no username or namespace prefix, and are therefore globally unique. These will be likely reserved for use cases where a succinct, unique, name is necessary, for example a token ticker symbol. To avoid needless name-squatting, global names attract a high fee (`2000 NXS`).&#x20;

Global Name registered and linked to a token will be referred by the ticker `NEX` instead of it's register address `8B5pKxGWgBM9nXzucrqmhdy9PuDHxDi41fqosXZsLTUXGfQ4CXn.`  This will also be convenient to launch the token on the P2P marketplace or DeFi when available.

