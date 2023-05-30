# Cryptographic subkeys in dApps
Authors: Nik Rykov <nik@hns.is>

License: CC0 Public domain

05.30.2023



## Abstract
In blockchain and dApp context, we are used to using single cryptographic key for everything - from spending coins to sending messages, encryption and decryption, smart contract interactions.
And while this system did work well during past decades since bitcoin invention, we are limited in UX (user has to review each action done via his key), security (even 1 wrongly reviewed action can result in loss of everything), and flexibility (we are limited to usages of key's keysystem, and some keysystems don't support something like encryption, only signatures).

This design document provides alternative approach, in which each dApp gets its own subkey which is used for user interactions with dApp.

## Motivation
To create flexible, secure, UX-friendly approach of interactions with dApps.

## Idea
In web2, the most convinient way to authorize into small services and apps is to use so called authentication services - for that we need to have one main account (google, twitter, microsoft, apple etc) which would give app/site an authorization token with limited scope of what can it do, user would see authorization window that would briefly describe permission scope of given app to your account, and app can do whatever it needs with given token to it (while authorization system will not allow token to go beyond user given permissions).

If we apply this idea to web3, we can come up with dApp having full wallet key in its management, without layer of user wallet somehow acting as a limiter to it. 
In perfect scenario, dApp would have to only ask wallet provider once - to make/give/derive/decrypt secret keypair to dApp, while further signing/decryption would be already performed using key given by wallet provider to dApp interface, freeing user from needing to interact with wallet provider and allow every action it needs, while not compromising security as main key is remaining untouched. 


## Requirements

This scheme requires either:
- Possibility of using master key for encryption/decryption (i.e RSA)
- Arbitrary data derivation possibility (hash) with usage of master key as a secret base for result

First way is easy to understand and is recommended to implement if available, however it requires master key's keysystem to support encryption. 
Some keysystems like RSA support it, but there are systems that don't (like BLS or ed25519), and for them implementing second variant might be optimal solution.

Second way requires master key provider (wallet interface, connector) to implement private hash function - function that would return cryptographic hash of key+dApp name.
It's recommended to use argon2 for private hash function as it's recommended hash scheme for password (and so keys) derivation.

## Algorithm

### Using possibility of using master key for encryption/decryption:

Algorithm of key creation: 
- dApp generates random keypair in its memory
- dApp asks wallet provider for master key to sign generated keypair
- dApp uses generated keypair to sign master key's pubkey (for creation of bidirectional link)
- dApp encrypts subkey to master key's pubkey
- dApp announces subkey creation on public channel that will be accessible later (for example Arweave)
- dApp can use generated keypair for any encryption or signing needs

Algorithm for recovery after first time authorization:
- dApp fetches previously announced pubkey and encrypted privkey
- dApp checks bidirectional signatures to ensure that key is fraud-free
- dApp asks wallet provider for master key to decrypt fetched encrypted privkey
- dApp can use recovered keypair for any encryption or signing needs

### Using arbitrary data derivation possibility (hash) with usage of master key as a secret base for result:

Algorithm of key creation: 
- dApp generates random keypair in its memory
- dApp uses generated keypair to sign master key's pubkey (for creation of bidirectional link)
- dApp asks wallet provider for private hash of master privkey+dApp name
- dApp uses provided private hash for symmetric (i.e AES) encryption of generated privkey
- dApp announces subkey creation on public channel that will be accessible later (for example Arweave)
- dApp can use generated keypair for any encryption or signing needs

Algorithm for recovery after first time authorization:
- dApp fetches previously announced pubkey and encrypted privkey
- dApp checks bidirectional signatures to ensure that key is fraud-free
- dApp asks wallet provider for private hash of master privkey+dApp name (this hash should be exact same hash as in creation step)
- dApp decrypts subkey's privkey using provided private hash 
- dApp can use recovered keypair for any encryption or signing needs

## Terminology

- dApp - decentralized application, application that needs wallet for some operations such as cryptographic authorization, cryptographic encryption, etc. 

- master key, wallet, single key - cryptographic keypair used for authorization in dApps.

- user - person or entity using dApp with master key. 

- subkey - cryptographic keypair which is either derived from master key or to which master key has full access.
 
