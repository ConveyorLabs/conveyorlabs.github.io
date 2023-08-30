---
icon: gift
label: Referral Program
---

# Introduction

Conveyor offers a referral reward to those who refer others to perform swaps using Conveyor. This referral reward is sent to the referrer when the referred wallet performs any swap through the Conveyor ecosystem. The reward is sent atomically at the time of the swap, and there is no need to collect your rewards via a dashboard or withdraw function

## How it works

Any wallet can become a referrer by signing a transaction that stores their wallet address to the [ConveyorRouterV1](/smartcontracts/). This can be done simply through the Conveyor [Trading App](https://app.conveyor.finance). The motivation for this method is to save on gas fees when performing the transaction where a referrer qualifies to receive rewards.

In order to receive rewards, you must write your address to each chain that you'd like to receive rewards for. When a referred user perform a swaps on each chain, the Conveyor website checks to see if the referred user visits the site using a referrer URL (eg. `https://app.conveyor.finance/?ref=9393e`). It then looks up any addresses of the referrer user based on the `?ref=` value. and passes the `referralID` to the Conveyor API if the user performing the swap is on a chain that the referrer has stored their address in the ConveyorRouterV1 address

Once a referred user connects their wallet to the website using your referral URL, their wallet is stored to our database as a referred user to you, and they no longer need to access the site via your referral URL in order for you to continue to receive rewards for each swap they perform.

<center>
<img src="/assets/png/ReferralModal.png" width="500" />
</center>

### Buttons

**Earn Rewards** - This button means your wallet provider is on the same network that correlates with the Chain Name. Pressing this button brings up a transaction for you to sign that stores your wallet address to that chain so you can begin receiving rewards

**Earning Rewards** - This button means that you are currently receiving rewards for that chain, and that any referred users that perform a swap using your referral URL on that chain will grant you rewards.

**Switch Network** - This button means your wallet provider is not on the same network that correlates with the Chain Name, and that you aren't currently receiving rewards on that chain. Pressing this button brings requests your wallet provider to switch network. After switching, the button will change to **Earn Rewards**
<br/>
<br/>

### Referral Link

Once you write your wallet address to at least one chain, you will be issued a referral reward with a referral reference ID. This URL is linked to the wallet you used when writing to the first chain and will stay the same through out the lifecycle of the wallet. This referral identifier cannot be changed, as it is derrived from a hash of your wallet.

!!!
In order for you to receive credit as the referrer of a referred user, they must connect their wallet in the same session as visiting your referral URL. If they revisit the site without your referral ID before first connecting their wallet, they will need to revisit the site using your referral URL again in order for you to receive credit as their referrer.
!!!
