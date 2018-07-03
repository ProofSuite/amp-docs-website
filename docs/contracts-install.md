---
id: contracts-install
title: Install
sidebar_label: Install
---


# Proof DEX
Official Repository for the Proof Decentralized Exchange

<!-- [![Build Status](https://travis-ci.org/ProofSuite/ProofCryptoFiat.svg?branch=develop)](https://travis-ci.org/ProofSuite/ProofCryptoFiat) -->
<!-- [![Code Coverage](https://codecov.io/gh/ProofSuite/ProofCryptoFiat/branch/develop/graph/badge.svg)](https://codecov.io/gh/ProofSuite/ProofCryptoFiat) -->

## Architecture

The proof decentralized exchange is a hybrid decentralized exchange that aims at bringing together the ease of use of centralized exchanges along with the
security and privacy features of decentralized exchanges. Orders are matched through the proof orderbook. After orders are matched, the decentralized exchange
operator has the sole ability to perform a transaction to the smart contract. This provides for the best UX as the exchange operator is the only party having to
interact directly with the blockchain. Exchange users simply sign orders which are broadcasted to the orderbook. This enables users to cancel their orders without
having to perform a blockchain transaction and pay the associated gas fees.

## Development and Testing Environment Setup

### Requirements :
- OSX or Linux (Windows setup is likely possible but not covered in this guide)
- Node (version 8.9.4 recommended)
- Solidity Compiler (v0.4.24)
- Ganache-cli (v6.1.3)
- Truffle (v4.1.12)
- npm (version 6.1.0)


### Testing Environment Setup :

- Clone the repository and install dependencies

```sh
git clone https://github.com/ProofSuite/proof-dex.git
cd prood-dex
npm install
```

- Install the latest version of truffle (Truffle v4.0.6)


```sh
npm install -g truffle
```

- Compile contracts
```sh
truffle compile
```

- Initialize testrpc (or geth)

```sh
./start_rpc.sh
```

- Migrate contracts to chosen network

```sh
truffle migrate --network development
```

- Make sure you are using the latest version of node

```sh
nvm install 8.9.4
nvm use 8.9.4
```


- Fill in `truffle.js` and `deploy_contracts.js` with appropriate wallet addresses. Unlock the corresponding addresses.

- Verify all tests are passing.

You need to re-migrate all contracts before running the test suite

```sh
truffle migrate --reset && truffle test
```

- You can interact with the contracts via the console

```sh
truffle console
```


- You can watch for changes on the fileystem with:

```sh
npm run watch
```

- Lint

```sh
npm run lint
```

## Contribution

Thank you for considering helping the Proof project !

To make the Proof project truely revolutionary, we need and accept contributions from anyone and are grateful even for the smallest fixes.

If you want to help Proof, please fork and setup the development environment of the appropriate repository.
In the case you want to submit substantial changes, please get in touch with our development team on our slack channel (slack.proofsuite.com) to
verify those modifications are in line with the general goal of the project and receive early feedback. Otherwise you are welcome to fix, commit and
send a pull request for the maintainers to review and merge into the main code base.

Please make sure your contributions adhere to our coding guidelines:

- Code must adhere as much as possible to standard conventions (DRY - Separation of concerns - Modular)
- Pull requests need to be based and opened against the master branch
- Commit messages should properly describe the code modified
- Ensure all tests are passing before submitting a pull request

## License

The Proof CryptoFiat smart contract (i.e. all code inside of the contracts and test directories) is licensed under the MIT License, also included in our repository in the
LICENSE file.




