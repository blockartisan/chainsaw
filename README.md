# Chainsaw

Chainsaw is a framework for syncing, indexing, and interacting with blockchain data. It enables a declarative approach for defining blockchain state changes, with components designed for observing networks, indexing transactions, building and broadcasting transactions, and tracking them in real time.

## Syncing the Blockchain

### Components Involved
There are two primary components for blockchain syncing:

1. **Observer**: Observes the blockchain network and sends new blocks to the Indexer for indexing.
   - Multiple observers can connect to different nodes across geographically distinct locations, each working independently and sending updates to the Indexer.
   - Observers can apply filters to manage data efficiently. To ensure data consistency, syncing and locking mechanisms may be implemented, ensuring changes align with the latest filters before reaching the Indexer.

2. **Indexer**: Processes and indexes data received from Observers, storing it in a structured format.
   - Marks blocks or transactions as persisted once they’ve been observed by a defined number of observers.

**TODO**:
   - [ ] Define data exchange protocol between Observer and Indexer
   - [ ] Establish update and filter structure
   - [ ] Implement filters and clock synchronization
   - [ ] Detail specifications for Indexer
   - [ ] Detail specifications for Observers

## Interacting with the Blockchain

Chainsaw aims to support a declarative approach for defining desired changes in blockchain state, allowing the framework to handle execution. This approach can support features like balance checks for conflict-free transaction publishing.

### Plan (Abstraction)
Plans represent the highest-level abstraction for defining blockchain state changes, specifying operations on accounts, UTXOs, or other resources. Plans may have attributes like expiration, dependencies, and custom operations (similar to CRDs in Kubernetes). 

### Builder (Per-Blockchain Component)
The Builder component converts Plans into network-specific transactions, with each blockchain requiring a custom Builder for its operations.

### Broadcaster (Per-Blockchain Component)
Once a transaction is built, the Broadcaster submits it to the blockchain nodes, repeating broadcasts until it is confirmed by the Transaction Tracker in the mempool or indexed on-chain.

### Transaction Tracker (Per-Blockchain Component)
Tracks the transaction’s status in the mempool and checks for expiration. If a transaction expires, the Builder can generate a new transaction per the Plan’s rules.

### Coordination
Plans help define resources engaged in transactions, reducing the risk of conflicts between pending transactions. This coordination allows resource allocation while managing overlapping operations.
