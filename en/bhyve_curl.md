# Working with bhyve VM (curl)

## Description

Most of the images you can use in MyB are virtual machine images to run in the bhyve hypervisor.
This is a full-fledged high-performance hypervisor of the second type, supplied with the FreeBSD OS.

Depending on the profile and network architecture, you can create virtual machines with one or more virtual interfaces,
with an external or private address, get both IPv4 and IPv6.

## Create a bhyve VM

To create a container, use [endpoint](api.md): **'/api/v1/create/'**.

Minimum payload to create an environment based on bhyve VM:

```
{
  "imgsize": "10g",
  "ram": "1g",
  "cpus": "2",
  "image": "debian11",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost"
}
```

, where 'debian11' is one of the available images from the [List of Images](images.md). For example, creating a Debian virtual machine:

1)
```
cat > debian11.json <<EOF
{
  "imgsize": "10g",
  "ram": "1g",
  "cpus": "2",
  "image": "debian11",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost"
}
```

2)
> curl --no-progress-meter -X POST -H "Content-Type: application/json" -d @debian11.json http://HOST/api/v1/create/vm1

Please note that the time to create the first virtual machine of each profile depends on the availability of the 'image' and the quality of your Internet connection.
It is recommended that you first get the image by running 'bhyve VM' in the shell shell of the admin console. If you don't, when query for /create,
the system will automatically first download the desired image, then convert it to ZFS Volume, and only after that it will create and start the VM. All subsequent
VM launches of a certain image will occur instantly.


## bhyve VM status

To view information about the virtual machine, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

1) If you don't know your token, get it from your pubkey:
>  md5sum -qs 'ssh-ed25519 AAAA..XXX your@localhost'
> 90af7aa8bc164240521753a105df6a05

2) View information on virtual machine 'vm1' using token:
> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/status/vm1

Among the environment status information, you can find the port (it can be dynamic if ports are exposed) and the IP address to connect to the container via SSH.


## Removing bhyve VM

To delete bhyve VM, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/destroy/vm1


---

Next: [Working with Kubernetes (curl)](k8s_curl.md)
