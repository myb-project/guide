# Working with jail (CBSDfile)

## Description

If you are using the FreeBSD platform, in addition to working with the API via [`curl`](jail_curl.md) and [`nubectl`](jail_nubectl.md), you have another option to work with MyBee - via CBSDfile. 
This file is processed by the `cbsd` command and for this reason, currently not available for other platforms.

The file format that describes the container for working through the API is completely the same as the <a target="_blank" href="https://www.bsdstore.ru/en/cbsdfile.html">CBSDfile</a> syntax that works locally, 
but variables are added `CLOUD_URL` (points to the MyBee server) and `CLOUD_KEY` (points to the path to the pubkey).

## Create a container

An example `CBSDfile` to create a jail1 container:

```
CLOUD_URL="https://rio-west.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_jail1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=0
}
```

When you create a container, you may want to copy an arbitrary file (for example: prepare.sh) into the environment and execute it. For example, a container named `minio1`:

```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_minio1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}

postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}
```

and being in the directory with CBSDfile, run the command:
> sudo cbsd up

Also, in one CBSDfile you can describe an unlimited number of environments, for example:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_minio1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}

jail_minio2()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio2()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}

jail_minio3()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio3()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}
```

and create a container, specifying the name: `sudo cbsd up minio2`. If you run `sudo cbsd up` without specifying environment names, 
you will create all containers in bulk, and if MyBee consists of several nodes, each subsequent container will be placed on the next host in the list by round-robin (by default).

##  Jail removal

To remove a jail via CBSDfile, use the `sudo cbsd destroy [env]` command, run in the directory with the CBSDfile.

## Execute commands, copy files, login, status

While in the directory with the CBSDfile, you can perform various operations on the remote environment, such as:

|      command      |  description                                                                   |
| ----------------- | ------------------------------------------------------------------------------ |
| cbsd login        | makes an SSH call with the key ~/.ssh/id_ed25519 to enter the container        |
| cbsd jexec <cmd>  | executes <cmd> command in container via SSH with key ~/.ssh/id_ed25519         |
| cbsd jscp         | copying files, e.g:                                                            |
|                   |  - from your environment into jail: cbsd jscp <local_file> <env>:<remote_file> |
|                   |  - from jail into your environment: cbsd jscp <env>:<remote_file> <local-file> |
| cbsd status [env] | show environment(s) status                                                     |


## Demo

[![asciicast](https://asciinema.org/a/492179.svg)](https://asciinema.org/a/492179)

---

Next: [working with VM (CBSDFile)](bhyve_cbsdfile.md)

