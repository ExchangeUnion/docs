# RaspiXUD

This guide helps market makers to build their own xud-in-a-box based on a Raspberry Pi4. Two options are available:
1. **Light setup** using [Neutrino](https://github.com/lightninglabs/neutrino) and [Infura](https://infura.io/). This keeps the setup light and cheap (~100 EUR), but is not fully trustless. Also, a Pi3 should suffice for this (not tested).
2. **Full setup** using [bitcoind](https://github.com/bitcoin/bitcoin/), [litecoind](https://github.com/litecoin-project/litecoin) and [geth](https://github.com/ethereum/go-ethereum). Requires a SSD, but keeps the setup trustless.

If you are not sure which one to choose, we recommend to start with the light setup. You can swith to the full setup any time.

## Reference Shopping List (Spain)
* [Pi4 (4GB)](https://www.tiendatec.es/raspberry-pi/placas-base/1100-raspberry-pi-4-modelo-b-4gb-765756931182.html): 59,95 €
* [Pi4 Power Supply](https://www.tiendatec.es/raspberry-pi/raspberry-pi-alimentacion/1093-alimentador-oficial-raspberry-pi-4-usb-c-5v-3a-15w-negro-644824914886.html): 8,95 €
* [Pi4 Cooling Case](https://www.tiendatec.es/raspberry-pi/cajas/1110-caja-cofre-alta-disipacion-con-dos-ventiladores-para-raspberry-pi-4-8472496015950.html): 15,95 €
* [64GB MicroSD card](https://www.amazon.de/dp/B07G3GMRYF/): 19,99 €
  * A performant microSD card is important; the wrong place to save some bucks
  * For more options, check [this storage benchmark list](https://jamesachambers.com/raspberry-pi-storage-benchmarks/)
* A USB Stick for writing backups (Any >512MB USB Stick will do, NAS/Samba works too)
* [1TB external SSD](https://www.amazon.es/gp/product/B074M774TW/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1): 185 €
  * **For full setup only!**
  * For more options, check [this storage benchmark list](https://jamesachambers.com/raspberry-pi-storage-benchmarks/).

## Pi Setup

1. [Download Ubuntu 64-bit for the Pi](https://ubuntu.com/download/raspberry-pi).
2. Insert the microSD card into your computer and follow the [flash instructions](https://ubuntu.com/download/iot/installation-media).
3. Insert the microSD card into your Pi, connect all cables and fire it up. Connecting a screen via HDMI and a USB Keyboard makes life easier. Otherwise check the assigned IP in your router and SSH in from another computer 
4. Follow the inital setup instructions. Default user + password is `ubuntu`. You will be asked to change the password on first login.
5. Update your system via `sudo apt update && sudo apt upgrade`
6. Install docker following the [official instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/) (select `arm64`). At the time of writing, ubuntu eoan was still not supported in the official docker repos, hence run this instead:
```
sudo add-apt-repository \
   "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
   disco \
   stable"
```
7. Add a new user `xud`:

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
```
8. Add the `xud` user to the docker group and test if docker is working:
```
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

```
9. Looking good! Add an alias to easily start xud by typing "xud": `sudo nano ~/.bash_aliases`, add the line `alias xud='bash ~/xud.sh'`, then CTRL+X, and save with Y, Enter, then run `source ~/.bashrc` to apply the alias.
10. Light setup - **DONE!** Continue [here](Market%20Maker%20Guide.md#basic-setup).
11. For the full setup, the first thing to consider is to sync full-nodes on a more powerful machine to speed things up. If you decide to do so, connect your external SSD to this machine, format it to one large ext4 partition (e.g. with [GParted](https://gparted.org/)) and start xud with the `--home-dir` parameter pointing to the external SSD (docker needs to be installed and set up as above):
```
curl https://raw.githubusercontent.com/ExchangeUnion/xud-docker/master/xud.sh -o ~/xud.sh
bash ~/xud.sh --home-dir /media/path/to/your/SSD
```
12. Create a new xud environment and monitor the sync progress using the `status` command with `xud ctl`. Once you see bitcoind, litecoind and geth showing `Ready`, `down` the environment.
13. Unmount & connect the SSD to your Pi. Run `ls -la /dev/ | grep sd`. This should list either `sda1` or `sdb1`. This is your SSD.
14. Set the SSD to automount by adding it to `fstab` with `sudo nano /etc/fstab`:
```
/dev/sda1    /media/T5   ext4    defaults     0        2
```
15. `reboot` your pi, and see if the automounting worked by running `df -h` and watch out for an entry like `/dev/sda1       916G   72M  870G   1% /media/T5`
16. Geth consumes a lot of RAM when syncing, so we will need to create a swap file (overflow RAM) of 12GB or larger (totalling min 16GB, recommended 32GB) on the external SSD. First let's do a quick performance test of the SSD. If you are close to these values, you are good to go, whereas <50MB/s (write/read) is too low:
```
xud@ubuntu:/media/T5$ sudo dd if=/dev/zero  of=/media/T5/deleteme.dat bs=32M count=64 oflag=direct
64+0 records in
64+0 records out
2147483648 bytes (2.1 GB, 2.0 GiB) copied, 12.8709 s, 167 MB/s
xud@ubuntu:/media/T5$ sudo dd if=/media/T5/deleteme.dat of=/dev/null bs=32M count=64 iflag=direct
64+0 records in
64+0 records out
2147483648 bytes (2.1 GB, 2.0 GiB) copied, 15.5791 s, 138 MB/s
xud@ubuntu:/media/T5$ sudo rm /media/T5/deleteme.dat
```
17. Let's create the swap file on the SSD `sudo fallocate -l 28G /media/T5/swapfile`, mark it as swap file `sudo chmod 600 /media/T5/swapfile && sudo mkswap /media/T5/swapfile` and enable it `sudo swapon /media/T5/swapfile`. Let's verify it worked with
```
xud@ubuntu:/media/T5$ sudo swapon --show
NAME               TYPE SIZE USED PRIO
/media/T5/swapfile file  28G   0B   -2
```
18. Let's set xud's home directory to the SSD with `sudo nano ~/.xud-docker/xud-docker.conf
```
home-dir=/media/T5/xud-docker
```
19. 