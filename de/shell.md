# CLI/shell

## Beschreibung

Sie können MyBee über die Administrationskonsole konfigurieren und verwalten. Verbinden Sie sich dazu per SSH auf Ihren Server.

Wenn Sie während der Installation einen unprivilegierten Benutzer angelegt heben, sollten Sie diesen für den Fernzugriff auf den Server benutzen. Verwenden Sie nach dem Einloggen den Befehl  `su -` auf der Kommandozeile und geben Sie das von Ihnen gewählte Passwort für den 'Root'-Benutzer ein, um zum Super-User zu wechseln und auf die MyBee-Konsole zuzugreifen.

Verbinden Sie sich stattdessen als Benutzer 'root', Wenn Sie keinen zusätzlichen Benutzer angelegt. In diesem Fall ist der Root-Zugriff per SSH erlaubt.

## MyBee-Menü

Wenn Sie als 'Root'-Benutzer angemeldet sind, steht Ihnen ein Menü mit Einträgen für typische Aktionen zur Verfügung. Es enthält beispielsweise Menüpunkte zum Ändern der Netzwerkeinstellungen, zum Wechseln der Profile, ACLs, usw.:

![mybcli](/images/mybconsole.png)

Um einen Menüpunkt auszuwählen, geben Sie die entsprechende Nummer ein und bestätigen mit der 'Eingabetaste'.

Je nach Typ der durchgeführten Installation, der Verfügbarkeit von Plugins oder dem Status von Einstellungen, können manche Menüpunkte inaktiv (ausgegraut) sein.

## MyB aktualisieren

Wenn Sie MyBee (einschließlich VM-Profile/Vorlagen) aktualisieren möchten, geben Sie an der Eingabeaufforderung „u“ ein und drücken Sie die Eingabetaste.
Der Prozess der Aktualisierung des gesamten Systems beginnt. Einige Updates erfordern möglicherweise einen Neustart des Knotens, damit die Änderungen wirksam werden.

![mybcli](/images/mybupgrade.png)

:bangbang: | :information_source: Bitte beachten Sie, dass die Aktualisierung von MyBee die Gastsysteme bereits erstellter virtueller Maschinen nicht aktualisiert. Mit einem MyBee-Update können Sie jedoch ein neues Profil eines zuvor fehlenden Betriebssystems (oder eine neue Version eines vorhandenen Profils) erhalten, wodurch ein neues Gold-Image für diesen Gast heruntergeladen wird, wenn das Profil aktiviert wird.
:---: | :---

---

Next: [Konfiguration der Netzwerkschnittstellen](network.md)
