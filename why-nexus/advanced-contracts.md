---
description: Contracts on Nexus are called as Advanced contracts
---

# Advanced Contracts

The Nexus Virtual Machine (NVM) is a `state machine` and exists as one single entity maintained by hundreds of connected Nexus nodes.

The Nexus software stack exists solely for the purpose of keeping the continuous, uninterrupted, and immutable operation of this special state machine; It's the environment in which all Nexus Accounts and Advanced contracts live. At any given point in the chain, Nexus has one and only one 'authorized' state, and the NVM is what defines the rules for computing a new valid state from block to block.

Even though all contract platforms are state machines, the fascination to be exceptional, nimble and agile culminated in the distinct architecture of NVM and to easily distinguish it was imperative to christen Nexus contracts to Advanced contracts. Below we will unravel the architecture.

### NVM Architecture

The NVM is designed to be a 64 bit, register based; this design was chosen as it matches the CPU's 64 bit and mimics the CPU cache registers. Register layer exists as the data layer in the software stack.

This design makes the NVM very fast compared to EVM as it's designed for the processors of today. To put it in numbers, EVM takes 1.7 million nano sec/instruction and NVM takes 33 nano sec/instruction. EVM is at a huge disadvantage as it takes 4 cycles to complete an instruction due to its 256 bit length on a 64 bit CPU and also its dated stack design.

The NVM is designed intentionally not to be turing complete. This design decision has a huge upside and that is free simple transactions, while EVM needs Gas to control computation requests due to bad code, which can grind the network to a halt. With the NVM design advanced contracts will have predictable fees which will be calculated before contract execution.

Nexus will have different types of contracts, for the higher level API's, templates will be provided, which a user can choose from a dropdown list. For advanced users augmented contracts will empower them to use contracts with their choice of domain specific language. Augmented contracts will be available at a later date. For more details refer the [roadmap](https://nexus.io/roadmap)

{% hint style="info" %}
More information will be provided in the future when new updates are released
{% endhint %}
