# CLI/shell

## Description

You can configure MyBee and perform various administrative operations through the admin console by logging into the MyBee server via SSH.

If you created an unprivileged user during installation, then use that account to access the server remotely. Once logged in, use `su -` on the command line and supply the password you chose for 'root' to change to the super user and the MyBee console.

If you didn't create an additional user, logging in with user 'root' over SSH will be allowed.

## MyBee Menu

When logged in as the 'root' user, a menu for typical operations will be available to you, containing things such as changing network settings, switching profiles, ACLs, etc.:

![image](https://user-images.githubusercontent.com/926409/163887427-850cc699-273d-4d28-b765-c7e6ba59cbde.png)

To select any menu entry, enter the appropriate number and press 'Enter'.

Depending on the type of installation, the presence of plugins or settings, some menu items may be inactive (grayed out).

---

Next: [Configuring Network Interfaces](network.md)
