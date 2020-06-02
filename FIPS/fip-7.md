# FIP7: Tiny Ethereum Network Transactions

## Motivation

Every cycle, one validator need to “update” the Ethereum Network about two things:
1. The new validator set.
2. The newly minted tokens amount.

Currently, it happens in two relatively expensive transactions.
The costs of these two transactions can take a toll on the validators and might be a good motivation for them not to transmit it at all. 
The high gas usage that makes these transactions expensive are made for several reasons:
1. There are two transactions that can be unified into one.
2. When updating the validator set, the whole set each cycle, even when the set is almost identical to the previous one.
3. A majority of the validators signatures are being checked on-chain which is a super expensive operation that grows linear in the number of validators.

In this FIP we will suggest a way to reduce the gas usage of the transactions to a fraction of it.

## Suggestion

First of all, we should merge the two transactions (new tokens amount, and new validator set) into one transaction.
Secondly, we will notify the network about the new validator set using deltas (newly added validators and removed validators) which can, in most cases be much less data to update.
Finally, we won’t send proof to be validated on-chain.
The last part will dramatically reduce the cost of the transaction but will create a major security breach.
To bridge that gap, we change the mechanism of Ethereum network updating from provable signatures to a delayed commit-challenge game between the validators in the following way:
1. A validator sends a transaction with the new token amounts, the new validator set deltas and the Fuse network txid that transmitted the signatures. (Fuse Network will still validate all the signatures).
2. The transaction also locks some amount of ETH (it can be 1 ETH for now).
3. Each validator oracle checks the validity of this transaction off-chain.
4. If the transaction is invalid, each validator can send the Ethereum Network contact that this transaction is bogus and send the valid delta and new token amount.
5. At the end of the cycle, if less than half of the validator marks this transaction as bogus (with the same deltas and new amount), the set is updated, the new amount is minted and the committed ETH will be sent back to the validator.
6. If more than half of the validators mark it as bogus (with the same delta and new token amount), the new delta and new amount set by the other validators will be updated and the ETH will be split among them.

This means that the validator set update is delayed and needs to be delayed in Fuse also. A new validator will start validating in the next-next cycle of the time he started staking, which can be 4 to 6 days in the current 2-days cycle time.

## Cost analyses

An honest validator will send a cheap transaction that only sets the deltas and new amount of tokens along with the fuse transaction id with the signatures. No signature checks should accrue on-chain. The transaction fee should be reduced by about 80-90%.
The validator should reserve some ETH to commit to this end which will return to him in the next cycle.
If a non-honest validator will try to manipulate the new validator set, all of the other validator’s oracles will transmit another transaction that also cost much less than the original one and will be paid by the ETH committed by the rouge validator.
