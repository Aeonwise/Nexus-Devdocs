---
description: How to transition from Ethereum to Nexus
---

# Ethereum to Nexus

If you are someone who has experience developing on Ethereum, EVM compatible or other protocol chains and interested in building on Nexus:

* Nexus has a disparate architecture compared to Ethereum and other protocol chains.
* Nexus is a _`Verification`_ engine, unlike Ethereum which is a _`Computation`_ engine
* Nexus is not EVM compatible
* Nexus is not interoperable with other chains and we have a very good reason for that.
* Nexus is still under heavy development. Refer to the [roadmap](https://nexus.io/roadmap) for more details.

## How to Build on Nexus:

Nexus does not have a smart-contract programming language similar to Ethereum. As said before being a unique architecture; the options to build on Nexus are as below.

### INTEGRATED API:

Today development is made possible with the REST API which is a higher level API. This empowers  web developers and even no-code developers to build blockchain applications.

{% content-ref url="../../api/api-overview/" %}
[api-overview](../../api/api-overview/)
{% endcontent-ref %}

### ADVANCED DEVELOPERS:

For more advanced developers who need full creative control, we have developed augmented contacts which can be used with any domain specific language of choice.

Today we have Conditional Contracts, Primitive Operators, and Registers. More programmable languages and things like maps, vectors, functions, etc will come with augmented contracts

Augmented Contracts are the second type of contracts that will be available in the Tritium Protocol. These types of contracts extend the Conditional VM (Virtual Machine that processes Conditional Statements) to provide additional benefits including, but not limited to, methods, functions, operation overloading, and encapsulation. Augmented contracts add a layer of complexity and processing, so will carry a higher fee to execute. This will require more on-chain processing, but overall makes our Contract Engine much more powerful

The goal of augmented contracts is to provide relative capabilities of complex languages like C++, so we will support polymorphism, operator overloading, functions, methods, public, private, protected, unique, etc. augmented contracts builds on the existing VM and register based architecture
