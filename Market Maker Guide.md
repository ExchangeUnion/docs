This guide is written for individuals and entities looking to become a market maker on OpenDEX.

# Supported Networks

## Simnet
Private chains which are maintained by Exchange Union. We’ll automatically open channels to you and push over some coins, you’ll be trading against our bots and anyone else running simnet. It’s the perfect play money playground to see how things work and play around with `xucli` commands. It’s easy: run the launch script, wait for about 15 minutes and you are ready to go. **You want to start with this!**

Status: `live` | Setup time: `~15 mins` | Required disk space: `<1 GB`

## Testnet
bitcoin testnet 3, litecoin testnet 4, ethereum ropsten. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.ropsten.be/) & [2](https://faucet.metamask.io/). Quite a bit of manual work to be done here. If you need help or some channels with testnet coins, hit us up on [Discord](https://discord.gg/YgDhMSn)!

Status: `live` | Setup time: `5-24h` | Required disk space (when using full nodes): `120 GB`

## Mainnet
Real money. Only with #reckless hat.

Status: `live` | Setup time: `1-3 days` | Required disk space (when using full nodes): `700 GB`


# Requirements

1. Linux or macOS. [Windows WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) support is currently experimental and not tested regularly. This guides was tested on ubuntu 18.04.

2. Hardware with minimum 12GB RAM and a SSD for testnet/mainnet if you want to avoid using [infura](https://infura.io/). The more, the better - `geth` likes RAM. A lot. If you have >=24GB RAM available, you can significantly shorten `geth`'s syncing time by increasing its `--cache=1024` to something larger. Since market makers should be online 24/7 and we are ushering in a post-cloud era, we recommend setting up a power-efficient linux box connected to a stable internet connection. The hardware requirements for this box differ: if you are ok connecting to [infura](https://infura.io/), or some other external geth node, a Pi4 is enough. If you want to run all full nodes yourself, a device with 16GB RAM + a 1TB SSD does the job. You can also opt for a small SSD (32GB) for `geth`s application data and store all chain data on a HDD; all other clients can run on a regular HDD. See [Tips 'n tricks](https://docs.exchangeunion.com/start-trading/user-guide#tips-n-tricks) below on how to split `geth`'s data between an HDD and SSD. Read about the root cause [here](https://medium.com/blockchain-studio/ethereum-client-geth-v1-9-0-released-whats-new-2b3de043ee16). Hardware guide **coming soon**!

3. `docker` >= 18.09. Check with `docker --version`. If you do not have docker installed yet, follow the official [install instructions](https://docs.docker.com/install/). Also make sure that the current user can run docker (without adding `sudo`). Test with `docker run hello-world`. If this fails, [follow these instructions](https://docs.docker.com/install/linux/linux-postinstall/).

# Basic Setup

Start the environment with

```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
```
The setup will ask you to choose the network (Simnet, Testnet, Mainnet).
```
1) Simnet
2) Testnet
3) Mainnet
Please choose the network: 3
🚀 Launching mainnet environment
🌍 Checking for updates ...
```
Then guides you through some basic setup, like setting your password to encrypt your environment's private keys and writing down your mnemonic phrase, which serves as backup for your xud node key and wallets (your on-chain assets). **Keep it somewhere safe!**

```
You are creating an xud node key and underlying wallets. All will be secured by a single password provided below.
  
Enter a password: 
Re-enter password: 

----------------------BEGIN XUD SEED---------------------
 1. you         2. won't       3. find        4. money      
 5. in          6. this        7. seed        8. but    
 9. good       10. thinking   11. we         12. encourage      
13. hacking    14. and        15. attacking  16. of     
17. xud        18. and        19. happily    20. pay  
21. ransom     22. for        23. your       24. hack    
-----------------------END XUD SEED----------------------

The following wallets were initialized: BTC, LTC, ERC20(ETH)
```
Off-chain assets, which are assets you are holding in lightning and raiden channels for trading on OpenDEX, are secured in a separate backup. This backup is as important as the mnemonic phrase above. We highly recommend using a separate external drive, like a USB stick or NAS, in case something happens to your main drive. Because backups are constantly written to this drive, it needs to be connected to the device running `xud-docker` (can be a network drive).

After this, the setup pulls docker containers, starts syncing chains and opens

```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
```

Use the command `status` to check on the syncing status of underlying L1 clients. If you are freshly syncing full nodes on testnet/mainnet, this takes several hours or even days. All clients should show `Ready` before you continue.

```
mainnet > status
┌───────────┬──────────────────────────────────────────┐
│ SERVICE   │ STATUS                                   │
├───────────┼──────────────────────────────────────────┤
│ bitcoind  │ Syncing 16.44% (265079/1612188)          │
├───────────┼──────────────────────────────────────────┤
│ lndbtc    │ Waiting for sync                         │
├───────────┼──────────────────────────────────────────┤
│ litecoind │ Syncing 12.17% (156165/1283134)          │
├───────────┼──────────────────────────────────────────┤
│ lndltc    │ Waiting for sync                         │
├───────────┼──────────────────────────────────────────┤
│ geth      │ Syncing 10.43% (725386/6949168)          │
├───────────┼──────────────────────────────────────────┤
│ raiden    │ Waiting for sync                         │
├───────────┼──────────────────────────────────────────┤
│ xud       │ Waiting for sync                         │
└───────────┴──────────────────────────────────────────┘
```

`xud ctl` takes [`xucli` commands](https://api.exchangeunion.com) without the need to prepend `xucli`, e.g. `getinfo`. Run `help` to get an always up-to-date list of commands. Once everything is synced and ready, you can see other xud nodes on the network via `listpeers`. Append `-j` to any command to get JSON instead of the formatted output.

```
mainnet > listpeers -j
{
  "peersList": [
    {
      "address": "rgz5icb5jdxzmu7r7tbis64q23ioytzd4tqikuyb5kz75w75rbe6veyd.onion:8885",
      "nodePubKey": "02529a91d073dda641565ef7affccf035905f3d8c88191bdea83a35f37ccce5d64",
      "lndPubKeysMap": [
        [
          "BTC",
          "035cb9afb06a83e65fbab15c900d78580673cf56ce38c5814fb71f1eb57fcba7ee"
        ],
        [
          "LTC",
          "036cf16cd7de6193efb2855e784409c3633f893662dd6edcf7a545a99659232373"
        ]
      ],
      "inbound": false,
      "pairsList": [
        "LTC/BTC",
        "WETH/BTC",
        "BTC/DAI",
        "LTC/DAI"
      ],
      "xudVersion": "1.0.0-mainnet",
      "secondsConnected": 100,
      "raidenAddress": "0xe802431257a1d9366BD5747F0F52bAd25A6C3092"
    }
  ]
}
```

Deposit some coins 

```bash
lndbtc-lncli newaddress p2wkh #Send BTC to this address
lndltc-lncli newaddress p2wkh #Send LTC to this address
getinfo -j #Send WETH/DAI to your raiden address
```

The next step will have an automated option in future, but currently is not trivial: choose xud nodes to open channels with. Ideally these are nodes you expect to trade with regularly. If you are unsure, you can open channels with our xud node hosted at xud1.exchangeunion.com, which is maintaining a good channel connectivity with other xud nodes in the OpenDEX Network. using the unified `openchannel` command: 
```
openchannel 02529a91d073dda641565ef7affccf035905f3d8c88191bdea83a35f37ccce5d64 btc 0.1
```
We are maintaining a good channel connectivity with other OpenDEX nodes for xud1.

Check on existing orders in the network with `orderbook`. Issue an order, e.g. `sell 0.1 btc/dai 9998` to sell 0.1 btc for 7171 DAI. Settlement of your order shouldn't take longer than a couple of seconds. Use `getbalance` to observe your balance before and after the swap and see it changing.

```
mainnet > orderbook

Trading pair: BTC/DAI
┌───────────────────────────────────────┬───────────────────────────────────────┐
│ Buy                                   │ Sell                                  │
├───────────────────┬───────────────────┼───────────────────┬───────────────────┤
│ Quantity          │ Price             │ Price             │ Quantity          │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│ 0.28918298        │ 7171.56           │ 7172.253          │ 0.1               │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│ 1                 │ 7171.1937         │ 7172.9757         │ 0.1               │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│                   │                   │ 7316.0663         │ 1                 │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│                   │                   │ 7316.44           │ 0.22393946        │
└───────────────────┴───────────────────┴───────────────────┴───────────────────┘
mainnet > getbalance

Balance:
┌──────────┬───────────────┬────────────────────────────┬───────────────────────────────┐
│ Currency │ Total Balance │ Channel Balance (Tradable) │ Wallet Balance (Not Tradable) │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ BTC      │ 126.10944853  │ 12.5                       │ 113.60944853                  │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ DAI      │ 50000         │ 50000                      │ 0                             │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ LTC      │ 13500.0980005 │ 1125                       │ 12375.0980005                 │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ WETH     │ 500           │ 500                        │ 0                             │
└──────────┴───────────────┴────────────────────────────┴───────────────────────────────┘
mainnet > sell 0.1 btc/dai 7171
swapped 0.1 BTC with peer order ca24fe00-1c1e-11ea-8b1b-3b2ec0335696
mainnet > getbalance

Balance:
┌──────────┬───────────────┬────────────────────────────┬───────────────────────────────┐
│ Currency │ Total Balance │ Channel Balance (Tradable) │ Wallet Balance (Not Tradable) │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ BTC      │ 126.00944842  │ 12.39999989                │ 113.60944853                  │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ DAI      │ 50717.156     │ 50717.156                  │ 0                             │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ LTC      │ 13500.0980005 │ 1125                       │ 12375.0980005                 │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ WETH     │ 500           │ 500                        │ 0                             │
└──────────┴───────────────┴────────────────────────────┴───────────────────────────────┘
```

# Beware

Raiden currently requires direct channels with trading partners. We have a temporary check in place, that discards raiden-related orders (all pairs which include WETH, DAI...), if `xud` can't find a direct channel to the trading partner. You can switch this check off by setting `raidenDirectChannelChecks=false` in your `xud.conf`. Before you do that, read [this explainer of the issue](https://github.com/ExchangeUnion/xud/issues/1068).

# Report Issues

Please report issues/bugs by running `report` from within `xud ctl`.

# Tips 'n Tricks

* Permanently set the alias `xud` to launch `xud ctl` from anywhere:
Add the line `alias xud="bash ~/xud.sh"` to the end of `~/.bashrc` or `~/.bash_aliases` on Linux and `bash_profile` on Mac, then `source` the file.
* `xud ctl` allows to use an L1/L2 client's cli:
```bash
#Simnet
ltcctl --help
#Testnet/Mainnet
bitcoin-cli --help
lndbtc-lncli --help
litecoin-cli --help
lndltc-lncli --help
geth --help
raiden --help
xucli --help
```
* To inspect logs (use `logs -f` if you want to tail the log live):
```bash
#Simnet
logs ltcd/geth/lndbtc/lndltc/raiden/xud
#Testnet/Mainnet
logs bitcoind/litecoind/geth/lndbtc/lndltc/raiden/xud
```
* Blockchain & wallet data is stored in the fixed home directory `~/.xud-docker` by default. Customize the wallet & chain data directory by creating a config file with `cp ~/.xud-docker/sample-xud-docker.conf ~/.xud-docker/xud-docker.conf`, then edit `xud-docker.conf`. For temporarily using another directory, you can also use parameters, e.g. `bash xud.sh --mainnet-dir /path/to/temp/mainnet/dir`.
* External full-nodes (including infura) can be configured in a network specific config file. Create the config file, e.g. in the mainnet directory with `cp sample-mainnet.conf mainnet.conf`, then edit `mainnet.conf`.

```bash
#To connect to an external bitcoin core node in your local network set the values
[bitcoind]
external = true
rpc-host = "192.168.1.42"
rpc-port = "8332"
rpc-user = "user"
rpc-password = "pass"
zmqpubrawblock = "192.168.1.42:28332"
zmqpubrawtx = "192.168.1.42:28333"

#To place geth's chain data onto a HDD set the value
[geth]
ancient-chaindata-dir = "/media/hdd/geth/chaindata"
```
* You can `exit` from `xud ctl` any time and re-enter with `bash ~/xud.sh`.
* A reboot of your host machine does **not** restart your `xud-docker` environment by default. You will need to run `bash ~/xud.sh` and `unlock` your environment.
* Shutdown the environment with `down`.
* Remove all data with
```bash
# Use with caution: this step removes all `xud` blockchain and wallet data from your system. If you have channels open without backup or lost your seed mnemonic, you are risking to loose funds.
sudo rm -rf ~/.xud-docker
rm -rf ~/xud.sh
rm -rf /custom/mainnet/dir
```
* `xud-docker` only uses official xud releases for mainnet. Simnet/Testnet are updated frequently. If you want to run a specific xud branch or master, follow [these instructions](https://github.com/ExchangeUnion/xud-docker/#developing). For mainnet, we recommend to run the latest [official release](https://github.com/ExchangeUnion/xud/releases/) only. #craefulgang
* Docker might not play nicely with a VPN you are running on the same machine. If you see `Failed to launch environment`, try disconnecting the VPN.

## References
* [bitcoind config options](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
* [litecoind config options](https://litecoin.info/index.php/Litecoin.conf#litecoin.conf_Configuration_File)
* [geth config options](https://github.com/ethereum/go-ethereum/blob/master/README.md#configuration)
* [lnd config options](https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf)
* [raiden config options](https://raiden-network.readthedocs.io/en/stable/config_file.html)
* [xud config options](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf)
