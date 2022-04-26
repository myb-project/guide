# Network profiles

## Description

Virtual environments are usually not an end in itself for the users who create them and are designed to perform a specific function, depending on the tasks set. Since users have different tasks, conditions and restrictions in which the platform is installed, this leads to certain requirements for configuring network settings and virtual machines. For example, a number of users have only one server with a single external IPv4. Others have one external IPv4, but also many external IPv6 addresses. Still others have external IPv4 addresses that must be assigned to virtual environments. And so on - a large number of different variations.

To save users from the complex process of reconfiguring host system settings, the MyB project implements sequences of configurations into named groups - profiles, among which the user chooses the most optimal scenario for himself.

Profiles affect which addresses and network routing will be used when ordering virtual machines and kubernetes clusters. You can switch profiles (and thereby reconfigure MyB ) an unlimited number of times. The only limitation is that in order to switch to a new profile, all virtual environments created under the old profile must be destroyed beforehand. As feedback is received from users, the number of profiles and their variety will increase.

## Profile switching

To change the profile, use the 'Change VM network profile' item:

![net](https://user-images.githubusercontent.com/926409/163693214-04a0f579-e36c-44d2-a877-2b790f90291d.png)

Not all profiles can be enabled until certain requirements are met, for example, you can't enable a profile that controls IPv6 addressing for virtual environments if you haven't configured IPv6 on the host itself.

Profiles are applied instantly by entering the corresponding profile number and confirming with the 'Enter' key.

![net](https://user-images.githubusercontent.com/926409/163694702-63a21af7-aaea-4d68-a97f-dd0e55b360bf.png)

Let's proceed to the description of profiles and possible network topologies.

## 1) Single NIC: private IPv4

Default profile. First classic installation when you have a server with one external IPv4 address. A typical network configuration of virtual environments is an IPv4 address from a private (rfc1918) network, with access to the Internet via NAT and the ability to redirect individual services/ports inside using free ports of the host's external IPv4 address.

Also, this profile can cover two cases:

Mapping / forward 1: 1 additional external IPv4 addresses to any of the virtual machines, elastic IP. So, the virtual machine starts with a private IPv4 address, but the external IPv4 address allocated for the VM guarantees that any call to any port of the external address using the tcp / udp protocols will be forwarded through the same ports to the address of the virtual machine. Also, when traffic goes outside, the VM will use the dedicated address as a NAT, and not a shared one.

## 2) Multi NIC: private IPv4

The second classic profile. An installation similar to the first profile, but in addition to a single (or very limited number of external IPv4 addresses), your host is configured to use IPv6 addresses. In this case, virtual environments are created with two virtual interfaces, where the first interface is similar to profile 1: an IPv4 address is assigned from a private (rfc1918) network, with access to the Internet when working with an IPv4 stack through NAT and the ability to redirect individual services/ports over IPv4 inside, using free ports of the external IPv4 address of the host. The second interface is configured and receives an external IPv6 address and a separate 'default router' with access to the Internet when the IPv6 stack is running.

In this case, if your home computer or other resources work using the IPv6 protocol, you will be able to work with the virtual environment directly at the external address, without any redirects or forwards.


---

Next: [Basic API endpoints](api.md)
