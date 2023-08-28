---
icon: Tag
label: Fee Structure
---

# Fee Structure

Conveyor charges a fee based on how much gas we can save you when executing the swap. A fee of 50% of the gas savings is applied only when when `gas saved > 0`.

For example, if a swap of ETH > USDC costs $20 through 1inch on Ethereum Mainnet and Conveyor provides the exact same swap outcome for only $10, a $5 worth of Ether fee is applied. This essentially provides a 25% gas discount to the trader for the same swap compared to 1inch.

```
1inch Swap: $20 in gas (Ether)
Conveyor Swap: $10 in gas (Ether)

Gas saving delta: $10 in Ether
Protocol fee: 50% ($5 in ether)
```

The fee is paid via the `msg.value` of the submitted transaction data for Token <> Token swaps, and Token > ETH swaps. For Eth > token swaps, the protocol fee is added to the `msg.value` of the transaction amount, so a swap of 1 ETH > USDC with a .00015 protocol fee would have a `msg.value` of 1.00015 sent from the users wallet to execute the swap and pay the protocol fee.

## How gas is calculated

Both 1inch and Paraswap, our aggregator partners, return a gas estimation in their API response, along with the route data. After decoding their routes and reconstructing the swap calldata, we estimate gas on our end as well. This accuracy is further enhanced once you have given the `ConveyorRouterV1` token approval for the quantity you want to swap, and is within +-200 gas (0.08% accuracy deviance) from the actual transaction once submitted.
