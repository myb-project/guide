# Working with VM (nubectl)

## Description

Working with MyBee via [`curl`](bhyve_curl.md) primarily demonstrates the [simplicity of the underlying API](api.md), but it is assumed that you can easily wrap API calls in your favorite programming language and be able to work with private cloud in a more comfortable way.

The `nubectl` utility is one example of such a wrapper, written as an example in the GO language, which allows you to get a binary file for all modern operating systems: Linux, BSD, MacOS and Windows.

You can get the `nubectl` utility from the front page of your MyBee installation or from <a target="_blank" href="https://myb.convectix.com/download/">MyBee</a>.

The `nubectl` utility is a 'thin client' for MyBee and allows you to perform environment creation/destruction operations, as well as login via SSH, including from the Microsoft Windows platform.

To work, you need to specify through the environment variables `CLOUD_URL` and `CLOUD_KEY` (or command line arguments: `-cloud_url` and `-ssh_key` respectively) the MyBee server and the path to your public key file.

## VM creation

To create a VM via `nubectl`, use the `create vm` argument with the name of the environment to create, e.g.:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys /nubectl create vm test1
```

Or you can use the arguments of the `nubectl` utility, for example on Windows OS (also, you may need the `-ssh_key` option to specify the path to the private key when entering the VM):
```
nubectl create vm test1 --cloud_key="c:\authorized_keys" --cloud_url=http://IP
nubectl ssh test1 --cloud_key="c:\authorized_keys" --cloud_url=http://IP [--ssh_key=c:\id_ed25519]
```

## Getting a status

To get the status of your 'namespace' (your public key), use `status`, for example:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl status
{
  "servers": [
    {
      "instanceid": "test1",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.8 -p22"
    },
    {
      "instanceid": "minio3",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.4 -p22"
    },
    {
      "instanceid": "minio2",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.6 -p22"
    },
    {
      "instanceid": "minio1",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.3 -p22"
    }
  ],
  "clusters": [
    {
      "total_environment": 4,
      "total_cpus": 2,
      "total_ram": 2,
      "total_imgsize": 2
    }
  ]
}
```
If you want to see the status of an individual environment, use `status ENV`:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl status test`
```


## VM removal

To remove, use the `destroy` command:
```
env CLOUD_URL="remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl destroy test1
```

## VM login via SSH

To login, use the `ssh` command:
```
env CLOUD_URL="remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl ssh test1
```

## Demo

[![asciicast](https://asciinema.org/a/492201.svg)](https://asciinema.org/a/492201)

---

Next: [Working with jail (CBSDfile)](jail_cbsdfile.md)
