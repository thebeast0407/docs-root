# Transactions

In BigchainDB, _Transactions_ are used to register, issue, create or transfer things (e.g. assets).

Transactions are the most basic kind of record stored by BigchainDB. There are two kinds: creation transactions and transfer transactions.

A _creation transaction_ can be used to register, issue, create or otherwise initiate the history of a single thing (or asset) in BigchainDB. For example, one might register an identity or a creative work. The things are often called "assets" but they might not be literal assets. A creation transaction also establishes the initial owner or owners of the asset. Only a federation node can create a valid creation transaction (but it's usually made based on a message from a client).

Currently, BigchainDB only supports indivisible assets. You can't split an asset apart into multiple assets, nor can you combine several assets together into one. [Issue #129](https://github.com/bigchaindb/bigchaindb/issues/129) is an enhancement proposal to support divisible assets.

A _transfer transaction_ can transfer one or more assets to new owners.

BigchainDB works with the [Interledger Protocol (ILP)](https://interledger.org/), a protocol for transferring assets between different ledgers, blockchains or payment systems.

The owner(s) of an asset can specifiy conditions (ILP crypto-conditions) which others must fulfill (satisfy) in order to become the new owner(s) of the asset. For example, a crypto-condition might require a signature from the owner, or from m-of-n owners (e.g. 3-of-4).

When someone creates a transfer transaction with the goal of changing an asset's owners, they must fulfill the asset's current crypto-conditions (i.e. in a fulfillment), and they must provide new conditions (including the list of new owners).

Today, every transaction contains exactly one fulfillment-condition pair. The fulfillment in a transfer transaction (input) must correspond to a condition (output) in a previous transaction.

When a node is asked to check the validity of a transaction, it must do several things; the main things are:

* double-spending checks (for transfer transactions),
* hash validation (i.e. is the calculated transaction hash equal to its id?), and
* validation of all fulfillments, including validation of cryptographic signatures if they’re among the conditions.

