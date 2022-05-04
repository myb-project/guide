# MyBee - The most simplified API for creating and destroying K8S & cloud VMs

<p align="center">
  <span>English</span> |
  <a href="/README.es.md">Español</a> |
  <a href="/README.fr.md">Français</a> |
  <a href="/README.de.md">Deutsch</a> |
  <a href="/README.ru.md">Русский</a> |
  <a href="/README.ch.md">中文</a> |
</p>

---

:information_source: This guide covers the administration and use of the [MyBee](https://myb.convectix.com) (**MyB**) distribution for cloud virtual environments based on the [bhyve](https://en.wikipedia.org/wiki/Bhyve) hypervisor. The distribution kit is free, without any restrictions on use in your needs.

---

MyBee is a software for working with virtual environments through the simplest API, mainly for use in a trusted environment and/or integration/building of a private cloud, as well as building your own hyperconverged cluster.

MyBee is a satellite project (as a demo of one of the goals) of the non-commercial project [CBSD](https://cbsd.io) and currently uses image libraries and infrastructure kept up-to-date with funds from [CBSD project donors](https://www.patreon.com/clonos) as one of the outcomes of project development funding.

OS and distributions tested in operation (but not limited to this list):

- **Linux**: [Ubuntu](https://ubuntu.com/), [Debian](https://www.debian.org/), [CentOS](https://www.centos.org/), [Rocky](https://rockylinux.org/), [Oracle Linux](https://www.oracle.com/linux/) and so on;
- **BSD**: [FreeBSD](https://www.freebsd.org/), [OpenBSD](https://www.openbsd.org), [NetBSD](https://netbsd.org), [DragonflyBSD](https://dragonflybsd.org) and so on;
- **Windows**: [Windows server](https://www.microsoft.com/en-us/windows-server);
- **Other**: [SmartOS](https://www.joyent.com/smartos), [Android x64](https://www.android-x86.org/) and so on;

MyBee only provides an API, if you are looking for a WEB interface to work with bhyve or jails, check out the [ClonOS](https://clonos.convectix.com/) project, which is also a fully open-source and BSD-licensed satellite project of CBSD.

## MyBee Overwiew

Despite the fact that MyBee can run any custom installation ISO images, the product is primarily focused on working only with cloud images. An important part of the project are the provisioning tools for easily creating your own cloud images with any set of applications and extensibility of the API.

The high speed of virtual infrastructure initialization on demand makes the product unique in terms of the basis for building SaaS/FaaS platforms. For example, the average timing of creating and launching environments to the state to process remote user requests:

1) creation of a VM of any configuration (RAM/vCPU/Storage) and launch is carried out within ~5 seconds; depending on the tuning of the boot parameters and the boot speed of the guest, the machine might be able to accept RDP or SSH connections after as little as ~35 seconds (without modifying the `grub` bootloader timeout) [^1]

![vmup1](https://user-images.githubusercontent.com/926409/165381489-f7a83818-ef09-4d3c-8044-8f91bab488bb.png)

2) creation and launch of a Kubernetes cluster with the ability to accept API requests: ~30 seconds for a single-master and ~1 minute 20 seconds for a cluster with any number of master/worker nodes. [^1], [^2]

[^1]: - in the absence of an overcommit at the physical level;
[^2]: - 30 seconds spent on bootstrap `etcd` service if master node > 1;

![kubetime](https://user-images.githubusercontent.com/926409/165322452-96f740bb-d7af-4970-affc-056432a17c46.png)

The product is built on completely alternative technologies, the code of which is distributed under the most liberal BSD/MIT licenses; none of the components and layers is affiliated with any company. In fact the whole product is based entirely on open-source projects:

- The wonderful [FreeBSD](https://www.freebsd.org) OS;
- The high performance [bhyve](https://en.wikipedia.org/wiki/Bhyve) hypervisor;
- The [NETMAP](https://man.freebsd.org/netmap/4)/[VALE](https://man.freebsd.org/vale/4) virtual switch;
- The [CBSD](https://cbsd.io) virtual environment manager;

## System Requirements

Any physical (bare metal) server with an Intel/AMD x86-64 processor that supports virtualization and POPCNT instructions is suitable for MyBee if it runs FreeBSD 13.1-RELEASE.

In thaory, [running MyBee on the ARM64 architecture is possible](https://github.com/freebsd-upb/freebsd-src/tree/projects/bhyvearm64). However there are currently no ARM64-based servers in the CBSD infrastructure, but work on the port can be done when ARM64-based hardware is made accessible to the project.

In [some cases](https://wiki.freebsd.org/bhyve#Q:_Can_I_run_multiple_bhyve_hosts_under_VMware_nested_VT-x_EPT.3F) MyBee can be run in a virtualized environment, however this is not recommended and has not been tested by the MyBee authors. If the bhyve hypervisor cannot be used, you can still create containers based on FreeBSD jails, but this is most likely not what you want from MyBee ;-)

* Minimum RAM requirements: 4 GB RAM;
* Minimum HDD/SSD requirements: 20 GB (for kubernetes cluster, using SSD/NVME is highly recommended);
* Availability of IPv4;
* the presence of IPv6 (optional, but highly recommended);
* Access to the Internet (80/443 ports), since MyBee will download (only once) gold-images for those operating systems whose environment you have requested;

The creation of environments is managed through a RestAPI (i.e. the command line and the `curl` utility are enough to control it!), as well as through a thin client available for modern operating systems. For the future support for Terraform and a mobile application may become available.

Components:

* [FreeBSD OS](https://www.freebsd.org) 13.1+
* [CBSD](https://cbsd.io) 13.1+

## MyBee Handbook

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
* [Working with jail (nubectl)](en/jail_nubectl.md)
* [Working with VM (nubectl)](en/bhyve_nubectl.md)
* [Working with jail (CBSDfile)](en/jail_cbsdfile.md)
* [Working with VM (CBSDfile)](en/bhyve_cbsdfile.md)
* Working with jail (Terraform)
* Working with VM (Terrafarm)
* [Support](en/support.md)
* [UI](en/ui.md)
