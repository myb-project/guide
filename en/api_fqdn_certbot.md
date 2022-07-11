# API FQDN and Certbot

By default, MyBee API is accessible via http://IP, however, if you have an external IP address and MyBee is accessible via the Internet, you can set the FQDN and automatically issue a [Let's Encrypt](https://letsencrypt.org) SSL certificate via certbot.

For this, you need:

- Register the IP address of MyBee in DNS. For example: mybee.example.com. Before proceeding to the next step, make sure that the registered host points to the IP address of MyBee;
- Using menu item '16) Set API FQDN ( + certbot )' enter your FQDN (for example: mybee.example.com);

If you did everything correctly, when you enter the FQDN, MyBee will receive an SSL certificate and change the MyBee API to use https://, as well as a cron entry to update the certificate in a timely manner.

![api_fqdn_cert](https://user-images.githubusercontent.com/926409/178330856-03182cf3-5367-42c8-968b-7f0130baec2d.png)

If you want to turn off or change the FQDN, first disable the FQDN API by selecting the '16) Set API FQDN ( + certbot )' option again and please answer 'yes' to disable FQDN question.

---

Next: [Working with jails (curl)](jail_curl.md)
