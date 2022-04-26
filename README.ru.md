# MyBee - The most simplified API for creating and destroying K8S & cloud VM

<p align="center">
  <a href="/README.md">English</a> |
  <a href="/README.es.md">Español</a> |
  <a href="/README.fr.md">Français</a> |
  <a href="/README.de.md">Deutsch</a> |
  <span>Русский</span> |
  <a href="/README.ch.md">中文</a> |
</p>

---

:information_source: В этом руководстве рассматриваются вопросы администрирования и использования дистрибутива [MyBee](https://myb.convectix.com) (**MyB**) для работы с облачными виртуальными средами на базе гипервизора [bhyve](https://en.wikipedia.org/wiki/Bhyve). Дистрибутив бесплатный, без каких-либо ограничений по использованию в ваших нуждах.

---

MyB представляет собой ПО для работы с виртуальными окружениями посредством простейшего API, в основном для использования в Trusted environment и/или интеграции/построения частного облака, а также для построения собственного гиперконвергентного кластера.

MyB является sattelite-проектом (как демонстрация одной из целей) некоммерческого проекта [CBSD](https://cbsd.io) и в настоящее время использует библиотеки образов и инфраструктуру, поддерживаемыми в актуальном состоянии на средства [донороров проекта CBSD](https://www.patreon.com/clonos) в качестве одного из результатов финансирования по развитию проекта. 

Проверенные в работе ОС и дистрибутивы (но не ограниченны этим списком):

- **Linux**: [Ubuntu](https://ubuntu.com/), [Debian](https://www.debian.org/), [CentOS](https://www.centos.org/), [Rocky](https://rockylinux.org/), [Oracle Linux](https://www.oracle.com/linux/) и т.д.;
- **BSD**: [FreeBSD](https://www.freebsd.org/), [OpenBSD](https://www.openbsd.org), [NetBSD](https://netbsd.org), [DragonflyBSD](https://dragonflybsd.org) и т.д;
- **Windows**: [Windows server](https://www.microsoft.com/en-us/windows-server);
- **Other**: [SmartOS](https://www.joyent.com/smartos), [Android x64](https://www.android-x86.org/) и т.д.;

MyB предоставляет только API, если вы ищите WEB интерфейс для работы с 'bhyve/jail', обратите внимание проект [ClonOS](https://clonos.convectix.com/) , также являющийся полностью opensource и BSD-licensed sattite-проектом CBSD.

## MyB overwiew

Несмотря на то, что MyB может запускать любые кастомные инсталляционные ISO образы, продукт в первую очередь ориентирован на работу только облачных образов. Немаловажной частью проекта является предоставление инструментария для легкого создания ваших собственных cloud-образов с любым набором приложений и расширяемость API.

Высокая скорость инициализации виртуальной инфраструктуры по-требованию делает продукт уникальным с точки зрения базы для построения SaaS/FaaS платформ. Например, средние показатели тайминга создания и запуска окружений до состояния обработать удаленные пользовательские запросы:

1) создание ВМ любой конфигурации (RAM/vCPU/Storage) и запуск осуществляется в течении ~5 секунд; в зависимости от тюнинга boot параметров и скорости загрузки гостевой системы до возможности принять RDP или SSH в течении ~35 секунд (без модификаций timeout загрузчика grub) [^1]

![vmup1](https://user-images.githubusercontent.com/926409/165381489-f7a83818-ef09-4d3c-8044-8f91bab488bb.png)

2) создание Kubernetes кластера, запуск и возможность принимать запросы на API: ~30 секунд для single-master и 1 минута 20 секунд для кластера с любым количеством master/worker [^1], [^2]

[^1]: - при отсутствии оверкоммита на физическом уровне;
[^2]: - 30 секунд уходит на bootstrap 'etcd' сервиса при наличии master нод > 1; 

![kubetime](https://user-images.githubusercontent.com/926409/165322452-96f740bb-d7af-4970-affc-056432a17c46.png)


Продукт построен на полностью альтернативных технологиях, код которых распространяется под максимально либеральными лицензиями BSD/MIT; ни один из компонентов и прослоек не аффилирован с какой-либо компанией, в частности, продукт базируется на полностью открытых open-source проектах: 

- замечательная ОС [FreeBSD](https://www.freebsd.org);
- высокопроизводительный гипервизор [bhyve](https://en.wikipedia.org/wiki/Bhyve);
- [NETMAP](https://man.freebsd.org/netmap/4)/[VALE](https://man.freebsd.org/vale/4) виртуальный свич;
- менеджер виртуальных окружений [CBSD](https://cbsd.io).

## System requirements

Для MyBee подойдет любой физический (bare metal) сервер с процессором Intel/AMD архитектуры x86-64, поддерживающим виртуализацию и POPCNT инструкцию, на котором запускается ОС FreeBSD 13.1-RELEASE.

Теоретически, [возможна работа на архитектуре ARM64](https://github.com/freebsd-upb/freebsd-src/tree/projects/bhyvearm64). На данный момент в инфраструктуре CBSD нет серверов на базе ARM64, но работа над портом может быть проведена при передаче проекту оборудования на базе ARM64.

В [некоторых случаях](https://wiki.freebsd.org/bhyve#Q:_Can_I_run_multiple_bhyve_hosts_under_VMware_nested_VT-x_EPT.3F) MyB может быть запущен в виртуализированной среде, однако это не рекомендуется и авторами MyB не тестировалось. Тем не менее, в случае неработоспособности гипервизора 'bhyve', вы можете создавать контейнера на базе FreeBSD jails, но скорее всего, это не то что вы хотите от MyBee ;-)

* Минимальные требования по RAM: 4 GB RAM;
* Минимальные требования HDD/SSD: 20 GB (для kubernetes кластера использование SSD/NVME крайне рекомендуется);
* наличие IPv4;
* наличие IPv6 (необязательно, но крайне рекомендуется);
* Доступ в сеть интернет (80/443 порты), поскольку MyBee будет скачивать (единоразово) gold-образы для тех ОС, окружение которых вы запросили.

Управление по созданию окружениями осуществляется через RestAPI (например, достаточно командной строчки и утилиты curl), а также посредством тонкого клиента, доступного под современные ОС. В перспективе - поддержка Terraform и мобильное приложение. 

Components:

* [FreeBSD OS](https://www.freebsd.org) 13.1+
* [CBSD](https://cbsd.io) 13.1+

## MyB Handbook

* [Получение и установка MyBee](ru/get-myb.md)
* [CLI/shell](ru/shell.md)
* [Configuring Network Interfaces](ru/network.md)
* [Сетевые профили](ru/netprofile.md)
* [Базовые эндпоинты API](ru/api.md)
* [ACL и безопасность](ru/acl.md)
* [Список GOLD образов](ru/images.md)
* [Работа с jail (curl)](ru/jail_curl.md)
* [Работа с ВМ (curl)](ru/bhyve_curl.md)
* [Работа с Kubernetes (curl)](ru/k8s_curl.md)
* [Работа с jail (CBSDfile)](ru/jail_cbsdfile.md)
* [Работа с ВМ (CBSDfile)](ru/bhyve_cbsdfile.md)
* [Работа с jail (nubectl)](ru/jail_nubectl.md)
* [Работа с ВМ (nubectl)](ru/bhyve_nubectl.md)
* Работа с jail (Terraform)
* Работа с ВМ (Terrafarm)
* [Поддержка](ru/support.md)
* [UI](ru/ui.md)
