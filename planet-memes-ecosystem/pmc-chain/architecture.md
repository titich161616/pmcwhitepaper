# Architecture

PMC-chain Architecture



### Libp2p <a href="#libp2p" id="libp2p"></a>

It all starts at the base networking layer, which utilizes **libp2p**.Libp2p is modular, extensible, and fast. Most importantly, it provides a great foundation for more advanced features.

### Synchronization & Consensus <a href="#synchronization--consensus" id="synchronization--consensus"></a>

The separation of the synchronization and consensus protocols allows for modularity and implementation of **custom** sync and consensus mechanisms - depending on how the client is being run.

PMC-Chain is designed to offer off-the-shelf pluggable consensus algorithms.

### Blockchain[​](https://wiki.polygon.technology/docs/edge/architecture/overview#blockchain) <a href="#blockchain" id="blockchain"></a>

The Blockchain layer is the central layer that coordinates everything in the PMC-Chain system. It is covered in depth in the corresponding _Modules_ section.

### State[​](https://wiki.polygon.technology/docs/edge/architecture/overview#state) <a href="#state" id="state"></a>

The State inner layer contains state transition logic. It deals with how the state changes when a new block is included. It is covered in depth in the corresponding _Modules_ section.

### &#x20;JSON RPC[​](https://wiki.polygon.technology/docs/edge/architecture/overview#json-rpc)

The JSON RPC layer is an API layer that dApp developers use to interact with the blockchain. It is covered in depth in the corresponding _Modules_ section.

### TxPool[​](https://wiki.polygon.technology/docs/edge/architecture/overview#txpool) <a href="#txpool" id="txpool"></a>

The TxPool layer represents the transaction pool, and it is closely linked with other modules in the system, as transactions can be added from multiple entry points.

### GRPC[​](https://wiki.polygon.technology/docs/edge/architecture/overview#grpc) <a href="#grpc" id="grpc"></a>

The GRPC layer is vital for operator interactions. Through it, node operators can easily interact with the client, providing an enjoyable UX.

\
