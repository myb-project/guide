# MyBee Quick-Start

Let's create a test guest machine with a minimum of steps. It is assumed that MyBee is already installed and you can get to the menu via Shell: http://IP/shell/

1) Disable ACL by public key. Attention! With this setting, anyone can create a virtual machine. Use this only in 'trusted' environments! Select the item **'Configure Pubkey WhiteList'**:

![quick1.png.png](https://myb.convectix.com/img/quick1.png?raw=true)

The status should change to 'disabled'.

2) This step is not mandatory, but it is recommended to warm up the image in advance - we must make sure that MyBee has access to external mirrors. Select the item **'Shell ( warm cloud image )'** and get into the shell.

![quick2.png.png](https://myb.convectix.com/img/quick2.png?raw=true)

We write 'debian12' and get the latest image of Linux Debian 12

![quick3.png.png](https://myb.convectix.com/img/quick3.png?raw=true)


3) On your local machine, create a file with an arbitrary name `debian12.json` and the following contents:

```
{
  "imgsize": "12g",
  "ram": "1g",
  "cpus": 2,
  "image": "debian12",
  "pubkey": "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain"
}
```

where in the pubkey field insert the line from your ~/.ssh/*.pub file (ED25519, ECDSA and RSA keys are acceptable).

4) Using the `curl` utility, we send a request to create a VM:

```
curl -X POST -H "Content-Type: application/json" -d @debian12.json http://IP/api/v1/create/vm1
```

where IP is the address of MyBee.

![quick4.png.png](https://myb.convectix.com/img/quick4.png?raw=true)

5) Using the `curl -H cid:CID http://IP/api/v1/status/vm1` request with the correct CID (client ID), wait until the information for connecting to the VM is printed. As a rule, the VM is available for use within ~10 seconds when using HDD, with NVME/SSD it is usually faster

note: CID is the md5 hash of your key, in our case:

```
md5 -qs 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain'
190000ee6d0e18a82d6e79a34537a616
```

![quick5.png.png](https://myb.convectix.com/img/quick5.png?raw=true)

6) Using your key, we connect to the guest.

![quick6.png](https://myb.convectix.com/img/quick6.png?raw=true)


---

**<<_**__[Back: installing MyBee](get-myb.md)__ $~~~$ | $~~~$ __[Next: CLI/shell](shell.md)__**_>>**

