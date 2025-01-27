# MyBee Quick-Start

Lassen Sie uns mit einem Minimum an Schritten einen Test-Gastcomputer erstellen. Es wird davon ausgegangen, dass MyBee bereits installiert ist und Sie über Shell auf das Menü zugreifen können: http://IP/shell/

1) Lassen Sie uns die ACL mithilfe des öffentlichen Schlüssels deaktivieren. Aufmerksamkeit! Mit diesem Setup kann jeder eine virtuelle Maschine erstellen. Verwenden Sie dies nur in „vertrauenswürdigen“ Umgebungen! Wählen Sie einen Artikel aus **'Configure Pubkey WhiteList'**:

![quick1.png.png](https://myb.convectix.com/img/quick1.png?raw=true)

Der Status sollte sich in „disabled“ ändern.

2) Dieser Schritt ist optional, es wird jedoch empfohlen, das Bild im Voraus aufzuwärmen – wir müssen sicherstellen, dass MyBee Zugriff auf die externen Spiegel hat. Wählen Sie den Punkt **'Shell (warmes Wolkenbild)'** und gelangen Sie in die Shell.

![quick2.png.png](https://myb.convectix.com/img/quick2.png?raw=true)

Wir schreiben „debian12“ und erhalten das neueste Linux Debian 12-Image

![quick3.png.png](https://myb.convectix.com/img/quick3.png?raw=true)


3) Erstellen Sie auf unserem lokalen Computer eine Datei mit einem beliebigen Namen „debian12.json“ und folgendem Inhalt:

```
{
  "imgsize": "12g",
  "ram": "1g",
  "cpus": 2,
  "image": "debian12",
  "pubkey": "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain"
}
```

Fügen Sie dabei im Pubkey-Feld eine Zeile aus Ihrer ~/.ssh/*.pub-Datei ein (ED25519-, ECDSA- und RSA-Schlüssel sind akzeptabel).

4) Mit dem Dienstprogramm „curl“ senden wir eine Anfrage zum Erstellen einer VM:

```
curl -X POST -H "Content-Type: application/json" -d @debian12.json http://IP/api/v1/create/vm1
```

Dabei ist IP die MyBee-Adresse.

![quick4.png.png](https://myb.convectix.com/img/quick4.png?raw=true)

5) Mit der Anfrage „curl -H cid:CID http://IP/api/v1/status/vm1“ mit der richtigen CID (Client-ID) warten wir, bis die Informationen für die Verbindung zur VM gedruckt werden. In der Regel ist eine VM innerhalb von ~10 Sekunden einsatzbereit, bei Verwendung einer HDD geht es in der Regel schneller

Hinweis: CID ist der MD5-Hash Ihres Schlüssels, in unserem Fall:

```
md5 -qs 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain'
190000ee6d0e18a82d6e79a34537a616
```

![quick5.png.png](https://myb.convectix.com/img/quick5.png?raw=true)

6) Mit Ihrem Schlüssel verbinden wir uns mit dem Gast.

![quick6.png](https://myb.convectix.com/img/quick6.png?raw=true)


---

**<<_**__[Zurück: MyBee installieren](get-myb.md)__ $~~~$ | $~~~$ __[Weiter: CLI/shell](shell.md)__**_>>**
