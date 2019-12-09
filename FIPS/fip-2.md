# FIP 002: Delegation rewards

## Motivation
In order to get validation rights, one should stake 3M FUSE tokens. Those tokens can be directly staked by the validator or delegated to him by a delegator. Up until now, the validator needed to actively pay rewards to its delegators. Although the delegator stake can’t be taken away by the validator, the reward can.

## Suggestion
The consensus contracts will split the block reward between the validator and his delegators proportionally minus a predefined fee that will go to the validator. In this case, the validator can choose its own fee, and update it from time to time, and the delegators can choose between validators for the best fees and availability.

## Examples
Validator 1 has 1M FUSE tokens staked, and 2M FUSE delegated between 2 delegators, 0.5M and 1.5M.
Let's assume the block reward is 3 FUSE tokens.

If the fee is 0% (meaning that the validator is taking no fee) then the rewards will be split in this manner:
1 FUSE for the validator.
2 FUSE for the delegators, 0.5 FUSE for one and 1.5 FUSE for the second.

If the fee is 5%, then the validator will receive 5% more from each delegator:
1.1 FUSE for the validator.
1.9 FUSE for the delegators, 0.475 FUSE for one and 1.425 FUSE for the second.

If the fee is 100% (meaning that the validator will take all of the reward) then all of the reward will go to the validator (as it is today):
3 FUSE for the validator.
0 FUSE for the delegators.

## Future work
* Now we can make a simple UI that rank the validators based on several attributes such as fee and availability.
* We can make an easy delegation UI to a chosen validator from that list.

## Exceptions and things to keep in mind
* All of the calculation should be taken from the relevant snapshot and not from the current state to avoid exploits.
* Since staking limits are exactly 3M and there is no proportional reward from overtaking (yet), over staking and delegating should be omitted. Meaning that if someone is staking 3M already, no one can delegate to him any more to avoid splitting the block reward with him.

