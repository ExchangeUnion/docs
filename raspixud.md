Shopping List:
Can't stress enough that a decently fast microSD card is important.
https://www.amazon.de/SanDisk-SDSQXCG-032G-GN6MA-Extreme-Adapter-Schwarz/dp/B06XYHN68L/ref=sr_1_6?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=sandisk+extreme+microSDHC&qid=1577037988&sr=8-6
https://jamesachambers.com/raspberry-pi-storage-benchmarks/

Sync Full-nodes on a more powerful machine:
```
kilrau@K-Yoga:~/.xud-docker$ xud -b --home-dir /media/kilrau/T5
1) Simnet
2) Testnet
3) Mainnet
Please choose the network: 3
Checking for updates...
* Image exchangeunion/raiden:latest: outdated
* Image exchangeunion/lnd:0.8.2-beta: outdated
* Container mainnet_bitcoind_1: missing
* Container mainnet_litecoind_1: missing
* Container mainnet_geth_1: missing
* Container mainnet_lndbtc_1: missing
* Container mainnet_lndltc_1: missing
* Container mainnet_raiden_1: missing
* Container mainnet_xud_1: missing
Pulling exchangeunion/raiden:latest__feat-full-node
Retagging exchangeunion/raiden:latest__feat-full-node to exchangeunion/raiden:latest
Pulling exchangeunion/~/.xud-docker$ xud -b --home-dir /media/kilrau/T5
Enter a password: 
Re-enter password: 

kilrau@K-Yoga:~/.xud-docker$ xud -b feat/full-node --home-dir /media/kilrau/T5
1) Simnet
2) Testnet
3) Mainnet
Please choose the network: 3
Checking for updates...
* Image exchangeunion/raiden:latest: outdated
* Image exchangeunion/lnd:0.8.2-beta: outdated
* Container mainnet_bitcoind_1: missing
* Container mainnet_litecoind_1: missing
* Container mainnet_geth_1: missing
* Container mainnet_lndbtc_1: missing
* Container mainnet_lndltc_1: missing
* Container mainnet_raiden_1: missing
* Container mainnet_xud_1: missing
Pulling exchangeunion/raiden:latest__feat-full-node
Retagging exchangeunion/raiden:latest__feat-full-node to exchangeunion/raiden:latest
Pulling exchangeunion/lnd:0.8.2-beta__feat-full-node
Retagging exchangeunion/lnd:0.8.2-beta__feat-full-node to exchangeunion/lnd:0.8.2-beta
Creating mainnet_bitcoind_1
Creating mainnet_litecoind_1
Creating mainnet_geth_1
Creating mainnet_lndbtc_1
Creating mainnet_lndltc_1
Creating mainnet_raiden_1
Creating mainnet_xud_1

You are creating an xud node key and underlying wallets. All will be secured by
a single password provided below.
  
Enter a password: 
Re-enter password: 

----------------------BEGIN XUD SEED---------------------
 1. abandon     2. destroy     3. moon        4. wish      
 5. tone        6. blast       7. news        8. electric  
 9. shrimp     10. abuse      11. curious    12. decorate  
13. stuff      14. such       15. mule       16. arch      
17. release    18. property   19. dutch      20. trouble   
21. bid        22. volcano    23. rifle      24. current   
-----------------------END XUD SEED----------------------

The following wallets were initialized: BTC, LTC, ERC20(ETH)

Please write down your 24 word mnemonic. It will allow you to recover your xud
node key and on-chain funds for the initialized wallets listed above should you
forget your password or lose your device. Off-chain funds in channels can NOT
be recovered with it and must be backed up and recovered separately. Keep it
somewhere safe, it is your ONLY backup in case of data loss.
    
YOU WILL NOT BE ABLE TO DISPLAY YOUR XUD SEED AGAIN. Press ENTER to continue...

                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
--------------------------------------------------------------

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
│ raiden    │ Container running                              │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for sync                               │


The following wallets were initialized: BTC, LTC, ERC20(ETH)

Please write down your 24 word mnemonic. It will allow you to recover your xud
node key and on-chain funds for the initialized wallets listed above should you
forget your password or lose your device. Off-chain funds in channels can NOT
be recovered with it and must be backed up and recovered separately. Keep it
somewhere safe, it is your ONLY backup in case of data loss.
    
YOU WILL NOT BE ABLE TO DISPLAY YOUR XUD SEED AGAIN. Press ENTER to continue...

                           .___           __  .__   
          ___  _____ __  __| _/     _____/  |_|  |  
          \  \/  /  |  \/ __ |    _/ ___\   __\  |  
           >    <|  |  / /_/ |    \  \___|  | |  |__
          /__/\_ \____/\____ |     \___  >__| |____/
                \/          \/         \/           
--------------------------------------------------------------

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
│ raiden    │ Container running                              │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for sync                               │
└───────────┴────────────────────────────────────────────────┘
```

After a while you should see all three full-nodes sync nicely:
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
│ raiden    │ Container running                              │
├───────────┼────────────────────────────────────────────────┤
│ xud       │ Waiting for sync                               │
└───────────┴────────────────────────────────────────────────┘
```
It starts fast (blocks were kinda empty in the beginning of the timechain, and gets slower towards the end.

## Setup Pi 4

https://ubuntu.com/download/raspberry-pi , 64bit version, follow instructions, update

add new user xud

```
ubuntu@ubuntu:~$ sudo adduser xud
Adding user `xud' ...
Adding new group `xud' (1001) ...
Adding new user `xud' (1001) with group `xud' ...
Creating home directory `/home/xud' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for xud
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 

Is the information correct? [Y/n] ubuntu@ubuntu:~$ Y
ubuntu@ubuntu:~$ sudo usermod -aG docker xud
ubuntu@ubuntu:~$ sudo su - xud
xud@ubuntu:~$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

xud@ubuntu:~$ 

```
Add alias for easy starting of xud: `sudo nano ~/.bash_aliases`, `alias xud='bash ~/xud.sh'` CTRL+X, Y, Enter, `source ~/.bashrc`.

https://docs.docker.com/install/linux/docker-ce/ubuntu/

select arm64, ubuntu eoan still not officially supported by docker, hence run:
```
sudo add-apt-repository \
   "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
   disco \
   stable"
```

Reminder: geth is RAM hungry
CREATE Swap file of 12GB or larger (totalling min 16GB):
https://hostadvice.com/how-to/how-to-add-swap-space-in-ubuntu-16-1/

Once you see bitcoind, litecoind and geth as `ready` in when running `status`, `down` the environment and connect the SSD to your pi4.

Guide on how to automount the SSD: https://www.cyberciti.biz/faq/mount-drive-from-command-line-ubuntu-linux/

Reminder: geth is RAM hungry
CREATE Swap file of 12GB or larger (totalling min 16GB):

Vamonos!
xud@ubuntu:~$ xud -b --home-dir /media/xud/T5





