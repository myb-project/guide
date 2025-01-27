# CLI/shell

## Description

You can configure MyB or perform various administrative operations through the admin console by accessing the MyB server via SSH or the WS console via a browser: http://IP/shell/

## MyB menu

When you log in as the 'root' user, you will have access to a menu for typical operations, such as: changing network settings, switching profiles, ACLs, etc.:

![mybconsole.png](https://myb.convectix.com/img/mybconsole.png?raw=true)

Use the number and/or arrow keys to navigate. Menu selection is done with the 'Enter' key.

When first installing, the following operations will be most in demand:

* 1) - update MyBee by pressing the 'u' key;

:bangbang: | :information_source: Please note that MyBee update does not update guest systems of already created virtual machines. However, with MyBee update you can get a new profile of a previously missing OS (or a new version of an existing profile), which will result in downloading a new gold image for this guest system when activating the profile.
:---: | :---

* 2) - exit to Unix shell, depending on the distribution it will be either Linux or FreeBSD OS. In Unix shell you can also 'warm up' cloud images (see below);
* 3) - creating a separate user other than 'root' with which you can log in via SSH and on behalf of which you can create VMs (via SSH and API);
* 4) - adjusting the ACL from IP addresses and/or pubkey list that can interact with the API;
* 5) - block access of the user 'root' via SSH (but not via http://IP/shell/ or local login) - this should be done after creating the user via point (4);
* 6) - switch on or off the color scheme;

## Warming up cloud images

contrib profiles that come with MyBee contain information about the guest OS, minimum requirements and a link to get a gold image (but not the images themselves), which are located on the project mirrors and are regularly updated by the CBSD project;

If you do not have enough existing profiles and / or they do not suit you, you can create any number of your own profiles and even upload them to the GitHub profile project - in this case, your profile will be able to be used by all other MyBee users;

When you request to create a guest, the system will automatically start downloading the image from the mirrors, which at the first creation introduces an indefinite delay from several seconds to several minutes - it depends on the speed of your network connection and the availability of mirrors for your instance.

Therefore, to ensure quick receipt of virtual machines, it is recommended to first get the image via CLI (warm up). To do this, use the 'Shell ( warm cloud image )' menu in the main menu and after receiving information about which

images are missing - get them

![mybconsole2.png](https://myb.convectix.com/img/mybconsole2.png?raw=true)

If the system writes 'not found' next to the profile name, by entering the profile name you can get and cache the required image. If you do not do this, the system will do it automatically when creating, however, in this case you will be able to control that there are no network problems, otherwise the VM creation may 'hang' for an indefinite time.

![mybconsole3.png](https://myb.convectix.com/img/mybconsole3.png?raw=true)

### if the speed of mirrors is slow

If you want, you can set up your own local mirror that will sync with the official CBSD mirrors and make MyBee use them. If there are no CBSD mirrors in your region and you want to help the project, you can also publish a link to your mirrors and other CBSD/MyBee users will start using them. Read more about all this [here](https://github.com/cbsd/mirrors).

---

Next: [Configuring Network Interfaces](network.md)
