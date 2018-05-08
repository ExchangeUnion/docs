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
    3.7.1. [The USDT Way](#371-the-usdt-way)  
    3.7.2. [The Bitshares Way](#372the-bitshares-way)  
    3.7.3. [The MakerDAO Way](#373-the-makerdao-way)  
    3.7.4. [The Basis (Basecoin) Way](#374-the-basis-basecoin-way)  
  3.8. [Exchange Earnings](#38-exchange-earnings)  
  3.9. [Incentivisation: The Role of XUC](#39-incentivisation---the-role-of-xuc)  
  3.10. [Business Model: XUC](#310-business-model---xuc)  
4. [How is XU different from](#4-how-is-xu-different-from-)  
  4.1. [Ripple/Stellar](#41-ripple--stellar)  
  4.2. [Blockstream's Liquid](#42-blockstreams-liquid)  
  4.3. [Lightning/Raiden](#43-lightning--raiden)  
  4.4. [Alphapoints Remarketer](#44-alphapoints-remarketer)  
  4.5. [Altcoin.io/Etherdelta/Airswap/0x](#45-altcoinio--etherdelta--airswap--0x)  
  4.6. [RaidEX.io](#46-raidexio)  
5. [Requirements](#5-requirements-overview)

## 1. Executive Summary

1.1. The Vision
---------------
Exchange Union (XU) connects digital asset exchanges by forming **a decentralized network**, which enables **instant and trustless** trades between digital asset exchanges. It enables cross-exchange trading for new trading pairs, access to the best price, and increased liquidity. Read the initial [White Paper](https://api.exchangeunion.com/pdf/en/ExchangeUnion-WhitePaper.pdf) which describes the idea and its use cases.

1.2. Project Intro
------------------
This specification describes and summarizes commonly agreed technical requirements for Exchange Union and thus should be referred to as “the bible” ;). Exchange Union’s implementation is challenging and can only be achieved with support of a greater developer community. The decision was made to open-source all documentation and the entire code base and to prioritize building a developer community, while also engaging full-time open-source developers to contribute to the development effort. Strategic partnerships with relevant companies will be formed. The business model for funding development is based entirely on Exchange Union's XUC token, which is already in circulation and doesn’t require an ICO.


## 2. Product Description
Exchange Union can be described as a *decentralized meta-exchange*, because it targets today's centralized exchanges as "users". This direction was chosen because the majority of users use and will use centralized exchanges for convenience and thus the liquidity is with centralized exchanges. Furthermore, executing and maintaining Exchange Union’s technology, XUD, short for **Exchange Union Daemon** is not expected to be suitable for personal or widespread usage in the near future. However, it is planned to further develop and simplify the technology and make it suitable for individual usage in the long-term. 

Simply put, Exchange Union’s vision is to offer individuals a choice to either:

**A)** Use a trusted and familiar exchange platform to handle the technical complexity of XU *or*

**B)** Use Exchange Union directly with taking on full responsibility of operating an XUD

For the foreseeable future, it is expected that the clear majority of users will choose option A, delegating the technical complexity and operational overhead of option B to an exchange. This complexity includes managing and securing private keys as well as running and maintaining a XUD, full nodes for each chain (light clients will be possible, but might need some optimizations), and Lightning Network and/or Raiden software to manage payment channels and routing. These users will opt to remain clients of digital asset exchanges. Furthermore, a unique selling position of today’s digital asset exchanges are the banking relationships and integrations that enable deposits and withdrawals of fiat currency, which most likely will never be realized by individuals. Ultimately, with XUC, Exchange Union enables exchanges to develop a sustainable business model for the decentralized future. It can be concluded that **Exchange Union is not cannibalizing the business of digital asset exchange platforms**, but rather making existing exchanges more versatile and attractive for users. New waves of retail investors in particular will rely heavily on easy-to-use interfaces and fiat currency deposit services.

The advantages for users:
- Improved liquidity by combining order books of participating exchanges
- Access to virtually all trading pairs
- Access to the best price
- All through one account and familiar UI

The advantages for exchanges:
- Access to a larger user base, anybody can trade on any exchange (eventually including individual users)
- Increased earnings through zero-fee-loss on outgoing trades, new incoming trades, and sophisticated fee and reward model with XUC - earnings only stand to increase
- Entire new product lines possible which use exchange union directly (wallets, merchant services, etc...)
- Robust, decentralized, and censorship-resistant trading infrastructure

Exchange Union doesn’t require trust between parties to exchange value. This allows virtually any exchange to join no matter the size or reputation. Furthermore, Exchange Union doesn’t need an intermediary currency to achieve the trustless transfer of different assets. Establishing a token as transfer intermediary has shown not to be effective in practice, as elaborated in [this example with XRP](https://medium.com/kilrau/dont-trust-me-why-ripple-is-not-the-future-of-money-d10081db1aaa).

As it is the case with existing decentralized exchanges, no single point of failure (SPOF) can cause a shutdown of Exchange Union, eliminating critical attack vectors and increasing censorship resistance. The main **difference from existing decentralized exchanges**, like [0x](https://0xproject.com), [etherdelta](https://etherdelta.com), [airswap.io](https://airswap.io/) or [altcoin.io](https://www.altcoin.io/) is that **trading on Exchange Union is instant**. Decentralized exchanges today execute trades on-chain. This is costly - transaction fees for bitcoin can exceed $5 - and the time it takes for a sufficient number of confirmations can be long and unpredictable. With Exchange Union trading works cross-chain, that means it is not limited to token of one specific chain. For instance, above mentioned Etherdelta and 0x are limited to ETH and ERC-20 tokens, whereas Exchange Union can expend any cryptocurrency supporting payment channels and [hashed timelock contracts](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts). Another differentiator is, that Exchange Union supports a decentralized exchange of order books.


## 3. Under the hood

**Exchange Union is the first instant, decentralized network of exchanges and also the first one supporting multiple chains**. This is realized via a novel technology combining ‘payment channels’ and ‘atomic swaps’. The XUD exposes a simple API designed for digital asset exchanges and, together with the XUD of other exchanges, forms the decentralized network connecting the exchanges. The XUD software will be provided in form of a package, for example a Docker image, that contains Payment Channel Protocol daemons (e.g. lnd and raiden), and full-node daemons for all supported chains (bitcoind, geth etc.) and a XU manager that controls those daemons and exposes an API to the exchange. 


3.1. Payment Channels
---------------------
The idea of payment channels is around for a while, even Satoshi Nakamoto already explored [*high-frequency transactions*](https://en.bitcoin.it/wiki/Payment_channels), then with bidirectional channels and the inception of the **Lightning Network** by Joseph Poon and Tadge Dryja, payment channels became more widely known. In short, payment channels enable instant, off-chain transfer of ownership of a digital token, like bitcoin, directly between two parties. Payment channels are opened and closed via one on-chain transactions each. Once a bidirectional payment channel is open, it uses unbroadcasted, fully valid signed transactions, which are sent back and forth between the two parties. These are held on each side of the channel, in our case on the exchange’s XUD, representing the balance of each party. These transactions will use a multi-signature address as their input (the funding address) and they will point at two different addresses for their output, each controlled by one party of the payment channel.

Through their sophisticated setup, payment channels are trustless even though only the opening and closing transaction are included in the underlying blockchain. This is because intermediary transactions are signed, thus valid and can be broadcast at any time by either party. Payment channels also allow secure, trustless routing of funds through intermediaries using a technology called [Hashed Timelock Contracts (HTLCs)](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts), thus a direct channel between two parties is not required and a path through other XUDs can be used. This is described in detail in the [Lightning White Paper](https://lightning.network/lightning-network-paper.pdf). This is useful since even a single payment channel to a well-connected XUD could be enough to reach the rest of the network. Setting up payment channels can be compared to the act of transferring coins into a sidechain - coins have to be committed to a payment channel via a native on-chain transaction.

Each participating exchange runs its own XUD instance, which opens payment channels on each chain it chooses to support. This means, if an exchange wants to enable its users to trade bitcoin and litecoin through Exchange Union, it has to open a payment channel on the litecoin blockchain AND on the bitcoin blockchain to at least one other exchange in the union. Payment channels in XU are meant to be kept open long-term. Also, the current design doesn't encourage high volume transactions in one single channel and most implementations currently set upper limits as a 'training wheels restriction' (e.g. lnd's max. channel size is: 0.16777216 BTC). To achieve scalability, we will rely on multiple channels, which carry out partial transfers of one atomic payment as proposed in the [new AMP protocol by Laolu and Conner Fromknecht](https://lists.linuxfoundation.org/pipermail/lightning-dev/2018-February/000993.html). This allows:
- to elegantly get around channel volume restrictions
- to elegantly increase/decrease volume when needed by simply opening/closing new channels
- achieve better connectivity (direct channels) between different XUDs


In general, all BIP 199 compatible payment channels can be supported directly, others might need to find a way to implement HTLC swaps via smart contracts or similar. The payment channel standard on bitcoin and litecoin is called ‘[lightning](https://github.com/lightningnetwork/lnd)’ and compatible implementations are maintained by the companies [ACINQ](https://github.com/ACINQ/eclair), [Lightning Labs](https://lightning.engineering/) and [Blockstream](https://blockstream.com/). Compatibility was only achieved [recently](https://medium.com/@lightning_network/lightning-protocol-1-0-compatibility-achieved-f9d22b7b19c4), implementation of such is still ongoing. Ethereum’s payment channel technology is called the ‘[Raiden Network](https://raiden.network/)’ and is maintained by the company [Brainbot](http://www.brainbot.com/). **XUDs are a subgraph of the lightning & raiden network, which means it is ensured that the XUD implementation remains fully compatible with the [lightning protocol specifications (BOLT)](https://github.com/lightningnetwork/lightning-rfc/blob/master/00-introduction.md) and also with the raiden network to support the growth and adoption of payment channels.** XUD acts as a wrapper and encapsulates lightning and raiden daemons with as little modifications as possible.

Also, for payment channels to fully work, transaction malleability has to be fixed on the underlying chain. This happened with the activation of SegWit on Bitcoin, Litecoin and some other Bitcoin-based chains like Vertcoin in 2017; Ethereum’s transaction malleability is already fixed since the Homestead release in 2016.


3.2. Atomic Swaps
-----------------
The actual exchange of the two assets happens via so-called ‘atomic swaps’ directly between exchanges on the respective payment channels. Atomic swaps use slightly modified HTLCs, a manifestation of the first successful lightning atomic swap between BTC and LTC can be found [here](https://blog.lightning.engineering/announcement/2017/11/16/ln-swap.html). Token swaps of ERC20 tokens are also supported by the Raiden Network as described [here](https://raiden-network.readthedocs.io/en/stable/api_walkthrough.html#token-swaps) and were [successfully tested by the Exchange Union team.](https://blog.exchangeunion.com/milestone-atomic-swap-of-erc20-tokens-on-the-raiden-network-84b918b86fc4)

In simple terms, this means, using the BTC/LTC example, real bitcoin flow from exchange A to exchange B on their bitcoin payment channel and real litecoin from exchange B to exchange A on their litecoin payment channel in real-time. The atomic swap with it's special HTLC ensures the atomicity of the swap: either the swap completes in full, or nothing happens.

Payment channel atomic swap technology is fairly new and was just recently successfully tested between [Bitcoin & Litecoin](http://bitcoinist.com/first-ever-cross-chain-atomic-swap-between-bitcoin-and-litecoin-has-now-taken-place/), [Bitcoin & Zcash](https://github.com/zcash-hackworks/zbxcat) and [Bitcoin & Ethereum](https://www.coindesk.com/bitcoin-ethereum-atomic-swap-code-now-open-source/) (via a smart contract [[1]](https://github.com/AltCoinExchange/ethatomicswap), [[2]](https://medium.com/@DontPanicBurns/ethereum-cross-chain-atomic-swaps-5a91adca4f43)). More [research](https://github.com/raiden-network/raiden/issues/399) is needed to achieve full compatibility between different HTLCs ([BIP 199](https://github.com/bitcoin/bips/blob/master/bip-0199.mediawiki)) on different chains. 

Since Ethereum includes all ERC20 tokens, Exchange Union supports the vast majority of today’s token market. [Other important chains](https://storeofvalue.github.io/posts/what-is-qtum-without-the-bullshit/) are expected to start developing Lightning/Raiden-style payment channel technology within the next months. Options how to support some in the short term, e.g. via [ERC20 wrapper](https://weth.io/) technology, can be explored.


3.3. Decentralized Order Book (DOB)
--------------------------
The DOB is a key task for XU and an unresolved problem on existing decentralized exchanges where, to the best of our knowledge, the order book part is realized in a centralized way e.g. [1](https://github.com/etherdelta/etherdelta.github.io/blob/master/docs/API.md) or [2](https://github.com/0xProject/standard-relayer-api/blob/master/http/v0.md). We believe order book propagation and matching has to be decentralized to the best degree possible to sustain the robustness and censorship resistance of the network.

DOB's mainly face the following issues:

**Latency**

The speed of order updates is crucial to keep order information as accurate as possible and also minimize noise ('useless' traffic) on the network resulting from outdated order information, such as nodes requesting to fill orders that are no longer available. Since we target to on-board high-volume exchanges, this network load is potentially huge. The same goes for delays in order filling, resulting from continuously failing orders because of outdated order information.

**Point of Execution**

There is no central point of execution (order matching) in Exchange Union's open, decentralized system, which exposes several new attack vectors: For instance, a node exploiting network latency could request multiple nodes to fill its order and trigger the execution of swaps. Even though the usage of atomic swaps prevents loss of funds (if I send A, you have to send B), it can potentially lead to large amounts of assets being locked due to the time-lock used in atomic swaps to resolve disputes. Large scale market manipulation with this technique could make this a profitable endeavor, which is unacceptable for exchanges.

**SPAM**

Like any other decentralized system, Exchange Union's DOB faces DDoS attacks, in particular fake order information.

XU's DOB protocol has the following goals:
=============================================================
1. Minimize latency of order updates
2. Single point of execution
3. Reward liquidity providers
4. Punish malicious behavior
5. Privacy between two trading parties

Order matching systems ('trade engines') of major digital asset exchanges differentiate between two participants: the taker and the maker. A maker is someone who submits an order which can't be matched immediately and joins a pool of unmatched orders. A taker on the other hand issues an order which can immediately be matched with an existing (maker) order upon entering the pool. A taker ‘fills’ the order of a maker. Makers are sometimes also called 'liquidity providers' and play an important role in stabilizing markets and thus are rewarded with lower or 0% fees on centralized exchanges, which is why we chose a similar incentivization model for XU. In short, a taker -> pays the maker as a reward for the provided liquidity. Details below.

Analogical, we distinguish between two types of participants in the DOB for each trade:
- The **Maker**, the liquidity provider propagating an order, which joins a pool of unmatched orders. Usually in the form of a limit order.
- The **Taker**, the impatient buyer or seller filling above limit orders. Usually in the form of a market order.

A description of how we realize above goals:

1. Minimize latency of order updates

For orders, the DOB protocol follows the first come, first served principle, which remains fully compatible with how order book systems of centralized exchanges work. To get the best achievable latency between two nodes which intend to receive order updates from each other, XUD requires a direct socket connection on Internet Protocol level without intermediary hop - a full mesh network. Direct payment channels are optional, payments can be routed through intermediary hops.

![Screenshot](images/Full%20Mesh%20vs.%20Partial%20Mesh.png "XUD DOB Full Mesh vs. Payment Channel Partial Mesh")

Obviously, direct socket connections between peers to exchange order book information can't scale infinitely. We are targeting to establish a relay network of 'super nodes' in a later phase. Individuals running XUD can choose to receive & forward order book information from these super nodes. To become a super node, one must provide a fast and reliable connection between peers. This can be achieved for example through a strategic location on a fiber connection, where peers wouldn't have direct access to and help to further scale and speed-up order book information exchange. [Efficient congestion control mechanisms](https://github.com/google/bbr) to further optimize latency are currently being evaluated.

2. Single point of execution 

There is one single point of execution for each order: the maker. The maker decides on which incoming taker order gets to fill the order. All orders contain payment information (e.g. a lightning invoice). Lightning implementations take care that an invoice can only be paid by the first one to successfully send a payment to an invoice issuer, in our case the maker. Also lightning takes care that a taker has a payment channel path with sufficient volume for a specific order. Under no circumstances a loss of funds can occur. Side note: Transferring order execution authority to a third party, in particular to a decentralized consensus, was considered and deemed not feasible for the following reasons: a) consensus=slow b) hard to sell; an exchange won't let anyone else decide on who fills its users orders c) hard to control since in our case exchanges could always treat their local order book with priority and are essentially in control since they control the release of a preimage of an invoice.

3. Reward liquidity providers

Makers are earning XUC, takers are paying for filling orders. Makers set a price for each order in a specified fee field, in XUC. We aim to establish a fee market where makers are competing with a combination of order quantity, price and XUC fee. A taker pays the full XUC fee via a default Raiden Channel to the maker. It's down to exchanges to take over the role of handling XUC payments and earnings for their users or integrate this as a business model for traders. 

4. Punish malicious behavior

Market makers are required to stake a certain amount of XUC in a smart contract in order to be able to act as a market maker. This will be used to execute punishments, for example if a makers XUD creates HTLCs for a payment, but never releases the preimage to execute them and thus locks up funds of other exchanges.

5. Privacy between two trading parties (not part of PoC)

As it is the case on regular digital asset exchanges, a maker and a taker should have the option to remain anonymous to avoid biased behavior and preserve privacy. On payment channels this is taken care of by [onion routing](https://github.com/lightningnetwork/lightning-rfc/blob/master/04-onion-routing.md). A tor-hidden service for order book information exchange is currently being explored, but is not a priority for the PoC for now.

Also, there will be no complete global order book as such, containing all orders for each and every trading pair. Instead, orders will be propagated based on an XUDs preferences, e.g. only specific trading pairs or only from specific peers. Therefore, XUDs only maintain relevant parts of the order book. XUDs constantly exchange order information and updates with connected peers.

[XUD Order Book Data Structure](https://github.com/ExchangeUnion/xud/blob/master/lib/orderbook/)
=============================================================

```
currencies {
Id (string - the currency symbol)
}
```

This table is automatically maintained by xud with currencies which have a well-synced wallet and open payment channels to minimum one other xud.

```
pairs {
Id (string - `{baseCurrencyId}/{quoteCurrencyId}`)
baseCurrencyId
quoteCurrencyId
swapProtocol ({LND, RAIDEN})
}
```

This table is automatically maintained by xud with pairs which are swap-enabled via payment channels.

```
Orders {
Id
pairId
peerId
quantity (decimal)
price (decimal)
]
```

Table will maintain the open orders which are received by peer nodes (defined in Peers table in the P2P domain database). A `peerID` is null for the exchange's own orders, the `quantity` is positive for buy orders, negative for sell orders.

```
orderThresholds {
Id
pairId
type({disable, peerId, minPrice, maxPrice, minQuantity})
value
}
```

This table defines filters for a particular `pairID` which are preferable to the exchange. For example filter out orders from a specific `peerID` or filter out orders below a minimum `quantity`, which exchanges typically have defined individually for each pair. General blacklisting of `peerIDs` is defined in the Peers table in the P2P domain database.

[XUD Order Book API](https://github.com/ExchangeUnion/xud/blob/master/lib/rpc/RpcMethods.js)
=============================================================
*`getOrders`*   
get orders from the XUD orderbook, called by the exchange to display XU orders in it's UI

*`getpairs`*    
get the orderbook's available pairs

*`placeOrder(pairId, price, quantity)`*   
Place an order in XUD, which will first attempt to get matched with existing orders, else propagated to connected peers.
```
Example: "I want to place a limit order of buying 10 LTC for 20 ETC."
placeOrder(LTC/ETC, 20, 10)

Example: "I want to place a market order of buying 10 LTC for ETC."
placeOrder(LTC/ETC, 0, 10)
```


3.4. Security
-------------
The Lightning Network and the Raiden Network take care of security of funds on the payment channel rails. This includes resolving disputes over balances on the underlying blockchain and disincentivizing malicious behavior by POS-style punishment (a dishonest party will lose all her funds to the honest party). It especially also includes security when routing payments through intermediary hops. Atomic Swaps bind two payments of a trade together, so that in no event one party can disappear after receiving the first part of the trade. Both payments in a trade have to complete successfully, for both payments to be valid and spendable. Exchange Union is secured against malicious order information in a similar POS-style punishment manner, to disincentivize market manipulation by sending out false order information.

XUD's ID Model
=============================================================

During setup, a XUD automatically creates a public/private key pair (secp256k1 ECDSA) specifically for identification and order signing purposes, the `XUD_ID`. The public key represents one XUD's unique ID within Exchange Union. Exchanges can let others know what their XUD's public key is by making such available on their website. In this way, exchanges in jurisdictions which require licenses can create a white-list and only connect to other exchanges which fulfill the local jurisdiction's legal requirements. This is achieved by signing orders.

XUD's Order Signing
=============================================================

XUD enables two modes for signing an order:
1. Sign order to verify authenticity of public key
2. Sign & encrypt order to verify authenticity and enforce white-list

*Mode 1* is the default and signs all orders with the XUD's private key. This ensures that the `XUD_ID` carried in the order's invoice is the actual ID of the XUD sending the invoice. We decided to place the `XUD_ID` directly in the invoice description field for a faster processing, since this avoids a payment hash vs. `XUD_ID` lookup for each incoming new order. This mode prevents XUD's pretending to be someone else and knowing the actual issuer of an order is important for the incentive & punishing mechanism to work. This mode connects to all available peers and is suitable for exchanges located in jurisdictions without a strict legal framework for digital assets. 

*Mode 2* is optional and additionally to everything *Mode 1* does, XUD encrypts orders with the public key of each white-listed peer. This mode is more resource intensive and requires maintaining a white-list of peers. It is suitable for exchanges located in jurisdictions with a strict legal framework to ensure trading is only enabled with users from licensed exchanges.

3.5. Visualization: Architecture
-----------------------------------------------------
![Screenshot](images/Exchange%20Union%20Nodes.png "Exchange Union System")

And an overview of XUD's components:

![Screenshot](images/xud%20components.png "XUD components")


3.6. Sample Trade
-----------------
This is the basic flow of a trade via XU:

3.6.1. Assuming `Exchange A` is an Exchange Union member and it wants to offer a rare trading pair to its users, **BTC x UnicornCoin**. In order to successfully execute this trade, `Exchange A` should run a XUD which handles opening payment channels, running full nodes and matching trades. In our case, the XUD is configured to open payment channels and run full nodes on both, the Bitcoin and the UnicornCoin network. Opening payment channels requires depositing funds into a channel with a so-called ‘funding transaction’. The deposited BTC amount, let’s say 100 BTC represents the total trading volume which `Exchange A` can execute using BTC.

3.6.2. `Exchange A` has successfully opened a payment channel to minimum one other XUD on Bitcoin and on UnicornCoin and uses a socket connection to its direct peers to send a limit order. This order, which is similar to a regular buy or sell request submitted in a cryptocurrency exchange, includes the amount of requested UnicornCoin with a price in BTC. Let’s assume `Exchange A` sends an **order for selling 100 UnicornCoin** with the requested price of **1 UnicornCoin = 0.1 BTC**. This order is published to all connected peers using a mechanism similar to ‘invoice adding’ and ‘subscription’ in the Lightning client.

3.6.3. XU members on UnicornCoin update their order books based on this new order.

3.6.4. If another XU member, `Exchange B`, wants to fill this 100 UnicornCoin order, the XUD takes care of the cross-chain atomic swap which means `Exchange A` gets **100 UnicornCoin** and `Exchange B` gets **10 BTC** on their payment channels on the respective chains. Behind the scenes, both parties create transactions on both chains to setup a trade which imposes time constraints i.e. if a trade is not successful both parties get their money back. Order filling is realized on a first-come-first-served basis.

Exceptions:
- An exchange can choose to subscribe to the order book of particular exchanges only (e.g. through legal requirements) via the exchange's public XUD ID.
- Order is filled by another user who was faster: XUD starts looking for the next best order. We use the ‘odd type’ messaging system as specified in the RFC. E.g. Type 17 for errors.
- Exchange B goes offline: time-out message comes back and XUD starts looking for the next best order. Exchange B repeatedly doesn’t fill orders: punishment, see chapter 3.4. 
- Trade volume for a particular asset exceeds (own) payment channel volume: XUD maintains a configurable threshold and automatically redeposits from an authorized hot wallet.


3.7. FIAT Tokenization / Stablecoin
-----------------------------------
To support FIAT/Crypto pairs, FIAT has to be ‘tokenized’ to be tradable on Exchange Union. Also it has to fulfill the requirement to be trustless, thus a simple USD or EUR IOU won’t do. Several approaches exist:

3.7.1. The [USDT](https://tether.to/) way
-----------------------------------------
Tether (the company) holds one USD in a “reserve bank account” for every USDT issued. And ideally be transparent about it.

**Good** This would work for all FIAT currencies that our bank supports

**Bad** History has shown that this is problematic for the following reasons:
- Even if I’m 100% transparent, the bank could just cut ties with me, as [happened multiple times with Tether Limited](https://news.bitcoin.com/tether-coin-may-be-a-precarious-us-dollar-peg/). News go public, Markets panic
- If I’m rather semi-transparent, like Tether Limited, people have to put trust and faith in Tether to really have those USD on their bank account. And to handle bank relationships. It simply is not trustless.
- We would have to act like a central bank and issue new USDX tokens to increase supply based on demand. This requires us to hold USDX somewhere under central control, which is a SPOF for hackers. [As happened with Tether.](https://techcrunch.com/2017/11/20/tether-claims-a-hacker-stole-31m/)


3.7.2. The [BitShares](https://bitshares.org/technology/price-stable-cryptocurrencies/) Way
-------------------------------------------------------------------------------------------
A smart contract holding 200% of the underlying amount worth in BTS, 100% as collateral. E.g. One bitUSD contains 2 USD worth of BTS at creation.

**Good**
- Tested for years, proves self-adjusting through ‘soft-peg’, traders will go short/long awaiting the coin to re-stabilize. See the BUT below.
- We could do our own version using XUC instead of BTS, good for XUC

**Bad**
- BUT The system only works because people BELIEVE the coin re-stabilizes and because it’s backed by Bitshares Company as lender of last resort which ensures that as a sort-of central bank. Nothing in the protocol automatically ensures bitUSDs peg to be one USD. It could be 0.5 if whales in the market go rogue and short accordingly.
- Requires exchanges to lockup minimum 200% of the FIAT trading volume
- Emergency liquidation is untested and will run into issue if a huge asset like bitUSD suddenly has to get emergency liquidated.


3.7.3. The [MakerDAO](https://blog.makerdao.com/2017/06/05/introducing-sai/) way
--------------------------------------------------------------------------------
DAI = Basically BitShares+: Collateral in Smart Contract (they call it Collateralized Debt Positions (CDPs)), users can create DAI’s which are backed by ETH, in future other (ETH-based) assets. Target Rate Feedback Mechanism ([TRFM](https://medium.com/cryptolinks/maker-for-dummies-a-plain-english-explanation-of-the-dai-stablecoin-e4481d79b90)) creates user incentives to HOLD or BORROW Dai and thus correct the price to a given price target, like 1 USD. It gets triggered if the price goes off by a ‘sensitivity parameter’ which is set by MKR token holders in POS style. TRFM increases the target rate if price is too low, which causes generation of new DAIs to get more expensive and gains for holding DAIs to increase. This reduces supply causes price to increase back to target price. Works the other way round.

**Good**
- We could use XUC as collateral, since it’s ERC20
- Several fallback mechanisms, global settlement being the last resort

**Bad**
- If we use XUC as collateral, it would again require a very liquid local FIAT/XUC pair
- Completely trusts one price oracle (only for start, decentralized price oracles are mentioned in the paper)
- For now, ETH has to be wrapped into PETH
- MKR as ‘stability fee’ - do I need to pay to hold DAI over time?
- Why are MKR holders to decide on the sensitivity parameter? It should be just pre-set. Sounds a bit like a forced use and potentially could be manipulated


3.7.4. The [Basis](http://www.basis.io/) (Basecoin) Way
------------------------------------------------------
A robust, price-stable cryptocurrency with an algorithmic central bank. It monitors USD or any other price (e.g. EUR or CPI) through external oracles, and smart contracts adjust the basecoin supply according to current market supply and demand (the basecoin system is aware of both) to keep the value close to the pegged asset. Basically what a central bank does. But the monetary policy gets handed over to a decentralized algorithm.

**Good** 3 coin system:
- basecoin: Pegged with USD (“baseUSD”) or to any other asset, directly faces users
- base bonds: tokens to be ‘auctioned off’ programmatically by the blockchain when supply needs to be reduced. 1 base bond will be exactly 1 basecoin in future when supply needs to be increased
- base shares: fixed-supply cryptocurrency that does not have a peg, holders of these will get basecoin when supply needs to be increased. Free money for initial investors?

**Bad**
- It’s not backed by anything
- It's untested. How fast can the system respond? How fast do prices adjust - long-term peg is not sufficient for trading purposes.
- Issuing new base coins is relatively easy. But buying back coins relies on a super liquid market of base bonds to buy back base coins. Speculators need to trust their base bonds will be worth more in the future when the basecoin supply is extended. All this needs to have a proven track record to work. It strongly depends on initial market management. Also, they promise an interest rate – not clear how this is done.
- How exactly are dead spirals avoided?
- Any fallback mechanisms?

**Conclusion** For now MakerDAOs DAI seems promising and a thought-through model. Exchange Union’s goal is to utilize one solution to tokenize FIAT and hide the complexity from the exchange.


3.8. Exchange Earnings
----------------------
**Executive summary: worst-case, exchanges will earn the same fees as compared to not being connected to Exchange Union. In almost all cases revenues are increased.**

How this is achieved: The XUD implementation enables the exchange to simply ‘keep’ the trading fee before the actual amount of a trade is sent over to the counterparty. This is how exchanges continue earning fees in general. Since exchanges are usually earning fees from both sides of a trade (maker & taker fee), Exchange Union’s software, the XUD, allows exchanges to charge the equivalent amount of maker+taker fee for one side of the trade. Naturally, both parties of a trade will want to do this. Consequently, every trade facilitated via Exchange Union enables two exchanges to earn the full maker & full taker trading fee. This means, even if all trades of an exchange ‘leave the platform’ and are executed on XU, the above described worst-case scenario, earnings stay exactly the same as if the trades would happen locally. It also means, that in the long run earnings will go up for exchanges for every trade executed via XU, since fees are earned twice for every trade on Exchange Union.

How this still benefits the trading individual: Trades only happen via XU, if:
```
(ExchA_price) > (ExchB_price+ExchA_fees+ExchB_fees)
```

Consequently, the same is true for traders: they can only end up with better prices. If the price difference is not large enough to cover the fees kept by the exchanges, the trade won’t happen via XU. It is expected that through market forces (arbitrage) price levels keep adjusting and not all trades leave the exchange. Through the real-time nature of XU, market forces should be able to adjust prices much faster than currently.

Additional revenue streams for exchanges are introduced in the following.



3.9. Incentivisation - The Role of XUC
--------------------------------------

Developers, exchanges and functionaries which contribute resources to the XU ecosystem are key for Exchange Union to get up and running, reach a ‘network-effect’ size and be successful in the long-term. We believe a functioning monetary incentive system is the success factor for future Open-Source projects. Bitcoin itself is the best example for a system driven by monetary incentives to build and maintain an ecosystem. Similarly, we believe Exchange Union creates value and should be financially self-sustaining.

Flawed incentivisation is believed to be the main reason why other solutions for connecting exchanges didn’t find adoption yet. Some solutions even require exchanges to pay hardware or monthly fees. From an exchange's point of view, this doesn’t add up without offering something in return, since the solutions essentially offer an easier way for user funds, and thus trading revenues, to leave the platform. Exchange Union will not charge any fees and, more importantly, will enable exchanges to increase revenue with trading fees as described above. Additionally, the different functionaries which are rewarded with XUC are as follows:

**Incentives for Developers**
- Each pull-request to the [Exchange Union github](https://github.com/ExchangeUnion) gets peer reviewed and a score by each reviewer. XUC is paid to the contributor based on that score.
- When the pull-request makes it into the master-branch, another XUC amount gets paid to the developer
- Reviewers get rewarded
- Testers get rewarded, test-net time gets rewarded

**Incentives for Exchanges**
- As discussed in chapter 3.3, the main use of XUC is to incentivize liquidity providers, which in particular targets 'large' exchanges
- In order to further increase interest of exchanges joining Exchange Union, especially in the beginning, the idea is to allocate a certain amount of XUC to new exchanges joining Exchange Union. These are to be distributed to an exchange's users ('airdrop'). Depending on the business model of the exchange (exchange pays taker's XUC fee, or user directly), the advantage would be that all users can start using XU right away.
- These XUC can only be used to pay for services on XU, like paying XUC to the maker. It cannot be sold on the market to ensure market stability.
- If the exchange decides to let end-users earn and pay XUC for trades, *end-users have to hold XUC in order to use XU as a taker*. This is beneficial for exposure, since this might be the only way end-users learn about Exchange Union. Compare Binance’s BNB model here, which gives discount on fees for holders of BNB. Similarly, with XUC, users can get access to new pairs and a better price. For this the XUD would have to check a particular users XUC balance. Exchanges can offer a BNB-like incentive model to get better prices to their users.

**Incentives for Functionaries which provide services (running a XUD)**
- Order book relays by super nodes will get awarded with XUC in a later stage. Peers choosing to obtain order information from a super node which provides a faster connection, will pay for this service.
- Watchtower service gets rewarded with XUC: especially individuals in the future will be interested in this service - in order for payment channels to be secure, the main chain has to be watched for illicit transactions, as introduced in the beginning of chapter two. If someone is not sure to be able to be online in certain time intervals, this responsibility can be outsourced to someone else who charges a tiny fee for this service. 
- **We support Lightning/Raiden adoption and growth** with above incentives also help to [solve the maintenance problem of such nodes](https://www.youtube.com/watch?v=dlJG4OHdJzs) (the exchange wants maintain the XU software anyways to be able to use it) 

**Incentives for traders**
- A user has X trading volume on Exchange Union, this user gets rewarded with XUC from his exchange platform
- This is a program of each individual exchange

*By default, each XUD opens a XUC payment channel to be able to pay makers, for order book relays and other services and also receive XUC payments for these services.*

3.10. Business Model - XUC
-------------------------
Exchange Union’s business model is based on XUC being useful and thus valuable. As discussed above, the main usage of XUC within XU is as fee payment from taker to maker, the liquidity provider. XUC’s overall supply is fixed to 3 billion, which naturally makes it deflationary the more the XU ecosystem & thus demand grows. The original proposal for Exchange Union used sidechains where every transaction would burn a small amount of XUC as a way to combat spam. However, the concern about spam transactions are mitigated to good extent by using payment channels.

Nevertheless, with a burn-per-trade we will burn the 30% excess supply (Last ‘reserve’ part of the token allocation in the white paper), slowly reducing the total supply to roughly 2 billion. A ‘transparency’ section on [the website](https://www.exchangeunion.com/) informs about every new XUC release and purpose. Smart contracts handle the burning.

The goal is to transform XU into a platform, where new products plug into XU to swap digital assets. Exchange Union will be developing a set of new products on top of XU with entirely new business models.


## 4. How is XU different from ...


4.1. [Ripple](https://ripple.com/) / [Stellar](https://www.stellar.org/)
----------------------------------------------------------------------
- Exchange Union is trustless - always.
  - Ripple/Stellar requires building up a trust network for exchanging IOU-based tokens, which exposes involved parties to counterparty risk. This would cause loss of funds when an exchange would go bankrupt or get hacked and thus is not acceptable.
- Exchange Union always transfers the native underlying asset. If for instance bitcoin and ethereum get exchanged, real bitcoin and real ethereum and transferred between the two involved parties
  - Ripple’s trustless approach uses XRP as intermediary, which proves to be risky in practice due to the lack of liquid XRP markets against all currencies.
- Exchange Union transacts value via payment channels between two parties only; transactions are are secured by the underlying blockchains, e.g. bitcoin, ethereum or litecoin, which are block-history-based
  - Ripple/Stellar maintain balances in a Distributed Ledger, important is just the last commonly agreed ledger state, history doesn’t matter.
- Different target users: Exchange Union targets digital asset exchanges, which especially includes small, unregulated and unlicensed exchanges. This is enabled by the trustless nature of the Exchange Union network: no requirements to build up a trust network or maintain list of untrusted parties
  - Ripple/Stellar target traditional banks; which sort-of know and trust each other


4.2. [Blockstream’s Liquid](https://blockstream.com/liquid/)
------------------------------------------------------------
- Exchange Union is a decentralized meta-exchange infrastructure, exchange of assets happens on Exchange Union
  - Liquid simply lets users transfer bitcoin from Exchange A to B
- Exchange Union is (network-latency) instant
  - Liquid is a sidechain with about 1 min, in future down to 10s block times. But never instant.
- Exchange Union requires a XUD, software only
  - Liquid requires special hardware
- Exchange Union is completely open source and free to use
  - Liquid requires exchanges paying a monthly fee (as of 2016: 2500 USD)
- Exchange Union automates a trade
  - Liquid asks users to handle of X, Q or H liquid addresses on both exchanges, needs verified accounts on both exchanges, need to manually trigger transactions
- Exchange Union benefits traders & exchanges, see [chapter 2](https://github.com/ExchangeUnion/Docs/blob/master/XU-TechnicalPaper.md#2-product-description)
  - Liquid benefits traders


4.3. [Lightning](http://lightning.engineering/) / [Raiden](https://raiden.network/)
-----------------------------------------------------------------------------------
- Exchange Union builds on top of lightning & raiden, it’s using pure Lightning/Raiden with atomic swaps in the background and adds some features like order book propagation to build a fully functioning decentralized meta-exchange


4.4. [Alphapoint’s Remarketer](https://www.alphapoint.com/remarketer.html)
-----------------------------------------------------------------------------------------------------------
- Exchange Union has the same goal of increasing liquidity for exchanges, but is fully decentralized, open-source & free to use.
  - Alphapoint is a centralized commercial remarketer, requires fees and onboarding
- Exchanges in Exchange Union swap assets directly with each other
  - Alphapoint's software does buy and sell on exchange's behalf on another exchange. It requires multiple accounts and manual maintenance of balances
- Exchange Union's infrastructure abstracts from different blockchains and supports all chains with a payment channel implementation
  - Alphapoint is limited to supported exchanges and FIAT currencies (USD)


4.5. [Altcoin.io](https://www.altcoin.io/) / [Etherdelta](https://etherdelta.com) / [AirSwap](https://airswap.io/) / [0x](https://0xproject.com/)
----------------------------------------------------------------------------------------------------------------
- Exchange Union is a decentralized network of exchanges, targeting today’s centralized exchanges as ‘users’ because liquidity currently is and will be here
  - All of the above mentioned are end-user oriented and are not believed to reach mass adoption and critical liquidity in the near future because they are simply too hard to use, too slow and too expensive for now
- Exchange Union targets to have a decentralized order book propagation to avoid a SPOF
  - All of the above provide access and order books via a centralized solution or don’t touch order book information exchange between individuals
- Exchange Union uses payment channels for atomic swaps - they are instant
  - All of the above mentioned use on-chain swaps which are costly and take up to one hour to complete. Only Altcoin.io announced plans to ‘explore’ lightning.
- Exchange Union supports multiple chains
  - Etherdelta/0x are limited to Ethereum-based assets (ERC20), altcoin.io promises BTC/ETH, but is still not live.


4.6. [RaidEX.io](https://www.raidex.io/)
----------------------------------------
Brainbot, the company behind the Raiden network, seems to be the only company already working on a decentralized exchange based on payment channels. For now, it’s unclear if RaidEX will target users directly or Exchanges first, but it’s more likely to target individuals. Also, it will be limited to Ethereum-based assets on Raiden. Exchange Union will closely collaborate with brainbot.



## 5. Requirements Overview
| Description          | Comments      | Solved in XU?  |
| -------------------- |:-------------:| -----|
| Trustless Swap | Swap assets trustless without middle-man | Yes |
| Trustless Transfer | Real token transfer, no IOUs | Yes |
| Fully Decentralized Infrastructure | Each exchange runs XUD, order book decentralized, swap decentralized | Yes |
| Instant TX finality | No second settlement phase, network latency real-time | Yes |
| Incentivisation | Honest behavior=reward, Provide service=reward, Malicious behavior=punishment | Yes |
| Confidential | Tx’s payment data visible for involved parties only | Yes |
| Smart Contract or ‘Scriptless scripts’ support | Needed for smart collateral or unforeseen future purposes | Depends on chain |
| XUC usage | Incentivisation + Fee | Yes |
| User knows exchange rate & fee in advance | Defined metadata standards, pre-quote exchange rate to user | Ordersbooks are propagated, order will contain amount and exchange rate similar to submitting a trade request on a centralized cryptocurrency exchange. |
| Any currency can be converted to any currency | multiple assets, path finding if no direct pair exists | No, later phase |
| FIAT token support | Support conversion from Exchange's FIAT to tokenized FIAT | No |
| Simple value transfer from user a to user b (no swap) | Support of multiple transaction types with different characteristics (e.g. atomic, partial), requires unique ID for users (idea: civic) | No, later phase |
| Decentralized, secure updates for exchanges | Automatic | No |
