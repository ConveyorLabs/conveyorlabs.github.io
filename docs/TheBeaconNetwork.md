---
label: The Beacon Network
icon: broadcast
---
# The Beacon Network

## Overview

Conveyor Finance is innovating DeFi automation in that the on-chain functions that execute users' automated swaps are publicly callable by anyone. There are no owner-only functions or allow-listed requirements in order to execute swaps for others. With this approach, a great deal of security and verifiability is required to prevent bad actors from abusing the system, which Conveyor Finance has gone to a great extent to prevent.

The Beacon is a decentralized, open-source, and highly optimized program that acts as the backbone of the Conveyor ecosystem. Each Beacon in the network monitors on-chain state changes in Conveyor contracts, receiving a reward for each transaction that it successfully completes. The Conveyor ecosystem is fully permissionless, meaning that Beacons compete amongst each other to execute transactions faster than their peers, which ensures that orders are guaranteed and executed as fast as possible. Through the Beacon network, Conveyor is able to enable trustless, fully decentralized contract automation for decentralized finance.

The Beacon interacts with the public functions of our protocols in order to execute automated swap transactions for our users. Beacon operators (those who execute swap transactions for our users) are able to listen to the public swap conditions that users of Conveyor Finance have stored on-chain, and interact with the functions of the smart contract to initiate the swap of users' assets when specific, verifiable conditions are met.

## How it works
The Beacon works by listening to on-chain state changes in the Conveyor Limit Order contract to keep track of all existing limit orders. Simultaneously, the Beacon listens to corresponding liquidity pools to keep track of price changes and know when to execute a limit order. All order execution is verified on-chain, meaning that a limit order will only execute when the price, order quantity, and all other necessary conditions are met on chain (conditions for order execution are discussed in detail in the Limit Orders section of the whitepaper). Beacons listen for events emitted from the Limit Order contract to update their local state whenever any order is placed, updated, canceled, or executed. When an active limit order hits its execution price, Beacons race to call the contract, with a reward going to the Beacon that successfully completes the transaction. This creates a financial incentive to support the Conveyor system, which ensures competition throughout the Beacon network. This competition creates a strong Beacon network, ensuring that limit orders are executed as fast as possible.

## Accessibility
The Beacon is fully open-sourced, compact, and extremely easy to install. Accessibility strengthens decentralization, and we wanted the Beacon to be accessible to anyone. While some protocols that offer automation either close source their off-chain logic or centralize their protocol so that only trusted nodes can call their contract, we decided to design the system in a way that the more Beacons there are, the stronger the network becomes.
Downloading the Beacon is as easy as one command in the terminal and setup takes less than 5 minutes.

The Beacon is built in GoLang and is compatible with almost any operating system/architecture. Additionally, you can supply any RPC endpoint to the Beacon. One point to note is that the Beacon network is competitive, so having a computer with higher performance as well as a local node will give a Beacon operator the greatest advantage.

Running a Beacon 
Instructions on how to download/run a Beacon are coming soon.