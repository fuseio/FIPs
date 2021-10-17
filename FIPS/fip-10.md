# FIP10: Unbounding period/ Delegation Lockin

## Problem

Currently a delegator can instantly remove their delegation immediately after delegating. This exposes a potential exploit.

## Goals

The goal of this FIP is to secure the network by implementing a "lock-in" period on delegations 

The benefits of this are as follows:
1. Puts a stop to the exploit.
2. Helps increase scarcity by reducing circ supply, as tokens will be lock for a short time.

## Suggestion

Alter the withdraw method of consensus contract too be a multi-step process:

* On the first call the current block number is taken and an offset is added to it. The delegation will be removed and stored in a new pool which no longer receives interest.
 
* On the second call the consensus checks that the block has now passed the unbounding block stored in step 1. If so then the staked tokens are returned back to the user if not then the call will fail and the user will have to wait until the unbounding period has passed. 

Offset - The offset is the number of blocks that a delegation will be locked for the offset needs to be >=1 cycle of blocks in order to solve the exploit.

## Example

Let offset = 30000blocks

A delegate, requests a withdraw from Validator X at block 100000 the unbounding block will be 100000 + 30000 = block 130000:

* At block 115000 the user tries to withdraw part/all of his/hers stake - The withdraw will be reverted.
* At block 130001 the user tries to withdraw part/all of his/hers stake - The withdraw will be successful.
 