# FIP13: Gas Price Increase

## Problem

Currently transactions on fuse cost 1gwei (resulting in a fee of just 0.000021 FUSE per native transaction). This isn't sustainable and leads to easy spamming/ chain DDOS which can resulting in consensus drops.

## Goals

Increase the minimum fee to 10gwei thus increasing transaction fees 10 fold. Thus mitigating the risk of spamming and bloating.

## Suggestion

Increase the minimum fee to 10gwei for validators.

## Things to consider

* Hardcoded gas prices inside Dapps may result in stuck transactions for users.
* Stuck transactions for user who are used to entering 1gwei.

## Mitigations to the above

To ensure that transactions are never stuck the fuse owned validator will continue to accept transactions below 10gwei. This will result in any tranctions below the new minimum to take upwards of NUM_VALIDATORS*5seconds

Make sure that all users and developers are aware of the changes.