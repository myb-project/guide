# Working with jail (curl)

## Description

The jail image is a very lightweight FreeBSD based container that you can run in just like a normal FreeBSD environment.
Containers can be with an RO base (nullfs is used to create a new environment), with an entry in the /etc, /var, /usr/local, /tmp, /home directories, or full-fledged RW containers (more difficult, because for
each container will create a new copy of the rootfs). Also, containers can be created with a virtual network stack (VNET/VIMAGE). See the official FreeBSD manual for details.

It is also possible to set limits on container resources with the 'imgsize', 'cpus', ram' parameters.

When creating a container, you can set the hostname/FQDN of the environment via the 'host_hostname' parameter.
When creating a container, you can install software available for FreeeBSD by specifying space-separated lists of packages in the 'pkglist' parameter.

## Create a container

To create a container, use [endpoint](api.md): **'/api/v1/create/'**.

Minimum payload to create a jail-based environment:

```
{
  "image": "jail",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost"
}
```

For example:

1)
```
cat > jail.json <<EOF
{
 "image": "jail",
 "pubkey": "ssh-ed25519 AAAA..XXX your@localhost"
}
```

2)
> curl --no-progress-meter -X POST -H "Content-Type: application/json" -d @jail.json http://HOST/api/v1/create/jail1

An example of payload to create a container with 'secret.place' hostname, with limit of 1 core, 10g disk space, 2g RAM, and with the `bash` package installed:
```
{
  "image": "jail",
  "host_hostname": "secret.place",
  "imgsize": "10g",
  "ram": "2g",
  "cpus": "2",
  "pkglist": "bash",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost"
}

```

Please note that the time to create the first container depends on the presence of the 'jail' image and the quality of your Internet connection.
It is recommended that you first get the image by running 'jail' in the admin console shell. If you don't, when asked for /create,
the system will automatically first download the rootfs archive for FreeBSD, then unpack it to the file system, and only after that it will create and start the container.
All subsequent launches of containers will occur instantly.


## Jail status

To view container information, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

1) If you don't know your token, get it from your pubkey:
>  md5sum -qs 'ssh-ed25519 AAAA..XXX your@localhost'
> 90af7aa8bc164240521753a105df6a05

2) View information on the jail1 container using the token:
> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/status/jail1

Among the environment status information, you can find the port (it can be dynamic if ports are exposed) and the IP address to connect to the container via SSH.

## Jail removal

To remove jail, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/destroy/jail1


---

Mext: [Working with VM (curl)](bhyve_curl.md)
