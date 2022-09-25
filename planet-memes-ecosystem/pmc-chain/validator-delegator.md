# Validator /Delegator

### PMP-Chain Configurations

An IBFT PoS with built-in systems contracts will be used as a core consensus algorithm by PMP-Chain.&#x20;

● The average block time is expected to be 2 seconds.&#x20;

● Initially, 21 nodes will be running to comply with BFT (Byzantine Fault Tolerance).&#x20;

● Block size will be dynamic and decided by the Validator set. The initial block gas limit is 20,000,000.&#x20;

● Anyone with any amount of stake can become a validator in the system. There will be validator auctions organized periodically (days mostly), where anyone can replace any current validator by proposing a higher amount of $PMC staked. So, in short, it is an open system where we cannot reserve places for anyone.

PMC token holders who do not wish to run their own node can delegate their tokens to a validator. Delegation increases the power of the validator. More the power, more probability of the validator to become the block producer and checkpoint proposer and more weight in the consensus.

There is no minimum amount required for delegation. Any amount, even 1 PMC, will be accepted in the system. However, it is up to validators to set a minimum limit or not while accepting delegations. Validators might charge a commission in exchange for their node-running services. Other than the commission charged, one needs to evaluate the track record of the validator, for example, average uptime or if the node was ever compromised.



● We allocate 10% of our total supply of 10 billion tokens to fund the staking rewards over the next 5 years. As a validator. We also provide annual rewards as incentives!

● These rewards are primarily meant to jump-start the network, while the protocol in the long run should be able to sustain itself based on transaction fees.

● MPC-Chain has pre-deployed contracts for staking. This allows for the staking of $PMC tokens, providing rewards to holders.&#x20;

● If the block is not produced or accepted within the expected time, the next validator would take over the proposer's duty.&#x20;

● There is no newly minted block reward for block production.&#x20;

## Validator Hosting

Below are the suggestions for properly hosting a validator node in PMC network. Please pay careful attention to all the items listed below to make sure that your validator setup is properly configured to be secure, stable, and performant.

### Minimum system requirements <a href="#minimum-system-requirements" id="minimum-system-requirements"></a>



| Type | Value                                                                                          | Influenced by                                                                                                                |
| ---- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| CPU  | 2 cores                                                                                        | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li><li>Block time</li></ul> |
| RAM  | 2 GB                                                                                           | <ul><li>Number of JSON-RPC queries</li><li>Size of the blockchain state</li><li>Block gas limit</li></ul>                    |
| Disk | <ul><li>10 GB root patition</li><li>30 GB root partition with LVM for disk extension</li></ul> | <p></p><ul><li>Size of the blockchain state</li></ul>                                                                        |



