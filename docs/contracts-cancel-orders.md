---
id: contracts-cancel-orders
title: Canceling Orders
sidebar_label: Canceling Orders
---

## Canceling orders

On a completely decentralized exchange, all actions from canceling orders to executing trades requires paying gas fees
and waiting for block confirmations. This is expensive and also opens several vulnerabilities such as **griefing** or **front running**. In the case of order cancels, blockchain reorganizations can alter the order in which transactions were initially planned to be made. Another possibility of exploit is to create a transaction matching an order that was just canceled. If the matching transaction has a higher gas fee than the canceling order, there is a high chance the matching transaction will succeed. In addition to this the trader that wanted to cancel his order has to pay a fee for his failing transaction.

This kind of problems is usually endurable when the liquidity on a certain exchange is not too high but becomes harder to deal when the amount of trades goes up.

## Soft-Cancels and Hard-Cancels

A possible solution to this problem is to let the operator of the exchange decide whether to broadcast a transaction or not. This involves for the trader to trust the operator of the matching engine he is currently trading with, similarly to how a trader trusts a centralized exchange to not execute any trades he has not requested to be made.

In most cases, this mechanism that we choose to call **soft cancels** is sufficient as exchange operators usually have no financial incentive to broadcast canceled orders. Every order also has an expiration field to prevent stale orders from being executed.

In some cases however, for example in the case a trader made a very large order or if the trader does not trust the exchange he is trading on, a trader might prefer to cancel their order and make sure the operator of the exchange has no way of executing the trade. For this use-case, we provide a **hard cancel** function on the exchange smart-contract. It gives the advantage of not having to rely on the operator to not broadcast your trade but requires to pay transaction fees .

## Hard-Cancel API

To hard-cancel an order on the chain, call the `cancelOrder` function on the Exchange.sol contract. The parameters required
when calling this function are the following:

| Argument            | Type                             | Description                     |
| ------------------- | -------------------------------- |-------------------------------- |
| amountBuy           | uint256                          | Amount of _tokenBuy_ tokens the maker wants to buy |
| amountSell          | uint256                          | Amount of _tokenSell_ tokens the maker wants to sell |
| expires             | uint256                          | Blocknumber at which the order will no longer be valuable |
| nonce               | uint256                          | Random number to ensure uniqueness of the order |
| feeMake             | uint256                          | Percentage of fee taken from maker token value |
| feeTake             | uint256                          | Percentage of fee taken from taker token value |
| tokenBuy            | address                          | Address of _tokenBuy_ (token the maker wants to buy) |
| tokenSell           | address                          | Address of _tokenSell_ (token the maker wants to sell) |
| maker               | address                          | Maker Address |
| v                   | uint8                            | Maker Signature v Parameter |
| r                   | bytes32                          | Maker Signature r Parameter |
| s                   | bytes32                          | Maker Signature s Parameter |

Each group of values must be packed together in array based on their type (See Exchange.sol)

