# Konfiguration der Netzwerkschnittstellen

## Beschreibung

Wenn Sie MyBees Netzwerkeinstellungen verändern wollen, können Sie dafür das `bsdconfig` TUI nutzen, indem **'1) (Re)Configure Host Network'** im Menü der Administrationskonsole auswählen.

![image](https://user-images.githubusercontent.com/926409/163883957-58a6c6fd-c44c-4c06-b2de-477e45936d39.png)

Alternativ können Sie dies über die Kommandozeile tun, indem Sie **'6) Shell'** wählen und dann die Datei **/etc/rc.conf** gemäß der Beschreibung im [FreeBSD-Handbuch](https://docs.freebsd.org/en/books/handbook/config/#config-network-setup) editieren.

### Netzwerkkonfiguration mit bsdconfig

Um beispielsweise die statische IPv4-Adresse 94.130.23.176/26 und ein Standard-Gateway von 94.130.23.129 für die Netzwerkkarte namens 'em0' einzurichten, würde der Dialog folgendermaße aussehen:

![image](https://user-images.githubusercontent.com/926409/163884405-c657c742-446c-40e2-b0b6-850a8727224b.png)

![image](https://user-images.githubusercontent.com/926409/163884441-9416766d-0c8c-44f7-afbc-a8e08f02c580.png)

![image](https://user-images.githubusercontent.com/926409/163884499-9fa1b8a9-c55c-403c-9203-aaeaa7b21a41.png)

![image](https://user-images.githubusercontent.com/926409/163884593-ab7db705-4086-4667-9f6e-80b4dcf8ea45.png)


Um die statische IPv6-Adresse 2a01:4f8:10b:1cd2::1/64 und ein Standard-Gateway von fe80::1 (gemäß der [Hetzner-Dokumentation](https://docs.hetzner.com/robot/dedicated-server/network/net-config-debian-ubuntu/), wo die Konfiguration behandelt wird) für die Netzwerkkarte namens 'em0' einzurichten, würde der Dialog folgendermaßen aussehen:

![image](https://user-images.githubusercontent.com/926409/163884951-31c738a2-e50c-4189-a173-4baa9bd0baa9.png)

![image](https://user-images.githubusercontent.com/926409/163884990-1f39a466-7b05-43e7-9bed-756015bd1040.png)

![image](https://user-images.githubusercontent.com/926409/163885066-d76669de-bc9a-423e-b067-18cf7e3e8224.png)

:bangbang: | :information_source: Beachten Sie, daß wir hier die Option nutzen, die Netzwerkschnittstelle festzulegen, indem wir sie nach dem **'%'**-Zeichen angeben, wenn wir den Standard-Gateway für das IPv6-Netzwerk angeben: **fe80::1%em0** , da **em0** unsere Uplink-Schnittstelle ist.
:---: | :---

Vergessen Sie nicht, gültige Resolver-Adressen anzugeben. Um beispielsweise die öffentlichen Nameserver von Google zu verwenden, können Sie diese Adressen nutzen:

| proto |         IP           |
|-------|----------------------|
| IPv4: |       8.8.8.8        |
| IPv4: |       8.8.4.4        |
| IPv6: | 2001:4860:4860::8888 |
| IPv6: | 2001:4860:4860::8844 |


![image](https://user-images.githubusercontent.com/926409/163885586-cdad3f13-c518-4cac-8688-953a6cb588ed.png)

### FreeBSD /etc/rc.conf

Es ist für die meisten Leute viel einfacher, das Netzwerk zu konfigurieren, indem sie die Systemkonfigurationsdatei '/etc/rc.conf' editieren. Um dies zu tun, starten Sie eine Shell und öffnen Sie '/etc/rc.conf' mit einem Editor Ihrer Wahl (z.B. vi, ee, mcedit, ...) und ändern Sie die folgenden Zeilen:

```
ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
defaultrouter="94.130.23.129"
ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
ipv6_defaultrouter="fe80::1%em0"
```

Eine andere bequeme Methode, um die Werte aus der '/etc/rc.conf' zu editieren, ist die Verwendung von [sysrc(8)](man.freebsd.org/sysrc/8). Damit lassen sich die Werte genau wie oben über die Kommandozeile und ohne Editor setzen:

```
sysrc ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
sysrc defaultrouter="94.130.23.129"
sysrc ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
sysrc ipv6_defaultrouter="fe80::1%em0"
```

Um die geänderten Einstellungen wirksam werden zu lassen, starten Sie den MyBee-Server mittels des Befehls `shutdown -r now` oder durch Auswahl des Menüpunktes **'7) Reboot Server'** neu. Alternativ können Sie den Befehl `service netif restart && service routing restart` ausführen, um den Neustart zu vermeiden. Allerdings wird dies wahrscheinlich Ihre SSH-Verbindung einfrieren und Sie müssen sich ebenfalls neu zum Server verbinden.


---

Next: [Netzwerkprofile](netprofile.md)
