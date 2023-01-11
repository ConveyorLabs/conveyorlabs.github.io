---
icon: flame
label: Execution Credits
---

## Execution Credits

Off-chain executors are constantly monitoring on chain conditions in order to fill orders placed through Conveyor. When an order is able to execute, the off-chain executor calls the `executeOrders` function on one of the execution contracts, resulting in the order being filled. This requires the off-chain executor to incur gas cost for execution. To insure incentivization at all times for the off-chain executor to fill orders, `execution credits`, paid in `ETH` (or the native token of the network) are deposited with each order. This creates a constant incentive for executors to be competitive and fill orders as quickly as possible. Competition amongst the executor network constantly raises the bar towards faster and more profitable execution for traders.


When placing an order through the frontend, the recommended execution credit will autofill. You are able to deposit more or less than the recommended amount. **Note that if your order does not have enough credits to cover the gas cost of execution, off-chain executors might not fill the order when expected. It is highly recommended that users do not use less than the front end's recommendation.**

Upon successful execution, the execution credits for that order are sent to the off-chain executor. If a user chooses to cancel an order, all execution credits will be returned to the order owner.

