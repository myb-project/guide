# CLI/shell

## Description

You can configure MyBee and perform various administrative operations through the admin console by logging into the MyBee server via SSH.

If you created an unprivileged user during installation, then use that account to access the server remotely. Once logged in, use `su -` on the command line and supply the password you chose for 'root' to change to the super user and the MyBee console.

If you didn't create an additional user, logging in with user 'root' over SSH will be allowed.

## MyBee Menu

When logged in as the 'root' user, a menu for typical operations will be available to you, containing things such as changing network settings, switching profiles, ACLs, etc.:

![mybcli](/images/mybconsole.png)

To select any menu entry, enter the appropriate number and press 'Enter'.

Depending on the type of installation, the presence of plugins or settings, some menu items may be inactive (grayed out).

## Update MyB

If you want to update MyBee (including VM profiles/templates), type 'u' at the prompt and press Enter.
The process of updating the entire system will begin. Some updates may require you to restart the node for the changes to take effect.

![mybcli](/images/mybupgrade.png)

:bangbang: | :information_source: Please note that updating MyBee does not update the guest systems of already created virtual machines. However, with a MyBee update, you can get a new profile of a previously missing OS (or a new version of an existing profile), which will cause a new gold image to be downloaded for this guest when the profile is activated.
:---: | :---

---

Next: [Configuring Network Interfaces](network.md)
