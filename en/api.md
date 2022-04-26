# Basic API endpoints

## Description

**MyB** API is an extensible API in which, depending on the tasks, you can remove and add any new endpoints that can either use the rich features of the [CBSD](https://cbsd.io) framework, or write your own own plugin, accessible through a unique endpoint.

So, the basic capabilities of CBSD allow, if necessary, to run microservices or configurators to create virtual machine checkpoints ( /api/v1/checkpoint ), manage disk snapshots - create, revert and clone ( /api/v1/snapshot ), live migration ( /api/ v1/migrate ), forwarding GPU and PCI devices to guests, access to helpers/CBSD forms ( want to create your own marketplace based on CBSD/MyB ? Read how CBSD works with templates: [FreeBSD Journal January/February 2022: CBSD part 1](https://issue.freebsdfoundation.org/publication/?m=33057&i=739644&p=27&id=26695&ver=html5) etc.

Depending on what kind of custom product you want to create based on MyB/CBSD - another Proxmox? Then the API set will be one. Do you want to create an analogue of Amazon AWS? In this case, the set of endpoints will be different. In other words, your API will never be saturated with endpoints you don't need and will contain only what you need.

In a basic installation, the MyB API is as simple as possible to perform tasks such as creating and acquiring virtual environments in a private cloud. Listed below are the endpoints available immediately after installing MyB.

:bangbang: | :information_source: Pay attention! Most operations work through a token. The token can be either a classic JWT or one obtained by any custom logic. For example, for your convenience and to simplify the API, md5 (your public key) is used as a token. Keep in mind that this scheme only implies using MyB in private clouds (trusted environments), but this is also configurable logic.
:---: | :---


| environments         | endpoint                 | is cid required? | description                                                     |
|----------------------|--------------------------|------------------|-----------------------------------------------------------------|
|                      | /images                  |                  |  getting a list of registered gold images                       |
|                      | /cluster                 |    cid:`<cid>`   |  a summary of all your environments                             |
| jail, VM, kubernetes | /api/v1/create/`<X>`     |    cid:`<cid>`   |  create a new environment `<X>`                                 |
| jail, VM, kubernetes | /api/v1/status/`<X>`     |    cid:`<cid>`   |  summary of the environment `<X>`                               |
| jail, VM, kubernetes | /api/v1/start/`<X>`      |    cid:`<cid>`   |  start environment `<X>` if it is in 'stopped' state            |
| jail, VM, kubernetes | /api/v1/stop/`<X>`       |    cid:`<cid>`   |  stop the `<X>` environment if it is in the 'running' state     |
| jail, VM, kubernetes | /api/v1/destroy/`<X>`    |    cid:`<cid>`   |  destroy the environment `<X>`                                  |
| kubernetes           | /api/v1/kubeconfig/`<X>` |    cid:`<cid>`   |  get kubeconfig for K8S cluster `<X>`, for **~/.kube/config**   |

:construction: <ins>By adding new plugins and extending API endpoints, you can build solutions of any complexity and any opportunity</ins> . :construction:
               

---

Next: [ACLs and Security](acl.md)
