# Connect Node to Interface

This guide will help to access your wallet daemon using a remote wallet interface. For this you will need the IP address of your node and also the interface computer. For the remote computer you can go to settings, network and click on details which will give your computer IP address.

On the computer which will run the interface go to [https://](https://crypto.nexus.io/wallet)[nexus.io/wallets](https://nexus.io/wallets) and download the wallet of your choice.

{% hint style="danger" %}
Always verify the wallet integrity. Follow the instructions in the link below&#x20;
{% endhint %}

{% embed url="https://nexus.io/ResourceHub/wallet-guide#download-verify-install" %}

After the download is complete to verify the installer integrity go to [https://nexus.io/ResourceHub/wallet-guide#download-verify-install](https://nexus.io/ResourceHub/wallet-guide#download-verify-install) and follow the guide

Run the wallet and after the wallet loads immediately go to _‘Settings’_, click on ‘_Core_‘, scroll down to the bottom and enable manual core mode using the slider button

![](https://thedigitalfuture.net/wp-content/uploads/2020/12/RPI-Interface1.png)

SSH into the node.

```
ssh ubuntu@192.168.3.144
```

Open the nexus config file.

```
nano ~/.Nexus/nexus.conf
```

With the nexus.conf and the interface side by side.

Use a unique apiusername and  apipassword and the same will be used in the username and password field in the interface tritium core settings, IP address replace the default 127.0.0.1 with the daemon node IP address. Leave the default port and click save settings.

On the interface go to ‘_File_‘ menu and click on ‘_Switch to Legacy Mode_‘ and now you will be in the legacy mode, manual core settings.

![](https://thedigitalfuture.net/wp-content/uploads/2020/12/RPI-Interface2.png)

Use a unique rpcusername and rpcpassword and the same will be used in the username and password field in the interface legacy core settings, IP address replace the default 127.0.0.1 with the daemon node IP address. Leave the default port and click save settings.

In the config file, the llpallowIp IP address should refer to the interface computer IP.

Use Ctrl s and Ctrl x to save and exit the config file.

Change directory to the Nexus executable file.

```
cd LLL-TAO
```

Start the Nexus wallet daemon.

```
./nexus
```

You should see your wallet interface connected to the daemon and the overview showing the node information. If the interface is not connected you need to stop the wallet and open port 8080 and 9336 on the firewall.

On the node, stop the wallet daemon.

```
./nexus system/stop
```

On the ufw firewall allow port 8080 for API

```
sudo ufw allow 8080/tcp
```

Allow port 9336 for RPC

```
sudo ufw allow 9336/tcp
```

Check the ufw firewall status

```
sudo ufw status
```

ufw status – You should see a output something like in the image above.

![](https://thedigitalfuture.net/wp-content/uploads/2020/12/RPI-ufw.png)

Next start the daemon

```
./nexus
```

Wait for a few seconds and

![](https://thedigitalfuture.net/wp-content/uploads/2020/12/RPI-Sync.png)

You should see the interface connected to the node and the information appear on the overview page.

Now you can login and transact normally using the interface.

To exit the SSH or putty terminal use the command. This will close only the SSH or putty terminal and the node will be running which can be confirmed with the interface.

```
exit
```
