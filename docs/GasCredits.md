---
icon: flame
label: Gas Credits
---

# Gas Credits

## Overview
For a limit order to execute, the Beacon must call the fufillOrder() function on the Conveyor Limit Order contract. The Beacon spends the gas necessary to execute the order, but what happens if the cost to execute the order is higher than the profit from the limit order itself? This is where gas credits come in. Gas credits are an important concept to the Conveyor ecosystem, ensuring that Beacon runners are incentivized to execute limit orders as fast as possible. 

## What Are Gas Credits?
When a user first places an order, they are required to deposit gas credits to cover the cost the Beacon incurs when executing the order. Simply put, a user stores a small amount of ETH in the Limit Order contract to pay the Beacon for order execution. A user must add a minimum amount of gas credits, dependent on the current fair gas price at order placement to ensure order completion.   In the case where gas price increases and the user does not have enough gas credits to cover order execution, the Beacon would not have an incentive to fulfill the order, thus risking order cancellation. It is recommended that a user deposits enough gas credits to account for gas price fluctuation.
All gas credits are accounted for within the gasCredits mapping stored in the Limit Order contract, keeping track of how many gas credits a user has at any time. Users can add to their gas credit balance whenever they would like and can withdraw all gas credits as long as they have no pending orders. If a user has a pending order and wants to withdraw their credits, they must first cancel the order, and then they will be able to withdraw their full balance. 
It is recommended that a user deposits at least 3x the current gas credits requirement in order to sufficiently cover the potential for volatility in gas prices. With this is mind, a user should always over estimate the amount of gas credits you put in to ensure coverage. The conveyor front-end web interface will always keep track of the gas prices in real-time and notify the user if their current gas credit balance is below the threshold for execution, however the user must maintain their gas credit balance above the necessary threshold.

## Gas Credits Strengthen The Ecosystem
In order to maintain full decentralization and trustlessness within the Conveyor ecosystem, all values must be verified on-chain from execution price to gas price. This ensures that a Beacon runner can not input arbitrary values and keeps the system fully trustless. In order to execute this logic the Beacon must spend gas, incurring a cost to interact with the contract. In the event that the profit gained from order execution does not cover the cost of order execution, the Beacon would be losing money to interact with the contract. This is where gas credits come in. Gas credits ensure that the order execution is profitable for the Beacon, meaning that the Beacon is assured an incentive to call the contract to execute the order. Since there is an incentive to fulfill the order, Beacons will race and compete to execute the order which benefits the user by ensuring order fills as fast as possible.

Gas credits also help to reduce contract storage bloat, which makes reading and writing from the contract's storage more expensive. Since there is a minimum gas credit upon each order, this discourages contract spamming. Additionally, due to order cancelation upon a user's gas credits falling below the necessary threshold for Beacon execution, the contract storage is able to stay lean.

## How Is Gas Price Estimated?
When determining how many gas credits should be spent for order execution, gas price is verified on-chain to ensure a fair market price for execution.
Initially Conveyor will be using ChainLink's Fast Gas Oracle, which updates gas price whenever gas fluctuates 25% from its current price, or after 7200 seconds has passed since the previous update. 

While the initial gas oracle will be Chainlink, Conveyor will be developing a fully decentralized, trustless gas oracle with block to block precision, cheaper execution costs, and fees paid in Eth instead of a non-native token.