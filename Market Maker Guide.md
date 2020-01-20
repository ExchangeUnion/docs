This guide is written for individuals and professionals looking to become a market maker on OpenDEX.

# Supported Networks

## Simnet
Private chains which are maintained by Exchange Union. We’ll automatically open channels to you and push over some coins, you’ll be trading against our bots and anyone else running simnet. It’s the perfect monopoly-money-playground to see how things work and play around with `xucli` commands. It’s easy: run one script, wait for about 15 minutes and you are ready to go. **You want to start with this!**

Status: `live` | Setup time: `~15 mins` | Required disk space: `<1 GB`

## Testnet
bitcoin testnet 3, litecoin testnet 4, ethereum ropsten. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.ropsten.be/) & [2](https://faucet.metamask.io/). Quite a bit of manual work to be done here. If you need help or some channels with testnet coins, hit us up on [Discord](https://discord.gg/YgDhMSn)!

Status: `live` | Setup time: `5-24h` | Required disk space (when using full nodes): `120 GB`

## Mainnet
Real money. Only with #reckless hat.

Status: `live` | Setup time: `1-3 days` | Required disk space (when using full nodes): `700 GB`


# Requirements

1. Linux or macOS. [Windows WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) support is currently experimental and not tested regularly. This guides assumes ubuntu 18.04.

2. Hardware with minimum 12GB RAM and a SSD for testnet/mainnet if you want to avoid using [infura](https://infura.io/). The more, the better - `geth` likes RAM. A lot. If you have >=24GB RAM available, you can significantly shorten `geth`'s syncing time by increasing its `--cache=1024` flag for [testnet](https://github.com/ExchangeUnion/xud-docker/blob/master/xud-testnet/docker-compose.yml) or [mainnet](https://github.com/ExchangeUnion/xud-docker/blob/master/xud-mainnet/docker-compose.yml) to something larger. Since market makers should be online 24/7 and we are ushering in a post-cloud era, we recommend setting up a power-efficient linux box connected to a stable internet connection. The hardware requirements for this box differ: if you are ok connecting to [infura](https://infura.io/), or some other external geth node, a Pi4 is enough. If you want to run all full nodes yourself, a 16GB RAM+2TB HHD+32 GB SSD box does the job. A small SSD is enough and only needed for `geth`; all other clients can run on a regular HDD. See [Tips 'n tricks](https://docs.exchangeunion.com/start-trading/user-guide#tips-n-tricks) below on how to split `geth`'s data between an HDD and SSD. Read about the root cause [here](https://medium.com/blockchain-studio/ethereum-client-geth-v1-9-0-released-whats-new-2b3de043ee16). Hardware guide **coming soon**!

3. `docker` >= 18.09 & `docker-compose` >= 1.24. Check with `docker --version` & `docker-compose --version`. If you do not have these installed yet, follow the official install instructions for [docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/). Also make sure that the current user can run docker (without adding `sudo`). Test with `docker run hello-world`. If this fails, [follow these instructions](https://docs.docker.com/install/linux/linux-postinstall/).

# Basic Setup

Start the environment with

```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
```
The setup will ask you to choose a network and guides you through some basics, like setting a password to encrypt your environment's private keys and writing down your mnemonic phrase, which serves as backup for your xud node key and wallets (meaning all your on-chain assets). **Keep it somewhere safe!**

```
You are creating an xud node key and underlying wallets. All will be secured by a single password provided below.
  
Enter a password: 
Re-enter password: 

----------------------BEGIN XUD SEED---------------------
 1. able        2. hero        3. shoot       4. lava      
 5. kind        6. nominee     7. prosper     8. change    
 9. confirm    10. inflict    11. observe    12. soap      
13. collect    14. pilot      15. tornado    16. cycle     
17. soul       18. wash       19. bike       20. artefact  
21. collect    22. onion      23. system     24. slogan    
-----------------------END XUD SEED----------------------

The following lnd wallets were initialized: BTC, LTC
The keystore for raiden was initialized.
```
Off-chain assets, which are assets you are holding in lightning and raiden channels for trading on OpenDEX, are secured in a separate backup. This backup is as important as the mnemonic phrase above. We highly recommend using a separate external drive, like a USB stick or NAS, in case something happens to your main drive. Because backups are constantly written to this drive, it needs to be connected to the device running `xud-docker` (can be a network drive). We enabled an experimental version of channel backups, which requires you to either specify the backup directory as parameter `bash xud.sh --backup-dir /media/hdd/xud-backup` or as `backup-dir = "/media/hdd/xud-backup"` in `mainnet.conf`, proper integration into the setup flow is [in the works](https://github.com/ExchangeUnion/xud-docker/issues/245).

After this, the setup pulls docker containers, starts syncing chains and opens

```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
```

Use the command `status` to check on the syncing status of underlying L1 clients. If you are syncing full nodes on testnet/mainnet, this takes several hours or even days. All clients should show `Ready` before you continue.

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

`xud ctl` takes [`xucli` commands](https://api.exchangeunion.com) without prepending `xucli`, e.g. `getinfo`. Run `help` to get an always up-to-date list of commands. Once everything is synced and ready, you can see the other nodes on the network via `listpeers`. Append `-j` to any command to get JSON instead of the formatted output.

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

The next step will have an automated option in future, but currently is not trivial: choose peers to open channels with. Ideally these are peers you expect to trade with regularly. If you are unsure, you can open channels with our xud node hosted at xud1.exchangeunion.com using the unified `openchannel` command: 
```
openchannel 02529a91d073dda641565ef7affccf035905f3d8c88191bdea83a35f37ccce5d64 btc 0.1
```
We are doing our best to keep good channel connectivity with other OpenDEX nodes. Use `getbalance` to check your total, channel and wallet balances. Check on existing orders in the network with `orderbook` and issue an order, e.g. `sell 0.1 btc/dai 9998` to sell 0.1 btc for 7171 DAI. Check your balance before and after the swap to see it changing.

```
simnet > orderbook

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
simnet > getbalance

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
simnet > sell 0.1 btc/dai 7171
swapped 0.1 BTC with peer order ca24fe00-1c1e-11ea-8b1b-3b2ec0335696
simnet > getbalance

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

# Report

Please report issues/bugs by running `report` from within `xud ctl`.

# Tips 'n Tricks

* Docker might not play nicely with a VPN you are running on the same machine. If you see `Failed to launch simnet environment`, try disconnecting the VPN.
* We placed `xud` & `lnd` behind TOR by default, which improves privacy and does away with the need to open ports in your firewall/router.
* If you want to test pre-release builds, follow [these instructions](https://github.com/ExchangeUnion/xud-docker/#developing).
* `xud ctl` allows to use an underlying client's cli:
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
* Permanently set xud alias to launch `xud ctl` from anywhere:
Add the line `alias xud="bash ~/xud.sh"` to the end of `~/.bashrc`/`~/.bash_aliases` on Linux or `bash_profile` on Mac, then `source` the file.
* To inspect logs (use `logs -f` if you want to follow the log live):
```bash
#Simnet
logs ltcd/geth/lndbtc/lndltc/raiden/xud
#Testnet/Mainnet
logs bitcoind/litecoind/geth/lndbtc/lndltc/raiden/xud
```
* Blockchain & wallet data is stored in `~/.xud-docker` by default. Customize this directory with `--home-dir`, which you need to append every time you run `xud.sh`:
```bash
bash ~/xud.sh --home-dir /path/to/your/xud-docker/home
```
* You can also customize the directory of L1 clients with:
```bash
#bitcoind
bash ~/xud.sh --bitcoind-dir  /path/to/your/bitcoind/dir
#litecoind
bash ~/xud.sh --litecoind-dir  /path/to/your/litecoind/dir
#geth (all on SSD)
bash ~/xud.sh --geth-dir  /path/to/your/geth/ssd/dir
#geth split (chain-data on HDD)
bash ~/xud.sh --geth-dir  /path/to/your/geth/ssd/dir --geth-chaindata-dir /path/to/your/geth/hdd/dir
```
* You can `exit` from `xud ctl` any time and re-enter with `bash ~/xud.sh`. A reboot of your host machine does not restart your `xud-docker` environment by default. You will need to `unlock` your environment after a restart.
* Shutdown environment & remove all data
```bash
docker-compose down
# Use with caution: this step removes all `xud` blockchain and wallet data from your system. If you still have channels open or lost your seed mnemonic, you are risking to loose funds.
rm -rf /path/to/your/xud-docker/home
rm -rf ~/xud.sh
```


## References
* [bitcoind config options](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
* [litecoind config options](https://litecoin.info/index.php/Litecoin.conf#litecoin.conf_Configuration_File)
* [geth config options](https://github.com/ethereum/go-ethereum/blob/master/README.md#configuration)
* [lnd config options](https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf)
* [raiden config options](https://raiden-network.readthedocs.io/en/stable/config_file.html)
* [xud config options](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf)