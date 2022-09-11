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

## Dedicated network
Solution can be found in dedicated Proof of Stake network for resolving queries.

When we use PoS as our consensus algorithm, we assume that we trust majority of validators in this network, so why not make them sign query replies for you?

We only need lite node to store list of voting powers of our validators to work well.

### Self-migrated list of voting powers
We can make upgrades of voting powers knowing only previous voting powers.

To do it, we assume that there's no big (>67%) changes in voting power between 2 different voting power states (We assume it in every PoS chain, if there's big change of voting powers, old validators can just reject signing delegation transaction and not let move themselves out of control).

If we take this assumption, we can make old validators sign new list of voting powers and serve it.

Scheme below describes how self-migrated voting power lists can work.

![self-updating-voting-powers-01](https://user-images.githubusercontent.com/40735471/189504918-d3b5866a-10e3-4307-917b-b3983557ebc3.png)

### Blind transactions

If we look precisely, then we can understand that we don't need any info about activity on chain to submit stateless transactions (transfers in state-based networks and similar, in UTXO model it not works because you need to lookup which UTXO can you use).

So we can avoid using centralized API endpoints for broadcasting transactions, just form transaction, sign and broadcast directly to p2p swam.

### Safe decentralized queries with only small piece of knowledge about chain

Now, when we understand how to keep voting powers always updated, we have ultimate method to verify data about chain without storing chain itself: We can just ask validators to sign reply to query and give it to you. Reply that will get 51%+ of voting power can be considered as truth.

#### Incentives

But if you don't incentivize validators to sign reply for your query, they most likely won't do it.

