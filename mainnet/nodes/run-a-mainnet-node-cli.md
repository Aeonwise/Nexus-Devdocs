---
description: How to run a CLI node on mainnet - stable release
---

# Run a Mainnet Node -CLI

{% hint style="info" %}
This guide is tailored for ubuntu / raspian distributions
{% endhint %}

## **1. Before Starting this guide:**

* A computer with a minimum of 1 CPU, 2 GB RAM and 64GB hard disk space, Raspberry Pi 4 with 2 GB RAM with 64 GB SD card.&#x20;
* For VPS 1GB RAM is sufficient for normal usage, for dapp backend contact [here](https://t.me/NexusDevelopers).
* [Ubuntu server 20.04 LTS](https://ubuntu.com/download/server) for AMD/IA 64 or [Ubuntu IOT](https://ubuntu.com/download/iot) for Raspberry Pi or any distribution of choice
* [Etcher ](https://www.balena.io/etcher/)– To burn the image file to SD card
* Putty if using SSH via windows.

{% hint style="info" %}
To build on Raspberry Pi 3 or 4 with 1 GB RAM, enable swap memory with instructions from link below. Proceed after setting up swap.
{% endhint %}

{% embed url="https://rayanfer32.medium.com/enable-swap-memory-on-ubuntu-on-raspberry-pi-a0f873a65e74" %}

{% embed url="https://www.youtube.com/watch?t=942s&v=sA-DUX9KBNU" %}
This is a video guide for setting up a CLI node
{% endembed %}

## **2. Prepare The Node:**

[Install ubuntu server 20.04 LTS](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview) or distro of choice, install open-ssh server during the install and once the installation is complete restart the node. SSH in node and follow the below commands.  Copy the commands and paste it in the terminal using keys CTRL+SHIFT+v

Update and upgrade the node:

```
sudo apt update; sudo apt upgrade -y
```

Open SSH port before enabling firewall:

```
sudo ufw allow ssh
```

Allow API port 8080

```
sudo ufw allow 8080/tcp
```

Allow RPC port 9336

```
sudo ufw allow 9336/tcp
```

Allow mining port 9325, only if connecting a miner

```
sudo ufw allow 9325/tcp
```

Enable firewall:

```
sudo ufw enable
```

Check firewall status:

```
sudo ufw status
```

Set the node timezone:

```
sudo dpkg-reconfigure tzdata
```

To change the hostname – Optional

```
sudo hostnamectl set-hostname <newhostname>
```

Reboot node:

```
sudo reboot
```

The computer is ready to install the Nexus core

## **3. Compiling Nexus Core:**

Installs the dependencies required for compiling nexus core CLI, It will take some time to complete depending on your internet speed

```
sudo apt-get install -y build-essential libssl-dev libdb-dev libdb++-dev libminiupnpc-dev git
```

Download the latest nexus core source code, and should only take a few seconds to complete,

```
git clone --depth 1 https://github.com/Nexusoft/LLL-TAO
```

Change into the source code directory

```
cd LLL-TAO
```

Lastly run this command to compile from source. This begins compiling the nexus core, please be patient, as this can take a very long time depending on your CPU. Replace the 1 in ‘j1’to the number of cores / threads for compiling faster.

```
make -f makefile.cli -j1 AMD64=1
```

For compiling on Raspberry Pi use

```
make -f makefile.cli -j1 ARM64=1
```

Will show “Finished building nexus” on a successful compile.

## **4. Configure The wallet (nexus.conf):**

Create Nexus core data directory (it’s a hidden directory, Nexus daemon creates it automatically on first start. We are creating it manually to create the configuration file. If the directory is already available, skip this step.)

```
mkdir ~/.Nexus
```

{% hint style="info" %}
The wallet configuration is stored in the nexus.conf file, located in the core data directory
{% endhint %}

Create the nexus.conf file

```
nano ~/.Nexus/nexus.conf
```

Copy the below code into the file. Provide a user and password for RPC and API and remove the `#` in the `#stake=1` if you intend to stake.

```
# This is an example nexus.conf. Tailor it to your deployment.
#RPC credentials, change as needed
rpcuser=<user for RPC>
rpcpassword=<pw for RPC>
#API http authentication credtnials. Change as needed
apiuser=<user for API>
apipassword=<pw for API>
#To run the nexus core as a daemon
daemon=1
#To enable mining with the node, 
#mining=1
#To enable staking
#stake=1
```

To access the wallet remotely (from a wallet interface on another machine), add the following lines in the config file.

{% hint style="info" %}
`<ipaddress>` is IP address of the machine connecting to your node remotely, it may be the interface or dapp server IP address. You can also use '\*' wildcard to allow all computers in a particular subnet. `llpallowip=192.168.10.*:8080` allows all computers on the 192.168.10 local network
{% endhint %}

```
#To enable RPC remote access
rpcremote=1
#To enable API authentication 
apiauth=1
#To enable remote API access. Local API access will be revoked
apiremote=1
# Whitelist IP address for API & RPC
llpallowip=<ipaddress>:8080  
llpallowip=<ipaddress>:9336
```

Press Ctrl s and Ctrl x to save and exit the editor

## **5. Bootstrapping Tritium Database:**

This step will download the nexus database and then extract it to the data folder, alternatively you can skip this step if you choose to synchronise the data from peers which will be slow depending on your internet connection.

```
cd ~
```

This will download the database to the home folder. The file is about 5 GB in size.

```
wget -c https://nexus.io/bootstrap/tritium/tritium.tar.gz
```

This will extract the database to the Nexus core data directory

```
tar -xf tritium.tar.gz -C ~/.Nexus
```

## **6. API's To Control Node:**

To interact with the nexus core daemon, use API commands via the terminal. Change the location to \~/LLL-TAO folder for the API’s to work. To run the node, stake and transact we will only be using the system, users and finance API’s. If you have any doubts refer to the API documentation [here](../../api/api-overview/tritium-api/).

### **Before we start:**

* Be careful as the login credentials in the terminal and can be seen by anyone around.
* Every transaction will need the PIN
* Every transaction on the Nexus blockchain is a debit to the sending account and credit to the receiving account, two transactions. (This will be useful to understand some API commands)

Change into the LLL-TAO directory to start nexus core (Change to the LLL-TAO folder to run the following commands)

```
cd LLL-TAO
```

### To start the daemon:

```
./nexus
```

Nexus core will be running in the background as a daemon. It will detect peers and synchronize the blockchain. It will take a few minutes to find peers

### To stop the daemon:

```
./nexus system/stop
```

### To get the node info:

```
./nexus system/get/info
```

The output of the API’s is in json format. You can confirm if the node is synchronized by checking the “synchronising”: false and “synccomplete”: 100. Check the block height on the explorer [here](http://explorer.nexus.io) or [here](https://nxsorbitalscan.com)

After the blockchain is fully synchronized, create a new user account (signature chain).

Username must be a minimum of 2 characters, passwords must be 8 characters and PIN 4 characters. The PIN can be a combination of letters/numbers/symbols.

```
./nexus users/create/user username= password= pin=
```

To log into the account (If the account was just created, wait for a few blocks to confirm new account)

```
./nexus users/login/user username= password= pin=
```

To unlock the account for staking and automatically credit incoming transactions set (notifications=1). If its not set, then manually credit the incoming transactions else it will be credited back to the sender’s account after 24 hrs. This is the reversible transaction function working as designed.

```
./nexus users/unlock/user pin= staking=1 notifications=1
```

To check stake info (works only after login and unlocking for staking)

```
./nexus finance/get/stakeinfo
```

To get details of the logged in user accounts and address. (Trust and default accounts are automatically created with new account)

```
./nexus users/list/accounts
```

This will list all the account and its details

```
./nexus finance/list/accounts
```

This will list all transactions sent to a particular genesis or username. It is useful for identifying transactions that need to accept such as credits.

```
./nexus users/list/notifications
```

If automatic credit (`notifications=1`) is not specified as an option with `unlock` command that will reflect as a pending transaction and will be listed in notifications.&#x20;

To credit a pending transaction, ‘txid’ is the debit transaction id from notifications

```
./nexus finance/credit/account pin= txid=
```

To send nexus coins, `name` is the account to be sent from and `name_to` is the recipient account, these can be changed to `address` and `address_to`

```
./nexus finance/debit/account name=username:name/namespace:name amount= name_to=username:name/namespace:name pin=
```

To check the full Nexus blockchain metrics.

```
./nexus system/get/metrics
```

Hope this guide was helpful !!
