This guide is written for users who want to buy and sell cryptocurrency via `xud`. It explains how to use [`xud-docker`](https://github.com/ExchangeUnion/xud-docker), the recommended and easiest way to get up and running.

# Networks

## Simnet
Private chains which are maintained by us. We’ll automatically open channels to you and push over some coins, you’ll be trading against our bots and anyone else running simnet. It’s the perfect playground to see how things work and play around with `xucli` commands. It’s easy: run one script, wait for about 10 minutes and you are ready to go. **You want to start with this!**

Status: `live` | Setup time: `~15 mins` | Disk space: `5 GB`

## Testnet
bitcoin testnet 3, litecoin testnet 4, ethereum ropsten. Faucets: [t-BTC](https://coinfaucet.eu/en/btc-testnet/), [t-LTC](https://faucet.xblau.com/), [t-ETH 1](https://faucet.ropsten.be/) & [2](https://faucet.metamask.io/). Quite a bit of manual work to be done here. If you need help or some channels with testnet coins, hit us up on [Discord](https://discord.gg/YgDhMSn)!

Status: `live` | Setup time: `~5-24h` | Disk space: `120 GB`

## Mainnet
Real money - only with #reckless hat.

Status: `live` (with $10-per-trade limit) | Setup time: `~1-3 days` | Disk space: `500 GB`


# Requirements

1. Linux or macOS. [Windows WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install) support is currently experimental and not tested regularly.

2. 12GB RAM for testnet & mainnet (we saw some weird container states with less). `geth` likes RAM. A lot. If you have >=24GB RAM available, you can significantly shorten `geth`'s syncing time by increasing its `--cache=1024` flag for [testnet](https://github.com/ExchangeUnion/xud-docker/blob/master/xud-testnet/docker-compose.yml) or [mainnet](https://github.com/ExchangeUnion/xud-docker/blob/master/xud-mainnet/docker-compose.yml) to something larger.

3. A SSD for testnet & mainnet. Based on painful experience: `geth` **cannot** catch up with the chain when running on a regular HDD. Read about it [here](https://medium.com/blockchain-studio/ethereum-client-geth-v1-9-0-released-whats-new-2b3de043ee16).

4. `docker` >= 18.09 & `docker-compose` >= 1.24. Check with `docker --version` & `docker-compose --version`. If you do not have these installed yet, follow the official install instructions for [docker](https://docs.docker.com/install/) and [docker-compose](https://docs.docker.com/compose/install/). Also make sure that the current user can run docker (without adding `sudo`). Test with `docker run hello-world`. If it fails, [follow these instructions](https://docs.docker.com/install/linux/linux-postinstall/).

5. Python 2.7+ or 3+ (pre-installed on most systems, check with `python --version` )

# How to run

Start the environment with
```bash
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh
```
This guides you through a setup on first run, pulls containers, starts syncing chains and opens
```
                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
--------------------------------------------------------------
```

The `status` command shows status of underlying clients, which is especially useful to track the sync status. If you are syncing full nodes on Testnet/Mainnet, this takes several hours or even days. All clients should show `Ready` before you continue.
```bash
btc	Ready
lndbtc	Ready
ltc	Syncing 2.40% (26887/1116528)
lndltc	Waiting for sync
geth	Syncing 42.61% (2489756/5842588)
raiden	Waiting for sync
xud	Waiting for sync
```

`xud ctl` takes [`xucli` commands](https://api.exchangeunion.com), like `getinfo`. Once everything is up and running, you can check existing orders of your connected peers with `orderbook` and issue an order, e.g. `sell 0.1 btc/dai 9998`. Check your `balance` before and after the swap to see it changing.

# Beware

Raiden currently requires direct channels with trading partners to swap reliably. We have a temporary check in place, that discards raiden-related orders (all pairs which include WETH, DAI...), if `xud` can't find a direct channel to the trading partner. You can switch this check off by setting `raidenDirectChannelChecks=false` in your `xud.conf`. Before you do that, read [this explainer of the issue](https://github.com/ExchangeUnion/xud/issues/1068).

# Report

Please report issues/bugs by running `report` from within `xud ctl`.

# Tips 'n Tricks

* Docker might not play nicely with a VPN you are running on the same machine. If you see `Failed to launch simnet environment`, try disconnecting the VPN.
* We placed `xud` & `lnd` behind TOR by default, which improves privacy and does away with the need to open ports
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
Add the line `alias xud="bash ~/xud.sh"` to the end of `~/.bashrc` on Linux or `bash_profile` on Mac, then source the file.
* To inspect logs (use `logs -f` if you want to follow the log live):
```bash
#Simnet
logs ltcd/geth/lndbtc/lndltc/raiden/xud
#Testnet/Mainnet
logs bitcoind/litecoind/geth/lndbtc/lndltc/raiden/xud
```

* Blockchain & wallet data is by default stored in `~/.xud-docker`:
```bash
#Simnet
~/.xud-docker/simnet/data
#Testnet
~/.xud-docker/testnet/data
#Mainnet
~/.xud-docker/mainnet/data
```

* Customize this directory with `--home-dir`, which you need to append every time you run `xud.sh`
```bash
bash ~/xud.sh --home-dir /path/to/new/xud-docker/home
```

* Shutdown environment & remove all data
```bash
docker-compose down
# Use with caution: this step removes all `xud` blockchain and wallet data from your system. If you still have channels open or lost your seed mnemonic, you are risking to loose funds.
rm -rf ~/.xud-docker/
rm -rf ~/xud.sh
```


## References
* [bitcoind config options](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
* [litecoind config options](https://litecoin.info/index.php/Litecoin.conf#litecoin.conf_Configuration_File)
* [geth config options](https://github.com/ethereum/go-ethereum/blob/master/README.md#configuration)
* [lnd config options](https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf)
* [raiden config options](https://raiden-network.readthedocs.io/en/stable/config_file.html)
* [xud config options](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf)