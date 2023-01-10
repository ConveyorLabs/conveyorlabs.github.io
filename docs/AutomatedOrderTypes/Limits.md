---
label: Limits
---

## Sandbox Limit Orders


Sandbox Limit Orders are a new type of order introduced by Conveyor, allowing a limit orders to be filled faster, more profitably and without any restrictions on how the order is filled. Unlike stoplosses, Sandbox limit orders have complete freedom in how they can be filled, as long as the exact market price specified by the order is met. During sandbox order execution, the contract optimistically transfers the order's `amountIn`, allowing the exectuor to utilize the liquidity in any way to fill the order. 


### How Sandbox Limit Orders Work

To demonstrate how Sandbox limit orders work, lets say that there is a Sandbox order to swap `1 WETH` for `1500 USDC`. Let's walk through a few ways that this order can fill. 

Since sandbox orders are not bound by any DEX, if there is a newly launched DEX with an LP that meets the exchange rate of the order, the offchain executor can route this order through this pool.

Another way that this order can fill is through peer to peer (p2p) transfers. Lets say that there is another sandbox order waiting to execute, looking to swap `1500 USDC` for `1 WETH`. The offchain-executor could use the first order to fill the second order, initiating a p2p swap between the two users.


There are also some more exotic ways to fill orders. 

Lets say that there is another limit order protocol on the blockchain and they have an active limit order to swap `1500 USDC` for `1 WETH`. Since the sandbox order's `amountIn` is transferred optimisitcally, the offchain-executor could create a new limit order on the other protocol, fill the limit order, receiving `1500 USDC` and fill the sandbox limit order with the profit, all in one atomic transaction.


Due to the fact that Sandbox orders have no restrictions on how they can fill, orders have the possibility to fill faster and more profitably than current market conditions. Offchain-executors can monitor the chain to close MEV opportunities, and use that profit to fill orders. 

Lets use the original example order above, swapping`1 WETH` for `1500 USDC` to demonstrate this. If current market conditions show that the best exchange rate is `1495 USDC` per `1 WETH`, but there is an arbitrage opportuntity that would change the price to `1500 USDC` per `1 WETH`, the off-chain executor could use the order's `amountIn` (and any other means of liquidity) to close the arbitrage opportunity and fill the sandbox order, all in one atomic transaction. While other traders see the price to be `1495 USDC` per `1 WETH`, the sandbox limit order filled at `1500 USDC` per `1 WETH`.

This opens a wide variety of exciting and synergistic possibilites for both traders and MEV searchers alike.

MEV searchers could monitor liquidations, arbitrage opportunties, and any other means of MEV to fill orders. This can also create a cascading affect, where closing an arbitrage opportunity allows the executor to fill a large order, which creates another arbitrage opportunity and so on until all opportunities are exhausted.

Within any of the simplistic or exotic exapmles above, if the full amount can not be filled, the off-chain executor can also partially fill an order as long as they are receiving the order's specified exchange rate for the amount they are paritially filling.

### How to Place Sandbox Limit Orders

Placing a sandbox limit order is very similar to placing a stoploss order. To place a sandbox order, head over to the [limit order interface](https://beta.conveyor.finance/#/limits/buy?chain=mainnet) and select the `tokenIn` & `tokenOut` from the drop down selector. Then, enter the `exactAmountIn` of tokens you would like to trade and the `exactAmountReceived`. The ratio of the `exactAmountIn/exactAmountReceived` defines the price that the sandbox order will fill at. If this is your first time placing an order on the `tokenIn` with Conveyor, you will be asked to give the `SandboxLimitOrderExecutor` approval. This will only be required once. 

Just like stoploss orders, the execution credit will autofill with a reccomendation based on the current gas price conditions. If you skipped the stoploss section or want to learn more about execution credits, you can [click here](). **Note that if your order does not have enough credits to cover the gas cost of execution, off-chain executors might not fill the order when expected. It is highly recommended that users do not use less than the front end's recommendation.**

<img src="https://i.imgur.com/lL3wF1Z.png" width="400"/>

In addition to the previously mentioned order settings, you can also set an order expiration timestamp, order slippage and an optional token tax. You are able to set any expiration timestamp for your order, however if no value is supplied, the expiration time will default to 30 days. Note that if you set an expiration time over 30 days, there will be a small fee (0.002 ETH) to refresh your order on every 30 day interval. This fee will be decremented from the execution credits deposited with the order so make sure to account for this when placing orders with extended expiration.

<img src="https://i.imgur.com/Z83oNEJ.png" width="400"/>


### How To Update/Cancel an Order

Just like stoploss orders, you can to modify the `price`, `amountIn`, and `executionCredits` of any active sandbox order. Additionally, you can cancel the order, removing it from the contract. To cancel or update an order, navigate to the `Orders` tab and select the dropdown on the order you wish to modify as seen below. 

<img src="https://i.imgur.com/zHepgUa.png" width="400"/>



### Viewing Current and Completed Orders

To view any active or filled orders, head over to the `Orders` section in the navigation bar. All active orders will be listed under the `Current Orders` tab, while all completed orders will be listed under the `Completed Orders` tab.  If an order has been successfully filled there will be a green `Filled` label listed as the order's status.

<img src="https://i.imgur.com/2SaXzsq.png" width="400"/>

Note that since sandbox orders can leverage MEV to fill orders, your order may execute at its specified price while a chart shows a lower price, however in this case, you are just filling at a better price than the chart shows! Your order will **always** fill at the specified execution price.

Similarly, since sandbox orders require that the order will receive the **exact amount out specified** for the amount in, it is possible that while a chart may say the execution price is met, the amount out received is actually less due to fees on the swap. The swap will execute once the exact amount out can be achieved.