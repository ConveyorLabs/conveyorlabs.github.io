---
label: Stoplosses
---

## Stoploss Orders

Conveyor Finance offers fully decentralized and non-custodial stoploss orders. If you are unfamiliar with how stoploss works, you can [follow this link to learn more](https://www.investopedia.com/terms/s/stop-lossorder.asp). 

Unlike centralized exchanges where stoplosses execute on a centralized orderbook, stoplosses through Conveyor are filled at market price across any of the available decentralized exchanges (DEXs) that Conveyor supports. 


The DEXs supported for stops on each chain all have at least 500M in liquidity and are reputable exchanges that have stood the test of time (UniswapV2, UniswapV3, Sushiswap, etc). To find a full list of the DEXs that are supported for stop losses for any chain Conveyor is deployed to, [click here]().


### How Stoploss Orders Work

[Off-chain executors]() are monitoring market conditions 24/7, ready to execute a limit order when any DEX meets the execution price. When an order reaches market price, an executor will call the `executeLimitOrders` function and pass in the `orderId` for the order. From there, the contract trustlessly verifies that the market conditions meet the order's execution price and fills the order on the most advantageous liquidity pool at the time of execution. The amount out less fees is then sent directly to the order owner's wallet. The result is a fully decentralized and trustless stoploss execution, without having to ever worry about the tokens leaving your wallet. Since all liquidity pool prices are verified on-chain, the gas cost of stoplosses is higher than a typical swap on a DEX.


### How to Place Stoploss Orders
To place a stoploss order, head over to the [stoploss interface](https://beta.conveyor.finance/#/stop?chain=polygon) and select the `tokenIn` & `tokenOut` from the drop down selector. Then, enter the `quantity` of tokens you would like to trade and the `stop price`, signifying the price that the stoploss should trigger at. If this is your first time placing an order on the `tokenIn` with Conveyor, you will be asked to give the `LimitOrderExecutor` approval. This will only be required once. 
The execution credit amount of the order will autofill a reccomendation. Although there is no required amount, a sufficient execution credit will ensure fufillment at execution time as this is used to compensate the off-chain executor the gas spent on the traders behalf. To compensate the gas spent by the off-chain executors when filling your order, [execution credits]() are should be deposited. Simply put, execution credits is ETH attached to your order and paid to the executor upon fufillment. The front end will recommend an amount of execution credits for your order based on the current gas prices across the network. However, you are able to deposit more or less than the recommended amount. **Note that if your order does not have enough credits to cover the gas cost of execution, off-chain executors might not fill the order when expected. It is highly recommended that users do not use less than the front end's recommendation.**

<img src="https://i.imgur.com/Xia3k36.png" width="400"/>

In addition to the previously mentioned order settings, you can also set an order expiration timestamp, order slippage and an optional token tax. You are able to set any expiration timestamp for your order, however if no value is supplied, the expiration time will default to 30 days. Note that if you set an expiration time over 30 days, there will be a small fee (0.002 ETH) to refresh your order on every 30 day interval. This fee will be decremented from the execution credits deposited with the order so make sure to account for this when placing orders with extended expiration.

<img src="https://i.imgur.com/Ht5yKvv.png" width="400"/>

### How To Update/Cancel an Order
After placing a stoploss order, you have the ability to modify the `price`, `amountIn`, and `executionCredits` of the order as well as the ability to cancel the order, removing it from the contract. To cancel or update an order, navigate to the `Orders` tab and select the dropdown on the order you wish to modify as seen below. 

<img src="https://i.imgur.com/yDDDuld.png" width="400"/>

### Viewing Current and Completed Orders

To view any active or filled orders, head over to the `Orders` section in the navigation bar. All active orders will be listed under the `Current Orders` tab, while all completed orders will be listed under the `Completed Orders` tab.  If an order has been successfully filled there will be a green `Filled` label listed as the order's status.

<img src="https://i.imgur.com/2SaXzsq.png" width="400"/>