# Turn MyBee instance into GitHub self-hosted runner

The article is devoted to the deployment of ephemeral *self-hosted* [GitHub runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners) based on the [bhyve](https://en.wikipedia.org/wiki/Bhyve) virtual machines runs under the control of the [MyBee OS](https://myb.convectix.com) with auto-scaling function.
The particular value of the solution, relevant for 'IT minorities', is the ability (in addition to a large selection of *Linux* distributions and *Windows OS*) to use exotic OS distributions that are not officially supported by GitHub
: [Android-x64](https://www.android-x86.org/download), [DragonflyBSD](https://dragonflybsd.org), [FreeBSD](https://www.freebsd.org), [NetBSD](https://netbsd.org) and [OpenBSD](https://openbsd.org).
Given the popularity of the GitHub platform for developing open and portable applications, this can be a small gift for those who are working to improve *CI/QA* processes in relation to such systems..

<center>
<img src="/images/art-gh_runners/bsd-action.png" width="1024">
</center>

## Introduction

[GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions) give private repositories the ability to easily create a pipeline [CI/CD](https://en.wikipedia.org/wiki/CI/CD), placing a certain [YAML](https://en.wikipedia.org/wiki/YAML) file in the repository structure. At the time of this writing (second half of 2022), by default, jobs are executed in the environment which is launched in [Azure](https://azure.microsoft.com) as [Standard_DS2_v2](https://docs.microsoft.com/en-us/azure/virtual-machines/dv2-dsv2-series) virtial machines:

|                                   |                                       |
|-----------------------------------| ------------------------------------- |
|vCPU:                              |                  2                    |
|Memory, GiB:                       |                  7                    |
|Temp storage (SSD) GiB:            |                 100                   |
|Max temp storage throughput:       | IOPS/Read MBps/Write MBps: 6000/93/46 |
|Max data disks:                    |                  8                    |
|Throughput, IOPS:                  |                 8x500                 |
|Max NICs:                          |                  2                    |
|Expected network bandwidth (Mbps): |                 1500                  |


## About Github Actions/runners

In Github-hosted runners, you have a certain set of images for various operating systems and versions that have a certain set of libraries and SDKs.
Workflows have a time limit of 6 hours, after which the job is automatically canceled and the Runner is cleared.
For most cases, this is more than enough. But what if you want to run your tests on an operating system that is not on the list of supported images?
Or do you need more disk space, more CPU cores and more RAM? Or you need to build a custom specialized environment, for example, with access to a graphic
processor or other specialized hardware? And of course, this solution should fit into an adequate price.
In this case, Github recommends using your own runners. You can register and use two types of runners:

- *Persistent* - each job can use this runner an infinite number of times - the environment is not reloaded and, accordingly, for each subsequent job, artifacts left over from the work of the previous one may remain in the environment;
- *Ephemeral* - this type of runner accepts only 1 task, after which the environment is rolled back to its original state, which ensures that the environment is always clean before the next tests. This is definitely a nice benefit and saves you a lot of time maintaining the environment. We will focus only on this type of runner and achieve auto-scaling.

So, let's summarize the difference between *Github-hosted* and *Self-hosted* so that we can better understand the problems being solved.

*GitHub-hosted runners:*

- The operating systems on which the runners run are updated automatically, have initial sets and versions of packages and tools determined by the GitHub team, which you cannot influence;
- Maintained by the GitHub team;
- Have restrictions in resources, type of equipment and access to physical resources (there is no access to the GPU, USB and other peripherals);
- There is an autoscale function, but everything depends on your tariff plan: as a cost, 'free minutes' of your GitHub plan are consumed, this affects the intensity of using runners: for example, on a free tariff, you have serious restrictions in the form of the number of tasks per minute;

*Self-hosted runners:*

- OS and components in the environment are installed, configured, updated and maintained only by you. This can be both a big plus (you fix the software versions and define a set of software, which significantly affects the speed of bootstrap / warming up a new runner), or a minus - the cost of creating and maintaining the infrastructure. We level this minus by using *MyBee*, read about it below;
- By default, there is no possibility of Auto-scaling, which we will also solve using *MyBee*.
- You can use cloud services or your own dedicated servers, thereby getting the maximum power without any extra charge - low cost;
- You can customize not only software, but also hardware, providing access to the peripherals you need for your cases;

## About MyBee

*MyBee* - it is a free open-source distribution kit that does not require knowledge and skills in working with Unix/command line in order to create and immediately start using a virtual machine. The installation field, the user has access to the simplest API to get the VM in the easiest way, but no one forbids entering the *MyBee* command line either. The project supports the work and timely updates of a large number of distributions, which can be used as high-performance self-hosted runners. At the time of writing, [list of images](images.md) available to create 'out of the box':

```
dflybsd-DragonflyBSD-hammer-x64-6
freebsd-FreeBSD-ufs-x64-12.3
freebsd-FreeBSD-ufs-x64-13.0
freebsd-FreeBSD-ufs-x64-13.1
freebsd-FreeBSD-ufs-x64-14
freebsd-FreeBSD-zfs-x64-12.3
freebsd-FreeBSD-zfs-x64-13.0
freebsd-FreeBSD-zfs-x64-13.1
freebsd-FreeBSD-zfs-x64-14
freebsd-OPNSense-22-RELEASE-amd64-22
linux-Alma-9-x86_64
linux-CentOS-7-x86_64
linux-CentOS-stream-8-x86_64
linux-CentOS-stream-9-x86_64
linux-Debian-x86-10
linux-Debian-x86-11
linux-Debian-x86-9
linux-Fedora-36-x86_64
linux-FreePBX-16-x86_64
linux-HomeAssistant-8
linux-Kali-2022-amd64
linux-Oracle-7-x86_64
linux-Oracle-8-x86_64
linux-Oracle-9-x86_64
linux-Rocky-8-x86_64
linux-Rocky-9-x86_64
linux-kubernetes-24
linux-rabbitmq
linux-ubuntudesktop-amd64-22.04
linux-ubuntuserver-amd64-20.04
linux-ubuntuserver-amd64-22.04
netbsd-netbsd-x86-9
openbsd-openbsd-x86-7
openbsd-openbsd-x86-70
```

The distribution kit can be installed on your dedicated server of any configuration. When installing several nodes, a cluster can be organized with a single L2 segment ( [https://en.wikipedia.org/wiki/Virtual_Extensible_LAN](vxlan) ), in which, through a single entry point (API), VMs will be created on different nodes, providing *DRS* (Dynamic Resource Scheduler). For those who do not have a dedicated server (or who do not want to constantly pay for a dedicated server due to rare tasks), there is a separate option - see the below.


## About bhyve

Type 2 hypervisor originally developed by [https://www.netapp.com/](NetAPP) and donated to the *FreeBSD* project. Now widely adopted by FreeBSD, [https://tritondatacenter.com/smartos](SmartOS)-based system,
and also on [https://www.apple.com/macos/](MacOS) under the [https://github.com/machyve/xhyve](xhyve) name. The distinguishing feature of the hypervisor is the so-called 'legacy-free' code. So, many users noted more efficient operation of VMs in conditions of high CPU overcommitting compared to widespread KVM/XEN, which can also be relevant for Runners when one host serves a large number of environments.


## Autoscaling issues

Designed to manage autoscaling is [https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks](Webhooks). Every time the *pipeline* of a new job starts, GitHub will send events via *Webhooks* that the job has been queued and a new worker is needed. If the worker process is already connected to the network and is idle, that worker process wakes up and takes over the job, transitioning to the Active state. The task of the *Webhook* handler is to prepare and start the next virtual machine. In addition, *Webhook* events signal us about the passage of the flow - the beginning and end of the pipeline. Also, events use labels (labels) to which this or that task is oriented. Some kind of automation is required that will launch runners, using labels as a criterion for launching the required OS and a set of software in them. Thus, you can define several types of runners, adjust the flavors of VM resources (cpu / ram / hdd), depending on the resources needed for certain pipelines, and also mark specialized runners that have access to peripherals, for example, with access to GPU or USB tokens. This task is solved by the GH runner extension for *MyBee*, which is a layer between *MyBee* and one of the many GH runner managers, in this case we will use https://github.com/cloudbase/garm.
 

# Setting

## Requirements

So, we need an installed MyBee distribution kit on any dedicated equipment. In addition, of course, you will need administrative access to the GitHub project to which you want to connect *MyBee* as a self-hosted runner. Because Github will fire a Webhook event (a normal HTTP/Rest request), *MyBee* must be available from the GH side - the easiest is to use a server with an external IP address that the DNS entry will point to. If you have not yet assigned a domain name for MyBee pointing to an external IP address, be sure to do so. Also, most likely, you may need to issue a Let's encrypt certificate for the given name - the MyBee console [allows you to do this](api_fqdn_certbot.md). For example, in this example, the domain name 'garm.convectix.com' will be used.


## GARM initialization

Working with the module is done through the *nix command line, for this, in the administrator console, select the item '6) Shell ( warm cloud image )'

The `cbsd garm` command is used for initial initialization (as well as viewing the status of the module). The script is quite self-documented and, depending on the state, displays certain hints as possible next steps. If you want to see all the features of a script, type:

```
cbsd garm --help
```

To reset all settings and roll back to the pre-initialization state, use the `cbsd garm mode=reset`.

If the *GARM* module has not been initialized, then the script will start with the message 'garm: PAT not found!' and instructions on how to get started.
*PAT* or [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token), required to work with the API and will be used by MyBee to register and destroy the Runner. Generate a new *PAT* via 'Settings' -> 'Developers settings' -> 'Personal access tokens' -> 'Generate new token'.
Give the token a name (e.g. 'mybee') and remove the 'Expiration' limit, otherwise you will have to recreate the settings each time the token expires:


![pat1 screenshot](/images/art-gh_runners/pat1.png)


![pat2 screenshot](/images/art-gh_runners/pat2.png)


![pat3 screenshot](/images/art-gh_runners/pat3.png)


In the list of token powers, select only those rights that we need, namely:


* public_repo - to access public repositories;
* repo        - to access private repositories;
* admin:org   - if you plan to use it with an organization to which you have access;


![pat4 screenshot](/images/art-gh_runners/pat4.png)

As a result, you will get a generated token like: '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' - copy it and paste it into the script request 'Please insert you PAT here':

The next question to be answered is 'Please enter the URL which you will use for the hook'. A valid URL is expected with your MyBee installation accessible from the Internet - since GitHub webhooks will work with the resource. In our example, we use the domain name garm.convectix.com, respectively, the URL will look like this: https://garm.convectix.com

If you entered the correct token and the URL is available from the GitHub network, you will see the status of pools and runners. This means you can move on to the next level of customization - creating pools for your organizations and projects.

![garm_init screenshot](/images/art-gh_runners/garm_init1.png)

## Connecting a GitHub project to MyBee GARM

Decide which GitHub project you want to connect self-hosted runners based on MyBee to. In this article, we will connect the https://github.com/cbsd/gtest repository. As we can see, the name of the repository is 'gtest' which is in the 'cbsd' organization: 'cbsd/gtest'.
To do this, run the script with the 'mode=addrepo' parameter and the corresponding name of the organization and repository:

```
cbsd garm mode=addrepo owner="cbsd" reponame="gtest"
```

The script will issue another instruction for integrating Webhook with MyBee, let's analyze it:

1) In GitHub, you must set the events that GitHub will trigger on your MyBee installation. Go to the Settings of your repository:

![repo1 screenshot](/images/art-gh_runners/repo1.png)

Select *Webhooks* from the menu on the left and click the button to add a new webhook: 'Add webhook' (right)

![repo2 screenshot](/images/art-gh_runners/repo2.png)

Fill in the fields where:

Payload URL:  - correct and accessible URL of your MyBee, for example in our case, this is: https://garm.convectix.com;
Content type: - change the value to 'application/json';
Secret: - the GARM script should show it to you, paste the information from the console into this field. Token is generated automatically;

From the list in the area 'Which events would you like to trigger this webhook?' we do not need to push MyBee to all events, but only one thing is required - to react to 'Workflow jobs' - it is the source for the new task start event.
Therefore, we select 'Let me select individual events. ' and check the 'Workflow jobs' event at the bottom of the list.

![repo3a screenshot](/images/art-gh_runners/repo3a.png)

![repo3b screenshot](/images/art-gh_runners/repo3b.png)

By clicking 'Add webhook', we add and enable sending events from GitHub to MyBee. In the console, press 'Enter' when ready, and if the connection between MyBee and GitHub is established correctly, the repository will be served.

![repo4 screenshot](/images/art-gh_runners/repo4.png)

## Pool creation

The next and last thing on the way to solving scaling and on-demand runners is the creation of a named group of runners - a pool. You can create an unlimited number of pools. The pool is characterized by the following properties:

- OS distribution kit ( image ), which will serve jobs for this pool. All runners in this pool will use only one distribution - if you want the pipeline to run on multiple distributions or OSes - you need to create as many pools as there are distributions you want to use;
- Flavors. Physical characteristics of virtual machines: vCPU, RAM, DSK. You can create custom flavors via the `cbsd vmpackages-tui` dialog) in the Unix shell, by default there are 3 flavors: 'small1' (1/2g/20g), 'medium1' (4/8g/60g) and 'large1' (8/16g/100g);
- Labels: Labels are markers (or filters) for selecting certain Runners. This can be an arbitrary criterion in the form of a word or words separated by commas if there are several labels. For example, with labels you can mark the architecture ( x86-64, armv8, etc.) or distribution ( openbsd ) or various features and abilities of this group of runners (gpu-passthru, usb-token, highperf), etc.;
- Minimum number of runners in 'idle' mode. This is the number of environments that *MyBee GARM* will always try to prepare. Since creating and starting a new runner can take some time, it is desirable to have a number of runners 'warmed up' beforehand that can process the new task immediately. Of course, if the number of pipeline launches is high and the cost of resources is in the background, it makes sense to set a high value. If the number of pipeline launches is rare, this option allows you to significantly save on resources. So, if you set this parameter to zero, the runner will only be created when the job is started. In this case, when using cloud MyBee, you will pay only for the time the virtual machine is running at the time the pipeline is running. If the repository has only a few changes per day or less often, this is a significant savings in costs and resources.
- The maximum number of runners. The previous parameter 'Idle' controls the number of free runners, however, if the load on the repository is high, *MyBee GARM* will try to create as many Runners as necessary to process all tasks - in other words, this is the Auto-scaling mechanism we need. However, it is fraught with a chance to exhaust all available resources (for example, several hundred changes per minute began to be made to the repository). You can protect yourself by setting an upper bound on the maximum number of Runners. In this case, upon reaching the set limit, new tasks will wait for the release of resources from previous runners.

For example, let's create a pool of OpenBSD environments:

```
cbsd garm mode=addpool
```

At the first step, select the flavor for the VM of this pool:

```
Available flavors, please select name for new pool:
NAME     CPU  RAM  DISK
small1   1    1g   10g
medium1  4    4g   20g
```

For example: 'small1'.

The next step is to select one of the available images:

```
Available images, please select name for new pool:
You choice [alma9 centos7 centos8 debian10 debian11 dflybsd6 freebsd13_ufs freebsd13_zfs freebsd14_ufs freebsd14_zfs netbsd9 openbsd7 opnsense21 oracle7 oracle8 oracle9 rocky8 rocky9 ubuntu20]:
```

For example: openbsd7

Next, select which repository the pool will be attached to. If you've added multiple repositories, you'll be presented with a space-separated list to choose from. You need to write the required resource via the 'owner/repo' input. If there is only one repository - you can just press 'Enter' to confirm.

```
Available repositories, please select for new pool:
You choice [cbsd/gtest]: 
SELECTED: cbsd/gtest
```

The next steps - aree expected integer values for the maximum number of runners (>0) and the number of runners in the idle state (0 or a value less than max runners):

```
Enter max runners (num): 5
SELECTED: 5
Enter min idle runners (num): 3
SELECTED: 3
```

and finally, the last step is listing the 'tag words' separated by commas. If you have nothing to think of - you can leave blank and press 'Enter' - runners will still get some set of labels (for example 'self-hosted', 'mybee' and OS type ( linux,freebsd,openbsd,dragonflybsd,netbsd,windows )

```
Enter comma-separated labels: bsd,small
SELECTED: bsd,small

1. Flavors: small2
2. Images: openbsd7
3. Repos: cbsd/gtest
4. Max runners: 5
5. Min idle runners" 3
6. Labels: bsd,small

Select 1-6 to change settings or 'Enter' to apply settings.
```

![pool1 screenshot](/images/art-gh_runners/pool1.png)

After confirming with 'Enter', the runner creation process will begin (when min idle > 0 ). You can view the status through `cbsd garm`. After a while, the required number of runners will appear on the Runners page: Settings -> Actions -> Runners.


![pool2 screenshot](/images/art-gh_runners/pool2.png)

Congratulations! the runners took up their combat duty.

## Workflow

To use self-hosted runners, use the normal Github actions syntax. Be aware that the original git checkout does not work on BSD systems, so you may want to use https://github.com/myci-actions/checkout as the 'checkout' step,
duplicated in the repository of our convectix project:

```
- name: checkout sources
      uses: convectix/checkout@master
```

For an example of Github Actions, you can look at https://github.com/cbsd/gtest/blob/main/.github/workflows/act.yml

To remove a pool and/or disable a GitHub repository from MyBee, use `mode=delpool` and `mode=delrepo` respectively.

## Known issues, bugs, future plans

*GARM*, as well as the module of the same name for *MyBee*, is a rather young project and its first inclusion in MyBee happened from version 13.1.1 (mid 2022).
Therefore, feedback is especially important. From tested guests as runner (those should work 100%):

- all BSD;
- Linux: Debian, Ubuntu, CentOS

Note1: some systems (especially NetBSD and DragonflyBSD) may take longer to prepare than others - this is mainly due to the fact that these projects do not have Geo-based mirrors used when installing software ( pkg, pkg_add ), since starting each runner is installing some additional software (basically, additional installation of 'git', 'curl', 'wget' packages and directly github runner). In a production environment, such an algorithm is not welcome, since the launch of each runner is accompanied by the corresponding traffic from the internal. Instead, generate your own GOLD image for runner - one of MyBee's strengths is to provide tools to generate your own cloud images. Or use local mirrors. In this case, new runners will start within 30 seconds.

Note2: Using Windows OS as a runner is also possible, but due to known licensing restrictions, this image is not supplied with the basic version of MyBee. You can send an email request for a Windows image and licenses: book-myb at convectix.com

Note3: In the case of FreeBSD, as a runner you can use not only bhyve virtual machines, but also lightweight containers based on [jail(8)](https://man.freebsd.org/jail/8).

If something went wrong:

- try to reload the service from CLI: `service garm stop` + `service garm start`
- check logs: /var/log/garm/ and up-\*.log files in /tmp directory.

Further work: GitLab support, Jenkins support.

## Epilogue

In summary, self-hosted runners can be beneficial:

- those who have 'highload' runners - you can adjust the number of pre-warmed runners;
- for those who have almost no launches during the day and who want to save on a constantly running or dedicated server: rent runners in the cloud on demand and pay only for the time it runs;
- for those who want to use custom resources, unique flavors, do not depend on the restrictions on the number of launches and cputime in the GitHub cloud, have high power at low cost;

*MyBee* allows you to do all this with ~10 minutes of setup time. In turn, this was made possible thanks to the support of cloud images of the CBSD project, which is used by MyBee as an example of reusing CBSD developments to easily create something complex. The project is being developed on a non-commercial basis and voluntary donations are very important: https://www.patreon.com/clonos

## Try

Don't have a dedicated server but want to try? You can use the cloud or rent MyBee from ConvectIX at no extra cost (you pay the price of your chosen dedicated server provider - the price is formed by your chosen provider).
For details, you can send a request by email: book-myb at convectix.com
