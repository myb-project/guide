# List of GOLD images

MyB will provide the user with tools to create and use custom cloud images. The list of images registered in (and operated on) MyB is available at [endpoint](api.md): **'/list'**.

For example:

> curl http://HOST/images

```
{
  "images": [
    "alma9",
    "centos7",
    "centos8",
    "centos9",
    "debian10",
    "debian11",
    "dflybsd6",
    "fedora37",
    "fedora38",
    "freebsd13_ufs",
    "freebsd13_zfs",
    "freebsd14_ufs",
    "freebsd14_zfs",
    "freefire14_ufs",
    "freepbx",
    "ghostbsd22",
    "homeass",
    "jail",
    "kali2022",
    "k8s",
    "netbsd9",
    "openbsd7",
    "opnsense22",
    "oracle7",
    "oracle8",
    "oracle9",
    "rocky8",
    "rocky9",
    "ubuntu20",
    "ubuntu22",
    "ubuntu22_vdi"
  ]
}
```

The basic MyB installation comes with cloud images supported by the [CBSD](https://cbsd.io) project, but their naming may change from version to version. The name of the image is specified as the 'images' parameter when creating a new environment.

Typically, image names characterize the image of the OS distribution:

|           name            |                                     description                                |
| ------------------------- | ------------------------------------------------------------------------------ |
|          centosX          | Linux CentOS                                                                   |
|          debianX          | Linux Debian                                                                   |
|         dflybsdX          | DragonFlyBSD OS, HAMMERFS on root                                              |
|          fedoraX          | Linux Fedora                                                                   |
|        freebsdX_ufs       | FreeBSD with UFS on root                                                       |
|        freebsdX_zfs       | FreeBSD with ZFS on root                                                       |
|          freepbx          | FreePBX (Asterisk VoIP), please open URL after VM start: http://IP             |
|        ghostbsd22         | GhostBSD: Desktop FreeBSD. Use RDP for accessing using login 'ghost' and password 'ghost' |
|          homeass          | Home Assistant, please open URL after VM start: http://IP:8123                 |
|           jail            | The FreeBSD rootfs image for creating containers based on FreeBSD jail is a base.txz archive from the official website of the FreeBSD project |
|          kali2022         | Linux Kali. In addition to accessing via ssh, you can also use RDP access using login 'kali' and password 'kali' |
|           k8s             | Deployment image for Kubernetes instances                                      |
|         netbsdX           | NetBSD OS                                                                      |
|         openbsdX          | OpenBSD OS:                                                                    |
|         opnsenseX         | OPNSense, please open URL after VM start: http:/IP, lg: 'root', pw: 'opnsense' |
|          oracleX          | Linux Oracle                                                                   |
|          rockyX           | Linux Rocky                                                                    |
|         ubuntuX           | Linux Ubuntu                                                                   |
|        ubuntu_vdi         | Linux Ubuntu Desktop, use RDP for accessing using login 'user' and password: 'password' |
|         rabbitmq          | Image for deploying RabbitMQ clusters                                          |


---

Next: [API FQDN and certbot](api_fqdn_certbot.md)

