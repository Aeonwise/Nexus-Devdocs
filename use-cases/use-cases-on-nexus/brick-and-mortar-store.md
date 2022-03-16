---
description: Real world use-case for Brick & mortar Stores
---

# 🏪 Brick and Mortar Store

Nexus has been designed for the real world, everyday use by the average joe and can be understood in this use-case. This can be easily implemented by any brick and motor stores to manage the store by integrating Nexus.

Nexus can be used for payments, invoices, logistics and tracking, online marketplace.  To make it easy we need to understand how these things can be implemented.

{% hint style="info" %}
Depending on the location and regulations crypto payments might not be feasible. Lot of things need to be fined tuned to local regulations. The POS system also will need further tuning depends on usage and regulations .
{% endhint %}

## How can it be Implemented

The brick & motor stores will use the hybrid network feature, where they can accept payments from customers and use invoices for accepting payments and paying distributors on the public chain. While they can manage the inventory and take care of logistics on the private chain. This makes it easy as there are no fees involved on the private chain and also the data is private.

The store can even have an online marketplace powered by the P2P API, which is an on-chain order book exchange.

The store needs to integrate the Nexus API into a totally new Point of Sale (POS) system or integrate into existing systems.

## Overheads For the store

The store owner will have to run nodes to maintain the hybrid network. The no of nodes will depend on the redundancy the database needs. Compared to existing systems the overheads are very low. If the store owner needs 3x redundancy, he needs to run 3 nodes which will be hosted on VPS or owned hardware or a mix of both.&#x20;

A new POS or integration into system maybe additional cost,  but if we can make a open source code which can be used by any store that can save that particular overheads.

## What things can be implemented by Stores

Brick & motor stores can use one or all the options listed below:

### Payments:&#x20;

The Nexus Interface will have a Point of Sale payments module. This will make it very easy for customers to pay using the mobile wallet. This will be similar to Apple pay or Google pay where the user scans a QR code to send the payment. The store can even integrate Nexus API into their existing system. With the Query DSL it is very easy to keep track of all the payments and generate reports.

### Invoices:

The store can issue invoices or accept invoices from distributors which will keep a track of all the payments done. This can be accessed using the Interface invoice module or integrating the Invoice API into existing system. Reports are built into the invoice API and can be further narrowed down using on-chain search powered by Query DSL.

### Inventory Tracking:

Nexus ha a supply API which can be used to track the inventory. This  data will be stored on the private side of the hybrid network which has no fees, is very fast and is free. Each item will have its own QR code generated when minting as an item and the existing product bar codes can be integrated into the supply items as the unique identifier or name.

### Online Marketplace:

Brick & motor stores also can have an online marketplace presence with the upcoming P2P marketplace which can be used to exchange tokens and assets. Store owners need to  create their physical goods as assets on the public chain, put it for sale with the location and store name. Users can search assets available in their area using the location and they can buy it online and the store owner can use the public chain supply API to track the goods sent by courier.

