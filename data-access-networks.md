# Introduction
Since bitcoin's invention, many blockchains were created with decentralization in mind. And it works: You really can install full chain node and have access to your blockchain in decentralized way. 

But operating full blockchain node requires relatively good technical knowledge, open ports, big SSD drive and CPU (sometimes even GPU to make it faster) powers.

At the moment of writing (09.08.2022) Bitcoin's full node requires around 420GB of disk space, Ethereum's full node requires around 900 GB of space (non-archival, archival requires 11.7 TB), and there's much more examples of growing chain sizes (while even if you have disk, operating node requires strong CPU and fast internet connection to sync and support full node).

Because of complexiety described above, most users use "Nodes-as-a-service" services like [Infura](https://infura.io/).

These services are not requesting your wallet key, so you still have control over your funds. However, centralization still stays as main lack of such services.

Existance and adoption of such services makes blockchains and platforms based on it not censorship-resistant and centralized in some way.

Tornado Cash, ethereum's crypto mixer that pretended to be decentralized, was blocked in several ways, one of these ways and one of main ways was blocking interactions with Tornado Cash using Infura API endpoint.

From this event we understood where should we decentralize our dApps, and interfaces for chain queries are one of the most important points.

# Solution

## Lite nodes

Lite node is blockchain node connects to blockchain's p2p swarm, acts like full chain node, but not contains and operates full chain. Such nodes usually rely on other full nodes in p2p swarm. [SPV](https://en.bitcoinwiki.org/wiki/Simplified_Payment_Verification) is one of ways of how lite nodes should work.
But SPV haves few lacks:
- It is very complex to use it with Proof of Stake blockchains.
- Lite nodes require healthy altruistic full nodes in network.
- Not suitable for smart contracts platforms, as there's big contract state needed to confirm.

