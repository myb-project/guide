# Netzwerkprofile

## Beschreibung

Virtuelle Umgebungen sind für gewöhnlich für die Benutzer, die sie anlegen, kein Selbstzweck, sondern werden entworfen um eine bestimmte Funktion zu erfüllen, je nach den anstehenden Aufgaben. Da Benutzer unterschiedliche Aufgaben zu bewältigen haben sowie für die installierte Plattform unterschiedliche Voraussetzungen und Einschränkungen gelten, ergeben sich daraus auch unterschiedliche Anforderungen an die Netzwerkkonfiguration und die Virtuellen Maschinen. Beispielsweise verfügen einige Benutzer nur über einen Server mit einer einzelnen externen IPv4-Adresse. Andere haben ebenfalls nur eine externe IPv4-Adresse aber zusätzlich viele externe IPv6-Adressen. Wieder andere verfügen über mehrere externe IPv4-Adressen, die den Virtuellen Umgebungen zugewiesen werden müssen. Und so weiter - es gibt eine große Menge unterschiedlicher Variationen.

Um den Benutzern den komplexen Prozess zu ersparen, die Einstellungen des Hostsystems zu verändern, bietet das MyBee-Projekt eine Anzahl von Konfigurationen als benannte Gruppen - Profile -, von denen der Benutzer die für den jeweiligen Anwendungszweck am besten passende auswählen kann.

Profile beeinflussen, welche Adressen und Routing-Informationen beim Anlegen von Virtuellen Maschinen und Kubernetes-Clustern. Sie können die Profile beliebig oft wechseln (und dadurch MyBee umkonfigurieren). Die einzige Einschränkung dafür ist, daß zuvor alle Virtuellen Umgebungen, die unter dem alten Profil erstellt wurden zerstört werden müssen. Basierend auf Rückmeldungen aus der Nutzerschaft werden die Anzahl der verfügbaren Profile und ihre Vielfalt weiter steigen.

## Profilwechsel

Um das aktive Profil zu wechseln, wählen Sie den Menüpunkt 'Change VM network profile':

![net](https://user-images.githubusercontent.com/926409/163693214-04a0f579-e36c-44d2-a877-2b790f90291d.png)

Nicht alle Profile können ausgewählt werden, sofern nicht gewisse Voraussetzungen erfüllt sind. Beispielsweise kann kein Profil aktiviert werden, welches IPv6-Adressen für die Virtuellen Umgebungen vorsieht, sofern die Hostmaschine selbst nicht für die Verwendung von IPv6 konfiguriert ist.

Profile werden sofort aktiv, nachdem die entsprechende Profilnummer eingegeben und die Auswahl mit der 'Enter'-Taste bestätigt wurde.

![net](https://user-images.githubusercontent.com/926409/163694702-63a21af7-aaea-4d68-a97f-dd0e55b360bf.png)

Im Folgenden werden die Profile und möglichen Netzwerktopologien beschrieben.

## 1) Einzelne Netzwerkschnittstelle: private IPv4-Adresse(n)

Dies ist das Standardprofil. Es ist gut geeignet für eine klassische Installation, wenn Sie einen Server haben, dem nur eine einzige externe IPv4-Adresse zur Verfügung steht. Die typische Netzwerkkonfiguration ist das Verwenden von IPv4-Adressen aus dem privaten Adressraum (rfc1918), wobei die Verbindung ins Internet über NAT erfolgt und bestimmte Dienste bzw. Ports in der VM mittels Portweiterleitung über die externe IPv4-Adresse des Hostsystems erreichbar gemacht werden.

Dieses Profil deckt zwei Anwendungsfälle ab:

Mapping / Weiterleitung 1: 1 zusätzliche externe IPv4-Adresse zu einer der Virtuellen Maschinen, elastische IP. Damit startet die Virtuelle Maschine mit einer privaten IPv4-Adresse, aber die der VM zugeordnete externe IPv4-Adresse stellt sicher, daß jeder Aufruf eines Ports der externen Adresse über die Protokolle TCP oder UDP durch die gleichen Ports an die Adresse der Virtuellen Maschine weitergeleitet werden. Darüberhinaus verwendet ausgehender Verkehr der VM die dedizierte Adresse als ein NAT, nicht die geteilte Adresse.

## 2) Mehrere Netzwerkschnittstellen: private IPv4-Adresse(n)

Das zweite klassische Profil. Eine Installation gleich des ersten Profils, aber zusätzlich zu einer einzelnen IPv4-Adresse (oder einer sehr beschränkten Anzahl externer IPv4-Adressen) wird der Host konfiguriert, um IPv6-Adressen zu nutzen. In diesem Fall werden Virtuelle Umgebungen mit zwei virtuellen Netzwerkschnittstellen erstellt, wobei die erste gleich wie bei Profil 1 funktioniert: Eine IPv4-Adresse aus dem privaten (rfc1918) Netzwerk wird zugewiesen. Der Zugang zum Internet erfolgt über NAT, wenn mit dem IPv4-Stack gearbeitet wird, wobei einzelne Dienste bzw. Ports über die interne IPv4-Adresse umgeleitet werden können, wozu freie Ports der externen Adresse des Hosts verwendet werden. Die zweite Schnittstelle wird konfiguriert und erhält eine externe IPv6-Adresse und einen separaten 'Default Router' mit Internetzugang wenn der IPv6-Stack verwendet wird.

Sofern Ihre Arbeitsstation oder andere Ressourcen das IPv6-Protokoll verwenden, können Sie in diesem Fall mit der Virtuellen Umgebung direkt über die externe Adresse Arbeiten, ohne irgendwelche Um- oder Weiterleitungen nutzen zu müssen.

---

Next: [Grundlegende API-Endpunkte](api.md)
