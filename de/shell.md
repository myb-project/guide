# CLI/shell

## Beschreibung

Sie können MyB konfigurieren oder verschiedene Verwaltungsvorgänge über die Administratorkonsole ausführen, indem Sie sich über SSH beim MyB-Server oder über einen Browser bei der WS-Konsole anmelden: http://IP/shell/

## MyB-Menü

Wenn Sie sich als „Root“-Benutzer anmelden, haben Sie Zugriff auf ein Menü für typische Vorgänge, wie zum Beispiel: Ändern von Netzwerkeinstellungen, Wechseln von Profilen, ACLs usw.:

![mybconsole.png](https://myb.convectix.com/img/mybconsole.png?raw=true)

Verwenden Sie zum Navigieren die Ziffern- und/oder Pfeiltasten. Die Menüauswahl erfolgt durch Drücken der „Enter“-Taste.

Bei der ersten Installation sind die beliebtesten Vorgänge die folgenden:

* 1) - Aktualisieren von MyBee durch Drücken der Taste „u“;

:bangbang: | :information_source: Bitte beachten Sie, dass durch die Aktualisierung von MyBee keine Gastsysteme bereits erstellter virtueller Maschinen aktualisiert werden. Mit einem MyBee-Update können Sie jedoch ein neues Profil für ein zuvor fehlendes Betriebssystem (oder eine neue Version eines vorhandenen Profils) erhalten, was dazu führt, dass bei der Aktivierung des Profils ein neues Gold-Image für diesen Gast heruntergeladen wird.
:---: | :---

* 2) - Beenden Sie die Unix-Shell. Abhängig von der Distribution handelt es sich entweder um Linux oder FreeBSD OS. In der Unix-Shell können Sie auch Cloud-Bilder „aufwärmen“ (siehe unten);
* 3) - Erstellen eines separaten Benutzers außer „root“, mit dem Sie sich über SSH anmelden und in dessen Namen Sie eine VM erstellen können (über SSH und API);
* 4) - Anpassen der ACL von IP-Adressen und/oder Pubkey-Listen, die mit der API interagieren können;
* 5) - Blockieren Sie den Zugriff des Benutzers „root“ über SSH (jedoch nicht über http://IP/Shell/ oder lokale Anmeldung) – dies sollte nach dem Erstellen des Benutzers durch Schritt (4) erfolgen;
* 6) - das Farbschema wechseln oder ausschalten;

## Aufwärmen von Wolkenbildern

Beitragsprofile, die mit MyBee geliefert werden, enthalten Informationen über das Gastbetriebssystem, Mindestanforderungen und einen Link, um ein Gold-Image zu erhalten (aber nicht die Images selbst), die sich auf Projektspiegeln befinden und regelmäßig vom CBSD-Projekt aktualisiert werden;

Wenn Sie nicht über genügend vorhandene Profile verfügen und/oder diese nicht zu Ihnen passen, können Sie beliebig viele eigene Profile erstellen und diese sogar in einem Profilprojekt auf GitHub hochladen – in diesem Fall können alle anderen MyBee-Benutzer Ihr Profil verwenden Profil;

Wenn Sie die Erstellung eines Gastes anfordern, beginnt das System automatisch mit dem Herunterladen des Images von den Spiegeln, was bei der ersten Erstellung zu einer unbestimmten Verzögerung von mehreren Sekunden bis zu mehreren Minuten führt – dies hängt von der Geschwindigkeit Ihrer Netzwerkverbindung und der Verfügbarkeit ab
Spiegel für Ihre Instanz.

Um sicherzustellen, dass virtuelle Maschinen schnell empfangen werden, empfiehlt es sich daher, das Image zunächst über die CLI zu beziehen (Aufwärmen). Nutzen Sie dazu im Hauptmenü das Menü „Shell (warmes Wolkenbild)“ und informieren Sie sich darüber
Bilder fehlen – holen Sie sie sich.

![mybconsole2.png](https://myb.convectix.com/img/mybconsole2.png?raw=true)

Wenn das System neben dem Profilnamen „nicht gefunden“ schreibt, können Sie durch Eingabe des Profilnamens das erforderliche Bild abrufen und zwischenspeichern. Wenn Sie dies nicht tun, wird das System dies beim Erstellen automatisch tun. In diesem Fall ist dies jedoch möglich
Stellen Sie sicher, dass keine Probleme mit dem Netzwerk vorliegen, andernfalls kann die Erstellung der VM für unbestimmte Zeit „einfrieren“.

![mybconsole3.png](https://myb.convectix.com/img/mybconsole3.png?raw=true)

### wenn die Geschwindigkeit der Spiegel langsam ist

Wenn Sie möchten, können Sie Ihren eigenen lokalen Spiegel einrichten, der sich mit den offiziellen CBSD-Spiegeln synchronisiert und MyBee dazu zwingt, diese zu verwenden. Wenn es in Ihrer Region keine CBSD-Spiegel gibt und Sie dem Projekt helfen möchten,
Sie können auch einen Link zu Ihren Spiegeln veröffentlichen und andere CBSD/MyBee-Benutzer werden damit beginnen, sie zu verwenden. Lesen Sie mehr darüber [hier](https://github.com/cbsd/mirrors).

---

Weiter: [Configuring Network Interfaces](network.md)
