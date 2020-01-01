# FIP 004: Multi validators

## Motivation:
In the current state, a validator needs exactly 3M tokens to be a validator. If he has less, he needs to find delegators to delegate to him, without him proving himself first as an ongoing reliable validator. If he has 3M tokens he doesn’t need (or even can add) more delegators, since there is a limit, and even if there was no limit, it will only dilute his share.

## Suggestion
First, we’re lowering the minimum staking requirement to 100K (see FIP-3)
Second, we are making the following changes:
Any validator can create its own “validator account” in the consensus contract.
The validator needs to set a list of different addresses that he controls or simply a xpubkey if it is possible.
The validator can stake using this contract by sending FUSE tokens to this contract. Delegators can send their delegations using this contract as their validator also.
Now every 100K FUSE tokens that a validator have staked is a Ticket.
A validator can have several tickets.
The consensus contract will fill exactly 100 validators (randomly) for the next cycle using the validators’ tickets.
The more tickets one has, the more slots he will get in the 100 length validator set.
The only problem is that the validator set can’t have duplicate addresses. So the consensus will take several addresses from the address set (or derive it from the xpubkey) of the validator contract.
The validator needs to be able to upload several validators as the amount of addresses he has that entered the validator set.

## Example and Illustrations to come
