# Working with bhyve (CBSDfile)

## Description

If you are using the FreeBSD platform, in addition to working with the API via [`curl`](bhyve_curl.md) and [`nubectl`](bhyve_nubectl.md), you have another option to work with MyBee - via CBSDfile. 
This file is processed by the `cbsd` command and for this reason, currently not available for other platforms.

The file format that describes the VM for working through the API is completely the same as the <a target="_blank" href="https://www.bsdstore.ru/en/cbsdfile.html">CBSDfile</a> syntax that works locally, 
but variables are added `CLOUD_URL` (points to the MyBee server) and `CLOUD_KEY` (points to the path to the pubkey).

## Create a VM

An example `CBSDfile` to create a vm1 VM:

```
CLOUD_URL="https://rio-west.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_vm1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=0
}
```

When you create a VM, you may want to copy an arbitrary file (for example: prepare.sh) into the environment and execute it. For example, a VM named `minio1`:

```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_minio1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}

postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}
```

and being in the directory with CBSDfile, run the command:
> sudo cbsd up

Also, in one CBSDfile you can describe an unlimited number of environments, for example:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_minio1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}
postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}

bhyve_minio2()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}
postcreate_minio2()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}

bhyve_minio3()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}
postcreate_minio3()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}
```

and create a VM, specifying the name: `sudo cbsd up minio2`. If you run `sudo cbsd up` without specifying environment names, 
you will create all VMs in bulk, and if MyBee consists of several nodes, each subsequent VM will be placed on the next host in the list by round-robin (by default).

##  VM removal

To remove a bhyve via CBSDfile, use the `sudo cbsd destroy [env]` command, run in the directory with the CBSDfile.

## Execute commands, copy files, login, status

While in the directory with the CBSDfile, you can perform various operations on the remote environment, such as:

|      command      |  description                                                                    |
| ----------------- | ------------------------------------------------------------------------------- |
| cbsd login        | makes an SSH call with the key ~/.ssh/id_ed25519 to enter the VM                |
| cbsd bexec <cmd>  | executes <cmd> command in VM via SSH with key ~/.ssh/id_ed25519                 |
| cbsd bscp         | copying files, e.g:                                                             |
|                   |  - from your environment into bhyve: cbsd bscp <local_file> <env>:<remote_file> |
|                   |  - from bhyve into your environment: cbsd bscp <env>:<remote_file> <local-file> |
| cbsd status [env] | show environment(s) status                                                      |

---

Next: [working with jail (Terraform)](jail_terraform.md)

