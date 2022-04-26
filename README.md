# MyBee - The most simplified API for creating and destroying K8S & cloud VM

<p align="center">
  <span>English</span> |
  <a href="/README.md">English</a> |
  <a href="/README.es.md">Español</a> |
  <a href="/README.fr.md">Français</a> |
  <a href="/README.de.md">Deutsch</a> |
  <a href="/README.ru.md">Русский</a> |
  <a href="/README.ch.md">中文</a> |
</p>

---

:information_source: This guide covers the administration and use of the [MyBee](https://myb.convectix.com) (**MyB**) distribution for cloud virtual environments based on the [bhyve](https:// en.wikipedia.org/wiki/Bhyve) hypervisor. The distribution kit is free, without any restrictions on use in your needs.

---

MyB is a software for working with virtual environments through the simplest API, mainly for use in a Trusted environment and/or integration/building a private cloud, as well as building your own hyperconverged cluster.

MyB is a sattelite project (as a demo of one of the goals) of the non-commercial project [CBSD](https://cbsd.io) and currently uses image libraries and infrastructure kept up-to-date with funds from [CBSD project donors](https://www.patreon.com/clonos) as one of the outcomes of project development funding.

OS and distributions tested in operation (but not limited to this list):

- **Linux**: [Ubuntu](https://ubuntu.com/), [Debian](https://www.debian.org/), [CentOS](https://www.centos.org/), [Rocky](https://rockylinux.org/), [Oracle Linux](https://www.oracle.com/linux/) and so on;
- **BSD**: [FreeBSD](https://www.freebsd.org/), [OpenBSD](https://www.openbsd.org), [NetBSD](https://netbsd.org), [DragonflyBSD](https://dragonflybsd.org) and so on;
- **Windows**: [Windows server](https://www.microsoft.com/en-us/windows-server);
- **Other**: [SmartOS](https://www.joyent.com/smartos), [Android x64](https://www.android-x86.org/) and so on;

MyB only provides an API, if you are looking for a WEB interface to work with 'bhyve/jail', check out the [ClonOS](https://clonos.convectix.com/) project, which is also a fully opensource and BSD-licensed sattite project by CBSD.

## MyB overwiew

Despite the fact that MyB can run any custom installation ISO images, the product is primarily focused on working only with cloud images. An important part of the project is the provision of tools for easily creating your own cloud images with any set of applications and extensibility of the API.

The high speed of virtual infrastructure initialization on demand makes the product unique in terms of the basis for building SaaS/FaaS platforms. For example, the average timing of creating and launching environments to the state to process remote user requests:

1) creation of a VM of any configuration (RAM/vCPU/Storage) and launch is carried out within ~5 seconds; depending on the tuning of the boot parameters and the boot speed of the guest to be able to accept RDP or SSH within ~35 seconds (without modifying the `grub` bootloader timeout) [^1]

![vmup1](https://user-images.githubusercontent.com/926409/165381489-f7a83818-ef09-4d3c-8044-8f91bab488bb.png)

2) creation of a Kubernetes cluster, launch and the ability to accept API requests: ~30 seconds for a single-master and 1 minute 20 seconds for a cluster with any number of master/worker [^1], [^2]

[^1]: - in the absence of an overcommit at the physical level;
[^2]: - 30 seconds spent on bootstrap 'etcd' service if master node > 1;

![kubetime](https://user-images.githubusercontent.com/926409/165322452-96f740bb-d7af-4970-affc-056432a17c46.png)

The product is built on completely alternative technologies, the code of which is distributed under the most liberal BSD/MIT licenses; none of the components and layers is affiliated with any company, in particular, the product is based on completely open-source projects:

- wonderful OS [FreeBSD](https://www.freebsd.org);
- high performance [bhyve](https://en.wikipedia.org/wiki/Bhyve) hypervisor;
- [NETMAP](https://man.freebsd.org/netmap/4)/[VALE](https://man.freebsd.org/vale/4) virtual switch;
- virtual environment manager [CBSD](https://cbsd.io);

## System requirements

Any physical (bare metal) server with an Intel/AMD x86-64 processor that supports virtualization and POPCNT instructions and runs FreeBSD 13.1-RELEASE is suitable for MyBee.

Theoretically, [working on ARM64 architecture is possible](https://github.com/freebsd-upb/freebsd-src/tree/projects/bhyvearm64). There are currently no ARM64-based servers in the CBSD infrastructure, but work on the port can be done when ARM64-based hardware is handed over to the project.

In [some cases](https://wiki.freebsd.org/bhyve#Q:_Can_I_run_multiple_bhyve_hosts_under_VMware_nested_VT-x_EPT.3F) MyB can be run in a virtualized environment, however this is not recommended and has not been tested by the MyB authors. However, if the 'bhyve' hypervisor fails, you can create containers based on FreeBSD jails, but this is most likely not what you want from MyBee ;-)

* Minimum RAM requirements: 4 GB RAM;
* Minimum HDD/SSD requirements: 20 GB (for kubernetes cluster, using SSD/NVME is highly recommended);
* Availability of IPv4;
* the presence of IPv6 (optional, but highly recommended);
* Access to the Internet (80/443 ports), since MyBee will download (one-time) gold-images for those operating systems whose environment you have requested;

The creation of environments is managed through RestAPI (for example, the command line and the curl utility are enough), as well as through a thin client available for modern operating systems. In the future - support for Terraform and a mobile application.

Components:

* [FreeBSD OS](https://www.freebsd.org) 13.1+
* [CBSD](https://cbsd.io) 13.1+

## MyB Handbook

* [Getting and installing MyBee](en/get-myb.md)
* [CLI/shell](en/shell.md)
* [Configuring Network Interfaces](en/network.md)
* [Network profiles](en/netprofile.md)
* [Basic API endpoints](en/api.md)
* [ACLs and Security](en/acl.md)
* [List of GOLD images](en/images.md)
* [Working with jail (curl)](en/jail_curl.md)
* [Working with VM (curl)](en/bhyve_curl.md)
* [Working with Kubernetes (curl)](en/k8s_curl.md)
* [Working with jail (CBSDfile)](en/jail_cbsdfile.md)
* [Working with VM (CBSDfile)](en/bhyve_cbsdfile.md)
* [Working with jail (nubectl)](en/jail_nubectl.md)
* [Working with VM (nubectl)](en/bhyve_nubectl.md)
* Working with jail (Terraform)
* Working with VM (Terrafarm)
* [Support](en/support.md)
* [UI](en/ui.md)
