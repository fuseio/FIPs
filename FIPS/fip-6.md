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
* Deploy `HomeProxyStorageOwner` & `ForeignProxyStorageOwner` that will replace the MOC address as `proxyOwner` of the bridge contracts on both networks.
* When a vote is closed and accepted - `ProxyStorage` contract emits `UpgradeBridge(_contractType, _contractAddress)` event
* (New) watcher process on bridge oracle listens to `UpgradeBridge` events
  * `<Network>` is according to the `_contractType` of the event
  * Get `<Network>Bridge` current implementation version
  * Create bridge upgrade message with new version and implementation
  * Call `submitSignatureOfBridgeUpgrade` on `HomeBridge` with the message
* When enough signatures are collected on `HomeBridge`, it will emit `CollectedBridgeUpgradeSignatures` event
* (New) watcher on bridge oracle listens to `CollectedBridgeUpgradeSigantures` events
  * Calls `upgradeTo` on `<Network>ProxyStorageOwner` which will call `upgradeTo` on the `EternalStorageProxy` of the relevant network bridge

**Note #1:** `ForeignProxyStorageOwner` has the ability to call `setBridgeContract` on FuseToken, therefore will be added as minter, and thus we can remove the MOC as minter.

**Note #2:** `<Network>ProxyStorageOwner` contracts will be changeable by voting with this process as well
