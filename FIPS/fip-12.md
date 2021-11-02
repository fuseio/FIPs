# FIP12: Block Size Increase

## Problem

During points of heavy network load we are saturating the current 10million gas block.

## Goals

Increase the block size to 15million this will allow a 50% uplift in the number of transactions which can fit in a single block, thus increasing overall TPS by 50%

## Suggestion

This FIP will not require a fork or contract changes, just a small update to the client software run by validators.

The spec can will be updated to adjust "gasLimit" to 0x5F5E100 (15million). Each block minned by a validator running the new client will raise the gas limit for that block by desiredBlockSize/gasLimitBoundDivisor (15000000/1024 ~= 14650) until the 15million limit is reached. 

## Things to consider

• If nodes fail to update there client then the block size will "ping-pong" by +14650 and -9765

• Increased overhead on validators - FIP11 (Jailing) should catch any downed validators who fail to keep up

• Faster growing chain size (more storage foot print)