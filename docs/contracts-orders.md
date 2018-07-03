---
id: contracts-orders
title: Orders and Trades
sidebar_label: Orders and Trades
---

## What are orders ? What are trades ?

In traditional centralized limit orderbooks (CLOB's), the sender of an order that is not matched immediately is
called the **maker** whereas the sender of an order that is immediately matched with an order that is already in the
orderbook is labeled the **taker**. As other decentralized exchanges have done before, we adopt the same terminology.

Traders send orders to the matching engine associated to a given exchange. When the matching engine matches two orders, it creates a trade object that can be sent to the exchange smart contract.

## Executing a Trade

To execute a trade, `executeTrade` needs to be called with the following arguments:

| Argument            | Type                             | Description                     |
| ------------------- | -------------------------------- |-------------------------------- |
| amountBuy           | uint256                          | Amount of _tokenBuy_ tokens the maker wants to buy |
| amountSell          | uint256                          | Amount of _tokenSell_ tokens the maker wants to sell |
| expires             | uint256                          | Blocknumber at which the order will no longer be valuable |
| nonce               | uint256                          | Random number to ensure uniqueness of the order |
| feeMake             | uint256                          | Percentage of fee taken from maker token value |
| feeTake             | uint256                          | Percentage of fee taken from taker token value |
| amount              | uint256                          | Amount of _tokenBuy_ the taker wants to **sell** |
| tradeNonce          | uint256                          | Random number to ensure uniqueness of the trade |
| tokenBuy            | address                          | Address of _tokenBuy_ (token the maker wants to buy) |
| tokenSell           | address                          | Address of _tokenSell_ (token the maker wants to sell) |
| maker               | address                          | Maker Address |
| taker               | address                          | Taker Address |
| vMaker              | uint8                            | Maker Signature v Parameter |
| vTaker              | uint8                            | Taker Signature v Parameter |
| rMaker              | bytes32                          | Maker Signature r Parameter |
| rTaker              | bytes32                          | Maker Signature r Parameter |
| sMaker              | bytes32                          | Maker Signature s Parameter |
| sTaker              | bytes32                          | Maker Signature s Parameter |

## Example

The current matching-engine is written in Golang. Below we give an example on how to create a trade corresponding to an order.

```javascript
let order = {
  amountBuy: 1000,
  amountSell: 1000,
  expires: 9999999,
  nonce: 1,
  feeMake: 1e17,
  feeTake: 1e17,
  tokenBuy: token2.address,
  tokenSell: token1.address,
  maker: trader1
}

orderHash = getOrderHash(exchange, order)

let trade = {
  amount: 500,
  tradeNonce: 1,
  taker: trader2
}

tradeHash = getTradeHash(orderHash, trade)

let {message: message1, messageHash: messageHash1, r: r1, s: s1, v: v1} = web3.eth.accounts.sign(orderHash, privateKeyOfTrader1);
let {message: message2, messageHash: messageHash2, r: r2, s: s2, v: v2} = web3.eth.accounts.sign(tradeHash, privateKeyOfTrader2);

let orderValues = getMatchOrderValues(order, trade)
let orderAddresses = getMatchOrderAddresses(order, trade)

await exchange.executeTrade(orderValues, orderAddresses, [v1, v2], [r1, s1, r2, s2])
```

You can find more examples on how to interact with the Exchange smart-contract in the [test](https://github.com/ProofSuite/amp-dex/blob/develop/test/exchange.js) folder for the amp-dex smart contracts.

## Note on how to sign messages prefix

Please be sure to prepend the `\x19Ethereum Signed Message:\n<length of message>`where length of message is`\x32`prefix when signing orders or trades if you plan to write your own client. (See [discussion](https://github.com/ethereum/go-ethereum/issues/3731))