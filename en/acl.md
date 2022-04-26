# ACLs and Security

## Description

The MyBee distribution is currently positioned as a tool for use in 'Trusted environments' and private infrastructure and for this reason should not be used on public public networks. However, to limit the use of the API, you can configure the following restrictions through the admin console:

1) Manage 'whitelist' on IP addresses that can work with the API. All IP addresses that are NOT present in the list will receive 'permission deny' when working with the API;
2) Manage 'whitelist' for 'pubkey' that can work with the API. All 'pubkey's (and hence their CID tokens) that are NOT in the list will receive 'permission deny' when working with the API;
3) Install proxy (or your own balancer) on the front of the MyB API, on which you can make additional protection, certificates, etc.;

To edit ACLs by IP addresses and/or pubkey, use menu items: **'11) Configure Hosts Allow for API'** and **'12) Configure Pubkey WhiteList'** respectively. Please note that immediately after installation, ACL is disabled by default and the platform serves any user.

![acl](https://user-images.githubusercontent.com/926409/163979082-679d4701-9dcc-47a4-b3fe-46a88b518507.png)


## IP ACL

To enable the filter by IP addresses, use access to the administrator console. When entering the menu, the trigger system works and if the ACL was turned off, the system asks to activate or save the previous state.:

> Turn ACL ON? ('n' or '0' - leave as disabled) 
> [yes(1) or no(0)]

If you disable the ACL, the interactive configuration ends at this point - you will return to the main menu and the platform will disable the ACL.
If you enable or leave the ACL enabled, you will enter the white list editing mode for IP addresses. Record format - 1 IP address per line. Both IPv4 and IPv6 are supported:

![image](https://user-images.githubusercontent.com/926409/163980936-83f07d24-12c8-4082-9580-04d847fc49d9.png)

By using 'F2 save' to save the list and exiting the editor (exit with 'Esc' or 'F10', you will also be prompted to save the file if you didn't with 'F2' ), the ACLs take effect and the calls non-public API endpoints for resources not listed will not be served:

```
$ curl http://mb.convectix.com/images
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx</center>
</body>
</html>
$ 
```

## PUBKEY ACL

A similar filter on access to non-public API endpoints can be achieved at the level of enumerating public keys ([as we remember](api.md), in the default installation they are the source of the ClientID, **cid** ).

![image](https://user-images.githubusercontent.com/926409/163996796-e046ed4b-c8ba-43f2-8eba-4168bd283638.png)

Insert OpenSSH public keys (full format: '<key type> <payload> <comment>') - one key per line:

![image](https://user-images.githubusercontent.com/926409/163998713-bb93af45-a450-4d80-bf1e-54c896d44200.png)

After saving the list and enabling ACL, attempts to work through the CID of unknown keys will not be served:

![image](https://user-images.githubusercontent.com/926409/163998402-063cf00f-a036-43cf-8922-e3df83209e6f.png)


:information_source: FYI: API and PUBKEY ACLs are not interchangeable and can be combined.


---

Next: [List of GOLD images](images.md)
