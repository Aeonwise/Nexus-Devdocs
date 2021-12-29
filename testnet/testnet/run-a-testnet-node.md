---
description: Testnet Node Setup
---

# Run a Testnet Node

This guide will help to setup a node on the nexus testnet. No permission is required to join the testnet and users have complete control of the dedicated node

The node configuration will differ as per the individual application requirement or amount of transactions generated.

{% hint style="info" %}
If you need any help on the node specifications for a particular application do contact developer support
{% endhint %}

## 1. Before you start:

* A computer with a minimum of 1 CPU, 1GB RAM, 20GB hard space, Raspberry Pi 4 with 2 GB RAM.
* [Ubuntu server 20.04 LTS](https://ubuntu.com/download/server#downloads) for AMD/IA64 or [Ubuntu IOT](https://ubuntu.com/download/raspberry-pi/thank-you?version=20.04.3\&architecture=server-arm64+raspi) for Raspberry Pi. (Use any linux distribution of choice, but this guide is tailored for ubuntu)
* USB drive or SD card to install ubuntu
* Etcher – To burn the OS image file to USB/SD card
* Putty if you are using ssh via windows

## 2. Testnet Mining Node Links:

The testnet mining nodes are listed below.

* http://testnet1.nexus-interactions.io
* http://testnet2.nexus-interactions.io
* http://testnet3.nexus-interactions.io

## 3. Testnet Ports:

The testnet uses a different set of ports compared to mainnet. If using firewall make sure to allow the following ports. Legacy is disabled and the reason RPC port 8336 is not allowed.

* 7080 -API port
* 8325 - Mining port, only enable if mining on testnet
* 8888 - Outgoing node connections

## **4. Prepare The Node:**

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

Allow API port 7080

```
sudo ufw allow 7080/tcp
```

Allow outgoing connections port 8888

```
sudo ufw allow 8888/tcp
```

Allow mining port 8325, only if mining

```
sudo ufw allow 8325/tcp
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

## **5. Compiling Nexus Core:**

Installs the dependencies required for compiling nexus core CLI, It will take some time to complete depending on your internet speed

```
sudo apt-get install -y build-essential libssl-dev libdb-dev libdb++-dev libminiupnpc-dev git
```

Download the latest nexus core source code, and should only take a few seconds to complete,

```
git clone --branch merging https://github.com/Nexusoft/LLL-TAO
```

Change into the source code directory

```
cd LLL-TAO
```

Lastly run this command to compile from source. This begins compiling the nexus core, please be patient, as this can take a very long time depending on your CPU. Replace the 1 in ‘j1’to the number of cores / threads for compiling faster.

{% hint style="info" %}
To build on Raspberry Pi with 1 GB of ram you have to enable Swap memory.

Proceed after setting up swap.
{% endhint %}

{% embed url="https://rayanfer32.medium.com/enable-swap-memory-on-ubuntu-on-raspberry-pi-a0f873a65e74" %}

```
make -f makefile.cli clean
```

For x86/IA64 computers use:

```
make -f makefile.cli -j1 AMD64=1 NO_WALLET=1
```

For the raspberry pi use:

```
make -f makefile.cli -j4 ARM64=1 NO_WALLET=1
```

Will show “Finished building nexus” on a successful compile.

## **6.** Testnet Node Configuration:

The nexus wallet configuration is kept in a nexus.conf file in `~/.Nexus` which is the nexus core directory

Create Nexus core directory (it’s a hidden directory, if you have the directory you can skip this step.)

```
mkdir ~/.Nexus
```

Create the nexus.conf file

```
nano ~/.Nexus/nexus.conf
```

Copy the below configuration in the nexus.conf file and change the values denoted by "< >" to suit your setup

```
#Nexus testnet config- PLEASE CHANGE TO SUITABLE VALUES
#Set API credentials
apiuser=<username>
apipassword=<password>
#To enable API authentication for testing
apiauth=1
#To enable API remote access
apiremote=1
#To whitelist IP address for API access
llpallowip=<ipaddress>:7080
#To enable multiuser mode: enables more than one user to be logged into the node
multiuser=1
#To enable debug mode
#debug=1
#To enable the daemon mode
daemon=1
#To accept incoming JSON-RPC commands
server=1
#Enable mining
#mining=1
#Enable Staking
#stake=1
#The testnet flag defaults the API port to 7080, RPC to 8336 & mining port to 8325
testnet=1
#To connect to the Nexus public testnet. 
connect=testnet1.nexus-interactions.io
connect=testnet2.nexus-interactions.io
connect=testnet3.nexus-interactions.io
#To disable mainet DNS nodes
nodns=1
#process notifications (incoming transactions) automatically in background
processnotifications=1
#To add a stop password to protect from accidental wallet exit
system/stop=<password>
```

ctrl+s & ctrl+x to save and exit the nano text editor

The wallet configuration is now complete.

## 7. API's to Control the Node

To start, stop and check the node info you have to use API's from the terminal

Change into the LLL-TAO directory to start nexus core (You have to be in the LLL-TAO folder to run all the following commands)

```
cd LLL-TAO
```

### To start the daemon:

```
./nexus
```

Nexus core will be running in the background as a daemon. It will connect to the mining nodes specified in the configuration and synchronize the blockchain. It will take a few minutes to find peers and synchronize the chain.

### To stop the daemon:

```
./nexus system/stop
```

```
./nexus system/stop password=<password>
```

### To get the node info:

```
./nexus system/get/info
```

Now the testnet node is ready to be used for your application.

Hope this guide was helpful !!
