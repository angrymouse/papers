# Cryptographic subkeys in blockchain wallets
Authors: Nik Rykov <nik@hns.is>

License: CC0 Public domain

05.30.2023



## Abstract
In blockchain and dApp context, we are used to using single cryptographic key for everything - from spending coins to sending messages, encryption and decryption, smart contract interactions.
And while this system did work well during past decades since bitcoin invention, we are limited in UX (user has to review each action done via his key), security (even 1 wrongly reviewed action can result in loss of everything), and flexibility (we are limited to usages of key's keysystem, and some keysystems don't support something like encryption, only signatures).

This design document provides alternative approach, in which each dApp gets its own subkey which is used for user interactions with dApp.

## Motivation
To create flexible, secure, UX-friendly approach of interactions with dApps.

## Terminology

dApp - decentralized application, application that needs wallet for some operations such as cryptographic authorization, cryptographic encryption, etc. 
master key, wallet, single key - cryptographic keypair used for authorization in dApps.
user - person or entity using dApp with master key. 
subkey - cryptographic keypair which is either derived from master key or to which master key has full access.

## Requirements

This scheme requires either:
- Possibility of using master key for encryption/decryption (i.e RSA)
- Arbitrary data derivation possibility (hash) with usage of master key as a secret base for result

First way is easy to understand and is recommended to implement if available, however it requires master key's keysystem to support encryption. 
Some keysystems like RSA support it, but there are systems that don't (like BLS or ed25519), and for them implementing second variant might be optimal solution.

Second way requires master key provider (wallet interface, connector) to implement private hash function - function that would return cryptographic hash of key+dApp name.
It's recommended to use argon2 for private hash function as it's recommended hash scheme for password (and so keys) derivation.

