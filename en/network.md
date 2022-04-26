# Configuring Network Interfaces

# Description

If you want to reconfigure MyB's network settings, you can use the bsdconfig TUI configurator by selecting **'1) (Re)Configure Host Network'** in the admin console.

![image](https://user-images.githubusercontent.com/926409/163883957-58a6c6fd-c44c-4c06-b2de-477e45936d39.png)

Alternatively, by entering a normal FreeBSD command line via **'6) Shell'**, you can edit the **/etc/rc.conf** file according to the [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/config/#config-network-setup).

## bsdconfig netconfig

For example, to configure static IPv4 94.130.23.176/26 and default gateway 94.130.23.129 for NIC named 'em0', the dialog would be:

![image](https://user-images.githubusercontent.com/926409/163884405-c657c742-446c-40e2-b0b6-850a8727224b.png)

![image](https://user-images.githubusercontent.com/926409/163884441-9416766d-0c8c-44f7-afbc-a8e08f02c580.png)

![image](https://user-images.githubusercontent.com/926409/163884499-9fa1b8a9-c55c-403c-9203-aaeaa7b21a41.png)

![image](https://user-images.githubusercontent.com/926409/163884593-ab7db705-4086-4667-9f6e-80b4dcf8ea45.png)


To configure static IPv6 2a01:4f8:10b:1cd2::1/64 and default gateway fe80::1 (according to [Hetzner documentation](https://docs.hetzner.com/robot/dedicated-server/network/net-config-debian-ubuntu/) where configuration is done) for a network card named 'em0':

![image](https://user-images.githubusercontent.com/926409/163884951-31c738a2-e50c-4189-a173-4baa9bd0baa9.png)

![image](https://user-images.githubusercontent.com/926409/163884990-1f39a466-7b05-43e7-9bed-756015bd1040.png)

![image](https://user-images.githubusercontent.com/926409/163885066-d76669de-bc9a-423e-b067-18cf7e3e8224.png)

:bangbang: | :information_source: Note that we use the option to fix the interface after the **'%'** character when specifying the default gateway for the IPv6 network: **fe80::1%em0** , where **em0** is our uplink interface.
:---: | :---

Don't forget to write correct resolver addresses. For example, public DNS servers from Google:

| proto |         IP           |
|-------|----------------------|
| IPv4: |       8.8.8.8        |
| IPv4: |       8.8.4.4        |
| IPv6: | 2001:4860:4860::8888 |
| IPv6: | 2001:4860:4860::8844 |


![image](https://user-images.githubusercontent.com/926409/163885586-cdad3f13-c518-4cac-8688-953a6cb588ed.png)

## FreeBSD /etc/rc.conf

It is much easier for most people to configure the network in FreeBSD through the regular /etc/rc.conf, to do this, go to the shell and open /etc/rc.conf in any editor (mcedit, vi), correct the lines:

```
ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
defaultrouter="94.130.23.129"
ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
ipv6_defaultrouter="fe80::1%em0"
```

Another way to conveniently edit /etc/rc.conf parameters is to use the [sysrc(8)](man.freebsd.org/sysrc/8) script, similar to the entries above, via the command line:

```
sysrc ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
sysrc defaultrouter="94.130.23.129"
sysrc ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
sysrc ipv6_defaultrouter="fe80::1%em0"
```

To apply the settings in effect, reload MyB:

> shutdown -r now

Or via **'7) Reboot Server'**


---

Next: [Network profiles](netprofile.md)
