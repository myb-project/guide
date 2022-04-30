# CLI/shell

## Beschreibung

Sie können MyBee über die Administrationskonsole konfigurieren und verwalten. Verbinden Sie sich dazu per SSH auf Ihren Server.

Wenn Sie während der Installation einen unprivilegierten Benutzer angelegt heben, sollten Sie diesen für den Fernzugriff auf den Server benutzen. Verwenden Sie nach dem Einloggen den Befehl  `su -` auf der Kommandozeile und geben Sie das von Ihnen gewählte Passwort für den 'Root'-Benutzer ein, um zum Super-User zu wechseln und auf die MyBee-Konsole zuzugreifen.

Verbinden Sie sich stattdessen als Benutzer 'root', Wenn Sie keinen zusätzlichen Benutzer angelegt. In diesem Fall ist der Root-Zugriff per SSH erlaubt.

## MyBee Menü

Wenn Sie als 'Root'-Benutzer angemeldet sind, steht Ihnen ein Menü mit Einträgen für typische Aktionen zur Verfügung. Es enthält beispielsweise Menüpunkte zum Ändern der Netzwerkeinstellungen, zum Wechseln der Profile, ACLs, usw.:

![image](https://user-images.githubusercontent.com/926409/163887427-850cc699-273d-4d28-b765-c7e6ba59cbde.png)

Um einen Menüpunkt auszuwählen, geben Sie die entsprechende Nummer ein und bestätigen mit der 'Eingabetaste'.

Je nach Typ der durchgeführten Installation, der Verfügbarkeit von Plugins oder dem Status von Einstellungen, können manche Menüpunkte inaktiv (ausgegraut) sein.

---

Next: [Konfiguration der Netzwerkschnittstellen](network.md)
