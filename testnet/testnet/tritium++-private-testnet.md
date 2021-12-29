# Tritium++ Private Testnet

{% hint style="info" %}
This guide is to run a single or island node for testing. It does not require mining or staking to produce blocks
{% endhint %}

## Introduction:

This guide will help to setup a private testnet for development. The private node has no consensus, and is a private database. This node can be used to test API calls, without spending on mining or the use of coins

## Understanding Public, Private & Hybrid

The Nexus wallet can be used to run public, private and hybrid networks, the configuration is what sets them apart. The private and hybrid networks will not be compatible with legacy.

Public network is the mainnet which is an open public network. Private mode is a permissioned network, it is not connected to the mainnet and is a standalone network. Hybrid network is the amalgamation of the private and the public network. Hybrid mode helps organisations to keep their sensitive data safe, private and in their control, but use the security of the public network and also transfer value between the two.

In a private network, throughput can be increased by adding additional nodes. In a private network there is no mining or staking needed to secure the network.

## Before Beginning this guide:

* Any computer with minimum of 1 CPU, 2GB RAM and 20GB hard disk space, Raspberry Pi 4 with 2 GB RAM
* Ubuntu server 20.04 LTS for AMD/IA64 or Ubuntu IOT for Raspberry Pi. (Use any linux distribution of choice, but this guide is tailored for ubuntu)
* USB drive or SD card to install ubuntu
* Etcher – To burn the OS image file to USB/SD card
* Putty if using ssh via windows.

## 1. Prepare the Node

[<mark style="background-color:blue;">Install ubuntu server 20.04 LTS</mark>](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview) or distro of choice, install open-ssh server during the install and once the installation is complete, restart the node. SSH into the node and follow the below commands. Copy the commands and paste it in the terminal using keys CTRL+SHIFT+v

Update and upgrade the node:

```
sudo apt update; sudo apt upgrade -y
```

Open SSH port before enabling firewall:

```
sudo ufw allow ssh
```

Enable firewall:

```
sudo ufw enable
```

Check firewall status:

```
sudo ufw status
```

Set timezone:

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

The computer is ready to install the nexus core

## 2. Compiling Nexus core:

Install the dependencies required for compiling nexus core, It will take some time to complete depending on the internet speed

```
sudo apt-get install -y build-essential libssl-dev libminiupnpc-dev git
```

Download the latest nexus core 5.1 source code, should only take a few seconds to complete (This link may change as development progresses)

```
git clone --branch merging https://github.com/Nexusoft/LLL-TAO
```

{% hint style="info" %}
`Tritium++ is in merging branch at the time of writing this guide`
{% endhint %}

If Tritium++ moved to merging use the following command

```
git clone --depth 1 https://github.com/Nexusoft/LLL-TAO
```

Change into the source code directory

```
cd LLL-TAO
```

Run the command to compile from source, please be patient, as this can take a very long time depending on your CPU. Replace the 1 in ‘j1’ to the number of cores / threads for compiling faster.

```
make -f makefile.cli clean
```

For x86/IA64 computers use:

```
make -f makefile.cli -j1 AMD64=1 NO_WALLET=1
```

For compiling on Raspberry Pi use:

```
make -f makefile.cli -j1 ARM64=1 NO_WALLET=1
```

Will show “Finished building nexus” on a successful compile.

The make command creates a new executable file named 'nexus'. To check use the list command

```
ls
```

!["ls" command lists the contents of the folder. The "nexus" executable is shown](https://nexus.io/ResourceHub/images/5.1\_testnet/testnet1.png)

There are two ways to access the wallet; from the LLL-TAO folder, API's can be accessed from this location only via terminal and for every command you have to specify the path (./) before the executable filename (./nexus) or if the executable file is moved to the /usr/bin directory, it can be accessed universally from any location without path (nexus). For this guide will not use the path.

To move the nexus executable to the /usr/bin folder:

```
sudo mv ~/LLL-TAO/nexus /usr/bin
```

## 3. Configuring The Wallet (nexus.conf)

Create Nexus core directory (it’s a hidden directory, Nexus daemon creates it automatically on first start. We are creating it manually to create the configuration file. If the directory is available, skip this step.)

```
mkdir ~/.Nexus
```

Th wallet configuration is stored in nexus.conf. Create the nexus.conf file

```
nano ~/.Nexus/nexus.conf 
```

Copy the configuration below, to the nexus.conf file with ctrl+shift+v and edit or disable the parameters as needed.

```
#Nexus private standalone node config- Only for 5.1 rc1 & above
#Default API user/pass to blank for private network 
#apiuser=<username>
#apipassword=<password>
#Disable authentication of API requests
apiauth=0
#To remotely access the node API's
apiremote=1
#To remotely access API's use the llpallowip flag. The <ipaddress> can use wildcards; (llpallowip=192.168.*.*:7080)
llpallowip=<ipaddress>:7080
#Run wallet as a daemon
daemon=1
#Run node in private mode (defaults to testnet in 5.1)
private=1
#Run as a standalone node. (Disable this on additional nodes) 
manager=0
#Run as a local node, disable for public node
nodns=1
#Enables multiple users to be logged in concurrently    
multiuser=1
#Latency is in ms, this is the min time between blocks
latency=500
#Enables creation of blocks in private mode.( Use only on one node) (Use a secure password)
generate=<password>
#To connect additional nodes use addnode flag, the ipaddress will be of the first node with the ‘generate’ flag)
addnode=<ipaddress>
#To avoid accidental node shutdown with the stop command
system/stop=<password>
```

Ctrl+s and Ctrl+x to save and exit the editor

{% hint style="info" %}
To add an additional node to the private network, disable ‘manager’ and ‘generate’ flags. Add the ‘addnode’ flag with ipaddress referring to the first node or the one with the ‘generate’ flag and an additional line for any other node in the network
{% endhint %}

## 4. API Commands

To interact with the nexus core, use API commands via the terminal or remotely. If you have any doubts you can refer to the API documentation here. The documentation for 5.1.rc1 may take some time to be updated at the time of writing the guide.

Open port 7080 and 8336 on the firewall.

```
sudo ufw allow 7080/tcp
```

```
sudo ufw allow 8336/tcp
```

If the executable is in the LLL-TAO directory to start nexus core, change into the LLL-TAO folder to run all the following commands "cd LLL-TAO To start the daemon, use the path and the executable file name ./nexus If you have moved the nexus executable file to the /user/bin then use the following command from any location nexus

To stop the daemon without password protection in config:

```
nexus system/stop
```

To stop the daemon with password protection in config:

```
nexus system/stop password=<password>
```

To get the node info:

```
./nexus system/get/info
```

To monitor your logs:

```
// tail -f ~/.Nexus/testnet1/log/0.log
```

To create a user account (signature chain). Username must be a minimum of 2 characters, passwords must be 8 characters and pin 4 characters. The PIN can be a combination of letters/numbers/symbols.

```
nexus users/create/user username=<username> password=<password> pin=<pin> 
```

{% hint style="info" %}
Multiuser mode creates a new session for every user logged in and the user has to use that particular sessionID with every API request for the particular user. Save the session ID.
{% endhint %}

To login the user

```
nexus users/login/user username=<username> password=<password> pin=<pin>
```

To unlock the account for automatically credit incoming transactions set (notifications=1). If it's not set you will have to manually credit the incoming transactions else it will be credited back to the sender's account after 24 hrs. This is the reversible transaction function working as designed.

```
nexus users/unlock/user pin=<pin> notifications=1 session=<sessionid> 
```

To check the full node metrics.

```
./nexus system/get/metrics 
```

The API commands can be used from the browser and output in JSON is displayed. (JSON formatter extension used to parse the JSON output)

Hope this guide was helpful !!
