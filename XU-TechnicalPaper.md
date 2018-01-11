# [Exchange Union](https://www.exchangeunion.com) - The first instant decentralized Exchange

**Table of Contents**
1. [Executive Summary](#1-executive-summary)  
  1.1. [The Vision](#11-the-vision)  
  1.2. [Project Intro](#12-project-intro)
2. [Product Description](#2-product-description)  
3. [Under the hood](#3-under-the-hood)  
  3.1. [Payment Channels](#31-payment-channels)  
  3.2. [Atomic Swaps](#32-atomic-swaps)  
  3.3. [Security](#33-security)  
  3.4. [Orderbook Propagation](#34-orderbook-propagation)  
  3.5. [Visualization](#35-visualization-an-instant-decentralized-exchange)  
  3.6. [Sample Trade](#36-sample-trade)  
  3.7. [Fiat Tokenization](#37-fiat-tokenization--stablecoin)  
    3.7.1. [The USDT Way](#371-the-usdt-way)  
    3.7.2. [The Bitshares Way](#372the-bitshares-way)  
    3.7.3. [The MakerDAO Way](#373-the-makerdao-way)  
    3.7.4. [The Basecoin Way](#374-the-basecoin-way)  
  3.8. [Exchange Earnings](#38-exchange-earnings)  
  3.9. [Incentivisation: The Role of XUC](#39-incentivisation---the-role-of-xuc)  
  3.10. [Business Model: XUC](#310-business-model-xuc)  
4. [How is XU different from](#4-how-is-xu-different-from-)  
  4.1. [Ripple/Stellar](#41-ripple--stellar)  
  4.2. [Blockstream's Liquid](#42-blockstreams-liquid)  
  4.3. [Lightning/Raiden](#43-lightning--raiden)  
  4.4. [Alphapoints ADLP](#44-alphapoints-adlp)  
  4.5. [Altcoin.io/Ethere ](#45-altcoinio--etherdelta--0x)  
  4.6. [RaidEX.io](#46-raidexio)  
5. [Requirements](#5-requirements)


## 1. Executive Summary

1.1. The Vision
---------------
Exchange Union (XU) connects digital asset exchanges by forming the **a decentralized exchange**, which enables **instant and trustless** trades. Use cases are cross-exchange trading for access to new trading pairs, access to the best price and increased liquidity. Read the initial [White Paper](https://www.exchangeunion.com/how_it_works), which describes the use case.


1.2. Project Intro
------------------
This specification summarizes commonly agreed requirements for the Exchange Union technology (and should be referred to as “the bible” ;). Exchange Union’s technical implementation is challenging and can only be achieved with support of a greater developer community. The decision was made to open-source all documentation, the entire code base and make it a main objective to have a coordinating function for building a developer community. Strategic partnerships with relevant companies will be formed, the business model for funding the development is entirely based on the XUC token, which is already in circulation, doesn’t require an ICO or similar and is introduced in the chapters below.


## 2. Product Description
Exchange Union connects digital asset exchanges and offers a value proposition which can be best described as a decentralized meta-exchange. Meta-exchange, because phase one targets todays centralized exchanges as ‘users’. This is primarily because the trading user base sits here and also handling and maintaining of Exchange Union’s technology, the XU node, is simply not expected to be suitable for personal mass usage in the near future. However, it is planned to further develop and simplify the technology for individual usage in later phases. Exchange Union’s vision is to offer individuals a choice:
- use a trusted and familiar exchange platform to take over complexity & responsibility or
- use Exchange Union directly with full responsibility & complexity

For the foreseeable future, it is expected that the vast majority of users will be group a), choosing not to handle a decentralized exchange’s general technical complexity which includes managing private keys and running and maintaining XU nodes, which includes full nodes for each chain. These users will still choose to become clients of digital asset exchange platforms. Further, a strong USP of today’s digital asset exchanges are the localized FIAT on- and off-ramps, in concrete terms bank and credit card integration relationships, which most likely will never be realized by an individual. It can be concluded that **Exchange Union is not cannibalizing the business of digital asset exchange platforms**, but rather making such more attractive for users. Especially the upcoming wave of retail investors will heavily rely on convenience & FIAT on-ramp services. The advantages of how Exchange Union instantly makes digital asset exchanges more attractive for its users can be summarized as follows:
- offer deep liquidity by ‘combining order books’ of all participating exchanges
- offer access to virtually all trading pairs
- offer access to the best price
- all through one account and one familiar UI

For exchanges, this means:
- access to a larger user base, anybody can trade on any exchange (includes individual users in future!)
- increased earnings, zero-fee-loss through sophisticated fee and reward model with XUC; earnings can only go up
- entire new product lines possible, which use exchange union directly (wallet, merchant service...)
- preparation for a decentralized, robust and censorship-resistant future: the internet of value

The main difference to the prior mentioned value proposition of ripple is, that through the Exchange Union doesn’t require trust between parties to exchange value. This allows virtually any exchange to join, no matter the reputation. Further, Exchange Union doesn’t need any intermediary currency, like Ripple’s XRP, to attempt trust-less transfers, which has [shown not to be effective in practice](https://medium.com/kilrau/dont-trust-me-why-ripple-is-not-the-future-of-money-d10081db1aaa). The technology stack is entirely different from ripple and is described in detail below.

As with existing decentralized exchanges, no single point of failure (SPOF) can cause a shut-down of Exchange Union, eliminating critical attack vectors and increasing censorship resistance. The **difference to existing decentralized exchanges**, like [0x](https://0xproject.com), [etherdelta](https://etherdelta.com) or [altcoin.io](https://www.altcoin.io/) is, that **trading on Exchange Union is instant**, one of the major pain points of decentralized exchanges today, which facilitate trades ‘on-chain’. This is costly (in the case of bitcoin currently about $5) and slow (up to one hour) and thus not expected to reach mass adoption. Also with Exchange Union, trading works cross-chain, that means it is not limited to tokens of one specific chain, like e.g. above mentioned 0x and etherdelta are limited to ETH and ERC-20 tokens.


## 3. Under the hood

3.1. Payment Channels
---------------------
**Exchange Union is the first instant, decentralized exchange and also the first one supporting multiple chains**. This is realized via a novel technology combining ‘payment channels’ and ‘atomic swaps’. Exchange Union builds an open source community & incentivisation model to develop the software “XU Node” which exposes a simple API designed for digital asset exchanges and, together with the XU nodes of other exchanges, forms the decentralized meta-exchange. The XU node software will be provided in form of a Docker image that contains Payment Channel Protocol daemons (Lightning and Raiden binaries), and full-node daemons for all supported chains (bitcoind, geth etc.) and a XU manager that controls those daemons and exposes an API to the exchange. In general, all BIP 199 compatible payment channels can be supported directly, others might need to find a way to implement HTLC swaps via smart contracts. Each participating exchange runs an XU node, which opens payment channels on each chain an exchange chooses to support. That means if Exchange A wants to enable its users to trade bitcoin and litecoin through Exchange Union with Exchange B, it has to open a payment channel on the litecoin blockchain AND on the bitcoin blockchain to at least one other exchange in the union which is connect to Exchange B via a payment channel path. More on payment channel paths below. Payment channels in XU are meant to be kept open long-term.

A payment channel is a mechanism introduced by Lightning Networks to ensure instant, off-chain transfer of ownership of cryptocurrencies. Payment channels are opened and closed via on-chain transactions. Once a bidirectional payment channel is open, it uses unbroadcasted, fully valid signed transactions, which are held on each side of the channel directly on the exchange’s XU node, enabling instant transactions. These transactions will use a multi-signature address as their input (the funding address) and they will point at two different addresses for their output, each controlled by one party of the payment channel.

Through their sophisticated setup, payment channels are trustless, because simply put, the said signed transactions are valid on the underlying chain and can be broadcasted any time by either party. Also, payment channels allow trust-less routing of funds through intermediaries using a technology called [Hashed Timelock Contracts (HTLCs)](https://en.bitcoin.it/wiki/Hashed_Timelock_Contracts), thus a direct channel between two parties is not required and a path through other XU nodes can be used. This is described in detail in the [Lightning White Paper](https://lightning.network/lightning-network-paper.pdf). This is useful, since setting up payment channels can be compared to the effort of transfering coins into a sidechain: coins have to be ‘locked’ into a payment channel via a native on-chain transaction.


3.2. Atomic Swaps
-----------------
The actual exchange of the two assets happens via ‘atomic swaps’ directly between exchanges on the respective payment channels using slightly modified HTLCs. A manifestation of the first successful lightning atomic swap between BTC and LTC can be found [here](https://blog.lightning.engineering/announcement/2017/11/16/ln-swap.html). Token swaps of ERC20 tokens are also supported by the Raiden Network as described [here](https://raiden-network.readthedocs.io/en/stable/api_walkthrough.html#token-swaps).

In simple terms, this means, using the BTC/LTC example, real bitcoin flow from exchange A to exchange B on their bitcoin payment channel and real litecoin from exchange B to exchange A on their litecoin payment channel in real-time. The HTLCs ensures the atomicity of the swap: either the swap completes in full, or nothing happens.

The payment channel standard on bitcoin and litecoin is called ‘[lightning](https://github.com/lightningnetwork/lnd)’ and compatible implementations are maintained by the companies [ACINQ](https://github.com/ACINQ/eclair), [Lightning Labs](https://lightning.engineering/) and [Blockstream](https://blockstream.com/). Full compatibility was only achieved [recently](https://medium.com/@lightning_network/lightning-protocol-1-0-compatibility-achieved-f9d22b7b19c4). Ethereum’s payment channel technology is called ‘[Raiden Network](https://raiden.network/)’ and is maintained by the company [Brainbot](http://www.brainbot.com/). Payment channel atomic swap technology is fairly new and was just recently successfully tested between [Bitcoin & Litecoin](http://bitcoinist.com/first-ever-cross-chain-atomic-swap-between-bitcoin-and-litecoin-has-now-taken-place/), [Bitcoin & Zcash](https://github.com/zcash-hackworks/zbxcat) and [Bitcoin & Ethereum](https://www.coindesk.com/bitcoin-ethereum-atomic-swap-code-now-open-source/) (via a smart contract [[1]](https://github.com/AltCoinExchange/ethatomicswap), [[2]](https://medium.com/@DontPanicBurns/ethereum-cross-chain-atomic-swaps-5a91adca4f43)). More [research](https://github.com/raiden-network/raiden/issues/399) is probably needed to achieve full compatibility between different HTLC ([BIP 199](https://github.com/bitcoin/bips/blob/master/bip-0199.mediawiki)) on different chains. Also, for payment channels to work, transaction malleability has to be fixed on the underlying chain. This happened with the activation of SegWit on Bitcoin, Litecoin and some other Bitcoin-based chains like Vertcoin in 2017; Ethereum’s transaction malleability is already fixed since the Homestead release in 2016.

Since Ethereum includes all ERC20 tokens, Exchange Union already supports the vast majority of today’s token market. [Other important chains](https://storeofvalue.github.io/posts/what-is-qtum-without-the-bullshit/) are expected to start developing Lightning/Raiden-style payment channel technology within the next months. Options how to support some in the short term, e.g. via [ERC20 wrapper](https://weth.io/) technology, can be explored.


3.3. Security
-------------
```
[TODO]
- some words on general payment channel security with references
- security inherited from underlying blockchains
- we will use tested implementations from blockstream/lightning labs/brainbot and embed them in our node
- some words on our development process and testing
- private key handling - responsibility for funds is with exchanges
- secure updating of nodes
- attack vectors: different attack scenarios, how our system will perform - compare to public blockchains
```


3.4. Orderbook Propagation
--------------------------
Orderbook propagation is a key task for XU and a problem on existing decentralized exchanges where, to the best of our knowledge, this is [realized in a centralized way](https://github.com/0xProject/standard-relayer-api/blob/master/http/v0.md). We believe, orderbook propagation has to occur in a decentralized manner to sustain the overall robustness and censorship resistance of the system. Ideally the propagation would happen directly within the payment channels.

Firstly, there will be no complete global orderbook as such, containing all orders for each and every trading pair. Instead, orders will be propagated using matching payment channels (BTC/LTC orders will only be broadcasted via LTC payment channels), therefore XU nodes only maintain relevant parts of the orderbook. This approach ensures a more efficient and decentralized system.

Secondly, orderbooks should be shared with all XU members in the network with minimal latency. In addition, error conditions regarding orders should be propagated in a similar manner. Given that XU members will establish payment channels with each other via the XU node, a generic multicast messaging mechanism to update local orderbooks on XU nodes continuously seems to be most appropriate. Messaging mechanism may be similar to [issuing invoices](http://api.lightning.community/#addinvoice) and [subscribing to them](http://api.lightning.community/#subscribeinvoices) `[TODO] but a feasibility assessment is needed`. A generic pub/sub API where string data is the payload may abstract protocol details, therefore it may be beneficial to use [standardized message formats](https://www.onixs.biz/fix-dictionary/4.4/msgs_by_msg_type.html) like [FIX](https://en.wikipedia.org/wiki/Financial_Information_eXchange). If a XU member wants to fill an order, a payment route between Exchange A and Exchange B has to exist, which is ensured by the way orderbooks are propagated. XU nodes only receive order updates with an existing payment route, which is likely a multi-hop route between Exchange A and B discovered by mechanisms similar to Border Gateway Protocol (BGP).

Finally, our XU node implementation will be continuously monitoring orderbooks on all chains and if an exchange behaves maliciously by either spamming orderbooks or not finalizing trades, it will be punished. Punishment options include temporary suspension in service or taking away deposited XUC (similar to PoS)

```
[TODO]
Research & detailed elaboration needed. All order propagation multi-cast via XUC payment channel, 
since all XU nodes need to open XUC payment channel for incentive system to work. Or separate 
multicast. New XU node coming online: ‘sync old orderbook for subscribed channels’ mechanism.
```


3.5. Visualization: An instant decentralized exchange
-----------------------------------------------------
![Screenshot](images/visualization.png "Exchange Union System")


3.6. Sample Trade
-----------------
This is the basic flow of a trade via XU:

1. Assuming `Exchange A` is an Exchange Union member and it wants to offer a rare trading pair to its users, **BTC x UnicornCoin**. In order to successfully execute this trade, `Exchange A` should run a XU node which handles opening payment channels, running full nodes and matching trades. In our case, the XU node is configured to open payment channels and run full nodes on both, the Bitcoin and the UnicornCoin network. Opening payment channels requires depositing funds into a channel with a so-called ‘funding transaction’. The deposited BTC amount, let’s say 100 BTC represents the total trading volume which `Exchange A` can execute using BTC.

2. `Exchange A` has successfully opened a payment channel to minimum one other XU node on Bitcoin and on UnicornCoin and uses the payment channel on UnicornCoin to send a limit order. This order, which is similar to a regular buy or sell request submitted in a cryptocurrency exchange, includes the amount of requested UnicornCoin with a price in BTC. Let’s assume `Exchange A` sends an **order for selling 100 UnicornCoin** with the requested price of **1 UnicornCoin = 0.1 BTC**. This order is published through the BTC payment channels (see Order Propagation) to all subscribed peers using a mechanism similar to ‘invoice adding’ and ‘subscription’ in the Lightning client.

3. XU members on UnicornCoin update their orderbooks based on this new order.

4. If another XU member, `Exchange B`, wants to fill this 100 UnicornCoin order, the XU node takes care of the cross-chain atomic swap which means `Exchange A` gets **100 UnicornCoin** and `Exchange B` gets **10 BTC** on their payment channels on the respective chains. Behind the scenes, both parties create transactions on both chains to setup a trade which imposes time constraints i.e. if a trade is not successful both parties get their money back. Order filling is realized on a first-come-first-served basis.

```
[TODO]
Exceptions:
An exchange can choose to subscribe to the orderbook of particular exchanges only (e.g. through legal requirements)

- Order is filled by another user who was faster than me: funds WITH error message come back and XU 
node starts looking for the next best order. We should use the ‘odd type’ messaging system as 
specified in the RFC. E.g. Type 17 for errors.

- Exchange B goes offline: time-out message comes back and XU node starts looking for the next best 
order. Exchange B repeatedly doesn’t fill orders: punishment, see chapter 3.4. 

- Trade volume for a particular asset exceeds payment channel volume. XU node maintains a configurable 
threshold and automatically redeposits from an authorized hot wallet. Details, sources needed.
```


3.7. FIAT Tokenization / Stablecoin
-----------------------------------
To support FIAT/Crypto pairs, FIAT has to be ‘tokenized’ to be tradable on Exchange Union’s decentralized exchange. Also it has to fulfill the requirement to be trustless, thus a simple USD or EUR IOU won’t do. Several approaches exist:

3.7.1. The [USDT](https://tether.to/) way
-----------------------------------------
Hold 1 USD in a “reserve bank account” for every USDT issued. And ideally be transparent about it

**Good** This would work for all FIAT currencies that our bank supports

**Bad** History has shown that this is problematic for the following reasons:
- Even if I’m 100% transparent, the bank could just cut ties with me, as [happened multiple times with Tether Limited](https://news.bitcoin.com/tether-coin-may-be-a-precarious-us-dollar-peg/). News go public, Markets panic
- If I’m rather semi-transparent, like Tether Limited, people have to put trust and faith and Tether to really have those USD on their bank account. And to handle bank relationships. It essentially is simply not trustless.
- We would have to act like a central bank and issue new USDX tokens to increase supply based on demand. This requires us to hold USDX somewhere under central control, which is a SPOF for hackers. [As happened with Tether](https://techcrunch.com/2017/11/20/tether-claims-a-hacker-stole-31m/)


3.7.2. The [BitShares](https://bitshares.org/technology/price-stable-cryptocurrencies/) way
-------------------------------------------------------------------------------------------
A smart contract holding 200% of the underlying amount worth in BTS, 100% as collateral. E.g. One bitUSD contains 2 USD worth of BTS at creation.

**Good**
- Tested for years, proves self-adjusting through ‘soft-peg’, traders will go short/long awaiting the coin to re-stabilize. See the BUT below.
- We could do our own version using XUC instead of BTS, good to give XUC a purpose

**Bad**
- BUT The system only works because people BELIEVE the coin re-stabilizes and because it’s backed by Bitshares Company as lender of last resort which ensures that as sort-of central bank. Nothing in the protocol automatically ensures bitUSDs peg to be one USD. It could be 0.5 if whales in the market go rogue and short accordingly.
- Requires exchanges to lockup minimum 200% of the FIAT trading volume
- Emergency liquidation is untested (`[TODO] to be confirmed!`) and will run into issue if a huge asset like bitUSD suddenly has to get emergency liquidated.


3.7.3. The [MakerDAO](https://blog.makerdao.com/2017/06/05/introducing-sai/) way
--------------------------------------------------------------------------------
DAI = Basically BitShares+: Collateral in Smart Contract (they call it Collateralized Debt Positions (CDPs)), users can create DAI’s which are backed by ETH, in future other (ETH-based) assets. Target Rate Feedback Mechanism ([TRFM](https://medium.com/cryptolinks/maker-for-dummies-a-plain-english-explanation-of-the-dai-stablecoin-e4481d79b90)) creates user incentives to HOLD or BORROW Dai and thus correct the price to a given price target, like 1 USD. It gets triggered if the price goes off by a ‘sensitivity parameter’ which is set by MKR token holders, POS style. TRFM increases the target rate if price is too low, which causes generation of new DAIs to get more expensive and gains for holding DAIs to increase. This reduces supply causes price to increase back to target price. Works the other way round.

**Good**
- We could use XUC as collateral, since it’s ERC20
- Several fallback mechanisms, Global settlement being last resort

**Bad**
- If we use XUC as collateral, it would again require a very liquid local FIAT/XUC pair
- Completely trusts one price oracle (?)
- For now ETH has to be wrapped into PETH
- MKR as ‘stability fee’ - do I need to pay to hold DAI over time?
- Why are MKR holders to decide on the sensitivity parameter? It should be just pre-set. Sounds a bit like a forced use and potentially rich guys could manipulate the parameter.


3.7.4. The [Basecoin](http://www.getbasecoin.com/) way
------------------------------------------------------
A robust, price-stable cryptocurrency with an algorithmic central bank. It monitors USD or any other price (e.g. EUR or CPI) through external oracles, and smart contracts adjust the basecoin supply according to current market supply and demand (the basecoin system is aware of both) to keep the value close to the pegged asset. Basically what a central bank does. But the monetary policy gets handed over to a decentralized algorithm.

**Good** 3 coin system:
- basecoin: Pegged with USD (“baseUSD”) or to any other asset, directly faces users
- base bonds: tokens to be ‘auctioned off’ programmatically by the blockchain when supply needs to be reduced. 1 base bond will be exactly 1 basecoin in future when supply needs to be increased (and other conditions
- base shares: fixed-supply cryptocurrency that does not have a peg, holders of these will get basecoin when supply needs to be increased. Free money for initial investors?

**Bad** It’s untested.
- It’s not backed by anything, just like our current USDs and EURs
- How fast can the system respond? How fast do prices adjust - long-term peg is not sufficient for trading purposes.
- Issuing new base coins is relatively easy. But buying back coins relies on a super liquid market of base bonds to buy back base coins. Speculators need to trust their base bonds will be worth more in the future when the basecoin supply is extended. All this needs to have a proven track record to work. It strongly depends on initial market management. Also they promise an interest rate - how is that done?
- How exactly are dead spirals avoided?
- Any fallback mechanisms?

**Conclusion** For now MakerDAOs DAI & basecoin seem both promising and thought-through models. Since both models are fundamentally different and untested, `[TODO] a thorough analysis & comparison is needed`. Exchange Union’s goal is to utilize one solution to tokenize all FIAT and ideally hide the complexity from the exchange.


3.8. Exchange Earnings
----------------------
**Executive summary: worst-case, exchanges will earn exactly the same fees as compared to not being connected to Exchange Union.**

How this is achieved: The XU node implementation enables the exchange to simply ‘keep’ the trading fee before the actual amount is sent over to the counterparty via the decentralized exchange. This is how exchanges continue earning fees in general. Since exchanges are usually earning fees from both sides of a local trade (maker & taker fee), Exchange Union’s software, the XU node, allows exchanges to charge the equivalent amount of maker+taker fee for one side of the trade. Naturally, both parties of a trade will want to do this. Consequently, every trade facilitated via Exchange Union enables two exchanges to earn the full maker & full taker trading fee. This means, even if all trades of an exchange ‘leave the platform’ and are executed on XU, the above described worst-case scenario, earnings stay exactly the same as if the trades would happen locally. It also means, that in the long run earnings go up for exchanges for every trade executed via XU, since fees are earned double.

How this benefits the trading individual: Trades only happen via XU, if:
```
(ExchA_price) > (ExchB_price+ExchA_fees+ExchB_fees)
```

Consequently the same is true for traders: they can only end up with better prices. If the price difference is not large enough to cover the fees kept by the exchanges, the trade won’t happen via XU. It is expected that through market forces (arbitrage) price levels keep adjusting and not all trades leave the exchange. Through the real-time nature of XU, market forces should be able to adjust prices much faster than currently.

Sidenote: The broadcasted orderbooks carry the fee amount of each order in a separate fee field. Additional revenue streams for exchanges are introduced in the following.

```
[TODO]
Detailed market economy study, fee prediction with formulas and "you are killing arbitrage" argumentation needed!
To be solved: Trades for pairs which are not locally available on ExchA, fees should only be charged by 
ExchB, because the user couldn’t do the trade locally on ExchA and would have eave the platform anyways 
to do the trade. This can be avoided with XU. Difficult: How to prevent cheating - exchanges would just 
go and list all pairs locally to always earn fees. Even if they had 0 local liquidity, that would work. 
Minimum local local liquidity? Give up on this rule?
```


3.9. Incentivisation - The Role of XUC
--------------------------------------
```
[TODO]
This is the most sensitive part of our project: a new token always raises controversy - we crucially 
need XUC to financially motivate stakeholders to participate & build exchange union. For now this is 
the least thought-through part of this document. A lot of work needed here!
```

Developers, exchanges and functionaries which contribute resources to the XU ecosystem are key for Exchange Union to get up and running, reach a ‘network-effect’ size and be successful in the long-term. We believe, a functioning monetary incentive system is the success factor for future Open-Source projects, maybe even the new decentralized economy as a whole. Bitcoin itself is the best example for a working monetary incentive to build an ecosystem. Exchange Union is an ambitious project, but if it works out, it definitely creates value and should be self-sustaining.

Flawed incentivisation is believed to be the main reason why other solutions didn’t find adoption yet. Some solutions for example ask exchanges to pay hardware or even monthly fees. From an exchanges point of view, this doesn’t add up without offering something in return, since the solutions essentially offer an easier way for user funds, and thus trading revenues, to leave the platform. Exchange Union will not charge any fees and more importantly enable the exchanges to not only maintain full trading fee revenue but in almost all cases increase such, as described above. Additionally, different functionaries are rewarded with XUC as follows:

**Incentives for Developers**
- Each pull-request to the Exchange Union github (https://github.com/ExchangeUnion) gets peer reviewed and a score by each reviewer. XUC is paid to the contributor based on that score.
- When the pull-request makes it into the master-branch, another XUC amount gets paid to the developer
- Reviewers get rewarded
- Testers get rewarded, test-net time gets rewarded

**Incentives for Exchanges**
- In order to have sufficient interest of exchanges joining the union in the beginning the idea is to reward exchanges with XUC for simply joining XU.
- Limitation: these XUC can only be used to pay for routing. How? Via a legal contract?
- Ideally these XUC get airdropped to all users of the exchange. This would enable all users to directly use Exchange Union “for free” for a while.
- Should the end-user deal with XUC and pay or only the exchange? The end-users would have to hold a certain amount of XUC to get ‘the advantage of accessing XU’. Compare Binance’s BNB business model, which gives discount on fees. With XUC, users can get access to new pairs and a better price. For this the XU node would have to check a particular users XUC balance.

**Incentives for Functionaries which provide services (running XU node)**
- Reward for payment channel routing: we are essentially rewarding for running lightning/raiden hubs, exchanges commit funds for their own usage anyways and can earn an incentive with it if other people of the regular Lightning network use it. **We incentivize Lightning/Raiden adoption** and even [solve the maintenance problem of such nodes](https://www.youtube.com/watch?v=dlJG4OHdJzs) (the exchange wants maintain the XU software anyways to be able to use it) 
- Orderbook relays get rewarded. How? An exchange wouldn’t want to pay just for continuous orderbook propagation.

**Incentives for traders**
- A user has X trading volume on Exchange Union, this user gets rewarded with XUC from his exchange platform

Each XU node by default opens a channel for XUC in order to be able to pay for channel routing and orderbook relays and also vice-versa receive XUC payments for these services. Also, we could use the XUC payment channel as some sort of emergency fallback if other channel’s volume is not sufficient.
```
[TODO]
Research https://twitter.com/VitalikButerin/status/940429788673536000
```


3.10. Business Model: XUC
-------------------------
Exchange Union’s business model is based on XUC being valuable, which not only makes the incentivisation work, but benefits all stakeholders. XUC’s overall supply is fixed to 3 billion, which naturally makes it deflationary the more the XU ecosystem (& thus demand) grows. This makes XUC a meaningful incentive. The original idea was, that every transaction burns a small amount of XUC as fee, which is needed to prevent spam on a sidechain. However, payment channels don’t face the problem of spamming and thus don’t need a blockchain-like fee. Also, with an artificial burn-per-trade we could get rid of the 30% excess supply (Last ‘reserve’ part of the token allocation in the white paper) in a more elegant way than just burning it at once. A ‘transparency’ section on the website informs about every new XUC release and purpose. Smart contracts will handle the burning.



## 4. How is XU different from ...


4.1. [Ripple](https://ripple.com/) / [Stellar](https://www.stellar.org/)
----------------------------------------------------------------------
- Exchange Union is trustless - always.
  - Ripple/Stellar requires building up a trust network for exchanging IOU-based tokens, which exposes involved parties to counterparty risk. This would cause loss of funds when an exchange would go bankrupt or get hacked and thus is not acceptable.
- Exchange Union always transfers the real underlying asset. If for instance bitcoin and ethereum get exchanged, real bitcoin and real ethereum and transferred between the two involved parties
  - Ripple’s trustless approach uses XRP as intermediary, which proves to be risky in practice due to the lack of liquid XRP markets against all currencies.
- Exchange Union transacts value via payment channels between two parties only; transactions are are secured by the underlying blockchains, e.g. bitcoin, ethereum or litecoin, which are block-history-based
  - Ripple/Stellar maintain balances in a Distributed Ledger, important is just the last commonly agreed ledger state, history doesn’t matter.
- Different target users: Exchange Union targets digital asset exchanges, which especially includes small, unregulated and unlicensed exchanges. This is enabled to the trustless nature of the: no requirements to build up a trust network or maintain list of untrusted parties
  - Ripple/Stellar target traditional banks; which sort-of know and trust each other


4.2. [Blockstream’s Liquid](https://blockstream.com/liquid/)
------------------------------------------------------------
- Exchange Union is a decentralized meta-exchange, exchange of assets happens ‘on Exchange Union’
  - Liquid simply let’s users transfer bitcoin from Exchange A to B
- Exchange Union is (network-latency) instant
  - Liquid is a sidechain with about 1 min, in future down to 10s block times. But never instant.
- Exchange Union requires a XU Node, software only
  - Liquid in it’s current form requires hardware
- Exchange Union is completely open source and free to use
  - Liquid requires exchanges paying a monthly fee (2016: 2500 USD
- Exchange Union automates a trade
  - Liquid asks users to handle of X, Q or H liquid addresses on both exchanges, manual triggering of transactions
- Exchange User is more end-user trader oriented
  - No multiple account setups on the different exchanges & multiple KYC needed


4.3. [Lightning](http://lightning.engineering/) / [Raiden](https://raiden.network/)
-----------------------------------------------------------------------------------
- Exchange Union builds on top of lightning & raiden, it’s actually using pure Lightning/Raiden with atomic swaps in the background and ads some features like orderbook propagation to build a fully functioning decentralized meta-exchange


4.4. [Alphapoint’s ADLP](https://bravenewcoin.com/news/alphapoint-provides-liquidity-to-bitcoin-exchanges/)
-----------------------------------------------------------------------------------------------------------
- Exchange Union is a free, open-source Meta-Exchange - everyone can join, users will be able to use it directly in future
  - Alphapoint is a centralized commercial remarketer which requires fees
- Users of Exchanges which are part of Exchange union directly swap assets with each other
  - Alphapoint does buy and sell on users behalf on another exchange, this is considered to become a legal issue in future
- Alphapoint is limited to supported assets (atm only bitcoin?), Exchange Union supports all chains with a payment channel  implementation (Lightning, Raiden, Lumino)


4.5. [Altcoin.io](https://www.altcoin.io/) / [Etherdelta](https://etherdelta.com) / [0x](https://0xproject.com/)
----------------------------------------------------------------------------------------------------------------
- Exchange Union is a decentralized meta-exchange, targeting exchanges as ‘users’ first
  - All of the above mentioned are end-user oriented
- Exchange Union targets to have a decentralized orderbook propagation to not have a SPOF
  - The above provide access and orderbooks via a centralized solution
- Exchange Union uses payment channels for atomic swaps - they are instant
  - All of the above mentioned use on-chain swaps which are costly and take up to one hour to complete
- Exchange Union supports multiple chains
  - Etherdelta/0x are limited to Ethereum-based assets


4.6. [RaidEX.io](https://www.raidex.io/)
----------------------------------------
Brainbot, the company behind the Raiden network, seems to be the only company already working on a decentralized payment channel exchange. For now it’s unclear if RaidEX will target users directly or Exchanges first and if it’s limited to Ethereum/Raiden. Exchange Union will closely collaborate with brainbot.



## 5. Requirements
| Description   | Comments      | Solved in XU?  |
| ------------- |:-------------:| -----|
| Each tx must settle near real-time (real tx finality) | Network latency real-time | Yes, currently only with payment channels Lightning & Raiden, block time: min about 10s |
| Decentralized Swap | Swap assets trust-less without middle-man | Yes, atomic swaps, example how this works on payment channels `[TODO] to be fleshed out` |
| Trustless Transfer | Real token transfer, no IOUs | Yes |
| Decentralized infrastructure | Each exchange runs own XU node | Yes |
| Incentivisation | Honest behavior=reward, Provide service=reward, Malicious behavior=punishment | No |
| Confidential | Tx’s payment data visible for involved parties only | Yes |
| Smart Contract or ‘Scriptless scripts’ support | Needed for smart collateral or unforeseen future purposes | No |
| XUC usage | Burned during tx, required amount of XUC declining (based on SDR?) | Yes: Incentivisation, fee part:No |
| User knows exchange rate & fee in advance | Defined metadata standards, pre-quote exchange rate to user | Ordersbooks are propagated via payment channels, order will contain amount and exchange rate similar to submitting a trade request on a centralized cryptocurrency exchange. |
| Any currency can be converted to any currency | multiple assets, path finding | Yes, all chains with payment channels are supported, path finding if no direct payment channel with target exchange exists |
| Security fallback to public chain |  | Yes |
| FIAT token support | | No, tbd |
| Easy, decentralized, secure updates for exchanges | | No, tbd |
| Byzantine Robust | The network is secure against Byzantine attacks | No, tbd in Security chapter “Forced Expiration Spam” is the only valid risk in terms of attack (Lightning paper 9.2). XU node will implement monitoring and counter-measure mechanism to identify and suspend spammers. |
| Simple value transfer from user a to user b (no swap) | Support of multiple transaction types with different characteristics (e.g. atomic, partial), requires unique ID for users (idea: civic) | No, later phase for pure value transfers |

