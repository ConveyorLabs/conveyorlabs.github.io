# Welcome to Conveyor!

## What is Conveyor?

Conveyor is a Decentralized Exchange (DEX) aggregator for EVM chains that leverages existing Aggregator API's and then further optimizes the routes in order to be cheaper on gas.

## Who made it?

Conveyor Labs is the for-profit organization that is responsible for the deployment, maintenance, upgrading, and migration of Smart Contracts within the Conveyor Finance ecosystem. Conveyor Labs is owned and operated by DEXbot, a registered LLC in the state of Wyoming, USA.

## Concept Overview

When queueing a swap through the Conveyor platform, Conveyor pings other partner aggregators (currently 1inch and Paraswap) to find the best route. It then sorts each route by which provides the most favorable output (output value - gas), and calculates the difference in gas saved. The results is a swap that leverages the same routes, LP interactions, and splits as 1inch and Paraswap, with the added benefit of being upwards of 50% cheaper than executing the swap through them directly.
