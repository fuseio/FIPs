# FIP6: Bridge implementations should be changeable by voting

## Motivation:

The bridge is used for a number of important purposes - moving Fuse tokens between Ethereum and Fuse networks, as well as minting the rewards distributed on Fuse to validators on Ethereum.
Currently, the bridge implementations can be updated by the MasterOfCeremony address (which deployed the contracts initially).
In order for the network to be more decentralized, this option should be made available by validators’ voting.

## Suggestion:

New `ContractTypes` will be added to the `Voting` contract on the Fuse network - `HomeBridge` and `ForeignBridge`.
New ballot should be open in order to update the bridge implementations on each of the networks.
After the ballot is closed, if accepted - the bridge oracle (run by all validators) will be responsible for collecting enough signatures and upgrading the implementation to the new suggested contract. This process is similar to the process of how the validator set is updated on Ethereum, and the rewards are minted (on each cycle end).

## Technical Spec:

* Add new `ContractTypes` to `ProxyStorage` contract - `HomeBridge` and `ForeignBridge`
* Update the relevant bridge contract address as `proxyOwner` instead of the MOC address on both networks
* When a vote is closed and accepted - `ProxyStorage` contract emits `UpgradeBridge(_contractType, _contractAddress)` event
* (New) watcher process on bridge oracle listens to `UpgradeBridge` events
  * Creates bridge upgrade message with new contract address and contract type to update
  * Calls `submitSignatureOfBridgeUpgrade` on `HomeBridge` with the message
* When enough signatures are collected on `HomeBridge`, it will emit `CollectedBridgeUpgradeSignatures` event
* (New) watcher on bridge oracle listens to `CollectedBridgeUpgradeSigantures` events
  * Calls `executeUpgradeBridgeSignatures` on Home/Foreign bridge contract which after verifying signatures calls `upgradeTo` on the `EternalStorageProxy` implementation of the bridge contract on the relevant network.
