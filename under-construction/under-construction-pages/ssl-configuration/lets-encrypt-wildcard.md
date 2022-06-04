# Let's Encrypt (Wildcard)



1.  Check if your DNS provider is supported

    See if your DNS provider is supported by Certbot by checking this list in our [documentation](https://certbot.eff.org/docs/using.html#dns-plugins).

    [check if your DNS provider supports Certbot](https://certbot.eff.org/docs/using.html#dns-plugins)

    #### Not supported?

    If your DNS provider is not supported, pause here: run Certbot with the manual plugin by using [these steps from our documentation](https://certbot.eff.org/docs/using.html#manual).

    #### Supported?

    If your DNS provider is supported, continue with the remaining instructions below.
2.  SSH into the server

    SSH into the server running your HTTP website as a user with sudo privileges.
3.  Install snapd

    You'll need to install snapd and make sure you follow any instructions to enable classic snap support.

    Follow these instructions on [snapcraft's site to install snapd](https://snapcraft.io/docs/installing-snapd/).

    [install snapd](https://snapcraft.io/docs/installing-snapd/)
4.  Ensure that your version of snapd is up to date

    Execute the following instructions on the command line on the machine to ensure that you have the latest version of `snapd`.

    ```
    sudo snap install core; sudo snap refresh core
    ```
5.  Remove certbot-auto and any Certbot OS packages

    If you have any Certbot packages installed using an OS package manager like `apt`, `dnf`, or `yum`, you should remove them before installing the Certbot snap to ensure that when you run the command `certbot` the snap is used rather than the installation from your OS package manager. The exact command to do this depends on your OS, but common examples are `sudo apt-get remove certbot`, `sudo dnf remove certbot`, or `sudo yum remove certbot`. If you previously used Certbot through the certbot-auto script, you should also remove its installation by following the instructions [here](https://certbot.eff.org/docs/uninstall.html).
6.  Install Certbot

    Run this command on the command line on the machine to install Certbot.

    ```
    sudo snap install --classic certbot
    ```
7.  Prepare the Certbot command

    Execute the following instruction on the command line on the machine to ensure that the `certbot` command can be run.

    ```
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    ```
8.  Confirm plugin containment level

    Run this command on the command line on the machine to acknowledge that the installed plugin will have the same `classic` containment as the Certbot snap.

    ```
    sudo snap set certbot trust-plugin-with-root=ok
    ```

    If you encounter issues with running Certbot, you may need to follow this step, then the "Install correct DNS plugin" step, again.
9.  Install correct DNS plugin

    Run the following command, replacing \<PLUGIN> with the name of your DNS provider.

    ```
    sudo snap install certbot-dns-<PLUGIN>
    ```

    For example, if your DNS provider is Cloudflare, you'd run the following command:

    ```
    sudo snap install certbot-dns-cloudflare
    ```
10. Set up credentials

    You'll need to set up DNS credentials.

    Follow the steps in the "Credentials" section for your DNS provider to access or create the appropriate credential configuration file. Find credentials instructions for your DNS provider by clicking the [DNS plugin's name on the Documentation list](https://certbot.eff.org/docs/using.html#dns-plugins).

    [find your DNS plugin's Credential steps](https://certbot.eff.org/docs/using.html#dns-plugins)
11. Get a certificate

    Run one of the commands in the "Examples" section of the [instructions for your DNS provider](https://certbot.eff.org/docs/using.html#dns-plugins).
12. Install your certificate

    You'll need to install your new certificate in the configuration file for your webserver.
13. Test automatic renewal

    The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command:

    ```
    sudo certbot renew --dry-run
    ```

    The command to renew certbot is installed in one of the following locations:

    * `/etc/crontab/`
    * `/etc/cron.*/*`
    * `systemctl list-timers`
14. Confirm that Certbot worked

    To confirm that your site is set up properly, visit `https://yourwebsite.com/` in your browser and look for the lock icon in the URL bar.
