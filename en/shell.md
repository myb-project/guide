# CLI/shell

## Description

You can configure MyB or perform various administrative operations through the admin console by logging into the MyB server via SSH.

If you created an unprivileged user during installation, then use this account to access the server remotely. From the command line, use `su -` and the user password 'root' to change to the super user and MyB console.

```
su -
```

If you didn't create an additional user, 'root' login via SSH will be allowed.

## MyB Menu

When logged in as a 'root' user, a menu for typical operations will be available to you, such as changing network settings, switching profiles, ACLs, etc.:

![image](https://user-images.githubusercontent.com/926409/163887427-850cc699-273d-4d28-b765-c7e6ba59cbde.png)

Menu selection - enter the appropriate number and 'Enter'.

Depending on the type of installation, the presence of plugins or settings, some menu items may be inactive (grayed out).

---

Next: [Configuring Network Interfaces](network.md)
