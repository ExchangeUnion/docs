# Exchange Union Daemon

The Exchange Union Daemon (`xud`) powers [Exchange Union](https://www.exchangeunion.com/), a decentralized exchange (DEX) built on the [Lightning](http://lightning.network/) and [Raiden](https://raiden.network/) networks to enable instant and trustless cryptocurrency swaps. The vision is to bring individuals and existing, centralized exchanges onto the same network and form one, global liquidity pool. This gives users a choice to either trade directly on the DEX, managing private keys and software stack, *or* to conveniently trade via a trusted exchange. Exchanges participating in the network aggregate the network's liquidity and can provide deeper order books and new trading pairs to their users. `xud` encompasses the following components:

* Integration with [lnd](https://github.com/lightningnetwork/lnd) and [raiden](https://github.com/raiden-network/raiden) nodes.
* Decentralized order book to locally aggregate orders from the known network.
* Matching engine to match new local orders with existing local and remote orders and initiate atomic swaps with remote peers.
* Peer-to-peer networking with and discovery of other nodes.
* gRPC API with web proxy to serve other applications, also accessible via the command-line interface `xucli`.