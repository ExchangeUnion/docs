This guide is written for individuals and entities looking to run xud in order to become a market maker on OpenDEX. Traders can use this guide to run xud to trade on OpenDEX directly without involvement of any third party.

# Prerequisites

## Two Modes

1. **Light setup** using [Neutrino](https://github.com/lightninglabs/neutrino) and [Infura](https://infura.io/). This keeps the setup light-weight & cheap, but requires to trust entities like [Infura](https://infura.io/).
2. **Full setup** using [bitcoind](https://github.com/bitcoin/bitcoin/), [litecoind](https://github.com/litecoin-project/litecoin) and [geth](https://github.com/ethereum/go-ethereum). Requires more resources and an SSD, but keeps the setup trustless.

## Three Networks

1. **Simnet**. `Status: live | Mode: light | Required CPUs: 2 , RAM: 2 GB , Disk: 1 GB , Time: 15 mins`

    Private chains which are maintained by Exchange Union. We’ll automatically open channels to you and push over some coins, you’ll be trading against our bots and anyone else running simnet. It’s the perfect playground to see how things work and get familiar with `xucli` commands. It’s easy: run the launch script, create the environment, wait for about 15 minutes for your balance (`getbalance`) and you are ready to go. **You want to start with this!** 

2. **Testnet**. `Status: live | Mode: light/full | Required CPUs: 2/4 , RAM: 2/16 GB , Disk: 1/200 GB , Time: 15 mins/5-24h`

    bitcoin testnet 3, litecoin testnet 4, ethereum rinkeby. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.rinkeby.io/) or [2](https://testnet.help/en/ethfaucet/rinkeby). If you need help or some channels with testnet coins, hit us up on [Discord](https://discord.gg/YgDhMSn)!

3. **Mainnet**. `Status: maintenance | Mode: light/full | Required CPUs: 2/4 , RAM: 2/16 GB , Disk: 1/700 GB , Time: 15 mins/24-72h`
    
    The real deal. Only with #reckless hat.


## Hardware
Since market makers should be online 24/7 and we are ushering in a post-cloud era, we recommend setting up a power-efficient linux box connected to your router. No special configurations, like port forwardings, are necessary. Running your xud setup in the cloud is obviously possible, just not something we encourage to do.
* **Standard**: Our [RaspiXUD guide](RaspiXUD.md) walks you through setting up a Raspberry Pi3/4. Costs: **70€-285€**
* **Pro**: Our [BeastXUD guide](BeastXUD.md) walks you through setting up a powerful Mini PC. Costs: **200€-500€**
* **Custom**: If you are using a different device or a cloud VPS:
  * Check the hardware requirements for the different networks and modes above
  * The full setup requires a SSD for geth being able to sync. For the light setup, a regular HDD/SD card is fine.
  * If you are using a VPS for testnet or mainnet, you can switch to 2 cores & 4 GB RAM after initial sync, given you use the default `cache = 256` for geth. If you run testnet & mainnet at the same time 2 cores & 8 GB RAM.
  * We support `amd64` (also called `x86`/`x64`) and `arm64` (also called `aarch64`/`armv8`), which should cover most devices and services.

## Software

* Linux or macOS. [Windows WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) support is currently experimental and not tested regularly. This guide was written assuming Ubuntu 20.04 LTS.

* Docker >= 18.09. Check with `docker --version`. If you do not have docker installed yet and you are using Ubuntu 20.04 LTS, install docker by running `sudo apt install docker.io`. Otherwise if you are using any version besides Ubuntu 20.04, follow the [official install instructions](https://docs.docker.com/install/). Also make sure that the current user can run docker (without adding `sudo`). Test with `docker run hello-world`. If this fails, [follow these instructions](https://docs.docker.com/install/linux/linux-postinstall/).

# The Setup

From here we assume that your device is running, with docker installed and backup drive connected. Check the guides in the hardware section above.

## Preparation Full Setup (skip this when using the default light setup)

```bash
xud@ubuntu:~$ mkdir -p ~/.xud-docker/mainnet/
xud@ubuntu:~$ nano ~/.xud-docker/xud-docker.conf
# add this line to permanently set `xud`'s mainnet directory to the SSD
mainnet-dir = "/media/SSD"
# CTRL+S, CTRL+X.
```

## Let's Roll

Start the environment with

```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
```
The setup will ask you to choose the network:
```
1) Simnet
2) Testnet
3) Mainnet
Please choose the network: 3
🚀 Launching mainnet environment
🌍 Checking for updates ...
```
Then you will be guided through some basics:
```
Do you want to create a new xud environment or restore an existing one?
1) Create New
2) Restore Existing
Please choose: 1
```
When creating a new XUD SEED, the setup asks you to set a password to encrypt your environment's private keys and to write down your mnemonic phrase. This serves as backup for your xud node key and wallets (your on-chain assets). This is your last resort in case something happens to your device. **Keep it somewhere safe!**

```
You are creating an xud node key and underlying wallets. All will be secured by a single password provided below.
  
Enter a password: 
Re-enter password: 

----------------------BEGIN XUD SEED---------------------
 1. you         2. won't       3. find        4. money      
 5. in          6. this        7. seed        8. but    
 9. good       10. thinking   11. if         12. you      
13. are        14. interested 15. in         16. getting     
17. rewarded   18. for        19. testing    20. xud's  
21. security   22. hit        23. us         24. up   
-----------------------END XUD SEED----------------------

The following wallets were initialized: BTC, LTC, ERC20(ETH)
```

Then you'll be asked to enter the path to your backup drive, e.g. a previously mounted USB drive:
```
Please enter a path to a destination where to store a backup of your environment. It includes everything, but NOT your on-chain wallet balance which is secured by your XUD SEED. The path should be an external drive, like a USB or network drive, which is permanently available on your device since backups are written constantly.

Enter path to backup location: /media/USB/
Checking... OK.
```

The entered backup drive location is persistet as `backup-dir = "/media/USB/"` in `mainnet.conf` and can be changed any time. Alternatively, you can consider running your environment on two hard drives in [RAID 1](https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_1) to protect against data loss.

After this, the setup pulls containers, starts syncing and opens:

```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
```

Use the `status` command to check on the syncing progress. The default light setup should be ready within 15-20 minutes:
```
mainnet > status
┌───────────┬────────────────────────────────────────────────┐
│ SERVICE   │ STATUS                                         │
├───────────┼────────────────────────────────────────────────┤
│ bitcoind  │ Ready (light mode)                             │
├───────────┼────────────────────────────────────────────────┤
│ litecoind │ Ready (light mode)                             │
├───────────┼────────────────────────────────────────────────┤
│ geth      │ Ready (light mode)                             │
├───────────┼────────────────────────────────────────────────┤
│ lndbtc    │ Syncing                                        │
├───────────┼────────────────────────────────────────────────┤
│ lndltc    │ Syncing                                        │
├───────────┼────────────────────────────────────────────────┤
│ connext   │ Ready                                          │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for lndbtc, lndltc                     │
└───────────┴────────────────────────────────────────────────┘
```
If you configured the full setup via config file or cli parameters, the sync will start fast and gets slower towards the end. You might see 0.00% progress for several minutes first.

```
mainnet > status
┌───────────┬────────────────────────────────────────────────┐
│ SERVICE   │ STATUS                                         │
├───────────┼────────────────────────────────────────────────┤
│ bitcoind  │ Syncing 0.00% (0/436000)                       │
├───────────┼────────────────────────────────────────────────┤
│ litecoind │ Syncing 0.00% (0/324000)                       │
├───────────┼────────────────────────────────────────────────┤
│ geth      │ Syncing 0.00% (55/9140561)                     │
├───────────┼────────────────────────────────────────────────┤
│ lndbtc    │ Waiting for sync                               │
├───────────┼────────────────────────────────────────────────┤
│ lndltc    │ Waiting for sync                               │
├───────────┼────────────────────────────────────────────────┤
│ connext   │ Waiting for sync                               │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for sync                               │
└───────────┴────────────────────────────────────────────────┘
```

After a while you should see all three full-nodes syncing nicely.
```
mainnet > status
┌───────────┬────────────────────────────────────────────────┐
│ SERVICE   │ STATUS                                         │
├───────────┼────────────────────────────────────────────────┤
│ bitcoind  │ Syncing 43.06% (262348/609123)                 │
├───────────┼────────────────────────────────────────────────┤
│ litecoind │ Syncing 35.94% (631593/1757002)                │
├───────────┼────────────────────────────────────────────────┤
│ geth      │ Syncing 10.16% (929072/9140623)                │
├───────────┼────────────────────────────────────────────────┤
│ lndbtc    │ Waiting for sync                               │
├───────────┼────────────────────────────────────────────────┤
│ lndltc    │ Waiting for sync                               │
├───────────┼────────────────────────────────────────────────┤
│ connext   │ Ready                                          │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for sync                               │
└───────────┴────────────────────────────────────────────────┘
```
Bitcoind/Litecoind should finish syncing within 12h, geth in about 72h on powerful hardware. A Pi4 needs about twice that long.


`xud ctl` takes `xucli` commands without the need to prepend `xucli`, e.g. simply type `getinfo` to get basic information about your xud node. Run `help` to get an always up-to-date list of commands. Append `-j` to any command to get JSON instead of the formatted output, e.g. using `listpeers` to see other xud nodes on the network:

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
        "ETH/BTC",
        "BTC/DAI",
        "LTC/DAI"
      ],
      "xudVersion": "1.0.0-beta",
      "secondsConnected": 100,
      "connextAddress": "0xe802431257a1d9366BD5747F0F52bAd25A6C3092"
    }
  ]
}
```

## Your First Trade

On Simnet simply wait for about 15 minutes and you'll have channels and are read to go (check with `getinfo` and `getbalance`). On Testnet/Mainnet, start by deposit some coins: 

```bash
walletdeposit btc #Send BTC to this address
walletdeposit ltc #Send LTC to this address
walletdeposit eth #Send ETH/ETH-ERC20 tokens to this address
```

The next step will have an automated option in future, but currently is not trivial: choose a xud node to open a channel with. Ideally, these are nodes you expect to trade with regularly. If you are unsure, you can open a channel with our xud node available at xud1.exchangeunion.com. You can get its alias via `listpeers` and opening a 0.1 btc channel would look like this, whereas `CheeseMonkey` is the node's alias:

```
openchannel BTC 0.1 CheeseMonkey
```

Check existing orders in the network with the command `orderbook` for all enabled trading pairs or with `orderbook btc/dai` for BTC/DAI only:

```
mainnet > orderbook btc/dai

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
│ 0.1               │ 7171.083          │ 7316.0663         │ 1                 │
├───────────────────┼───────────────────┼───────────────────┼───────────────────┤
│ 0.1               │ 7170.899          │ 7316.44           │ 0.22393946        │
└───────────────────┴───────────────────┴───────────────────┴───────────────────┘
```

Use `getbalance` to check your balance *before* the swap.

```
mainnet > getbalance

Balance:
┌──────────┬───────────────┬────────────────────────────┬───────────────────────────────┐
│ Currency │ Total Balance │ Channel Balance (Tradable) │ Wallet Balance (Not Tradable) │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ BTC      │ 6.10944853    │ 2.5                        │ 3.60944853                    │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ DAI      │ 5000          │ 5000                       │ 0                             │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ LTC      │ 21            │ 11                         │ 10                            │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ ETH      │ 500           │ 500                        │ 0                             │
└──────────┴───────────────┴────────────────────────────┴───────────────────────────────┘
```

Issue a regular limit order with e.g. `sell 0.1 btc/dai 7171` to sell 0.1 btc for a price of 7171 DAI per BTC. Settlement of your order shouldn't take longer than a couple of seconds. 

```
mainnet > sell 0.1 btc/dai 7171
swapped 0.1 BTC with peer order ca24fe00-1c1e-11ea-8b1b-3b2ec0335696
```

Use `getbalance` to check your balance *after* the swap. You are now owning 0.1 BTC less and 717 Dai more.

```
mainnet > getbalance

Balance:
┌──────────┬───────────────┬────────────────────────────┬───────────────────────────────┐
│ Currency │ Total Balance │ Channel Balance (Tradable) │ Wallet Balance (Not Tradable) │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ BTC      │ 6.00944842    │ 2.39999989                 │ 3.60944853                    │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ DAI      │ 5717          │ 5717                       │ 0                             │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ LTC      │ 21            │ 11                         │ 10                            │
├──────────┼───────────────┼────────────────────────────┼───────────────────────────────┤
│ ETH      │ 500           │ 500                        │ 0                             │
└──────────┴───────────────┴────────────────────────────┴───────────────────────────────┘
```

## Connect to Exchange (for Market Makers) 

**Coming soon!**

# Report Issues

Please report issues/bugs by running `report` from within `xud ctl`.

# Tips 'n Tricks
* If you are syncing the full setup, and `geth` shows sync status **99.99%** for longer than 72h, you are likely running geth on a drive that is simply too slow for geth to catch up with the chain. In this case, `down` the environment and run a performance test of the disk as desribed [here](RaspiXUD.md#pi-full-setup). If results are below the 100 MB/s mark, you can either switch to a faster SSD or combine bitcoind and litecoind with infura instead of a local geth.
* If you only have a small SSD available, you can place your entire setup on a HDD, except for a small part of geth's data, which needs to be located on a fast SSD. To do so, create the config file, e.g. in the mainnet directory located on the HDD with `cp sample-mainnet.conf mainnet.conf`, then edit the following two options in `mainnet.conf`:
```bash
[geth]
# internal SSD
dir = "/home/<user>/.xud-docker/mainnet/geth"
# external HDD
ancient-chaindata-dir = "/media/HDD/xud/03-Mainnet/data/geth"
```
* The xud-docker setup uses the fixed home directory `~/.xud-docker` where blockchain & wallet data is stored in by default. Customize the wallet & chain data directory by creating a config file with `cp ~/.xud-docker/sample-xud-docker.conf ~/.xud-docker/xud-docker.conf`, then edit `xud-docker.conf`. For temporarily using another directory, you can also use parameters, e.g. `bash xud.sh --mainnet-dir /path/to/temp/mainnet/dir`.
* External full-nodes (including infura) can be configured in a network specific config file. Create the config file, e.g. in the mainnet directory with `cp sample-mainnet.conf mainnet.conf`, then edit `mainnet.conf`.
```bash
# connect to an external bitcoin core node in your local network
[bitcoind]
external = true
rpc-host = "192.168.1.42"
rpc-port = "8332"
rpc-user = "user"
rpc-password = "pass"
zmqpubrawblock = "192.168.1.42:28332"
zmqpubrawtx = "192.168.1.42:28333"
```
* Permanently set the alias `xud` to launch `xud ctl` from anywhere:
Add the line `alias xud="bash ~/xud.sh"` to the end of `~/.bashrc` or `~/.bash_aliases` on Linux and `bash_profile` on Mac, then `source` the file.
* `xud ctl` allows to use an L1/L2 client's cli:
```bash
bitcoin-cli --help
litecoin-cli --help
geth --help
lndbtc-lncli --help
lndltc-lncli --help
xucli --help
```
* To inspect logs:
```bash
logs bitcoind/litecoind/geth/lndbtc/lndltc/connext/xud
```
* You can `exit` from `xud ctl` any time and re-enter with `bash ~/xud.sh`.
* A reboot of your host machine does **not** restart your `xud-docker` environment by default. You will need to run `bash ~/xud.sh` and `unlock` your environment.
* Shutdown the environment with `down`. Restart with `down`, then run `bash ~/xud.sh` again.
* Remove all data with
```bash
# Use with caution: this step removes all `xud` blockchain and wallet data from your system. If you have channels open without backup or lost your seed mnemonic, you are risking to loose funds.
sudo rm -rf ~/.xud-docker
rm -rf ~/xud.sh
rm -rf /custom/mainnet/dir
```
* `xud-docker` only uses official xud releases for mainnet. Simnet/Testnet are updated frequently. If you want to run a specific xud branch or master, follow [these instructions](https://github.com/ExchangeUnion/xud-docker/#developing). For mainnet, we strongly recommend to run the latest [official release](https://github.com/ExchangeUnion/xud/releases/) only. #craefulgang
* Docker might not play nicely with a VPN you are running on the same machine. If you see `Failed to launch environment`, try disconnecting the VPN.

## References
* [bitcoind config options](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
* [litecoind config options](https://litecoin.info/index.php/Litecoin.conf#litecoin.conf_Configuration_File)
* [geth config options](https://github.com/ethereum/go-ethereum/blob/master/README.md#configuration)
* [lnd config options](https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf)
* [connext config options](https://docs.connext.network/en/latest/quickstart/clientInstantiation.html#client-options)
* [xud config options](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf)
