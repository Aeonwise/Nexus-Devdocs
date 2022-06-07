---
description: Designed for the real world, the everyone's everyday contracts.
---

# 📃 Advanced Contracts

The Nexus Virtual Machine (NVM) is a **`state machine`** and exists as one single entity maintained by hundreds of connected Nexus nodes.

The Nexus software stack exists solely for the purpose of keeping the continuous, uninterrupted, and immutable operation of this special state machine; it's the environment in which all Nexus Accounts and Advanced contracts live. At any given point in the chain, Nexus has one and only one **`canonical`** state, and the NVM is what defines the rules for computing a new valid state from block to block.

Even though all smart-contract platforms are state machines, the obsession to be scalable for real world usage, culminated in the distinct architecture of NVM. To easily distinguish the better architecture, it was imperative to name contracts on Nexus as Advanced contracts. Below we will unravel the architecture.

### NVM Architecture

The NVM is designed as a "`64 bit"`, "`register based"`; this design was chosen as it matches the CPU's 64 bit and mimics the CPU cache registers.

This design makes the NVM very fast compared to EVM as it's designed for the processors of today. To put it in numbers, EVM takes 1.7 million nanoseconds/instruction and NVM takes 33 nanoseconds/instruction. EVM is at a huge disadvantage as it takes 4 cycles to complete an instruction due to its 256 bit length on a 64 bit CPU and also its dated stack design.

The NVM is designed intentionally not to be turing complete, this decision also stems from the fact the Nexus is a **`Verification`** engine. This design has a huge upside and that is free simple transactions, while EVM needs Gas to control computation requests due to bad code, which can grind the network to a halt. With the NVM design advanced contracts will have predictable fees which will be calculated before contract execution.

Nexus will have different types of contracts, for the higher level API's, templates will be provided, which a user can choose from a dropdown list. For advanced users augmented contracts will empower them to use contracts with their choice of domain specific language. Augmented contracts will be available at a later date. For more details refer the [roadmap](https://nexus.io/roadmap)

## OPERATIONS & REGISTER LAYERS

To put in a simple way, contracts are a request to perform a specific type of instruction on data which results in change in the data. In the Nexus software stack the `Operations layer` contains the  instructions or actions that give registers context, and define more complex contract logic and `Register layer` is the data layer.  A contract is an object containing: a register pre-state (the register that is being operated on that was passed upwards from the Register Layer), a primitive operation (only one primitive operation per contract), and a set of conditions (any amount of conditional operations).

### **Primitive Operations**

The current iteration of the Operations Layer contains 16 primitive operations, and 54 conditional operations and types. The primitive operations can be best described as the actions taking place on the register such as: WRITE, DEBIT, TRANSFER, APPEND, etc. These actions themselves cause the register to change its state in some way or form, including its movement from one signature chain to another.

### **Conditional Contracts**

Conditional Contracts are an agreement between participating parties, outlining a set of requirements that must be met for a transaction to complete. They are the building blocks that allow users to engage with one another, such as contract expiration, or the exchanging of items. More advanced forms of non-custodial escrow or arbitration are also possible. Conditional statements have no limit to their complexity, being capable of handling groups of groups of conditions that together evaluate to either true or false. In the case the conditions return true, this allows the recipient of the transaction to claim their funds or object (depending on if this was a TRANSFER or DEBIT). In the case that the recipient is unable to satisfy the conditions, after a period of time set by the sender, the transaction will be redeemable.

{% hint style="info" %}
More information will be provided in the future when new updates are released
{% endhint %}

## API Layer

The API layer extrapolates from the contracts (registers and operation layers). The developers can easily use the API, rather than having to code _`"a smart-contract".`_ Even though API layer is not a contract layer it uses the same contract layers which can be directly accessed by domain specific languages (DSL), to create custom contracts. This highlights the brilliant architecture of the software stack as same contracts can be made available to no-code, web developers or to integrate into existing applications, at the same time abstracting them from the complexity of code or blockchain. &#x20;

## Contracts on Nexus

Nexus has designed a multi pronged approach to tackle development on Nexus. This gives everyone an opportunity to build and integrate truly decentralized solutions. The details of the various contracts on Nexus are given below

### Query DSL

Query DSL is one of the contract languages that adds capabilities similar to SQL query, including wildcard search and logical operators, enabling one to search or filter any digital asset or token on Nexus directly through the API. For example, one could search Property Titles in Arizona. This feature sets the foundation for a decentralized search engine.

The Query DSL allows you to search any aspect of the chain, you can also filter and even operate on the queried dataset. Supported operators are: _`min, max, mean, mode, sum, floor, array, sdev, median`._ More operators will be available at a later date.

Query basically gives SQL functionality over a blockchain dataset. The operators make it have Excel functionality, among others. It's really powerful for developers. It can even search the entire chain:

```
register/list/names where='object.name=l*' 
```

Above will show every name that starts with the letter `'l'`. It can then run operators on that, by filtering out fields:

```
register/list/names/name/array where='object.name=l*'
```

This will give same dataset, but give you a JSON array of the names that start with `'l'`, not including the other fields. It also supports multiple filters:

```
register/list/names/name,owner where='object.name=l*'
```

This will respond with name and owner fields, somewhat like a SQL query where you do select name FROM names. So it gives a developer data modelling capabilities on top of the blockchain functionality.

A global search of entire chain takes 1 to 2 seconds. Running the first register command I get:

`[Completed in 1862.234494 ms]`

For more information, check out the link below:

{% content-ref url="../../tritium++-api/tritium++-api/register.md" %}
[register.md](../../tritium++-api/tritium++-api/register.md)
{% endcontent-ref %}

### Conditional Contracts

Conditional contracts are binary contract templates, users will be able to select a contract standard to send with. This will lead into dynamic allocation of contract standards and object standards per command-set. Standardization where we can allocate new contracts that will automatically populate in standards lists, without the need for a wallet upgrade.

What that means is we will be able to deploy new contract standards without needing to release new wallet. It will read a public sigchain that records contract and object standards. This will lead into dynamic constants as well, such as fees so that we can adjust consensus critical values without the need for hard forks or updates to the core daemon.

When a user sends funds using the Nexus Interface, they get an extra drop-down that allows to select from a list of standard template contracts to send under. This is a major step towards real world use of smart contracts.

### Conditional Contracts DSL (Coming Soon)

Conditional Contracts DSL, a new standard to be written in a higher-level language and compiled into bytecode. Designed for users who will be developing new API’s or contract standards. It enables conditional contracts to be written directly into the API with the use of English, which is a crucial step in realising the full potential of Tritium’s contract functionality. This approach also allows people to be able to read or interpret contracts. This will reveal a lot of new functionality to developers.

We thus far have maintained the standard contracts as embedded constants in the codebase. The ability for developers to code new contracts, provides the opportunity for a dynamic standardization using Nexus to manage this state, similar to dynamic object modelling.

More specific information will be updated when available.

### Augmented Contracts ( Roadmap: 2022-2023)

For advanced developers who need full creative control, we have developed Augmented Contacts which can be used with any domain specific language of choice. Today we have Conditional Contracts, Primitive Operators, and Registers.&#x20;

Conditional VM will be a subset of Augmented, such as in my function I have an _`if`_ statement, this is executed as a condition. But augmented will allow writing complex code. For instance I have an account, I do an operator overload function for `DEBIT`, that can add additional requirements for a debit from that account such as max amount per time interval, etc. I could add another public method that could be invoked by another user to draw an amount on my account if they fulfill a certain condition, so forth. It will be the full implementation of our interpreter where we will have the functionality of most languages like Java and C++.

Augmented Contracts are the second type of contracts that will be available in the Tritium Protocol. These types of contracts extend the Conditional VM (Virtual Machine that processes Conditional Statements) to provide additional benefits including, but not limited to, methods, functions, operation overloading, and encapsulation. Augmented contracts add a layer of complexity and processing, so will carry a higher fee to execute. This will require more on-chain processing, but overall makes our Contract Engine much more powerful.

The goal of Augmented Contracts is to provide relative capabilities of complex languages like C++, so we will support polymorphism, operator overloading, functions, methods, public, private, protected, unique, etc. augmented contracts builds on the existing VM- and register-based architecture. More programmable languages and things like maps, vectors, functions, etc. will come with augmented contracts.

More specific information will be updated when available.
