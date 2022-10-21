---
label: Risk Mitigation
icon: shield-check
---

# Risk Mititgation
Several implementations and design decisions were made to create a system that benefits users of the protocols, as well as those who host an off-chain Beacon application. 

## Flash Loan Attacks
Flash loan attacks on our user base pose a significant threat to the protocol by bad actors in the beacon network. Traditional DeFi automation networks such as The Gelato Network have an internal executor network to call the execution functions of the on-chain logic. The Conveyor beacon network is completely democratized, allowing anyone to spin up a beacon client and start executing incoming orders in the queue. The reason why most other well-known DeFi automation protocols keep their executor network internal is because of the risk of bad actors in the beacon network. Since the beacon network will be reaping the rewards of execution on the transactions in the queue, it is possible for a beacon to manipulate the spot price in a liquidity pool to drive the price to the execution trigger for a batch of transactions to execute, in such cases where the execution reward is significantly large. 

The Conveyor protocol has deeply analyzed this problem, and mathematically formulated a model on when such an attack will be profitable. Our protocol's beacon reward is analytically determined with flash loan attacks in mind and is hard-capped at the exact value that would make such an attack profitable. Meaning, a beacon could take out a flash loan to manipulate the price of an lp to the execution trigger for a batch of transactions and at best will be breaking even - the gas accumulated in the transaction. Alternatively, the beacon could simply play by the rules and reap the max execution reward getting a net-positive return. 

The overall philosophy of the Conveyor protocol has been to mechanistically design the system in such a way that the most profitable way to use the platform is also the correct way to use the platform, and flash loan attack prevention has been mechanistically designed into the system to be unprofitable so attempting such attacks will invariably lose money for bad actors in the network. 

## Contract Security
The conveyor contract has been designed with security as the top priority. Our engineers have designed a custom EOA library to further strengthen the security of our execution function from external contract calls. This library protects against sophisticated ghost contract calls which several modern protocols which only check the code size of the caller are still vulnerable to. 
The Conveyor contract will be audited by QuantStamp prior to our protocol launch to ensure a deep level of security and professional oversight to our userbase.

Although we have made deep mechanistic design decisions to prevent flash loan attacks on the liquidity pools to trigger order executions. We further implement price oracles as secondary price indicators alongside the liquidity pool spot prices based on reserve size. This is to have redundancy in our execution price triggers in order to prevent execution on a manipulated spot price in a liquidity pool. For example, if a user on our platform places a limit sell order and a bad actor takes out a flash loan in the respective lp to depreciate the spot price to the execution trigger. Our contract will not execute as the oracle price will conflict with the liquidity pool price. So, our userbase can feel safer when trading in volatile liquidity pools that their orders will not execute on atomic manipulated price data in the liquidity pools. 