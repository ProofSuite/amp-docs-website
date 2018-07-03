---
id: contracts-appendix
title: Appendix
sidebar_label: Appendix
---

## WETH (Wrapped Ether as a Token fee)

Decentralized platforms running on Ethereum use smart contracts to facilitate trades directly between users, every user needs
to have the same standardized format for every token they trade. Which ensures tokens don’t get lost in translation. ETH does
not confirm to it's own ERC20 standard as it was built before the ERC20 standard existed. Thus by converting ETH to W-ETH
enables users to trade ETH for other ERC20 tokens on decentralized platforms.

The current implementation of W-ETH can be found at :
https://etherscan.io/address/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2#code

When you "wrap" ETH, you aren't really wrapping so much as trading via a smart contract for an equal token called W-ETH.
If you want to get plain ETH back you need to "unwrap" it. AKA trade it back for plain ETH.

In order to pay fees for orders/trade in exchange they do not need to deposit any ETH in exchange, instead the user should hold
the W-ETH fees amount & have to provide allowance to exchange just like they do for other tokens they trade, so that exchange
can transfer the W-ETH fees amount to feesAccount when executing a trade.

Benifits of W-ETH :
- The source code for the proposed W-ETH implementation has been audited and held significant value over a period of time

- The W-ETH contract support for unlimited allowances, which means that the user can trade without making any deposits by sending a single allowance transaction to the exchange smart contract.

- Simplifies the exchange contract execution. The 'executeTrade' function does not need to conditionally manage and transfer ERC20 tokens and Ether. Since Ether is replaced by W-ETH, both WETH and other tokens follow the same ERC20 implementation, this allows for a much simpler contract execution and a better user experience


##  Rounding error check process

When matched orders are exrcuted, the trade amount by taker can be less than the buy token amount defined by maker. In this
case the order will be partially filled & the partial sell token amount to transfer to maker is calculated using
'getPartialAmount' function. 'getPartialAmount' involves division process & the actual value could be a fractional.
Unfortunaltely solidity does not support floating point type yet & hence the value would get floored/rounded out. This floored
value would be returned by 'getPartialAmount' function.

In order to prevent the maker from the loss caused due to the flooring of actual value, rounding error check is
implemented. The 'isRoundingError' functions checks wether the percentage rounding error exceeds 0.1%, if yes then the function returns 'true' else it returns 'false'.

Formula to calculate % error is:
```bash
% Error = [|(Actual value - Accepted value)|/|Accepted value|] * 100
```

Here,

Actual value = floor((tradeAmount * tokenSellAmount)/tokenBuyAmount)
Accepted value = (tradeAmount * tokenSellAmount)/tokenBuyAmount

But before we substitute the equatons of 'Actual value' & 'Accepted value', We have :
```bash
((a*b/c) - floor(a*b/c)) / (a*b/c) = ((a*b)%c)/(a*b)
```

So the % Error formula above can be simplied to :
```bash
% Error = [((tradeAmount * tokenSellAmount) % tokenBuyAmount) / (tradeAmount * tokenSellAmount)] * 100
```
where (tradeAmount * tokenSellAmount) % tokenBuyAmount = R = the remainder of calculation. This simplifies the equation to :
```bash
% Error = [R / (tradeAmount * tokenSellAmount)] * 100
```


Now the 'isRoundingError' functions checks wether the percentage rounding error exceeds 0.1%. So,
```javascript
R / (tradeAmount * tokenSellAmount) * 100 > 0.1
R / (tradeAmount * tokenSellAmount) > 0.001
```

Division in Solidity can only return an integer, so multiplying each side by 1000 yields:
```javascript
1000 * R / (tradeAmount * tokenSellAmount) > 1
```
Integer division is still unable to detect 3 decimal places (for 0.001) so multiply by 1000 again to get 3 decimal places:
```javascript
1,000,000 * R /(tradeAmount * tokenSellAmount) > 1000
```
If the calculation is greater than 1000, this means there's a rounding error greater than 0.001 and the function returns true.
So the final implementaion becomes:

```javascript
function isRoundingError(uint numerator, uint denominator, uint target)
public
pure
returns (bool)
{
    uint remainder = mulmod(target, numerator, denominator);
    if (remainder == 0) return false;
    // No rounding error.

    uint errPercentageTimes1000000 = (remainder.mul(1000000)).div(numerator.mul(target));
    return errPercentageTimes1000000 > 1000;
}
```

The concept is inspired by 0x.


### Contribution

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



