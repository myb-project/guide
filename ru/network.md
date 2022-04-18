# Configuring Network Interfaces

# Описание

Если вы хотите переконфигурировать сетевые настройки MyB, вы можете воспользоваться TUI-конфигуратором bsdconfig, выбрав в консоли администратора пункт **'1) (Re)Configure Host Network'**.

![image](https://user-images.githubusercontent.com/926409/163883957-58a6c6fd-c44c-4c06-b2de-477e45936d39.png)

Либо, выйдя в обычную командную строчку FreeBSD через пункт **'6) Shell'**, можете отредактировать файл **/etc/rc.conf** в соответствии с [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/config/#config-network-setup).

## bsdconfig netconfig

Например, для конфигурирования статического IPv4 94.130.23.176/26 и шлюза по-умолчанию 94.130.23.129 для сетевой карты с именем 'em0', диалог будет таким:

![image](https://user-images.githubusercontent.com/926409/163884405-c657c742-446c-40e2-b0b6-850a8727224b.png)

![image](https://user-images.githubusercontent.com/926409/163884441-9416766d-0c8c-44f7-afbc-a8e08f02c580.png)

![image](https://user-images.githubusercontent.com/926409/163884499-9fa1b8a9-c55c-403c-9203-aaeaa7b21a41.png)

![image](https://user-images.githubusercontent.com/926409/163884593-ab7db705-4086-4667-9f6e-80b4dcf8ea45.png)


Для конфигурирования статического IPv6 2a01:4f8:10b:1cd2::1/64 и шлюзом по-умолчанию fe80::1 (в соответствии с [документацией Hetzner](https://docs.hetzner.com/robot/dedicated-server/network/net-config-debian-ubuntu/), где производится настройка) для сетевой карты с именем 'em0':

![image](https://user-images.githubusercontent.com/926409/163884951-31c738a2-e50c-4189-a173-4baa9bd0baa9.png)

![image](https://user-images.githubusercontent.com/926409/163884990-1f39a466-7b05-43e7-9bed-756015bd1040.png)

![image](https://user-images.githubusercontent.com/926409/163885066-d76669de-bc9a-423e-b067-18cf7e3e8224.png)

:bangbang: | :information_source: Обратите внимание, мы используем возможность зафиксировать интерфейс после символа **'%'** при указывании шлюза по-умолчанию для сети IPv6: **fe80::1%em0** , где **em0** - наш uplink интерфейс.
:---: | :---

Не забудьте прописать корректные resolver адреса. Например, публичные DNS сервера от Google:

| proto |         IP           |
|-------|----------------------|
| IPv4: |       8.8.8.8        |
| IPv4: |       8.8.4.4        |
| IPv6: | 2001:4860:4860::8888 |
| IPv6: | 2001:4860:4860::8844 |


![image](https://user-images.githubusercontent.com/926409/163885586-cdad3f13-c518-4cac-8688-953a6cb588ed.png)

## FreeBSD /etc/rc.conf

Большинству людей сконфигурировать сеть в FreeBSD через штатный /etc/rc.conf значительно проще, для этого выйдите в shell и открыв /etc/rc.conf в любом редакторе (mcedit, vi) , откорректируйте строчки:

```
ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
defaultrouter="94.130.23.129"
ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
ipv6_defaultrouter="fe80::1%em0"
```

Другой способ удобного редактирования параметров /etc/rc.conf - использование скрипта [sysrc(8)](man.freebsd.org/sysrc/8), аналогично записям выше, через командную строчку:

```
sysrc ifconfig_em0="inet 94.130.23.176 netmask 255.255.255.192"
sysrc defaultrouter="94.130.23.129"
sysrc ifconfig_em0_ipv6="inet6 2a01:4f8:10b:1cd2::1/64"
sysrc ipv6_defaultrouter="fe80::1%em0"
```

Для применения параметров в силу, перезагрузите MyB:

> shutdown -r now

Или через пункт **'7) Reboot Server'**


---

Дальше: [Сетевые профили](netprofile.md)
