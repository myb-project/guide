# Working with Kubernetes (curl)

## Description

The [Kubernetes](https://kubernetes.io/) tool is very popular in the DevOPS environment and is positioned as an operating system for containers, where the main product features are automatic scaling, deployment automation, container management, etc. From the point of view of the system, Kubernetes are isolated environments (dedicated servers, virtual machines) that provide runtime environments for containers and, in turn, these environments can scale. So, a kubernetes cluster can consist of either a single host (or virtual machine) or tens or hundreds of environments. Given the benefits that virtualization gives us (DRS, power management, an easy way to unify environments on different types of physical hosts), Kubernetes deployments often look very beneficial in virtualized environments such as MyB. Also, the deployment of Kubernetes to MyB is one example of extending the API and using the gold image generation toolkit, when the distribution of Kubernetes was copied to any Linux-based system for the fastest bootstrap of the service.

You can create any Kubernetes cluster configuration:

1) One virtual machine that combines the role of master ( controller / control plane ) and worker. This is the default cluster type in a typical MyB installation.
2) Separation of the controller/control plane role into separate virtual machines from the worker role (container runtime). Since this affects multiple virtual machines, this type of cluster is possible by adding additional MyB nodes to a single MyB cluster.

In addition, the tandem of such a framework as [CBSD](https://cbsd.io) and Kubernetes cluster deployment makes it very easy to switch network profiles for both [MyB](netprofile.md) and network profiles for Kubernetes. This means that with one profile switch you can:

1) Run Kubernetes API on IPv4 over SSL, where the user (kubeconfig) certificate will regulate the correct upstream when you run multiple K8S clusters;
2) Run Kubernetes with API access directly via external IPv6. We applaud the acceleration of IPv6 adoption!;
3) Run Kubernetes API and Pods with external IPv6 addresses. Deploy services to Kubernetes and work with them directly over the Internet!;
4) is your case here?;


## Creating single node K8S

To create a K8S cluster, use [endpoint](api.md): **'/api/v1/create/'**.

Minimum payload to create a Kubernetes cluster:

```
{
  "image": "k8s",
  "init_masters": "1",
  "master_vm_ram": "4g",
  "master_vm_cpus": "2",
  "master_vm_imgsize": "20g",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost",
 }
```

- The (required) 'image' parameter controls the GOLD image that we use to order Kubernetes. For the base delivery MyB is **'k8s'**;
- (mandatory) parameter 'init_masters' adjusts the number of controller/control plane cluster environments, the minimum number is 1. Usually, a fuzzy number of controllers is created to maintain a quorum: **1, 3, 5, 7** ..;
- (mandatory) parameter 'master_vm_ram' adjusts the amount of RAM for each virtual machine with 'controller' functions;
- (mandatory) parameter 'master_vm_cpus' adjusts the number of vCPUs (cores) for each virtual machine with 'controller' functions. The minimum number of cores for a controller VM is **'1'** for a dev environment and **'2'** for a production one;
- (mandatory) parameter 'master_vm_imgsize' adjusts the amount of disk space for each virtual machine with 'controller' functions (not to be confused with the amount of disk space for PV;
- (mandatory) parameter 'pubkey'. your public key. Used to generate a CID token, as well as to access the MyB K8S console;

Some of the parameters are also mandatory, but they are omitted in the minimum payload, since the most appropriate parameters are set by default.

For example, save the above payload as k8s.json and send a request to create a cluster named **'test1'**:

```
curl -X POST -H "Content-Type: application/json" -d @k8s.json http://HOST/api/v1/create/test1
```


## Creation of multi node K8S

:bangbang: | :information_source: Pay attention! The described additional parameters in the payload work in a MyB cluster consisting of more than one physical node, since in this case, kubernetes consists of several VMs, since creating a multi-node Kubenetes cluster on one physical machine is pointless from the point of view of fault tolerance.
:---: | :---

The full payload for creating Kubernetes clusters might look like this:

```
{
  "image": "k8s",
  "init_masters": "3",
  "init_workers": "3",
  "master_vm_ram": "2g",
  "master_vm_cpus": "2",
  "master_vm_imgsize": "20g",
  "worker_vm_ram": "16g",
  "worker_vm_cpus": "16",
  "worker_vm_imgsize": "20g",
  "pv_enable": "10g",
  "kubelet_master": "0",
  "email": "your@example.com",
  "pubkey": "ssh-ed25519 AAAA..XXX your@localhost",
  "callback": "https://callback_api"
}
```

Description of other fields not previously covered:

- (mandatory) parameter 'init_workers' - controls the number of VMs with the 'worker' role. The minimum number is 0, but in this case, the 'kubelet_master' parameter must always be set to '1' (see below), otherwise you will not be able to deploy the container.;
- (mandatory) parameter 'worker_vm_ram' adjusts the amount of RAM for each virtual machine with 'worker' functions;
- (mandatory) parameter 'worker_vm_cpus' adjusts the number of vCPUs (cores) for each virtual machine with 'worker' functions. The minimum number of cores for a worker VM is **'1'**.
- (mandatory) parameter 'worker_vm_imgsize' adjusts the amount of disk space for each virtual machine with 'worker' functions (not to be confused with the amount of disk space for PV;
- (mandatory) parameter 'pv_enable', enable PV by default ? '0' - no. Parameters like '1024m', '20g' and so on will automatically create PV of the corresponding volume;
- (mandatory) parameter 'kubelet_master' - whether to combine the role of the VM that performs the role of 'controller' with the role of 'worker';
- (optional) 'email' parameter, which will receive kubeconfig and information on working with the cluster when creating a new cluster;
- (optional) 'callback' parameter, in which you can specify a URI/URL, for example 'HTTP(s)://IP/myclu', to which the API will send json events that happen to the cluster (for example, you can track the fact of creating or deleting a cluster by receiving a corresponding request from the API). Used mainly to build automations and higher abstractions over MyB;

## Kubernetes cluster status

To view information about the cluster, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

1) If you don't know your token, get it from your pubkey:
>  md5sum -qs 'ssh-ed25519 AAAA..XXX your@localhost'
> 90af7aa8bc164240521753a105df6a05

2) View information on the K8S cluster using the token:
> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/status/test1

Among the kubernetes status information, you can find the address and port to connect to the **MyB** container based on 'jail' via SSH, through which you can access the Kubernetes PV/PVC, and also use this container as 'jump' ' host that has utilities such as 'kubectl', 'helm' and 'k9s'. The parameters for connecting to the jump container are set in **ssh_console_*** variables:

![image](https://user-images.githubusercontent.com/926409/164258520-e3b38167-63a2-44d6-9a28-2daab62824c2.png)

This is a limited console through which the following utilities are available to you: 'kubectl', 'helm', 'k9s' and 'curl'. Also, in the home directory of the 'fcp' user you are accessing, you will find the **'pv'** directory - this is the PV root of your Kubernetes cluster. Thus, using scp/sftp, you can communicate with your PVCs.

## Jail as a jump-host to access PV/kubectl/helm/k9s

With the order of each Kubernetes cluster, **MyB** runs a personal FreeBSD jail-based container that provides IPv4 access via SSH/SCP to the PV/PVC of your Kubernetes cluster. Also, inside the container you can find 'kubectl', 'helm' and 'k9s' utilities. At startup, the container is already configured via 'kubeconfig' to work with your cluster!

Also, if you have switched to a network profile that only uses external IPv6 (which your Kubernetes control plane will respond to), but you don't have IPv6, you can use this container to manage your Kubernetes cluster by logging into the container via SSH using IPv4.


## Removing Kubernetes

To remove kubernetes, use your token/CID and [endpoint](api.md): **'/api/v1/status/'**, for example:

> curl -H cid:90af7aa8bc164240521753a105df6a05 http://HOST/api/v1/destroy/test1


---

Next: [Working with jail (CBSDfile)](jail_cbsdfile.md)
