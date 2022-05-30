---
description: Procedure to install Let's Encrypt single certificate
---

# Let's Encrypt (Default)

{% hint style="info" %}
This guide is tailored for Ubuntu and if you need the same for other OS's use the below link:
{% endhint %}

{% embed url="https://certbot.eff.org/" %}

1.  **SSH into the server**

    SSH into the server running your HTTP website as a user with sudo privileges.
2.  **Install snapd**

    You'll need to install snapd and make sure you follow any instructions to enable classic snap support.

    Follow these instructions on [snapcraft's site to install snapd](https://snapcraft.io/docs/installing-snapd/).

    [install snapd](https://snapcraft.io/docs/installing-snapd/)
3.  **Ensure that your version of snapd is up to date**

    Execute the following instructions on the command line on the machine to ensure that you have the latest version of `snapd`.

    ```
    sudo snap install core; sudo snap refresh core
    ```
4.  **Remove certbot-auto and any Certbot OS packages**

    If you have any Certbot packages installed using an OS package manager like `apt`, `dnf`, or `yum`, you should remove them before installing the Certbot snap to ensure that when you run the command `certbot` the snap is used rather than the installation from your OS package manager. The exact command to do this depends on your OS, but common examples are `sudo apt-get remove certbot`, `sudo dnf remove certbot`, or `sudo yum remove certbot`. If you previously used Certbot through the certbot-auto script, you should also remove its installation by following the instructions [here](https://certbot.eff.org/docs/uninstall.html).
5.  **Install Certbot**

    Run this command on the command line on the machine to install Certbot.

    ```
    sudo snap install --classic certbot
    ```
6.  Prepare the Certbot command

    Execute the following instruction on the command line on the machine to ensure that the `certbot` command can be run.

    ```
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    ```
7.  Choose how you'd like to run Certbot

    Are you ok with temporarily stopping your website?

    #### Yes, my web server is not currently running on this machine.

    Stop your webserver, then run this command to get a certificate. Certbot will temporarily spin up a webserver on your machine.

    ```
    sudo certbot certonly --standalone
    ```

    #### No, I need to keep my web server running.

    If you have a webserver that's already using port 80 and don't want to stop it while Certbot runs, run this command and follow the instructions in the terminal.

    ```
    sudo certbot certonly --webroot
    ```
8. #### Important Note:
9. To use the webroot plugin, your server must be configured to serve files from hidden directories. If `/.well-known` is treated specially by your webserver configuration, you might need to modify the configuration to ensure that files inside `/.well-known/acme-challenge` are served by the webserver.
10. Install your certificate

    You'll need to install your new certificate in the configuration file for your webserver.
11. Test automatic renewal

    The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command:

    ```
    sudo certbot renew --dry-run
    ```

    The command to renew certbot is installed in one of the following locations:

    * `/etc/crontab/`
    * `/etc/cron.*/*`
    * `systemctl list-timers`
12. Confirm that Certbot worked

    To confirm that your site is set up properly, visit `https://yourwebsite.com/` in your browser and look for the lock icon in the URL bar.

If running the node as an `user` you need to give proper permission to the lets encrypt folder so the&#x20;

```javascript
// Create group with root and nodeuser as members
$ sudo addgroup nodecert
$ sudo adduser <user> nodecert
$ sudo adduser root nodecert

// Make the relevant letsencrypt folders owned by said group.
$ sudo chgrp -R nodecert /etc/letsencrypt/live
$ sudo chgrp -R nodecert /etc/letsencrypt/archive

// Allow group to open relevant folders
$ sudo chmod -R 750 /etc/letsencrypt/live
$ sudo chmod -R 750 /etc/letsencrypt/archive
```

\
