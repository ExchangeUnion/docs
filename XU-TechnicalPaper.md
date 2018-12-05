# [Exchange Union](https://www.exchangeunion.com) - The first decentralized network for instant and trustless trades between digital asset exchanges

**Table of Contents**
1. [Executive Summary](#1-executive-summary)  
  1.1. [The Vision](#11-the-vision)  
  1.2. [Project Intro](#12-project-intro)
2. [Product Description](#2-product-description)  
3. [Under the hood](#3-under-the-hood)  
  3.1. [Payment Channels](#31-payment-channels)  
  3.2. [Atomic Swaps](#32-atomic-swaps)  
  3.3. [Decentralized Order Book](#33-decentralized-order-book-dob)  
  3.4. [Security](#34-security)  
  3.5. [Visualization: Architecture](#35-visualization-architecture)  
  3.6. [Sample Trade](#36-sample-trade)  
  3.7. [Fiat Tokenization](#37-fiat-tokenization--stablecoin)  
    3.7.1. [The USDT Concept](#371-the-usdt-concept)  
    3.7.2. [The Bitshares Concept](#372the-bitshares-concept)  
    3.7.3. [The MakerDAO Concept](#373-the-makerdao-concept)  
    3.7.4. [The Basis Concept](#374-the-basis-basecoin-concept)  
  3.8. [Exchange Earnings](#38-exchange-earnings)  
  3.9. [Incentivisation: The Role of XUC](#39-incentivisation---the-role-of-xuc)  
  3.10. [Business Model: XUC](#310-business-model---xuc)  
4. [How is XU different from](#4-how-is-xu-different-from-)  
  4.1. [Ripple/Stellar](#41-ripple--stellar)  
  4.2. [Blockstream's Liquid](#42-blockstreams-liquid)  
  4.3. [Lightning/Raiden](#43-lightning--raiden)  
  4.4. [Alphapoints Remarketer](#44-alphapoints-remarketer)  
  4.5. [Altcoin.io/Etherdelta/Airswap/0x](#45-altcoinio--etherdelta--airswap--0x--idex)  
  4.6. [sparkswap](#46-sparkswap)  
  4.7. [RaidEX.io](#47-raidexio)  


## 1. Executive Summary

1.1. The Vision
---------------
Exchange Union (XU) connects digital asset exchanges by forming **a decentralized network**, which enables **instant and trustless** trades between digital asset exchanges, as well as individual traders. It enables cross-exchange trading for increased liquidity, access to the best price and an additional revenue stream for liquidity providers.

1.2. Project Intro
------------------
The Exchange Union project is powered by [XUD](https://github.com/ExchangeUnion/xud/), short for **Exchange Union Daemon**, the  open source node software which is operated by exchanges (or traders) and comprises the network. This document describes the inner workings of XUD from a product & technical perspective and summarizes technical requirements. For an overview of idea, use case & benefits read the [White Paper](https://www.exchangeunion.com/static/files/ExchangeUnion-WhitePaper_en.pdf), for hands-on guides check our [Wiki](https://github.com/ExchangeUnion/xud/wiki) and for code-level documentation our [TypeDoc](https://exchangeunion.github.io/xud-typedoc/). Chat with us on [Gitter](https://gitter.im/exchangeunion/Lobby) and follow major event & development milestones on our [blog](https://blog.exchangeunion.com/).

XUD's code base and documentation is available under the [AGPL-3.0](https://github.com/ExchangeUnion/xud/blob/master/LICENSE) open-source license. The objective is to build a open-source developer community, while additionally employing full-time developers under the Exchange Union brand. Development is funded through an initial private sale of 2% of Exchange Union's XUC token, which is since in circulation. No ICO or further fundraising is planned.


## 2. Product Description
From a business perspective, Exchange Union can be described as a *decentralized meta-exchange*: target users are existing centralized exchanges, in the following 'exchanges' for short.

From a technical perspective, XUD spins up an *instant [DEX](https://en.wikipedia.org/wiki/Decentralized_exchange)* which can be openly accessed and used by everyone. However, due to its technical complexity, it is not expected that XUD will see a widespread adoption by individuals in the short-term.

Simply put, there are two ways of how an individual can trade on the Exchange Union network:
* Use a trusted exchange (which integrated XUD and handles the technical complexity of operation and custody of funds) *or*
* Operate XUD directly (with full control of funds & trades, managing private keys & other technical aspects).

Currently, appropriate liquidity can only be found on exchanges and we believe the majority of traders will continue using these for convenience, delegating the technical complexity and operational overhead to an exchange operator. Furthermore, exchanges provide the necessary banking relationships and integrations that enable deposits and withdrawals of fiat currency. Retail & new traders in particular will rely on these fiat currency services and easy-to-use interfaces.

Using XUC, Exchange Union enables exchanges and other liquidity providers to adopt a new, additional [business model](#39-incentivisation---the-role-of-xuc) to trading fees. It can be concluded that Exchange Union is not cannibalizing the business of exchanges, but rather enhancing it.

Advantages for traders:
- Access to liquidity of combined order books of all participating exchanges
- Access to new trading pairs
- Access to the best price across all participating exchanges
- All through one account on one exchange of the users choice

The advantages for exchanges:
- Access to larger user base, including individual traders in the network
- Additional revenue stream from individual traders & traders of other exchanges, while keeping trading fee revenues on outgoing trades
- Robust, decentralized, and censorship-resistant trading infrastructure
- New product opportunities which use XUD: wallets, merchant services etc.

Using [atomic swaps](#32-atomic-swaps), XUD eliminates the need for trust between peers on the network. This allows any exchange and any trader to join the network and trade, while both sides can rest assured that their funds are safe throughout the whole cycle of a trade. Importantly, to achieve this, Exchange Union does not use an intermediary asset or debt representation of an asset ([IOU](https://www.investopedia.com/terms/i/iou.asp)). It uses the actual native underlying currency, like BTC or ETH, which only live on the bitcoin and ethereum blockchain respectively. Consequently, Exchange Union doesn't have its own blockchain. It connects native blockchains like bitcoin or ethereum on layer 2 via [payment channels](#31-payment-channels), atomic swaps and adds a p2p layer to exchange & match orders which we call [decentralized order book (DOB)](#33-decentralized-order-book-dob).

A major design goal of XUD is, that no single point of failure (SPOF) can cause a shutdown of the network, eliminating critical attack vectors like DDOS and increasing censorship resistance. The main **difference from existing decentralized exchanges**, like [IDEX](https://idex.market/), [Etherdelta](https://etherdelta.com), [Airswap](https://airswap.io/) or relayers on [0x](https://0xproject.com) is, that **orders are matched decentralized** and **trades on Exchange Union settle instantly**. Instant settlement is achieved by using above mentioned payment channels, whereas decentralized exchanges today settle trades on-chain; which can take up to an hour or more. Also, costs for on-chain transactions can be high depending on the network  costly; transaction fees can be equivalent to several dollars at peak times which doesn't make it a reasonable method for smaller trades. Finally, XUD enables cross-chain trading, that means it is not limited to tokens of one specific chain. Most DEXes today are limited to ERC-20 tokens on ethereum, whereas XUD can expend any token or cryptocurrency supporting payment channels and [HTLCs](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts).


## 3. Under the hood

This chapter treats the technologies ‘payment channels’, ‘atomic swaps’ the basic functionality of the decentralized order book. XUD exposes a simple API designed for exchanges and, together with XUD nodes of other exchanges, forms the decentralized network connecting the exchanges. XUD is available as [installable software](https://github.com/ExchangeUnion/xud/wiki/Installation) and will additionally be provided in form of a docker container which packages all necessary things to operate XUD.


3.1. Payment Channels
---------------------
The idea of payment channels is around for a while, even Satoshi already explored [*high-frequency transactions*](https://en.bitcoin.it/wiki/Payment_channels). Then, with bidirectional channels, the inception of today's **Lightning Network** by Joseph Poon and Tadge Dryja, payment channels became more widely known. In short, payment channels enable instant, off-chain transfer of a digital token or coin, like bitcoin, directly between *two* parties. Payment channels are opened and closed via one on-chain transaction. A payment channel uses unbroadcast, but signed and fully valid on-chain transactions, which are sent back and forth between the two parties to do transactions. Since these unbroadcast transactions are merely data packets which are send via the internet layer and don't need to be validated on a blockchain, transfers are as fast as the internet connection between the two parties.

Transactions are held on each side of the channel, in our case on the exchange’s XUD, representing the balance of each party. They use a multi-signature address as their input (the funding address) and point at two different addresses for their output, each controlled by one exchange. Setting up payment channels can be compared to transferring coins into a sidechain - coins have to be committed to a payment channel via a native on-chain transaction. This transaction 'locks' coins on the underlying chain and 'unlocks' them in the channel.

With this mechanism, payment channels are trustless. Trustless means one bitcoin in a payment channel is backed by one locked bitcoin on the blockchain and can be transferred between two parties securely, even though only the opening and closing transaction are included in the blockchain. Simplified, this is secure, because all transactions are valid on the underlying blockchain and can be broadcast by either party at any time. So both parties can rest assured to be in possession of the respective funds when receiving a transaction from the other party in a channel.

Payment channels also allow secure and trustless *routing* of funds through intermediaries using a technology called [Hashed Timelock Contracts (HTLCs)](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts), which eliminate the need for a direct channel between everyone. The [Lightning White Paper](https://lightning.network/lightning-network-paper.pdf) describes this in detail. This is useful since usually a few payment channels to some other nodes are enough to reach the rest of the network. The payment channel layer of XUD is comprised by the feature-rich [LND](https://github.com/lightningnetwork/lnd/) implementation for payment channels on BTC & LTC and by the [raiden](https://github.com/raiden-network/raiden/) implementation for payment channels on ETH, including all ERC20 tokens. XUD will rely on improved [auto-pilot](https://github.com/lightningnetwork/lnd/tree/master/autopilot) features to manage channels and routing fees automatically. Exact amounts of routing fees are hard to quote in advance of a trade and since we expect these to be negligibly low in future, exchanges are meant to use their [revenue from Exchange Union](#39-incentivisation---the-role-of-xuc) to offset routing fees.

Each participating exchange runs its own XUD instance, which opens payment channels on each chain it desires to enable trading for. This means, if for example an exchange wants to enable its users to trade bitcoin and litecoin through XUD, it has to open a payment channel on the litecoin blockchain & on the bitcoin blockchain to at least one other exchange in the union. Payment channels in XU are meant to be kept open long-term. Since routing is and will be relatively unreliable for larger amounts in the foreseeable future, the current design encourages larger, direct channels between exchanges. Currently, lnd's maximum channel size is temporarily limited to 0.16777216 BTC. This can be lifted using a so-called [wumbo](https://github.com/lightningnetwork/lightning-rfc/wiki/Lightning-Specification-1.1-Proposal-States)-bit. To achieve long-term scalability, even with routing, we will rely on multiple channels, which carry out partial transfers of one atomic payment as proposed in the [AMP protocol by Laolu and Conner](https://lists.linuxfoundation.org/pipermail/lightning-dev/2018-February/000993.html).

In general, all BIP 199 compatible payment channels can be supported by XUD directly, other chains may implement HTLC swaps via smart contracts. The payment channel standard on bitcoin and litecoin is called the [lightning network](https://lightning.network/) and compatible implementations are maintained by the companies [ACINQ](https://github.com/ACINQ/eclair), [Lightning Labs](https://lightning.engineering/) and [Blockstream](https://blockstream.com/) which are all [intercompatible] by following the so-called [BOLT](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) specifications. Ethereum’s most advanced payment channel technology is the ‘[Raiden Network](https://raiden.network/)’ and is mainly maintained by the company [Brainbot](http://www.brainbot.com/).

**XUD, using LND and Raiden, represents a sub-graph of the lightning & raiden network. It's a primary goal to remain fully compatible with the [lightning BOLT specifications (BOLT)](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) and with the specifications of the raiden network.** XUD acts as a wrapper for the payment channel layer and encapsulates lightning and raiden daemons with as little modifications as possible.

Summary: XUD uses payment channels in combination with atomic swaps to instantly settle trades on the Exchange Union network. Check [this](https://github.com/ExchangeUnion/Docs/blob/master/2018-09-19%20Lightning%20Dev%20Meetup%20Ellis%20%26%20Bob.pdf) visual walk-through of a trade.


3.2. Atomic Swaps
-----------------
For each successful trade between two exchanges A & B on XU, e.g. LTC against BTC, there is a transfer of BTC on the BTC lightning network and LTC on the LTC lightning network in form of an atomic swap. This means, actual bitcoin flow from exchange A to exchange B on their bitcoin payment channel and actual litecoin from exchange B back to exchange A on their litecoin payment channel. This is considered the settlement of a trade and final. Atomic swaps use [HTLCs](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts) to bind these two transfers cryptographically and ensure the atomicity of a swap: either the swap completes in full, or nothing happens. [This document](https://github.com/ExchangeUnion/Docs/blob/master/2018-09-19%20Lightning%20Dev%20Meetup%20Ellis%20%26%20Bob.pdf) explains visually how atomic swaps are set up on Exchange Union. A manifestation of the first successful atomic swap between BTC and LTC on lightning by the Lightning Labs team can be found [here](https://blog.lightning.engineering/announcement/2017/11/16/ln-swap.html) and the second one by the Exchange Union team [here](https://blog.exchangeunion.com/product-update-4-atomic-swaps-on-lightning-diy-follow-our-swap-guide-185401a06a2d). Token swaps of ERC20 tokens on raiden are described [here](https://raiden-network.readthedocs.io/en/stable/api_walkthrough.html#token-swaps) and were also [successfully tested by the Exchange Union team.](https://blog.exchangeunion.com/milestone-atomic-swap-of-erc20-tokens-on-the-raiden-network-84b918b86fc4)

Since raiden is designed for ERC20 tokens, Exchange Union will be able to support the vast majority of today’s token market. Other major chains are expected to start adopting lightning/raiden-style payment channel technology in the mid-term. Options how to support some in the short term, e.g. via pegged [ERC20 wrappers](https://weth.io/), can be explored but wouldn't be trustless.


3.3. Decentralized Order Book (DOB)
--------------------------

Order matching systems ('trade engines') of major exchanges differentiate between two participants: the taker and the maker. A maker is someone who submits an order which can't be matched immediately and joins a pool of unmatched orders. A taker on the other hand issues an order which can immediately be matched with an existing (maker) order upon entering the pool. A taker ‘fills’ the order of a maker. Makers are sometimes also called 'liquidity providers' and play an important role in establishing liquid markets and are usually rewarded with lower or 0% fees on centralized exchanges, which is why we chose a similar incentivization model for XU. In short, in Exchange Union, a taker pays the maker as a reward for the provided liquidity. Details below.

Analogically, we distinguish between two types of participants in Exchange Union:
- The **Maker** (the liquidity provider) propagating an order to its peers because it couldn't find a match in current the order book. The order then joins a pool of unmatched orders on all connected peers. Usually in the form of a limit order.
- The **Taker** (the liquidity consumer) issuing an order which immediately can be matched with an existing (maker) order in the order book. Usually in the form of a market order.

There will be no complete global order book as such, containing all orders for each and every trading pair. Instead, orders will be propagated based on preferences, e.g. only specific trading pairs or only from specific peers. Therefore, XUD only maintains relevant parts of the order book. XUD constantly exchanges order information and updates with connected peers.

The DOB is a key task for XU and an unresolved issue on existing decentralized exchanges where, to the best of our knowledge, the order book is realized in a centralized way e.g. on [Etherdelta](https://github.com/etherdelta/etherdelta.github.io/blob/master/docs/API.md) or [0x relayers](https://github.com/0xProject/standard-relayer-api/blob/master/http/v0.md). We believe order propagation and matching has to be decentralized to the best degree possible to sustain the robustness and censorship resistance of the network. Nevertheless, decentralization always comes at a cost. Based on the [PACELC theorem](https://en.wikipedia.org/wiki/PACELC_theorem) our protocol chooses latency over consistency.

The DOB faces the following attack vectors:

**Latency**

The speed of order updates is crucial to keep order information as accurate as possible and also minimize noise on the network. The higher the latency, the more would nodes be busy requesting to fill orders that are no longer available. Also, this would increase the resulting network traffic. Since we target to on-board high-volume exchanges, this network load is expected to be significant.

**Point of Execution**

There is no central point of execution (order matching) in Exchange Union's open, decentralized system, which exposes several new attack vectors: For instance, a node exploiting network latency could request multiple nodes to fill its order and trigger the execution of swaps. Even though stealing funds is prevented by atomic swaps, this can lead to large amounts of assets being locked up because of the time-locks used in atomic swaps to issue refunds in case of disputes. Large scale market manipulation with this technique could make this a profitable endeavor.

**SPAM**

Like any other decentralized system, Exchange Union's DOB faces DDoS attacks, in particular with false order information.

Goals of our DOB Protocol
=============================================================

**1. Minimize latency of order updates**

For orders, the DOB protocol follows the first come, first served principle, which remains compatible with how order book systems of centralized exchanges work. To get the best achievable latency between two nodes which intend to receive order updates from each other, XUD requires a direct socket connection on Internet Protocol level without intermediary hops - a full mesh network. Further, not routing orders through other nodes prevents critical attacks like withholding, delaying or altering orders. Direct payment channels are recommended, but payments can also be routed through intermediary hops.

![Screenshot](images/Full%20Mesh%20vs.%20Partial%20Mesh.png "XUD DOB Full Mesh vs. Payment Channel Partial Mesh")

Obviously, direct socket connections between peers to exchange order book information can't scale infinitely. We are targeting to establish a relay network of 'super nodes' in a later phase which are eligible to forward orders. Individuals running XUD can choose to receive & forward order book information from these super nodes. To become a super node, one must fullfil certain requirements, like e.g. a fast and reliable connection to peers. [Efficient congestion control mechanisms](https://github.com/google/bbr) to further optimize latency are currently being evaluated.

**2. Single point of execution**

There is one single point of execution for each order: the taker. The taker decides on which maker orders it wants to fill. All orders contain payment information (e.g. a lightning invoice). From a high-level, the first version of our swap protocol works like this:
* taker finds a match
* taker creates `r_hash` and `r_preImage` for atomic swap
* taker sends a `swapRequest` message to the maker which includes `r_hash`
* maker confirms full or partial quantity in `swapResponse` message
* taker initiates the swap with the first HTLC to the maker using `r_hash` (with the maker's confirmed quantity)
* maker checks if incoming HTLC quantity and price matches `swapResponse` and creates second HTLC to taker using `r_hash`
* taker releases `r_preImage`
* taker sends `swapCompleted` to maker

Before the taker releases `pre_Image`, a swap can be cancelled gracefully. After the release of `r_preImage` a swap is considered completed.

XUD takes care that both sides have a payment channel path with sufficient volume for a specific order. Also, it is taken care, that an invoice can only be paid by the first one to successfully send a payment to an invoice issuer.

Transferring order execution to a decentralized consensus (compare e.g. [loopring](https://github.com/Loopring/whitepaper/raw/master/en_whitepaper.pdf)), was considered and deemed not feasible for the following reasons:
a) consensus is slow, too slow for an acceptable trading experience
b) an exchange probably won't let anyone else decide on who fills its users orders
c) hard to control since in our case exchanges could always treat their local order book with priority and are essentially in control since they control the release of `r_preImage`.

**3. Reward makers (liquidity providers)**

Makers are earning [XUC](#310-business-model---xuc); takers are paying XUC to the makers for filling orders. All orders carry a XUC price in a specified fee field. We aim to establish a fee market where makers are competing with a combination of order quantity, order price and XUC fee. A taker pays the XUC fee via a default Raiden channel to the maker in a 3-way atomic swap with one `r_preImage`. Not part of the PoC.

Exchanges can either completely take over handling XUC fees and revenue for their users or they can use XUC as a new business model to attract market makers on their platform. This would require takers to hold XUC in order to trade on XU.

**4. Preventing malicious behavior**

Every XUD is required to stake a certain amount of XUC in an ethereum smart contract in order to be able to act as a maker or taker. This staked XUC amount can be taken away as punishment in case of a low enough reputation score. Not part of the PoC. 

XUD's ID is called `nodePubKey`. When depositing XUC into the smart contract, XUD will write it's `nodePubKey` into the data field of the deposit transaction. Since it is expected that almost every XUD will run an ethereum full node, XUD can verify that a new node staked the required amount directly at handshake. 

**5. Privacy between two trading parties**

Not part of PoC. As it is the case on regular digital asset exchanges, a maker and a taker should have the option to remain anonymous to avoid biased behavior and in general to just preserve privacy. On payment channels this is taken care of by [onion routing](https://github.com/lightningnetwork/lightning-rfc/blob/master/04-onion-routing.md). A tor-hidden service for order book information exchange could be an option, but would require order forwarding and slow down things significantly. Further research pending.

XUD Order Book Data Structure
=============================================================

XUD stores orders and related information in a db, the full structure can be found [here](https://github.com/ExchangeUnion/xud/blob/master/lib/orderbook/). Two examples:

```
pairs {
Id ({baseCurrencyId}/{quoteCurrencyId})
baseCurrencyId
quoteCurrencyId
swapProtocol ({LND, RAIDEN})
}
```

This table is automatically maintained by xud with enabled currencies pairs which fullfil all requirements to be swap enabled.

```
Orders {
Id
pairId
peerId
quantity (decimal)
price (decimal)
nodePubKey
}
```

This table maintains the open orders which are received by peer nodes (defined in Peers table in the P2P domain database). A `peerID` is null for the exchange's `ownOrder`s, the `quantity` is positive for buy orders, negative for sell orders.

[XUD's API](https://github.com/ExchangeUnion/xud/blob/master/docs/api.md)
=============================================================

From a high-level perspective, XUD supports two modes: matching and nomatching mode. 

Default is `matching` mode, which means that XUD acts as order book & matching engine for all orders on the exchange. An exchange chooses this mode if it is not live yet or comparably small since changes to the existing exchange system are required. An exchanage's `ownOrder`s need to be added to XUD's order book and it needs to listen to order events. For instance to display new incoming `peerOrder`s to users and update user account balances, e.g. after a successful swap. The advantage of this mode is, that it's more native to Exchange Union and requires less operational oversight from the exchange.

`nomatching` mode, which can be enabled via [xud.conf](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf), is designed for exchanges which want to continue using their own matching engine and require minimal changes to the system in general. Here, all order matching is handled by the exchange's existing matching engine. XUD mainly a) informs about new & removed orders from other peers and
b) executes swaps with peers when a match in the matching engine was found. The advantage of this mode is, that it is designed to require less adjustments, but is currently less tested and will need a decent amount of operational oversight.

Both modes use a different set of API's, a guide can be found [here](https://github.com/ExchangeUnion/xud/wiki/Exchange-Guide#api-integration). Examples of some calls:

*`SubscribeAddedOrders` / `SubscribeRemovedOrders`*

informs about orders being added / removed from the XUD orderbook; used by the exchange to match orders (`nomatching` mode) and display XU orders in it's UI


*`placeOrder`*   

places an order in XUD, which will first attempt to get matched with existing orders (as taker order) and if no match could be found, propagated to connected peers (as maker order).
```
Example: "I want to place a limit order of buying 10 LTC for 20 ETC."
placeOrder(LTC/ETC, 20, 10)

Example: "I want to place a market order of buying 10 LTC for ETC."
placeOrder(LTC/ETC, 0, 10)
```


3.4. Security
-------------
The Lightning Network and the Raiden Network take care of security on the payment channel layer. This includes resolving disputes over channel states on the underlying blockchain and disincentivizing malicious behavior by POS-style punishment (a dishonest party will lose all her funds to the honest party). It especially also includes security when routing payments through intermediary hops. We use atomic swaps to bind two payments of a trade together, so that one party cannot steal funds after receiving the first part of the trade. Both payments in a trade have to complete successfully to be valid and spendable. Exchange Union will be secured against malicious order information in a POS-style manner, to disincentivize market manipulation by sending out false order information. As mentioned above, this is not part of the PoC and will be fleshed out in a later stage.

XUD's ID Model
=============================================================

During setup, XUD automatically creates a public/private key pair (using bitcoin's `secp256k1 ECDSA`) for identification and order signing purposes, the `nodePubKey`. The private key is saved on disk and encrypted with a password. The `nodePubKey` represents one XUD's unique ID within Exchange Union and is backed by a staked amount XUC as described above. Exchanges can let others know what their XUD's `nodePubKey` is by e.g. making such available on their website. XUD will support whitelisting specific `nodePubKey`s for trading. In this way, exchanges in strict jurisdictions can limit trading to exchanges which fulfill the local jurisdiction's legal requirements. To harden whitelists, XUD will support signing & encrypting orders to make sure the order was indeed placed by the authenticated party and not tempered with.

XUD's Order Signing
=============================================================

XUD enables two modes for signing an order:
1. Sign order to verify authenticity of public key
2. Sign & encrypt order to verify authenticity and add security against spoofing

*Mode 1* is the default and signs all orders with the XUD's private key. This ensures that the `nodePubKey` in the order's invoice is the actual ID of the XUD sending the invoice. We decided to place the `nodePubKey` directly in the invoice description field for a faster processing, since this avoids a payment hash vs. `nodePubKey` lookup for each incoming new order on the receivers side. This prevents another XUD pretending to be someone else and knowing the actual issuer of an order is important for our reputation mechanism to work.

*Mode 2* is optional and additionally to everything *Mode 1* does, XUD encrypts orders with the public key of each peer. This mode is more resource intensive. Combined with a white-list, this mode is suitable for exchanges located in jurisdictions with a strict legal framework. It ensures that order information can only be received & filled by white-listed trading partners, e.g. other licensed exchanges in the same jurisdiction. It is not part of the PoC.

3.5. Visualization: Architecture
-----------------------------------------------------
![Screenshot](images/Exchange%20Union%20Nodes.png "Exchange Union System")

And an overview of XUD's components:

![Screenshot](images/xud%20components.png "XUD components")


3.6. Sample Trade
-----------------
This is the basic flow of a trade via XU:

3.6.1. Assuming `Exchange A` is part of Exchange Union and runs XUD. It offers a rare trading pair to its users: **UnicornCoin / BTC**. In order to offer this pair to users in the Exchange Union network, `Exchange A`'s XUD is configured to open payment channels and run full nodes on both, the bitcoin and the UnicornCoin chain. `Exchange A` funds XUD with e.g. 100 BTC. XUD then opens a payment channel with these deposited funds to `Exchange B`. The 100 BTC represent `Exchange A`'s & `Exchange B`s total BTC trading volume. `Exchange B` does the same with UnicornCoin.

3.6.2. `Exchange A`'s XUD has successfully opened a payment channel to `Exchange B`'s XUD on Bitcoin and on UnicornCoin. `Exchange A` now places an order in XUD, which sends this to `Exchange B`, using it's P2P socket connection. This order, which is similar to a regular buy or sell request submitted in a cryptocurrency exchange, includes the amount of requested UnicornCoin with a price in BTC. Let’s assume `Exchange A` sends a **limit order for selling 100 UnicornCoin** with the requested price of **1 UnicornCoin = 0.1 BTC**. This order is published to `Exchange B`, as well as all other connected peers.

3.6.3. XUD's subscribed to UnicornCoin, update their order books with this new order.

3.6.4. If `Exchange B` now wants to fill this 100 UnicornCoin order, it does so by placing a corresponding **buy 100 UnicornCoin for 0.1 BTC** order. Everthing after that is handled automatically. XUD takes care of confirming availability with `Exchange A` and executed a cross-chain atomic swap on the payment channels on Unicorn chain and the bitcoin chain it created before. `Exchange A` atomically receives **100 UnicornCoin** and `Exchange B` **10 BTC** on the respective payment channels.

Comments on specific failures:
- If the order is filled by another, faster XUD: `Exchange A`'s XUD will tell `Exchange B` about this and XUD starts looking for the next best matching order. If none can be found, the order will get added to the order book and propagated to all peers as maker order.
- `Exchange A` goes offline: trade times out and `Exchange B`'s XUD starts looking for the next best order. 
- Payment channel exhausted: XUD maintains a configurable threshold on when to alert the exchange's system to redeposit funds into XUD.


3.7. FIAT Tokenization / Stablecoins
-----------------------------------
To support FIAT/Crypto pairs, FIAT has to be ‘tokenized’ to be tradable on Exchange Union. Also it has to fulfill the requirement to be trustless, thus a simple USD or EUR IOU won’t do. Several approaches exist:

3.7.1. The [USDT](https://tether.to/) Concept
-----------------------------------------
A company holds one USD in a 'reserve bank account' for every USD token issued. And ideally is transparent about it.

**Good** This would work for all FIAT currencies that a bank supports without major technical hurdles.

**Bad** History has shown that transparency is difficult for some companies and lack of regulation brings the following problems:
- Even if a company would be 100% transparent about circulation and bank account balance, it would be hard to trust since this company is not regulated. Also, banks tend to cut ties with unregulated companies when they feel pressure from regulators, as [happened multiple times with e.g. Tether Limited](https://news.bitcoin.com/tether-coin-may-be-a-precarious-us-dollar-peg/). News go public, Markets panic and the stable coin is not stable anymore.
- Hacks are not insured and procedures are not defined as [happened e.g. with Tether Limited](https://techcrunch.com/2017/11/20/tether-claims-a-hacker-stole-31m/)
- If one is semi-transparent, like Tether Limited, people have to put trust and faith in Tether to actually have the corresponding USD in their bank account. 

Newer, regulated stable coins improve the situation above. Examples are [TUSD](https://www.trusttoken.com/trueusd/), [USDC](https://www.circle.com/en/usdc), [GUSD](https://gemini.com/dollar/) and [PAX](https://www.paxos.com/standard/).


3.7.2. The [BitShares](https://bitshares.org/technology/price-stable-cryptocurrencies/) Concept
-------------------------------------------------------------------------------------------
A smart contract holds up to 200% of one USD in crypto. In simple terms, each USD token contains 2 USD worth of crypto.

**Good**
- Tested for years, proves self-adjusting through ‘soft-peg’, traders will go short/long awaiting the coin to re-stabilize. See the BUT below.
- We potentially could use this concept to use XUC instead of BTS

**Bad**
- BUT The system only works because people BELIEVE the coin re-stabilizes and because it’s backed by Bitshares Company as lender of last resort which ensures that as a sort-of central bank. Nothing in the protocol automatically ensures bitUSDs peg to be one USD.
- Requires exchanges to lock up minimum 200% of the FIAT trading volume
- Emergency liquidation is untested and will probably run into issue if a huge asset like bitUSD suddenly has to get emergency liquidated.


3.7.3. The [MakerDAO](https://blog.makerdao.com/2017/06/05/introducing-sai/) Concept
--------------------------------------------------------------------------------
DAI = Basically BitShares+: works with crytpo collateral in a smart contract. Here this is called Collateralized Debt Positions (CDPs). Users can create DAI’s which are backed by ETH, in future other (ETH-based) assets. The Target Rate Feedback Mechanism ([TRFM](https://medium.com/cryptolinks/maker-for-dummies-a-plain-english-explanation-of-the-dai-stablecoin-e4481d79b90)) creates user incentives to HOLD or BORROW Dai and thus correct the price to a given price target, like 1 USD. It gets triggered if the price goes off by a ‘sensitivity parameter’ which is set by MKR token holders in POS-style governance. TRFM increases the target rate if price is too low, which causes generation of new DAIs to get more expensive and gains for holding DAIs to increase. This reduces supply causes price to increase back to target price. Works the other way round.

**Good**
- Several fallback mechanisms, global settlement being the last resort
- We could use XUC as collateral, since it’s ERC20

**Bad**
- Completely trusts one price oracle (decentralized price oracles are mentioned in the paper though)
- No native ETH support (has to be wrapped into PETH)
- MKR as ‘stability fee’ - do I need to pay to hold DAI over time?
- Why are MKR holders to decide on the sensitivity parameter? It should be just pre-set. Sounds a bit like a forced use of MKR and potentially could be manipulated


3.7.4. The [Basis](http://www.basis.io/) (Basecoin) Concept
------------------------------------------------------
A robust, price-stable cryptocurrency with an algorithmic (automatic) central bank. It monitors USD or any other price (e.g. EUR or CPI) through external oracles, and smart contracts adjust the basecoin supply according to current market supply and demand (the basecoin system is aware of both) to keep the value close to the pegged asset.

**Good** 3 coin system:
- basecoin: Pegged with USD (“baseUSD”) or to any other asset, directly faces users
- base bonds: tokens to be ‘auctioned off’ programmatically by the blockchain when supply needs to be reduced. 1 base bond will be exactly 1 basecoin in future when supply needs to be increased
- base shares: fixed-supply cryptocurrency that does not have a peg, holders of these will get basecoin when supply needs to be increased. Free money for initial investors?

**Bad**
- It’s not backed by anything (!)
- It's untested. How fast can the system respond? How fast do prices adjust - long-term peg is not sufficient for trading purposes.
- Issuing new base coins is relatively easy. But buying back coins relies on a super liquid market of base bonds to buy back base coins. Speculators need to trust their base bonds will be worth more in the future when the basecoin supply is extended. All this needs to have a proven track record to work. It strongly depends on initial market management. Also, they promise an interest rate – not clear how this is done.
- How exactly are dead spirals avoided?
- Any fallback mechanisms?

**Conclusion** For now, MakerDAOs DAI seems promising and a thought-through model. We'll adopt the solution with the highest adoption among exchanges.


3.8. Exchange Earnings
----------------------
**Executive summary: worst-case, exchanges will earn the same fees as compared to not being connected to Exchange Union. In almost all cases revenues are increased.**

How this is achieved: XUD enables the exchange to subtract the trading fee before the actual amount of a trade is swapped as taker, or add the trading fee before an order is sent to the network. Since exchanges are usually earning fees from both sides of a trade, maker & taker fee, XUD allows exchanges to charge the equivalent amount of maker+taker fee for each side of the trade. Naturally, both exchanges of a trade will want to do this. Consequently, every trade facilitated via Exchange Union enables two exchanges to earn the full maker & taker trading fee. This is how exchanges continue earning the same trading fees, even if all `ownOrder`s are filled via the Exchange Union network, the above described worst-case scenario. It also means, that in the long run earnings will go up for exchanges for every trade executed via XU, since fees are earned twice for every trade on Exchange Union.

How does this still benefit an trader on `ExchA`? Trades only get filled by `ExchB`, if

```
(ExchB_price + ExchA_makertakerfees + ExchB_makertakerfees) < (ExchA_price+ExchA_takerfee) 
```

Consequently, traders can only end up with better prices. If the price difference is not large enough to cover the fees kept by the exchanges, orders won’t be matched. It is expected that through market forces (arbitrage) price levels keep adjusting and not all trades leave the exchange. Through the real-time nature of swaps in the Exchange Union network, market forces should be able to adjust prices much faster than currently. Combined with the liquidity advantage over other exchanges, this should lead to an increased trading volume being executed on the exchange in the long-term.

The following section introduces an additional revenue stream for exchanges, using XUC.


3.9. Incentivisation - The Role of XUC
--------------------------------------

Exchanges, developers and other functionaries which contribute resources to the XU ecosystem are key for bootstrapping Exchange Union, reach a ‘network-effect’ size and be successful in the long-term. We believe a functioning monetary incentive system is the success factor for Open-Source projects in the future. Bitcoin itself is the best example for a system driven by monetary incentives to build and maintain an ecosystem. Similarly, we believe Exchange Union creates value and should be financially self-sustaining.

Flawed incentivisation is believed to be the main reason why other solutions for connecting exchanges didn’t find adoption yet. For example, some solutions require exchanges to pay hardware and/or monthly fees. From an exchange's point of view, this doesn’t add up without offering something in return. Most solutions essentially offer an easier way for user funds, and thus trading revenues, to leave the platform. Exchange Union is an open P2P network, doesn't require any special hardware and free of charge to join. More importantly, it will enable exchanges to increase revenue by offering more liquidity to it's users. Additionally we introduce a maker (liqudity provider) reward scheme, using XUC.

**Incentives for Exchanges**
- As discussed in chapter 3.3, the main use of XUC is to incentivize makers (liquidity providers)
- Makers can set a XUC price tag for their orders, which is to be paid by takers to gain the right to fill their order. All this happens automatically in the atomic swap
- In order to further increase interest of exchanges joining Exchange Union, the idea is to allocate XUC to new exchanges joining Exchange Union. These are to be distributed to an exchange's users ('airdropped') or managed by the exchange, depending on the business model of the exchange. Either the exchange pays maker's XUC fee, or user directly
- If the exchange decides to let end-users earn and pay XUC for trades, *end-users have to hold XUC in order to use XU as a taker*. Compare Binance’s BNB model, which gives discount on fees for holders of BNB.

Additionally, the different functionaries which are rewarded with XUC are:

**Incentives for Developers**
- Each pull-request to the [Exchange Union github](https://github.com/ExchangeUnion) gets peer reviewed and a score by each reviewer. XUC is paid to the contributor based on that score. In operation since 04/18.
- Reviewers get rewarded
- Testers get rewarded, test-net time gets rewarded

**Incentives for Functionaries which provide services (not part of PoC)**
- Order book relays by super nodes will get awarded with XUC in a later stage. Peers choosing to obtain order information from a super node which provides a faster connection, might pay for this service.

**Incentives for traders (not part of XU)**
- A user has X trading volume on Exchange Union: this user gets rewarded with XUC by exchange
- This is a program of each exchange

3.10. Business Model - XUC
-------------------------
Exchange Union’s business model is based on XUC. If XUC is useful and  being used within in Exchange Union, it's valuable. As discussed above, the main usage of XUC within XU is as fee payment from taker to maker, the liquidity provider. XUC’s overall supply is fixed to 3 billion, which makes it deflationary the more the XU ecosystem & demand grows.

The goal is to transform XU into a platform, where new products plug into XU to swap digital assets. We will support a set of new products on top of XU with entirely new business models.


## 4. How is XU different from ...


4.1. [Ripple](https://ripple.com/) / [Stellar](https://www.stellar.org/)
----------------------------------------------------------------------
- Exchange Union is trustless - always.
  - Ripple/Stellar requires building up a trust network for exchanging IOU-based tokens, which exposes involved parties to counterparty risk. This would cause loss of funds when an exchange would go bankrupt or get hacked and thus is not suitable for including unregulated exchanges.
- Exchange Union always transfers the native underlying asset. If for instance bitcoin and ethereum get exchanged, actual bitcoin and actual ethereum are transferred between the two exchanges
  - Ripple’s trustless approach uses XRP as intermediary, which proves to be risky in practice due to the lack of liquid XRP markets against other currencies
- Exchange Union transacts value via payment channels between two parties only; transactions are are secured by the underlying blockchains, e.g. bitcoin, ethereum or litecoin, which are block-history-based
  - Ripple/Stellar maintain balances in a Distributed Ledger, important is just the last commonly agreed ledger state
- Different target users: Exchange Union targets digital asset exchanges, which especially includes small, unregulated and unlicensed exchanges. This is possible due to the trustless nature of the Exchange Union network: no requirements to build up a trust network or maintain list of untrusted parties
  - Ripple/Stellar target traditional banks; which sort-of know and trust each other


4.2. [Blockstream’s Liquid](https://blockstream.com/liquid/)
------------------------------------------------------------
- Exchange Union is a decentralized mexchange infrastructure, exchange of assets happens on Exchange Union
  - Liquid simply lets users transfer bitcoin from Exchange A to B
- Exchange Union is (network-latency) instant
  - Liquid is a sidechain with about 1 min block times, in future down to 10s block times. But never instant.
- Exchange Union requires XUD, software only
  - Liquid requires special hardware
- Exchange Union is completely open source and free to use
  - Liquid requires exchanges paying a monthly fee (as of 2016: 2500 USD)
- Exchange Union automates a trade
  - Liquid asks users to handle of X, Q or H liquid addresses on both exchanges, needs verified accounts on both exchanges, need to manually trigger transactions
- Exchange Union benefits traders & exchanges, see [chapter 2](https://github.com/ExchangeUnion/Docs/blob/master/XU-TechnicalPaper.md#2-product-description)
  - Liquid benefits traders only


4.3. [Lightning](http://lightning.engineering/) / [Raiden](https://raiden.network/)
-----------------------------------------------------------------------------------
- Exchange Union *IS* Lightning & Raiden. It builds on top by using atomic swaps on Lightning & Raiden. It adds some features like order book propagation to build a fully functioning decentralized exchange.


4.4. [Alphapoint’s Remarketer](https://www.alphapoint.com/remarketer.html)
-----------------------------------------------------------------------------------------------------------
- Exchange Union has the same goal of increasing liquidity for exchanges, but is fully decentralized, open-source & free to use.
  - Alphapoint is a centralized commercial remarketer, requires fees and onboarding
- Exchanges in Exchange Union swap assets directly with each other
  - Alphapoint's software does buy and sell on exchange's behalf on another exchange. It requires multiple accounts and operational maintenance of balances on other exchanges.
- Exchange Union's infrastructure abstracts from different blockchains and supports all chains with a payment channel implementation
  - Alphapoint is limited to supported exchanges and FIAT currencies (USD)


4.5. [Altcoin.io](https://www.altcoin.io/) / [Etherdelta](https://etherdelta.com) / [AirSwap](https://airswap.io/) / [0x](https://0xproject.com/) / [IDEX](https://idex.market/)
----------------------------------------------------------------------------------------------------------------
- Exchange Union is a decentralized network of exchanges, targeting centralized exchanges as stage one ‘users’
  - All of the above mentioned are trader oriented and are not believed to reach mass adoption and critical liquidity in the near future because they are simply illiquid, too hard to use and too slow (on-chain based)
- Exchange Union targets to have a decentralized order book propagation to avoid a SPOF
  - All of the above provide access and order books via a centralized solution or don’t handle order book information exchange
- Exchange Union uses payment channels for atomic swaps - they are instant
  - All of the above mentioned use on-chain swaps which are costly and take up to one hour to complete. Only Altcoin.io announced plans to ‘explore’ lightning, but seems to have chosen [Plasma](https://blog.altcoin.io/plasma-dex-v1-launching-next-month-4cb5e5ea56f6) by now.
- Exchange Union supports multiple chains
  - Etherdelta/0x are limited to Ethereum-based assets (ERC20), altcoin.io promises BTC/ETH, but is still not live.

4.6. [sparkswap](https://sparkswap.com/)
----------------------------------------
Is a relatively new, well received project offering atomic swaps on lightning
- main difference are the target users institutional traders, hence the [ccxt](https://github.com/ccxt/) compatible API and really nice (!) CLI
- centralized spark swap order book, revenue goes to sparkswap
- a smaller technical difference lies in the details how atomic swaps were implemented (modified lightning invoices)

4.7. [RaidEX.io](https://www.raidex.io/)
----------------------------------------
Brainbot, the company behind the Raiden network, also explores the idea of a decentralized exchange based on payment channels. For now, it’s unclear if RaidEX will target users directly or Exchanges first, but it’s more likely to target individuals. Also, it will likely be limited to Ethereum-based assets on Raiden.

### Help us to improve!

`xud` is in alpha stage, as well as this tech paper. Please help us to improve by [opening issues (or even better PRs)](https://github.com/ExchangeUnion/Docs/issues).

Feel like talking? Chat with us on [Gitter](https://gitter.im/exchangeunion/lobby)!