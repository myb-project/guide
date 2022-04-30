# Getting and Installing MyBee

## Installation via ISO on a dedicated server

You can get the installation ISO image from the official website [under Downloads](https://myb.convectix.com/download/). The installation uses the normal `bsdinstall` FreeBSD installer and the MyBee installation procedure is no different from the normal [FreeBSD installation](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-start) process.

:bangbang: | :warning: Note! If you DO NOT create an additional (non-privileged) user when installing MyBe, be sure to use strong passwords for the 'root' user, as this will allow 'root' login via SSH by default (you can block access to the administrative console).
:---: | :---


## Installation via FreeBSD rescue (e.g. Hetzner Rescue System)

:bangbang: | :warning: A word of warning: These instructions destroy the information on the disks of the computer where MyBee is installed.
:---: | :---

1) First, you must reboot your Hetzner server in Rescue mode using FreeBSD 13.x. Make sure you select the correct architecture and OS:

![mybee_hz1](https://user-images.githubusercontent.com/926409/163261607-a1d909fc-d909-4eaa-9273-83c70d9f3409.png)


2) After rebooting the server and accessing the shell as 'root', use the `fetch` command to get the installation script:

```
fetch https://myb.convectix.com/auto
```

The script is a `sh` script, so just run it with the '/bin/sh' shell:

```
sh auto
```

![mybee_hz2](https://user-images.githubusercontent.com/926409/163675520-f2784da1-e62c-42ba-91ac-927a0e6ef012.png)


3) If you are familiar with `bsdinstall`, you will be familiar with the following steps. All further steps are similar to a normal FreeBSD installation. The only exception is that at the end of the script, you will return to Hetzner's 'rescue' shell - just reboot the server with the `shutdown -r now` command yourself.

## MyB/FreeBSD installer

:information_source: If you are familiar with the FreeBSD installation process and the `bsdinstall` installer, you can [skip the rest of this chapter](shell.md).

Be sure to enter the fully qualified name (FQDN) of the server:

![mybee_hz3](https://user-images.githubusercontent.com/926409/163675559-4ceb5b37-b5cf-4421-9632-aee829c4a855.png)

MyBee uses the rich features and stability of ZFS, the ability to use UFS is disabled:

![mybee_hz4](https://user-images.githubusercontent.com/926409/163675561-135cc875-142e-4610-9c22-6506bb8325d9.png)

Choose your RAID type wisely, depending on how many disks you have and what your goals are.

![mybee_hz5](https://user-images.githubusercontent.com/926409/163675562-29b2cffc-d658-4db5-8ccb-3599dd4980e8.png)
![mybee_hz6](https://user-images.githubusercontent.com/926409/163675563-eb5b3bb4-0dde-403f-a97a-9efbe30504ac.png)

In our example, we only have the two disks that were used as stripes in the previous step, for maximum storage size with no redundancy in case either disk fails:

![mybee_hz7](https://user-images.githubusercontent.com/926409/163675564-2ebfd4d9-337a-4f54-8d6b-6fb1124e1890.png)

This is your last chance to back off!

![mybee_hz8](https://user-images.githubusercontent.com/926409/163675565-afd6a60c-9af2-43b2-8ebd-603f4a979975.png)

Choose a strong password for the 'root' user. If you don't create an additional user in the next step, MyBee will by default allow 'root' access remotely via SSH.

![mybee_hz9](https://user-images.githubusercontent.com/926409/163675566-fc65fee4-782c-46a4-a097-8ee1e0d5e18a.png)

Selecting and configuring a network interface.

![mybee_hz10](https://user-images.githubusercontent.com/926409/163675543-1ea23001-9a67-4fbc-a329-c48d13f5fead.png)

An IPv4 address is required:

![mybee_hz11](https://user-images.githubusercontent.com/926409/163675545-5ad1f06e-c2c2-43d7-ab18-2b8ecc072981.png)


![mybee_hz12](https://user-images.githubusercontent.com/926409/163675546-fd344806-6ddf-437e-9e9f-300994c6754f.png)
![mybee_hz13](https://user-images.githubusercontent.com/926409/163675547-8b6256b3-2e15-4a4e-9036-6aae1ed9253e.png)

Without configuring DNS servers, MyBee will not be able to get gold images from the repository. If you don't have preferred DNS servers, you e.g. can use the public '8.8.8.8' and '8.8.4.4' nameservers from Google:

![mybee_hz14](https://user-images.githubusercontent.com/926409/163675549-1417a25c-fff1-4189-b94c-743b97bc98fd.png)

Choose the most appropriate time zone for you:

![mybee_hz15](https://user-images.githubusercontent.com/926409/163675550-22527c00-ded5-4d9f-af68-816197602e0e.png)
![mybee_hz16](https://user-images.githubusercontent.com/926409/163675551-b7446919-20d7-4c96-86a1-b332d8b81ef8.png)

You can create an unprivileged user to access the server remotely:

![mybee_hz17](https://user-images.githubusercontent.com/926409/163675552-0bb4dd4d-6104-45f5-be4d-4ecaff00c41b.png)

:bangbang: | :warning: Be sure to add the user you are creating to the 'wheel' group, otherwise you will not be able to switch to 'root' via the `su` command!
:---: | :---

![mybee_hz18](https://user-images.githubusercontent.com/926409/163675553-98c8eee6-c966-489c-a9a3-5c30d4561478.png)

At this point, you've done everything required to install MyBee. To finish the installation exit `bsdinstall`:

![mybee_hz19](https://user-images.githubusercontent.com/926409/163675554-10af0f73-d95e-49d2-b041-0c61ef16c334.png)

Select 'no' in the final dialog window to exit `bsdinstall` and, in the case of Hetzner, return to the 'rescue' shell:

![mybee_hz20](https://user-images.githubusercontent.com/926409/163675558-72a96aca-b7cf-4c0a-97c7-23e719e09abd.png)

All you have to do is restart the server with the command `shutdown -r now` . If you're installing from the ISO, the installer gives you the option to reboot.

After the system reboots, you can use the server IP to access the machine via SSH or contact it via a web browser to get introductory information: http://IP-address-of-your-server .


---

Next: [CLI/shell](shell.md)
