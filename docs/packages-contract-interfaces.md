---
id: packages-contract-interfaces
title: Contract Interfaces
sidebar_label: Contract Interfaces
---

## Contract Interfaces

To install the corresponding npm repository, use the github remote url:
```bash
npm install https://github.com/ProofSuite/proof-contract-interfaces
```

## Decentralized Exchange

### Contracts deployed on Private Geth Chain

* Exchange: 0x7cc4b1851c35959d34e635a470f6b5c43ba3c9c9
* Migrations: 0x0f5ea0a652e851678ebf77b69484bfcd31f9459b
* Token1: 0xc4c7497fbe1a886841a195a5d622cd60053c1376
* Token2: 0x9c47796bc1e469a60dcbf680273ff011e45a1327
* Token3: 0x69bd17ead2202072ae4a117b036305a94ccf2e06
* WETH: 0x85a84691547b7ccf19d7c31977a7f8c0af1fb25a

## Usage

#### Import an exchange contract
```
import { Exchange } from 'proof-contracts-interfaces'
```

- The abi of the contract is accessible through the .abi field of the provided object.
- If required, the address of the contract is accessible through the address field of the provided object

- Note: This only imports the contract interfaces and you will need to create the contract instance yourself by using
an appropriate library.

#### Import any of the ERC20 Tokens
```
import { ERC20Token } from 'proof-contract-interfaces'
```

In this case the address field will not be set and you will need to provide your own address when instantiating
the contract

#### Import a specific Token
```
import { Token1 } from 'proof-contract-interfaces'
```

The address of that token will then be accessible through the .address field. There are currently three
tokens deployed with a predefined amount that has been sent to each of the main addresses.

#### Import WETH Contract

```
import { WETH } from 'proof-contract-interfaces'
```

### Network ID

The network ID for this test network is 1000
