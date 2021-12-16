---
description: How to run a CLI node on mainnet - stable release
---

# Run a Mainnet Node -CLI

{% hint style="info" %}
This guide is tailored for ubuntu/raspian distributions
{% endhint %}

## **1. Before you begin this guide:**

* A computer with a minimum of 1 CPU, 2 GB RAM and 64GB hard disk space, Raspberry Pi 4 with 2 GB RAM with 64 GB SD card
* [Ubuntu server 20.04 LTS](https://ubuntu.com/download/server) for AMD/IA 64 or [Ubuntu IOT](https://ubuntu.com/download/iot) for Raspberry Pi or any distro of your choice
* [Etcher ](https://www.balena.io/etcher/)– To burn the image file to SD card
* Putty if you are using ssh via windows.

{% hint style="info" %}
To build on Raspberry Pi 3 or 4 with 1 GB of RAM you have to enable swap memory.

Proceed after setting up swap.&#x20;
{% endhint %}

{% embed url="https://rayanfer32.medium.com/enable-swap-memory-on-ubuntu-on-raspberry-pi-a0f873a65e74" %}

{% embed url="https://www.youtube.com/watch?t=942s&v=sA-DUX9KBNU" %}

## **2. Prepare The Node:**

[Install ubuntu server 20.04 LTS](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview) or distro of your choice, install open-ssh server during the install and once the installation is complete restart the node. SSH in your node and follow the below commands. You can copy the commands and paste it in the terminal using keys CTRL+SHIFT+v

Update your node:

```
sudo apt update
```

Upgrade your node:

```
 sudo apt upgrade -y
```

Open SSH port before you enable firewall:

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

Set your timezone:

```
sudo dpkg-reconfigure tzdata
```

If you need to change the hostname – Not compulsory if you already set it during the install

```
sudo hostnamectl set-hostname <newhostname>
```

Reboot node:

```
sudo reboot
```

we have our computer ready to install the nexus core

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

Create Nexus core directory (it’s a hidden directory, Nexus daemon creates it automatically on first start. We are creating it manually to create the configuration file. If you have the directory you can skip this step.)

```
mkdir ~/.Nexus
```

Create the nexus.conf file

```
nano ~/.Nexus/nexus.conf
```

Copy the below code into the file. Provide a user and password for RPC and API and remove the # in the #stake=1 if you intend to stake.

```
#
# This is an example nexus.conf. Tailor it to your deployment.
#
rpcuser=<user for RPC>
rpcpassword=<pw for RPC>
apiuser=<user for API>
apipassword=<pw for API>
daemon=1
#mining=1
#stake=1
```

If you intend to access your wallet remotely (from a wallet interface on another machine), you’ll also need the following lines in the config file. (\<ipaddress> is your host with the interface IP from which you will remotely control you daemon)

```
apiremote=1
apiauth=1
rpcremote=1
llpallowip=<ipaddress>:8080  
llpallowip=<ipaddress>:9336
```

Press Ctrl s and Ctrl x to save and exit the editor

## **5. Bootstrapping Tritium Database:**

It will download the nexus database and then extract it to the data folder, alternatively you can skip this step if you choose to synchronise the data from peers which will be slow depending on your internet connection.

```
cd ~
```

This will download the database to your home folder. The file is just about 5 GB in size so have patience.

```
wget -c http://bootstrap.nexus.io/tritium.tar.gz
```

This will extract the database to the Nexus core data directory

```
tar -xf tritium.tar.gz -C ~/.Nexus
```

## **6. API's To Control Node:**

To interact with the nexus core daemon, use API commands via the terminal. Change your location to \~/LLL-TAO folder for the API’s to work. To run the node, stake and transact we will only be using the system, users and finance API’s. If you have any doubts you can refer to the API documentation [here](../tritium-api/).

If you are using the GUI wallet and want to just check out the API commands you can use them on the Nexus wallet Interface console. Just use the API commands minus the ./nexus. Just check the video below.

Use Nexus API commands on the interface console – (Nexus wallet matrix theme)

### **Before we start:**

* Be careful as you will be typing in the login credentials in the terminal and can be seen by anyone around you.
* Every transaction will need you to enter your pin
* Every transaction on the Nexus blockchain is a debit to the sending account and credit to the receiving account, two transactions. (This will be useful to understand some API commands)

Change into the LLL-TAO directory to start nexus core (You have to be in the LLL-TAO folder to run all the following commands)

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

The output of the API’s is in json format. You can confirm if your node is synchronized by checking the “synchronising”: false and “synccomplete”: 100. You can even check the block height on the explorer [here](http://explorer.nexus.io) or [here](https://nxsorbitalscan.com)

After the blockchain is fully synchronized, create a new user account (signature chain).

Username must be a minimum of 3 characters, passwords must be 8 characters and pin 4 characters. The PIN can be a combination of letters/numbers/symbols.

```
./nexus users/create/user username= password= pin=
```

To log into your account (If you just created your account wait for a few blocks to confirm new account)

```
./nexus users/login/user username= password= pin=
```

To unlock the account for staking and automatically credit incoming transactions set (notifications=1). If its not set you will have to manually credit the incoming transactions else it will be credited to the sender’s account after 24 hrs. This is the reversible transaction function working as designed.

```
./nexus users/unlock/user pin= staking=1 notifications=1
```

To check stake info (works only after login and unlocking for staking)

```
./nexus finance/get/stakeinfo
```

To get details of your accounts and address. (Trust and default accounts are automatically created with new account)

```
./nexus users/list/accounts
```

This will list all your account and its details

```
./nexus finance/list/accounts
```

This will list all transactions sent to a particular genesis or username. It is useful for identifying transactions that you need to accept such as credits.

```
./nexus users/list/notifications
```

If automatic credit (notifications=1) is not specified as an option with unlock command that will reflect as a pending transaction and will be listed in notifications.

This will credit a pending transaction, ‘txid’ is the debit transaction id from notifications

```
./nexus finance/credit/account pin= txid=
```

To send nexus coins, name is the account to be sent from and name\_to is the recipient account, these can be changed to address and address\_to if you are using the address.

```
./nexus finance/debit/account name=username:name/namespace:name amount= name_to=username:name/namespace:name pin=
```

To check the full Nexus blockchain metrics.

```
./nexus system/get/metrics

```

Hope this guide was helpful !!

## Update the node:

When there is a new version of the core you need to update your node.&#x20;

{% hint style="info" %}
Wallet mandatory or incremental upgrades can be understood by looking at version number of the new and old wallets. If the new core version first digit is incremented then it is a mandatory update which might be a hard fork or major protocol changes. If second or third digit is incremented then, it is an incremental upgrade. Incremental updates can be given a miss, but our recommendation is to upgrade.  &#x20;
{% endhint %}

To update the node you need to change into the LLL-TAO folder

```
cd LLL-TAO
```

Stop the core

```
./nexus system/stop
```

Change to the home directory

```
cd
```

It is better to get your node operating system updated. This command may take some time

```
sudo apt update; sudo apt upgrade -y
```

Reboot the node. This command will restart the node and also close SSH or putty connection

```
sudo reboot
```

Give two minutes for the node to restart and then log in via SSH or putty

```
ssh ubuntu@192.168.3.144
```

Rename the existing ‘LLL-TAO’ folder to ‘old-LLL’

```
mv LLL-TAO old-LLL
```

Next download the latest nexus wallet source code from github, The LLL-TAO master branch is linked to the merging branch.

For the core version 5.0.5 use the below link

```
git clone -b 5.0.5 https://github.com/Nexusoft/LLL-TAO
```

For the master branch which in turn will refer to the merging use the link below, this is the 5.1 core which is not compatible with the stable release

```
git clone --depth 1 https://github.com/Nexusoft/LLL-TAO
```

Change into the new source code directory

```
cd LLL-TAO
```

Run make to compile from source. The 4 in ‘j4’ refers to the no. of cores / threads available on the CPU of the node for compiling faster. (RPI-4B has 4 cores). More compiling threads consumes memory, if you have 1GB memory recommend you use j1 or you will get ‘out of memory’ error.

This begins compiling the wallet, please be patient, as this can take a very long time depending on your CPU.

For X86/IA64 based computers use the below command

```
make -f makefile.cli -j1 AMD64=1
```

For raspberry pi use the below command

```
make -f makefile.cli -j4 ARM64=1
```

Will show “Finished building nexus” on a successful compile.

To start the core

```
./nexus
```

To check the node info

```
./nexus system/get/info
```

Check the core version is showing the new version.

The nexus remote interface wallet also needs to be updated to the latest version, failing to update may cause issues with usability or functionality. Go to [https://nexus.io/wallets](https://nexus.io/wallets) to download the latest wallet

Hope this guide was helpful !!
