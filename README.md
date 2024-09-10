# papers
All my papers

## Relay ring protocol 
Relay ring protocol is specification for very much "classic" cryptocurrency bridge design, using threshold signatures for bridging.

[Read](./relay-ring-protocol.md)


## Data Access Networks

Data access network is a specification that allows for accessing arbitrary blockchain's data without synchronizing own node nor trusting any centralized party for it.
It uses BFT consensus and self-migrated list of voting powers to achieve high levels of confidence in integrity of data

[Read](./data-access-networks.md)

## Cryptographic Subkeys in Decentralized Applications

Describes a way for dApps to interact with blockchains without directly involving users' wallet, but rather deriving one for each dApp and user (on login), making it perfect way for non-financial or low-stake financial apps to interact with blockchains.

[Read](./subaccounts.md)

## Tradeable-Mintable-SmartWeave-NFT

This research describes how a functioning NFT contract with marketplace capability can work even in read-only envinroment, providing the ability to trade NFTs even when smart contract cannot directly hold value that's being transferred in exchange for an NFT.
Paper focuses on usage within arweave, where smartweave contracts cannot hold AR (native coin).

[Read](https://ucfobtm3o5iblpls37lmzbktytxjoo24guajfciszltxbetweaba.arweave.net/oIrgzZt3UBW9ct_WzIVTxO6XO1w1AJKJEsrncJJ2IAI)

## Highlayer: A New Architecture for Effective Rollups

Describes Highlayer, a layer 2 on top of bitcoin that does keeps majority of transactions off chain, yet still secure and decentralized. Not even outcome state has to be published to bitcoin, thus the system is able to achieve exceptionally high throughput for an l2 solution that still inherits all the properties of base chain.

[Read](https://highlayer.io/litepaper.pdf)



# Co-authoring

Following papers weren't written by me nor did I do majority of writing and designing for them. Yet, I made contributions to them and thus can be considered co-author:

## EIP-7689: Smart Blobs

This ERC proposes a way to utilize EIP-4844 blobs as a foundation for lazily-evaluated smart contracts. External storage solution is used for persisting data after blob is forgotten.
The system allows for flexibility and better scalability of smart contracts on top of modern EVMs, even though compromising native interoperability with chain it's running on.

[Read](https://ethereum-magicians.org/t/erc-7689-smart-blobs/19672)
