# Installation von MyBee

## Installation von ISO-Abbild auf einem dedizierten Server

Abbilder eines Installationsmediums können auf der offiziellen Webseite [unter Downloads](https://myb.convectix.com/download/) heruntergeladen werden. Die Installation erfolgt mittels des üblichen FreeBSD-Installationsprogramms  `bsdinstall` und die MyBee-Installation unterscheidet sich nicht vom gewöhnlichen [FreeBSD-Installationsprozess](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-start).

:bangbang: | :warning: Achtung! Wenn Sie bei der Installation von MyBee KEINEN zusätzlichen (unprivilegiertes) Benutzer anlegen, sollten Sie darauf achten, ein starkes Passwort für den 'root'-Benutzer zu vergeben, da in diesem Fall standardmäßig der Root-Zugang über SSH erlaubt wird (Sie können diesen Zugang über die Administrationskonsole sperren).
:---: | :---


## Installation über FreeBSD-Rettungssystem (z.B. von Hetzner)

:bangbang: | :warning: Eine Warnung vorweg: Diese Anleitungen überschreiben die vorhandenen Daten auf den Festplatten des Rechners, auf dem MyBee installiert wird.
:---: | :---

1) Booten Sie zuerst Ihren Hetzner-Server in den Rettungsmodus. Leider hat sich das Hetzner-Team entschieden, ab 2022-06 die Bereitstellung einzustellen
Rettungsmodus mit FreeBSD, damit Sie über FreeBSD/MyBee über mfsBSD installieren können. Nämlich: Wählen Sie das übliche Linux in den Rescue-Registerkarten aus:


![mybee_hz1.png](/images/mybee_hz1.png)

Merken Sie sich das Passwort, setzen Sie den Server zurück und melden Sie sich über SSH beim Server an, wenn er bereit ist. Als nächstes holen Sie sich das mfsBSD-Image und schreiben es auf das erste (/dev/sda) Laufwerk.
Wenn Sie nicht sicher sind, ob der Server immer von der ersten Festplatte bootet, können Sie das Image nur für den Fall auf alle Festplatten schreiben:

```
wget https://myb.convectix.com/DL/mfsbsd-13.1.img
dd if=mfsbsd-13.1.img of=/dev/sda bs=4M
dd if=mfsbsd-13.1.img of=/dev/sdb bs=4M
sync && shutdown -r now
```

![mybee_hz1a.png](/images/mybee_hz1a.png)

2) Melden Sie sich nach dem Neustart des Servers über SSH als Benutzer 'root' und Passwort 'mfsroot' am Server an. Sobald Sie Zugriff haben, verwenden Sie den Befehl 'fetch', um das Installationsskript abzurufen:

```
fetch https://myb.convectix.com/auto
```

Das Skript ist ein `sh`-Skript, so daß Sie es einfach mit der '/bin/sh'-Shell ausführen können:

```
sh auto
```

![mybee_hz2.png](/images/mybee_hz2.png)


3) Wenn Sie mit `bsdinstall` vertraut sind, werden Ihnen die folgenden Schritte bekannt vorkommen. Das weitere Vorgehen entspricht einer normalen FreeBSD-Installation. Der einzige Unterschied besteht darin, daß nach Abschluß des Skripts Sie wieder in der Shell des Hetzner-Rettungssystems landen - starten Sie nun einfach den Server mit dem `shutdown -r now`-Befehl neu.

## MyB/FreeBSD-Installationsprogramm

:information_source: Wenn Sie mit dem Installatinsprozess von FreeBSD und `bsdinstall` vertraut sind, können Sie [den Rest des Kapitels überspringen](shell.md).

Stellen Sie sicher, daß Sie den vollen Hostnamen ("fully qualified name", FQDN) Ihres Servers angeben:

![mybee_hz3](https://user-images.githubusercontent.com/926409/163675559-4ceb5b37-b5cf-4421-9632-aee829c4a855.png)

MyBee setzt auf die besonderen Fähigkeiten und die Stabilität von von ZFS - daher wurde die Option, auf UFS zu installieren, deaktiviert:

![mybee_hz4](https://user-images.githubusercontent.com/926409/163675561-135cc875-142e-4610-9c22-6506bb8325d9.png)

Wählen Sie Ihren RAID-Typ sorgfältig aus, je nachdem, wie viele Laufwerke Ihnen zur Verfügung stehen und was Ihre Ziele sind.

![mybee_hz5](https://user-images.githubusercontent.com/926409/163675562-29b2cffc-d658-4db5-8ccb-3599dd4980e8.png)
![mybee_hz6](https://user-images.githubusercontent.com/926409/163675563-eb5b3bb4-0dde-403f-a97a-9efbe30504ac.png)

In unserem Beispiel haben wir nur zwei Platten, die im vorigen Schritt als *Stripes* konfiguriert wurden, um maximalen Plattenplatz unter Verzicht auf jegliche Redundanz für den Fall, daß eine der Platten ausfällt, zu erhalten:

![mybee_hz7](https://user-images.githubusercontent.com/926409/163675564-2ebfd4d9-337a-4f54-8d6b-6fb1124e1890.png)

Letzte Chance, es sich noch anders zu überlegen!

![mybee_hz8](https://user-images.githubusercontent.com/926409/163675565-afd6a60c-9af2-43b2-8ebd-603f4a979975.png)

Vergeben Sie ein starkes Passwort für den 'root'-Benutzer. Wenn Sie im folgenden Schritt darauf verzichten, einen zusätzlichen Benutzer anzulegen, wird MyBee standardmäßig den entfernten Zugriff für 'root' über SSH erlauben.

![mybee_hz9](https://user-images.githubusercontent.com/926409/163675566-fc65fee4-782c-46a4-a097-8ee1e0d5e18a.png)

Netzwerkschnittstelle auswählen und konfigurieren.

![mybee_hz10](https://user-images.githubusercontent.com/926409/163675543-1ea23001-9a67-4fbc-a329-c48d13f5fead.png)

Eine IPv4-Adresse wird benötigt:

![mybee_hz11](https://user-images.githubusercontent.com/926409/163675545-5ad1f06e-c2c2-43d7-ab18-2b8ecc072981.png)


![mybee_hz12](https://user-images.githubusercontent.com/926409/163675546-fd344806-6ddf-437e-9e9f-300994c6754f.png)
![mybee_hz13](https://user-images.githubusercontent.com/926409/163675547-8b6256b3-2e15-4a4e-9036-6aae1ed9253e.png)

Wenn keine DNS-Server konfiguriert werden, wird MyBee nicht in der Lage sein, die Gold-Images für die Betriebssysteme herunterzuladen. Sofern Sie keine bevorzugten DNS-Server haben, können Sie z.B. die öffentlichen Nameserver '8.8.8.8' und '8.8.4.4' von Google verwenden:

![mybee_hz14](https://user-images.githubusercontent.com/926409/163675549-1417a25c-fff1-4189-b94c-743b97bc98fd.png)

Wählen Sie die für Sie zutreffende Zeitzone aus:

![mybee_hz15](https://user-images.githubusercontent.com/926409/163675550-22527c00-ded5-4d9f-af68-816197602e0e.png)
![mybee_hz16](https://user-images.githubusercontent.com/926409/163675551-b7446919-20d7-4c96-86a1-b332d8b81ef8.png)

Sie können nun einen unprivilegierten Benutzer für den Fernzugriff auf Ihren Server einrichten:

![mybee_hz17](https://user-images.githubusercontent.com/926409/163675552-0bb4dd4d-6104-45f5-be4d-4ecaff00c41b.png)

:bangbang: | :warning: Stellen Sie sicher, daß Sie den Benutzer, den Sie erstellen, der 'wheel'-Gruppe hinzufügen, da Sie sonst nicht in der Lage sein werden, mittels des `su`-Befehls zum 'root'-Benutzer zu wechseln!
:---: | :---

![mybee_hz18](https://user-images.githubusercontent.com/926409/163675553-98c8eee6-c966-489c-a9a3-5c30d4561478.png)

MyBee ist nun auf dem System installiert. Verlassen Sie `bsdinstall`, um die Installation abzuschließen:

![mybee_hz19](https://user-images.githubusercontent.com/926409/163675554-10af0f73-d95e-49d2-b041-0c61ef16c334.png)

Wählen Sie 'no' im letzten Dialogfenster, um `bsdinstall` zu beenden und, etwa im Hetzner-Fall, zum Rettungssystem zurückzukehren:

![mybee_hz20](https://user-images.githubusercontent.com/926409/163675558-72a96aca-b7cf-4c0a-97c7-23e719e09abd.png)

Alles, was nun noch zu tun ist, ist einen Neustart mittels des Befehls `shutdown -r now` durchzuführen. Wenn Sie von der ISO installieren, bietet Ihnen der Installer eine Neustart-Option an.

Nach dem Neustart können Sie die IP-Adresse verwenden, um sich per SSH zu Ihrem Server zu verbinden, oder sie in einem Web-Browser eingeben, um Einführungsinformationen zu erhalten: http://IP-Adresse-ihres-Servers .


---

Next: [CLI/shell](shell.md)
