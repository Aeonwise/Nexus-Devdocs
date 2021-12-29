---
description: Nexus Mobile Wallet
---

# 📱 Mobile Wallet

Currently, Nexus is developing a mobile wallet which allows users the ability to manage coins, [tokens](https://tech.nexus.io/what-tritium?s=tokens) and [assets](https://tech.nexus.io/what-tritium?s=assets) from their smartphone and will eventually, combined with the three-dimensional chain ([3DC](https://tech.nexus.io/3dc)), help to secure the Nexus blockchain. This article will delve into the inner workings of the mobile wallet and give an understanding of how it all works.&#x20;

Traditionally, mobile wallets use a central node run by the wallet designer to communicate with the rest of the network**.** The central node acts as a trusted proxy and is the sole provider of information regarding blockchain state. The Nexus wallet, on the other hand, operates as a lite node by downloading a list of peers from the network seed nodes and forming connections to several of its peers. This allows the wallet to broadcast transactions directly to the network and detect confirmations by checking for inclusion against block headers. The Nexus mobile wallet gains added security by being able to verify block headers provided by multiple peers instead of through one central node. For the tech-savvy, this is also known as [Simplified Payment Verification](https://wiki.bitcoinsv.io/index.php/Simplified\_Payment\_Verification).

![](../.gitbook/assets/MW.png)

Following the principle of the Nexus [software stack](https://tech.nexus.io/software-stack), new mobile dapps will be able to interface and interact with the local wallet instead of relying on a separate server to obtain information regarding blockchain state. The Nexus mobile wallet is also completely non-custodial, meaning that your [signature chain](https://tech.nexus.io/signature-chains) remains completely private. All transactions are processed and packaged on the local device and then broadcast to the rest of the network, just like a full node.&#x20;

By necessity, the Nexus mobile wallet will only store and process block headers and the user's signature chain, starting with the hash of the very first Tritium block hard-coded into the wallet. For an advanced user running their own full node and certain API functionality, it is possible to enable the mobile wallet to establish a direct connection. This enables a user to offload heavy computational work to their desktop node, such as searching through historical data or maintaining a full copy of the blockchain.&#x20;

\
