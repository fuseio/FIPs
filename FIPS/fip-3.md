# FIP 003: Lowering the staking requirements.

## Motivation
We want to give the validator a way to actually benefit from having a lot of delegators. But for this to happen FIP4 needs to be implemented and it can take some time. FIP2 is just around the corner and it will reduce the massive rewards that validators are getting by splitting them with its delegators. So in the meantime, we want to lower the minimum staking requirements to 100K from 3M so validators won’t need delegations to stake and validate.

## Suggestion
As mentioned already, lowering the staking requirements to 100K FUSE tokens exactly (no more no less). It can still be split between validator its delegates.
To do so, we need to strictly limit the number of validators to 100, and to choose randomly each cycle up to 100 validators from the eligible validators. Also, we need to keep in mind that if someone has more than 200K FUSE tokens and he wants to validate more, he needs to set up two or more validator machines by himself with a different address. This will be solved in FIP4.

## Examples
* If someone has 100K FUSE tokens he can start to self validate by setting up a node and delegate to himself.
* If someone has 350K FUSE tokens he can setup 3 nodes with 3 different accounts and delegate each of them 100K FUSE tokens. If he will get 50K FUSE tokens more, he can start a fourth validator node.
* If there are 20 valid stakers (each has a different account and exactly 100K FUSE tokens stake delegated to him), then the validator set should be compounded of the 20 of them for the next cycle.
* If there are 120 valid stakers, then each cycle 100 validators will randomly be chosen between that list to be the current cycle validator set. This means that each 100K stake has about 83.33% chance to validate each cycle.
