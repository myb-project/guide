# MyBee - Die einfachste API zum Erstellen und Löschen von K8S & Cloud-VMs

(Dies ist eine deutsche Übersetzung aus dem Englischen. Im Falle von Abweichungen hat die englische Anleitung Vorrang und die deutsche Version ist als veraltet zu betrachten.)

<p align="center">
  <a href="/README.md">English</a> |
  <a href="/README.es.md">Español</a> |
  <a href="/README.fr.md">Français</a> |
  <span>Deutsch</span> |
  <a href="/README.ru.md">Русский</a> |
  <a href="/README.ch.md">中文</a> |
</p>

---

:information_source: Diese Anleitung behandelt Administration und Verwendung der [MyBee](https://myb.convectix.com)-Distribution (**MyB**) für virtuelle Cloud-Umgebungen basierend auf dem [Bhyve](https://en.wikipedia.org/wiki/Bhyve)-Hypervisor. Diese Distribution ist kostenlos verfügbar ohne irgendwelche Einschränkungen der Anwendungszwecke.

---

MyBee ist eine Software zum Arbeiten mit Virtuellen Umgebungen über eine sehr einfache API. Sie ist in erster Linie zur Verwendung in einer sicheren ("trusted") Umgebung gedacht und/oder zu Integration bzw. Aufbau einer privaten Cloud, sowie zum Aufbau Ihres eigenen *Hyperconverged Clusters*.

MyBee wird als Satellitenprojekt (als eine Art Demo für eines der Ziele) des nichtkommerziellen [CBSD](https://github.com/cbsd/cbsd)-Projektes entwickelt. Es verwendet derzeit Image-Bibliotheken sowie Infrastruktur, die mit Mitteln aus Spenden der [CBSD-Unterstützer](https://www.patreon.com/clonos) aktuell gehalten werden, also eine der Ergebnisse der Projektfinanzierung darstellen.

Betriebssysteme und Distributionen, deren Betrieb getestet wurde (weitere in dieser Liste nicht aufgeführte sind verfügbar):

- **Linux**: [Ubuntu](https://ubuntu.com/), [Debian](https://www.debian.org/), [CentOS](https://www.centos.org/), [Rocky](https://rockylinux.org/), [Oracle Linux](https://www.oracle.com/linux/) usw.;
- **BSD**: [FreeBSD](https://www.freebsd.org/), [OpenBSD](https://www.openbsd.org), [NetBSD](https://netbsd.org), [DragonflyBSD](https://dragonflybsd.org) usw.;
- **Windows**: [Windows server](https://www.microsoft.com/en-us/windows-server);
- **Other**: [SmartOS](https://www.joyent.com/smartos), [Android x64](https://www.android-x86.org/) usw.;

MyBee stellt dabei nur eine API bereit; wenn Sie auf der Suche nach einem Web-Interface sind, um mit Bhyve oder Jails zu arbeiten, sehen Sie sich einmal das [ClonOS](https://clonos.convectix.com/)-Projekt an, welches ebenfalls ein vollständig quelloffenes und permissiv lizenziertes (BSD-Lizenz) Satellitenprojekt ist.

Wenn Sie einen grafischen Client verwenden möchten, schauen Sie sich um: [MyBee-QT](https://github.com/myb-project/mybee-qt/);

## Übersicht über MyBee

Wenngleich MyBee jegliche gewünschten ISO-Abbilder zur Installation verwenden kann, ist das Projekt in erster Linie darauf ausgelegt, mit Cloud-Abbildern zu arbeiten. Ein wichtiger Teil von MyBee sind die Provisionierungswerkzeuge um leicht Ihre eigenen Cloud-Abbilder mit einer beliebigen Liste an Anwendungen erstellen zu können. Ein anderer Schwerpunkt liegt auf der Erweiterkeit der API.

Die hohe Geschwindigkeit bei der Initialisierung der Virtuellen Infrastruktur bei Bedarf macht MyBee als Basis zum Aufbau von SaaS- und FaaS-Plattformen. Die Durchschnittszeiten zum Erstellen und Hochfahren von Umgebungen bis zu dem Punkt, an dem sie in der Lage sind, entfernte Verbindungen zuzulassen, sehen beispielsweise so aus:

1) Erstellen einer VM beliebiger Ausstattung (RAM/vCPU/Storage) und Hochfahren derselben wird in ~5 Sekunden bewerkstelligt; Abhängig vom Tuning der Bootparameter und der Bootgeschwindigkeit des Gastsystems, kann die VM nach gerade einmal ~35 Sekunden in der Lage sein, RDP- oder SSH-Verbindungen anzunehmen (ohne den `GRUB`-Bootloader-Timeout anzupassen) [^1]

![vmup1](https://user-images.githubusercontent.com/926409/165381489-f7a83818-ef09-4d3c-8044-8f91bab488bb.png)

2) Ersteuuen und Hochfahren eines Kubernetes-Clusters mit der Fähigkeit, API-Anfragen anzunehmen: ~30 Sekunden für eine Single-Master-Konfiguration und ~1 Minute und 20 Sekunden für ein Cluster mit einer beliebigen Anzahl von Master- und Worker-Knoten. [^1], [^2]

[^1]: - unter der Voraussetzung, daß kein Overcommitment der physischen Hardware vorliegt;
[^2]: - 30 Sekunden werden für das Bootstrapping des `etcd`-Dienstes benötigt, wenn Anzahl der Master-Knoten > 1;

![kubetime](https://user-images.githubusercontent.com/926409/165322452-96f740bb-d7af-4970-affc-056432a17c46.png)

Die Software ist komplett aus alternativen Technologien aufgebaut, deren Quellcode unter den sehr permissiven BSD- bzw. MIT-Lizenzen steht; keine der Komponenten oder Schichten ist mit irgendeiner Firma verbunden. Tatsächlich baut MyBee vollständig auf Open-Source-Projekten auf:

- Das wunderbare Betriebssystem [FreeBSD](https://www.freebsd.org);
- Der hochperformante Hypervisor [Bhyve](https://en.wikipedia.org/wiki/Bhyve);
- Der [NETMAP](https://man.freebsd.org/netmap/4)/[VALE](https://man.freebsd.org/vale/4) Virtual Switch;
- Der [CBSD](https://github.com/cbsd/cbsd) Virtual Environment manager;

## Systemvoraussetzungen

Jeder beliebige physikalische ("bare metal") Server mit einem Intel/AMD x86-64 Prozessor, der Virtualisierung und die POPCNT Instruktionen unterstützt, ist für den Einsatz mit MyBee geeignet, wenn darauf FreeBSD 13.2-RELEASE läuft.

Theoretisch ist MyBee auch auf der [ARM64-Architectur lauffähig](https://github.com/freebsd-upb/freebsd-src/tree/projects/bhyvearm64). Jedoch verfügt die CBSD-Serverinfrastruktur derzeit über keine ARM64-basierten Servers. Die Arbeit an einem solchen Port kann jedoch beginnen, wenn solche Hardware dem Projekt zur Verfügung gestellt werden sollte.

Unter [gewissen Umständen](https://wiki.freebsd.org/bhyve#Q:_Can_I_run_multiple_bhyve_hosts_under_VMware_nested_VT-x_EPT.3F) kann MyBee selbst in einer Virtuellen Umgebung betrieben werden; dies wird jedoch nicht empfohlen und wurde durch die MyBee-Entwickler nicht getestet. In Fällen, in welchen der Bhyve-Hypervisor nicht benutzt werden kann, können dennoch Container erstellt werden, die auf der Technologie von FreeBSD-Jails basieren - jedoch ist dies vermutlich nicht das, was sie von MyBee erwarten. ;-)

* Minimale Anforderungen Hauptspeicher: 4 GB RAM;
* Minimale Anforderungen HDD-/SSD-Speicher: 20 GB (für Kubernetes-Cluster wird die Verwendung von SSD/NVME stark empfohlen);
* Verfügbarkeit von IPv4;
* Vorhandensein von IPv6 (optional, aber stark empfohlen);
* Internetverbindung (Ports 80/443), da MyBee (pro Abbild natürlich einmalig) die "gold-images" für die Betriebssysteme herunterläd, die für eine Umgebung ausgewählt wurden;

Die Erstellung von Virtuellen Umgebungen wird über eine REST-API durchgeführt (d.h. die Kommandozeile und das Werkzeug `curl` sind völlig ausreichend, um MyBee zu kontrollieren!). Alternativ steht eine kleine Client-Applikation zur für moderne Desktop-Betriebssysteme zur Verfügung. In Zukunft könnte zusätzlich Unterstützung für Terraform und für eine Mobil-Applikation verfügber werden.

Komponenten:

* [FreeBSD Betriebssystem](https://www.freebsd.org) 14.2+
* [CBSD](https://github.com/cbsd/cbsd) 14.2+

## MyBee Handbuch

* [Installation von MyBee](de/get-myb.md)
* [CLI/shell/Aktualisierung](de/shell.md)
* [Konfiguration der Netzwerkschnittstellen](de/network.md)
* [Netzwerkprofile](de/netprofile.md)
* [Basic API endpoints](en/api.md) (noch nicht übersetzt)
* [ACLs and Security](en/acl.md) (noch nicht übersetzt)
* [List of GOLD images](en/images.md) (noch nicht übersetzt)
* [API FQDN and certbot](en/api_fqdn_certbot.md) (noch nicht übersetzt)
* [Working with jail (curl)](en/jail_curl.md) (noch nicht übersetzt)
* [Working with VM (curl)](en/bhyve_curl.md) (noch nicht übersetzt)
* [Working with Kubernetes (curl)](en/k8s_curl.md) (noch nicht übersetzt)
* [Working with jail (nubectl)](en/jail_nubectl.md) (noch nicht übersetzt)
* [Working with VM (nubectl)](en/bhyve_nubectl.md) (noch nicht übersetzt)
* [Working with jail (CBSDfile)](en/jail_cbsdfile.md) (noch nicht übersetzt)
* [Working with VM (CBSDfile)](en/bhyve_cbsdfile.md) (noch nicht übersetzt)
* Working with jail (Terraform) (noch nicht übersetzt)
* Working with VM (Terrafarm) (noch nicht übersetzt)
* [Support](en/support.md) (noch nicht übersetzt)
* [UI](en/ui.md) (noch nicht übersetzt)


## Sponsor this project

Wenn Ihnen dieses Projekt gefällt, möchten Sie sich bedanken und uns unterstützen:

<a href="https://www.patreon.com/clonos"><img src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" alt="Patreon donate button" /></a>
