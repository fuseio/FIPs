# FIP5: Validators Key Splitting

## Motivation:

In the current state, the validator key has an inherent security weakness since it is on a hot machine. To reduce the risk, the key should:
* Never leave the machine by importing it to another wallet to use in the voting DApp or to delegate, withdraw and transfer tokens.
* Not have access to the validator stake or rewards.
* Be able to be replaced and managed by another (possibly cold wallet) key.

## Suggestion:

We suggest to split the validator key into two keys. One that validates the blocks and stays on the hot validator machine and a second key which manages the validator, can vote in the governance process and have access to the stake and rewards.
We will call them the validation key and the manager key.
In this way, the validation key sits on the hot validator machine and can only sign on blocks. While the manager key sits in whatever wallet the validator chooses, possibly in a cold storage or can even be a smart contract, like a multi-sig.

## Technical spec:

* The consensus contract will have a new method called `createValidator(address validationAddress)`.
* The `createValidator` method will create a contract with two attributes:
  * `validationAddress`: set to be the `validationAddress` param.
  * `managerAddress`: set to be `msg.sender`.
* The `createValidator` method will register this contract to the consensus contract.
* The `validationAddress` in this contract can be changed only by a call from the `managerAddress`.
* The `managerAddress` cannot be changed since if the `managerAddress` is compromised it doesn't matter anyway, and if the validator thinks it is compromised, he just needs to create a new contract with new addresses and transfer the delegations to it.
* The consensus contract will have a map between the `validationAddress` and its validator contract, which means that the `validationAddress` should be unique between the validator contracts.
* The empty payable method of the consensus contract will be removed.
* In order to delegate, one needs to send FUSE tokens to the consensus while invoking the `delegate(address validator)` method with the validator contract address as the parameter.
* The consensus contract will save the delegation mapped to the validator contract, only if it a registered contract.
* Block rewards will go to the delegators, all of them minus the fee.
* The fee will go to the manager address of the validator.
* The validator can choose to self delegate using the manager address or any other address that he have in order to get rewards.
