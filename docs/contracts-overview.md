---
id: contracts-overview
title: Overview
sidebar_label: Overview
---

The proof decentralized exchange is a hybrid decentralized exchange that aims at bringing together the ease of use of centralized exchanges along with the security and privacy features of decentralized exchanges. Orders are matched through the proof orderbook. After orders are matched, the decentralized exchange operator has the sole ability to perform a transaction to the smart contract.This provides for the best UX as the exchange operator is the only party having to interact directly with the blockchain. Exchange users simply sign orders which are broadcasted to the orderbook.This enables users to cancel their orders without having to perform a blockchain transaction and pay the associated gas fees.

## Contracts

The main contract in the current decentralized exchange smart-contract is the [Exchange.sol](https://github.com/ProofSuite/amp-dex/blob/develop/contracts/Exchange.sol) contract which handles settlement and canceling orders. As mentioned in previous parts of this documentation, order matching is handled off-chain for increased efficiency.

## Overview of the exchange contract functions

| Function            | Description                      |
| ------------------- | -------------------------------- |
| setWethToken        | Allows owner to set a given address as WETH token. WETH token are accepted as fees for executed trades.|
| setFeeAccount       | Allows owner to set a given address as fees account. All the fees received in executed trades will be deposited in this address.|
| setOperator         | Allows owner to set/unset a given address as exchange operator. Operator's have the ablity to access 'executeTrade' functions & settle orders.|
| executeTrade        | Allows owner/operator to settle matched order between a maker & taker on chain.|
| cancelOrder         | Allows maker to cancel an input order.|
| cancelTrade         | Allows taker to cancel an input trade.|
| isValidSignature    | Verifies whether a given input signature is valid or not.|
| isRoundingError     | Checks if rounding error is greater than 0.1% for given numerator, denominator & target. It is used to verify that the rounding error in the calulated makerTokenAmount for given tradeAmount, does not exceed 0.1%|
| getPartialAmount    | Calculates partial value for given numerator, denominator & target. It is used to calculate partial makerTokenAmount for a trade as well as partial maker & taker fees for a trade. |
| getOrderHash        | Calculates Keccak-256 hash of order.|
| getTradeHash        | Calculates Keccak-256 hash of trade.|

## Allowance versus Transfer

When trading via an AMP exchange, you are not making any token deposit in the exchange contract. Instead you are allowing the exchange contract to use your tokens on behalf (by the means of the ERC20 `allow` interface). This enables you keep using these tokens while still allowing the exchange contract to perform trades if one of your orders gets matched.

Example on how to allow tokens to the AMP-DEX
```
let exchange = await Exchange.deployed();
let token = await Token.deployed()
let trader = web3.eth.accounts[0]

await token.approve(exchange.address, 1000, {from: trader})
```

You can find more examples in the [test](https://github.com/Proofsuite/amp-dex/blob/develop/tests/) folder of the AMP smart contracts repository.

## Fees

The AMP decentralized exchange relies on the WETH token for settling fees to the exchange operator. You can think of WETH as a tokenized version of ether that allows the AMP decentralized exchange to give its users a better user experience. Therefore, before starting to trade on the AMP decentralized exchange, you will need to own some WETH tokens in your trading account which you can retrieve by simply converting when initially depositing ether.

(insert screencast here)

Fees are set by the decentralized exchange operator and acknowledged by the maker and the taker when signing orders and trades. If an order maker tries to set a different maker fee, the order will be dismissed by the matching-engine (and ultimately by the exchange contract).

You can read about WETH more in details more in details [here]()

## Operators

Operators are the Ethereum addresses/nodes in charge of sending trade transactions to the exchange smart contract.
Currently the exchange smart-contract assumes all operators are managed by the same entity. Ultimately, in order to transform the exchange contract into
a protocol level smart contract, we will introduce a way for different operators to use the same smart-contract and to collaborate and/or share liquidity.

An operator address is linked to the matching engine and has the sole ability to execute trades on the exchange smart contract after two orders are matched. Even then,
this does not mean that the operator contract can execute any trade. For a trade to be executed the operator needs to have a trade that has been signed both by the taker and by the maker of the initial order.
