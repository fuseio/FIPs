# FIP8: Adjusting Block Rewards to Validator's Stake

## Problem

The current consensus mechanism has one important constraint:

Validators cannot stake more than a minimum value of 100K Fuse. **Virtually, both the minimum stake and maximum stake are 100K**. While even if they could stake more, this wouldn't increase their reward.

This introduces a number of disadvantages:

*   Validators have to own multiple validator accounts and run multiple nodes to stake more than the maximum amount for account.
*   While the delegation mechanism is already enabled the maximum stake of 100K makes it a inefficient. Validators have no incentive to open delegation.

## Goals

The goals of this FIP are: 

(a) Allowing for validators to stake more than the mimimum stake amount

(b) introducing a new block reward function, so validators will receive block rewards in accordance to their stake.

## Block reward function

### Why is a new block reward function required?

By [Aura consensus](https://openethereum.github.io/wiki/Aura) which we are using, all validators get to validate the same amount of blocks, and receive the same block reward. So all validators, without considering their stake expected to receive the same rewards. The block reward function is needed to take validator’s stake into account, so the validators would lower the number of the nodes they are running.

Another important property of Aura consensus, is that the time is partitioned into time slots that are divided between the validators, when each validator is allowed to validate only his time slots.

### What happens when the validator misses blocks?

When the validator misses his time slot to validate the block, no block is generated and hence no block reward is sent. Then the new time slots starts and the next validator is allowed to create his block in the new time slot. Meaning that, missing blocks not affect the reward of other validators, and reducing the inflation because less block rewards are generated.


### Block reward formula 
The new block reward formula should have the following properties:


1. Do not change the inflation of the token over time.
2. Splitting or combining the validator’s stake to multiple accounts should not affect the validator's reward over time. Meaning that a validator with the total stake of 1M can run one machine with 1M staked, two machines with 500K on each, or 10 machines with 100K. All these options should give to the validator the same block reward.

I suggest the following formula for Ra, the block reward of validator A:

_Ra = (Sa / S)  * n * R_

_Where:_


    _Sa - the stake of validator A_


    _S - total stake of top 100 validators_


    _n - number of validators_


    _R - current reward with adjustment to the inflation planed_

Meaning that the new block reward will be proportional to the ratio of validator’s stake. We will see in a while why multiplying by the number of validators is needed.

### Examples

Let’s look at a simple example of two validators A and B, with the stake of 100K and1M.

We are going to inspect a certain amount of time that ‘s sufficient to validate 100 blocks. With the current block time of time seconds it is 500s, but every time period can be used.

**Without the proposal**


<table>
  <tr>
   <td>Validator
   </td>
   <td>Stake
   </td>
   <td># of nodes
   </td>
   <td>Blocks validated
   </td>
   <td>Reward per block
   </td>
   <td>Validator’s reward
   </td>
  </tr>
  <tr>
   <td>A
   </td>
   <td>100K
   </td>
   <td>1
   </td>
   <td>10 (=Ba)
   </td>
   <td>R
   </td>
   <td>10R (=Va)
   </td>
  </tr>
  <tr>
   <td>B
   </td>
   <td>900K
   </td>
   <td>9
   </td>
   <td>90 (=Bb)
   </td>
   <td>R
   </td>
   <td>90R (=Rb)
   </td>
  </tr>
  <tr>
   <td>Total
   </td>
   <td>1M
   </td>
   <td>10
   </td>
   <td>100
   </td>
   <td>X (Average)
   </td>
   <td>100R
   </td>
  </tr>
</table>


Va = 100 / 

Validator reward is number of blocks A validated multiply the block reward

Va = Ba * R = 10R

vb = Bb * R = 90R

**With the proposal**

Validator B can combine his 9 accounts into one


<table>
  <tr>
   <td>Validator
   </td>
   <td>Stake
   </td>
   <td># of nodes
   </td>
   <td>Block validated
   </td>
   <td>Reward per block
   </td>
   <td>Validator’s reward
   </td>
  </tr>
  <tr>
   <td>A
   </td>
   <td>100K
   </td>
   <td>1
   </td>
   <td>50
   </td>
   <td>0.2R (=Ra)
   </td>
   <td>10R (=Va)
   </td>
  </tr>
  <tr>
   <td>B
   </td>
   <td>900K
   </td>
   <td>1
   </td>
   <td>50
   </td>
   <td>1.8R (=Rb)
   </td>
   <td>90R (=Vb)
   </td>
  </tr>
  <tr>
   <td>Total
   </td>
   <td>1M
   </td>
   <td>10
   </td>
   <td>100
   </td>
   <td>R
   </td>
   <td>100R
   </td>
  </tr>
</table>


Using the new formula, the reward should be:

_Ra = (Sa / S)  * n * R_

Ra = (100k / 1rM) * 2 * R=  0.2R

Rb = (900k / 1M) * 2 * R=  1.8R

Va = 50 * 0.2R = 10R ✅

Vb = 50 * 1.8R = 90R ✅

_We can see that the Validator reward over time didn’t change, fulfilling (1) property of the formula._

Now, Assuming that B wants to split his stake to 3 nodes


<table>
  <tr>
   <td>Validator
   </td>
   <td>Stake
   </td>
   <td># of nodes
   </td>
   <td>Block validated
   </td>
   <td>Reward per block
   </td>
   <td>Validator’s reward
   </td>
  </tr>
  <tr>
   <td>A
   </td>
   <td>100K
   </td>
   <td>1
   </td>
   <td>25
   </td>
   <td>0.2R (=Ra)
   </td>
   <td>10R (=Va)
   </td>
  </tr>
  <tr>
   <td>B1
   </td>
   <td>500K
   </td>
   <td>1
   </td>
   <td>25
   </td>
   <td>2R (=Rb1)
   </td>
   <td>50R (=Vb1)
   </td>
  </tr>
  <tr>
   <td>B2
   </td>
   <td>200K
   </td>
   <td>1
   </td>
   <td>25
   </td>
   <td>0.8R (=Rb2)
   </td>
   <td>20R (=Vb2)
   </td>
  </tr>
  <tr>
   <td>B3
   </td>
   <td>200K
   </td>
   <td>1
   </td>
   <td>25
   </td>
   <td>0.8R  (=Rb3)
   </td>
   <td>20R (=Vb3)
   </td>
  </tr>
  <tr>
   <td>Total
   </td>
   <td>1M
   </td>
   <td>10
   </td>
   <td>100
   </td>
   <td>R
   </td>
   <td>100X
   </td>
  </tr>
</table>


Ra = (100k / 1M) * 4 * R=  0.4R

Rb1 = (500k / 1M) * 4 * R=  2R

Rb2 = (200k / 1M) * 4 * R=  0.8R

Rb3 = (200k / 1M) * 4 * R=  0.8R

Va = 25 * 0.4R = 10R ✅

Vb1 = 25 * 2R = 50R

Vb2 = 25 * 0.8R = 20R

Vb3 = 25 * 0.8R = 20R

Vb = Vb1 + Vb2 + Vb3 = 90R ✅

_We can see that splitting the funds for Validator B, didn’t change his reward over time, fulfilling (2) property of the formula_


### Calculating the validator stake and total stake
Validator's stake and the total stake for the formula should be calculated on start of every cycle, locking the validator block rewards per cycle. Otherwise the validators could manipulate the block reward by transfering and staking Fuse tokens between the multiple validator accounts.


### Math Proof

*Work in Progress* 🏗👷‍♂️


## Futher Development

FIP8 introduces principal changes to the consesnsus mechanism, therefore it got implications to other parts of the network as well. We would like to list the implications here, and we expect to solve or address them in the following FIP's.

### Governance

Governance is an integral part of the network. **With the current voting mechanism**, the validator’s votes are equal as all validator accounts have the same amount of tokens staked.

We propose that the weight of validator’s vote, going to be proportional to the amount of tokens staked. Getting back to example 1: validator A with 100K fuse is going to have 1 vote, while validator B with 900K will have 9 votes.


### Locking period for staking

It makes sense to disallow for the current validators to withdraw their stake. Allowing them to do so only when they leave the validator set or performing both of the actions together.

### Minimum delegation fee

We are also considering to introduce the minimum delegation fee for validators. So big players couldn't take control over the network by introducing zero fees.


## Pros and Cons

### Pros
-   One validator account and one node per validator
-   Delegation becomes a simple and open market. Giving revenues both to validators and the delegators as well

### Cons

- While the block reward is generated per stake, the validation fees are per block. Meaning that splitting validator accounts, the validator will receive more fees.
- When all validators have the same amount of blocks to validate, malfunctioning of small validators have the same implications as of the large ones. That got certain implications to stability of the network, opening new attack vector. Increasing the minimum stake amount 
