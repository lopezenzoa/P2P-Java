# P2P-Java

*Enzo Agustín López*

## Motivation

After reading the Bitcoin's white paper and seeing some videos on YouTube about it,
I decided to recreate the system in Java, to expand my knowledge with the language and
learning more about blockchain technology as I think is a really powerful tool.

There is a principal aspect to consider when working with a P2P network: there's no a central node
to store all transactions and validate them, since everyone participating in the network is in fact
a node where to store all transactions. It means everyone must have a copy of an updated version of the Ledger.
The Ledger itself is the currency, is what makes valuable the protocol.

There are some problems related to the aspect described, such as:
* A malicious node can alter the Ledger.
* A malicious node can add fraudulent transactions in the Ledger.
* 

That work is the result of find a solution to these problems.

## Infrastructure

In this section, I describe every class, enumerate and interface at the protocol. 

### Account

Every Account counts with 6 properties:
* ID: a random identifier to search for an account in a database.
* privateKey: an integer to sign transactions.
* publicKey: an integer that works as an address for the account.
* username: a String to display in a dashboard.
* lastTransactions: a queue to store the last 10 transactions.

### Miner

Every Miner inherits from the Account class, and it adds a property:
* pendingBlocks: a map of the most recent block waiting to be added to the Ledger. It lets the miner
decide for which block it is going to work for validate.

### Transaction

Every Transaction counts with 8 properties:
* ID: a random identifier to search for a transaction.
* timeStamp: the exact moment when the transaction is made.
* amount: the actual amount is sending in the transaction.
* emitter: the public key of the emitter of the transaction.
* recipient: the public key of the recipient.
* blockID: the ID of the block that owns the transaction.
* status: the status of the transaction.

### Block

Every Block counts with 5 properties:
* ID: a random identifier to search for a block.
* hash: a secret integer that works as key to validate the block.
* previousHash: the secret integer (already discovered) of the previous block.
* transactions: the list of the 10 transactions that build the block.
* miner: the public key of the miner who is going to receive the reward.

### TxStatus (Enum)

The TxStatus enumerate class counts with 2 constants:
* PENDING
* VALIDATED

## Creating Accounts

Creating accounts is simply generate a new random ID and the key pair public and private
related to it and then setting it with a username for UI purposes.

## Creating Miners

Creating a Miner is the same process of creating an Account (see above), but specifying to create
the account as a Miner. It's good to say that once an Account or Miner is created, there's no chance of changing
the rol.

## Grouping Transactions

Every Transaction, once PENDING, is grouped with 9 transactions
to build a Block. Then, the Block is push to the pendingBlocks map at the Miner and waits for the validation
of the Block.

When the Block is validated (more on that below) is added to the Ledger and all transactions change to VALIDATED. In the Account, every VALIDATED transaction is added to
the lastTransactions queue.

## Validating Blocks

Every Block, once is full of transactions, is served to the Miner so he/she can decide
which block validate.

As it was defined int the Infrastructure section, every Block generate a hash in order to
perform a proof-of-work over the protocol. Miners can just generate random numbers until one of
them matches the required block hash.