# Private Node



This guide will help to set up a private testnet. The Nexus wallet is used to run public, private and hybrid networks, the configuration is what sets them apart. The private and hybrid networks will not be compatible with legacy.

In a private network, throughput can be increased by adding additional nodes and that can be done at a later stage. In a private network there is no mining or staking needed to secure the network.

This guide is based on ubuntu server 20.04 LTS, use any distribution of your choice. I am using the [raspbian 64 bit](https://downloads.raspberrypi.org/raspios\_lite\_arm64/images/) on a Raspberry pi 4.

**Before you begin this guide you will need a few things:**

* [Ubuntu server 20.04 LTS](https://ubuntu.com/download/server) for AMD/IA64 or [Ubuntu IOT](https://ubuntu.com/download/iot) for Raspberry Pi. (Use any linux distribution of choice, but this guide is tailored for ubuntu)
* USB drive or SD card to install ubuntu
* [Etcher](https://www.balena.io/etcher/) – To burn the OS image file to USB/SD card
* Putty if you are using ssh via windows.

Note: Ubuntu IOT default login credentials:

`username=ubuntu`\
`password=ubuntu`

### Prepare The Node

Install ubuntu server 20.04 LTS or distro of your choice, install open-ssh server during the install and once the installation is complete, restart the node. SSH in your node and follow the below commands. You can copy the commands and paste it in the terminal using keys CTRL+SHIFT+v

Update and upgrade your node:

```
sudo apt update; sudo apt upgrade -y
```

Open SSH port before you enable firewall:

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

If you need to change the hostname – Not compulsory if you already set it during the install

```
sudo hostnamectl set-hostname
```

Reboot node:

```
sudo reboot
```

Now the computer is ready to install the nexus core

### Compile The Core

Install the dependencies required for compiling nexus core, It will take some time to complete depending on your internet speed

```
sudo apt-get install -y build-essential libssl-dev libminiupnpc-dev git
```

Download the latest nexus core 5.1 source code, should only take a few seconds to complete (This link may change as development progresses)

```
git clone --branch merging https://github.com/Nexusoft/LLL-TAO
```

Change into the source code directory

```
cd LLL-TAO
```

Run the command to compile from source, please be patient, as this can take a very long time depending on your CPU. Replace the 1 in ‘j1’ to the number of cores / threads for compiling faster.

```
make -f makefile.cli clean
```

```
make -f makefile.cli -j1 AMD64=1 NO_WALLET=1
```

For compiling on Raspberry Pi use

```
make -f makefile.cli -j1 ARM64=1 NO_WALLET=1
```

Will show the message “Finished building nexus” on a successful compile.

The make command creates a new executable file named 'nexus'. To check use the command

```
ls
```

![](https://nexus.io/ResourceHub/images/5.1\_testnet/testnet1.png)

There are two ways to access the wallet; from the LLL-TAO folder, API's can be accessed from this location only via terminal and for every command you have to specify the path (./) before the executable filename (./nexus) or if the executable file is moved to the /usr/bin directory, it can be accessed universally from any location without path (nexus). For this guide will not use the path.

To move the nexus executable to the /usr/bin folder:

`sudo mv ˜ /LLL-TAO/nexus /usr/bin`

`Configure the Core`

Create Nexus core directory (it’s a hidden directory, Nexus daemon creates it automatically on first start. We are creating it manually to create the configuration file. If you have the directory you can skip this step.)

`mkdir ~/.Nexus`

Create the nexus.conf file

`nano ~/.Nexus/nexus.conf`

Copy the code below, to the nexus.conf file with ctrl+shift+v and edit or disable the parameters as per your needs

```
#Nexus private standalone node config- Only for 5.1 rc1 & above
#
#Default API user/pass to blank for private network
#apiuser=<username>
#apipassword=<password>
#Disable authentication of API requests
apiauth=0
#To remotely access the node API's
apiremote=1
#Change API port to 7080 (API port in private mode defaults to 7080, RPC defaults to 8336)
#apiport=7080
#To remotely access API's use the llpallowip flag.
#The <ipaddress> can use wildcards; (llpallowip=192.168.*.*:7080)
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
**Note:** To add an additional node to the private network, disable _‘manager’_ and _‘generate’_ flags. Add the _‘addnode’_ flag with ipaddress referring to the first node or the one with the _‘generate’_ flag and an additional line for any other node in the network
{% endhint %}

\
