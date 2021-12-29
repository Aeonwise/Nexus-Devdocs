---
description: How to update the mainnet node
---

# Update Mainnet Node

When there is a new version of the core, it needs to be updated.

{% hint style="info" %}
This guide is tailored for ubuntu/raspian distributions
{% endhint %}

{% hint style="info" %}
Wallet mandatory or incremental upgrades can be understood by looking at version number of the new and old wallets. If the new core version first digit is incremented then it is a mandatory update which might be a hard fork or major protocol changes. If second or third digit is incremented then, it is an incremental upgrade. Incremental updates can be given a miss, but our recommendation is to upgrade.
{% endhint %}

To update the node change into the LLL-TAO folder

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

It is recommended to get the node operating system updated. This command may take some time

```
sudo apt update; sudo apt upgrade -y
```

Reboot the node. This command will restart the node and also close SSH or putty connection

```
sudo reboot
```

Give two minutes for the node to restart and then log in via SSH or putty

```
ssh ubuntu@<IPaddress>
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

Change into the source code directory

```
cd LLL-TAO
```

Run `make` to compile from source. The 4 in ‘j4’ refers to the no. of cores / threads available on the CPU (RPI-4B has 4 cores). More compiling threads consumes memory, if you have 1GB memory recommend to use j1 to avoid ‘out of memory’ error.

{% hint style="warning" %}
To build on Raspberry Pi 3 or 4 with 1 GB RAM, enable swap memory with instructions from link below. Proceed after setting up swap.
{% endhint %}

{% embed url="https://rayanfer32.medium.com/enable-swap-memory-on-ubuntu-on-raspberry-pi-a0f873a65e74" %}

This begins compiling the wallet, please be patient, as this step can take a very long time depending on the CPU.

For X86/IA64 based computers use the below command

```
make -f makefile.cli -j1 AMD64=1
```

For raspberry pi use the below command

```
make -f makefile.cli -j1 ARM64=1
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

{% hint style="warning" %}
if using the nexus interface to remotely access the node, update to the latest version, failing to update may cause issues with usability or functionality. Go to [https://nexus.io/wallets](https://nexus.io/wallets) to download the latest interface
{% endhint %}

Hope this guide was helpful !!
