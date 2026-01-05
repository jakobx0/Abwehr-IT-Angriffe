#### Network analysis
- erkennen von Netzwerkanomalien
	- z.B lang andauernde ssh Verbindung 
#### Data Driven Network Analysis
- Ziel: Daten sammeln
- wichtige daten rausfilten und Bedrohungen erkennen
	- Auswertung der Massendaten ist ein großes Problem
#### Netzwerksensoren
- informationsüberwachung -> Messystellen (Sensoren)-> greifen Daten ab und speichern
- z.B: Netzwerkmitschnitte, Log Files
- drei Gesichtspunkte:
	- Vantage(Blickrichtung)- Platzierung des Sensors innerhalb  eines Netzwerks
	- Domain (Arbeitsgebiet) - was wird überwacht - ein Host, ein Client, ein Service
	- Action (Maßnahmen) - was tut der Sensor - nur loggen, aktiv melden, in den Netzverkehr eingreifen
#### Vantage
![[Pasted image 20250108142222.png]]
 jeder Sensor sieht etwas anderes -> siehe Graph
An einer Kante sehen Sensoren nur den Verkehr auf dem Weg. An einem Knoten sehen Sensoren Verkehr aus mehreren Wegen. An einem Endpunkt -> nur für diesen bestimmten verkehr

Beispiel:
- A: sieht den ausgehenden und eingehenden Traffic, aber nicht den Traffic von 128.2.1.1 und 128.1.1.1
- B: sieht alles zwischen Router und Switch
- C: ist überflüssig
- D: sieht alles zwischen 128.1.1.1 und dem Hub
- E: überwacht das selbe wie F
- F: überwacht das selbe wie E
- G: schneidet nur http mit -> überwacht Server
- H: Überwacht alles zwischen Hub und 128.1.1.3 - 32 

- A-E-G-H -> beste alternative
#### Netzwerksensoren vs Servicesensoren
- Service Sensoren sind oft der einzige Beweis, dass etwas stattgefunden hat.
- Netzwerksensoren können oft nur Indizien geben.
Actions von Sensoren:
- **Report**: Liefert einfach Informationen über alles was der Sensor beobachtet. Reporting Sensoren bilden das Fundament der Überwachung und sind geeignet Angriffe zu Fingerprinten. Beispiele: NetFlow, tcpdump, Serverlogs
- **Event**: Vereinigen Beobachtungen zu Events. Vereinzeln also die Daten. Wird eine bestimmte Bedingung erfüllt lösen Sie ein Ereignis aus
- **Control**: sammeln Daten -> Bewertung -> modifizieren oder blockieren Netzwerkverkehr
![[Pasted image 20250201221533.png]]
R -> Report | E -> Event | C -> Control

Das Problem mit Netzwerksensoren ist die ungeheure Datenmenge die anfällt, dabei sind viele Daten nicht interpretierbar. Netzwerksensoren sind jedoch auch die beste (einzige) Methode blind Spots im Netzwerk zu finden. Netzwerksensoren liefern Ergebnisse mit nur minimalen Annahmen.
#### <mark style="background: #FF5582A6;">Log Dateien</mark>
- sind mitgeschnittenem Netzwerktraffic häufig vorzuziehen
- haben Event gespeichert, dass aus Netzwerkmittschnitten rekonstruiert wurde
	- Formate nicht einheitlich
	- bekannt sein dass und wo es existiert
	- Logdateien sind oft für Debugging-Zwecke ausgelegt und enthalten nicht immer alle sicherheitsrelevanten Informationen​
- BS schreiben viele log Dateien (Unix ) -> /var/log

- **Dokumentation von Aktivitäten:** Protokollieren von Zugriffen, Systemänderungen und Netzwerkereignissen.
- **Nachweisbarkeit:** Dienen als Beweismittel bei Sicherheitsvorfällen oder rechtlichen Fragen.
- **Analyse:** Unterstützung bei der Erkennung von Angriffen, Fehlerbehebung und Performance-Monitoring.
##### **gute Log-Message:**
- Descriptive (Beschreibend)
- Relatable (kann in Beziehung gesetzt werden)
- Complete (Vollständig)
**Beispiel: Nicht Descriptive**
- Mar 29 11:22:45:221 myhost sshd[213]: Failed login attempt
**Beispiel: Mehr Descriptive durch Parameter:**
- Mar 29 11:22:45.221 myhost (192.168.2.2) sshd[213]: Failed login attempt from host 192.168.3.1 as 'admin‘, incorrect password

Gute Überprüfung für die Qualität eines Logeintrags, mit den 6 W's:
Wer? Was? Wann? Wo? Wie? Warum?
**Beispiel: Nicht Relatable**
- Mar 29 11:22:45.221 myhost (192.168.2.2) myspamapp[213]: Message <21394.283845@spam.com> title 'Herbal Remedies and Tiny Cars‘ from 'spammer@spam.com' rejected due to unsolicited commercial content
**Beispiel: Besser Relatable**
- Mar 29 11:22:45.221 myhost (192.168.2.2) myspamapp[213]: Message <21394.283845@spam.com> title 'Herbal Remedies and Tiny Cars‘ from 'spammer@spam.com' at SMTP host 192.168.3.1:2034 rejected due to unsolicited commercial content

**Beispiel: Nicht sehr Complete:**
- Mar 29 11:22:45.221 myhost (192.168.2.2) myspamapp[213]:
	- Received Message <21394.283845@spam.com> title 'Herbal Remedies and Tiny Cars' from 'spammer@spam.com' at SMTP host 192.168.3.1:2034 
- Mar 29 11:22:45.321 myhost (192.168.2.2) myspamapp[213]: 
	- Message <21394.283845@spam.com> passed reputation filter 
- Mar 29 11:22:45.421 myhost (192.168.2.2) myspamapp[213]:
	- Message <21394.283845@spam.com> FAILED Bayesian filter
- Mar 29 11:22:45.521 myhost (192.168.2.2) myspamapp[213]:
	- Message <21394.283845@spam.com> Dropped
**Beispiel: Besser und Complete**
- Mar 29 11:22:45.521 myhost (192.168.2.2) myspamapp[213]:
	Received Message <21394.283845@spam.com> title
	'Herbal Remedies and Tiny Cars' from 'spammer@spam.com' at
	SMTP host 192.168.3.1:2034 reputation=pass Bayesian=FAIL decision=DROP
##### Auswertung
Bei der Auswertung verschiedener Log-Dateien müssen diese sinnvoll
zusammengefasst werden:
- Alle Zeitstempel nach Epoch Time konvertieren
- Sicherstellen, dass die Sensoren synchron sind.
- Markieren wo Informationen herkommen. Angeben des Flow-5- Tupel(Quell-IP, Ziel-IP, Quell-Port, Ziel-Port, Protokoll)
- Delimeter Nutzung (Zwischenzeichen in Logfiles Interpretierbar durch Logger oder änderbar durch Quellerfassung oder nicht?)
- Eigene Fehlercodes anstelle von Text verwenden

**Vorhandene Logfiles haben 3 Typen:**
- Spaltenweise (wie das CLF Format vom HTTP-D mit Trennzeichen)
	- Escaped Trennzeichen
	- Truncation
- Templated (unformatierter Text aus Textbausteinen zusammengesetzt)
	- Ist Liste aller Textbausteine verfügbar?
- Annotativ (Viele Einträge, die durch eine gleiche ID auf ein Event zeigen)
Klassische Parser schreiben der alles nach Spaltenweise konvertiert.

**Sensordaten:**
Die gewonnenen Sensordaten müssen abgelegt werden, so dass sie einfach
ausgewertet werden können. In der Regel ergeben sich 3 Möglichkeiten:
- Flat - einfach in einem Dateisystem
	- Einfach abzulegen
	- Schwer auszuwerten
- Datenbanken - wie Oracle, Postgres
	- Schwer abzulegen, Datenbanken sind nicht für Trafficdaten gemacht
	- Viel der Datenbankfunktionalität verpufft
- NoSQL / Big Data Konzepte - wie MongoDB, Hadoop, Monet, Redis, SOLR
	- Sehr mächtige Werkzeuge in der Auswertung großer Daten
	- Aufwändig einzurichten
##### **RDBMS vs. LogCollector**
Unterschied RDBMS - Create Read Update und Delete Data durch alle User an allen Stellen der Schnittstelle implementiert, mit deren Vor- und Nachteilen (CRUD- Paradigma, Geschwindigkeits- einbußen) Nutzung eines LogCollector, Sensoren nur schreibende Funktionalität (schnell) User nur lesende Funktionalität (langsam).

Ein **Log Collector** ist ein System oder ein Werkzeug, das zur Erfassung, Speicherung und Analyse von Logdaten eingesetzt wird. In der IT-Forensik und Netzwerksicherheit spielen Logdateien eine entscheidende Rolle, da sie eine dokumentierte Aufzeichnung von Ereignissen enthalten, die auf einem System oder innerhalb eines Netzwerks stattfinden.
##### **Funktionsweise eines Log Collectors**:
1. **Erfassung**: Ein Log Collector sammelt Logdaten aus verschiedenen Quellen wie Betriebssystemen, Anwendungen, Firewalls, Netzwerkgeräten oder Intrusion Detection Systems (IDS).
2. **Normalisierung**: Da Logeinträge oft in unterschiedlichen Formaten vorliegen, wandelt der Log Collector sie in ein einheitliches Format um.
3. **Speicherung**: Die gesammelten Logs werden zentral gespeichert, oft in einer dedizierten Datenbank oder einem Log-Management-System.
4. **Analyse & Filterung**: Log Collector-Systeme bieten oft Funktionen zur Echtzeit-Analyse, Mustererkennung oder Filterung relevanter Einträge.
5. **Alerting & Reporting**: Basierend auf definierten Regeln können Alarme oder Berichte erzeugt werden, um Administratoren auf ungewöhnliche oder sicherheitskritische Ereignisse aufmerksam zu machen​

Ein Log Collector ist somit ein essenzielles Werkzeug zur Erkennung und Analyse von Sicherheitsvorfällen und wird häufig in SIEM-Systemen (Security Information and Event Management) integriert.
#### **<mark style="background: #FF5582A6;">Netzwerksensor:</mark>**
- **Definition:** Ein Netzwerksensor überwacht den Datenverkehr in einem Netzwerksegment oder an einem zentralen Punkt, z. B. an einem Router oder Switch.
- **Einsatzort:**
    - An zentralen Netzwerkkomponenten (z. B. Router, Switches, Spanning-Ports).
    - Ideal für die Analyse des gesamten Netzwerkverkehrs (z. B. ein- und ausgehender Internet-Traffic oder Traffic innerhalb eines Segments).
- **Zweck:**
    - Erkennung von Anomalien, Bedrohungen oder Angriffen im Netzwerk.
    - Überwachung der Kommunikation zwischen Geräten.
    - Erfassung von Netzwerkprotokollen und -mustern (z. B. TCP, UDP, DNS, HTTP).
- **Beispiele:**
    - **Intrusion Detection Systems (IDS):** Sensoren, die eingehenden und ausgehenden Netzwerkverkehr analysieren (z. B. Snort).
    - **Traffic Monitoring Tools:** Erfassen Traffic-Metriken, z. B. Bandbreitennutzung.
#### **Hostsensor**
- **Definition:** Ein Hostsensor wird direkt auf einem Endgerät (Client, Server oder Workstation) installiert und überwacht Aktivitäten auf diesem Gerät.
- **Einsatzort:**
    - Direkt auf Endgeräten wie Workstations, Servern oder kritischen Infrastrukturen (z. B. Datenbanken, Domaincontroller).
- **Zweck:**
	- sitzen auf einem Host und beobachten die Aktivitäten dort, wie Logins, Dateizugriffe, .. Beispiele sind: IPS Tools wie Tripwire, HIPS, Logles
	- Logdateien
    - Überwachung lokaler Ereignisse wie Benutzeraktivitäten, Systemaufrufe und Dateioperationen.
    - Erkennung von Angriffen, die auf das Endgerät abzielen (z. B. Malware, unerlaubte Zugriffe).
    - Ergänzt die Netzwerksensoren durch detaillierte Informationen über das Verhalten einzelner Hosts.
- **Beispiele:**
    -  Installiert auf kritischen Servern oder Workstations (z. B. Admin-PCs)
    - Überwacht lokale Zugriffe, Prozesse und Änderungen an Dateien.
    
	##### **Servicesensor**
	- **Definition:** Ein Servicesensor ist eine Spezialform den Hostsensors und überwacht gezielt einen bestimmten Dienst (z. B. Webserver, Datenbank, Mailserver) auf einem Host.
	- **Einsatzort:**
	    - Direkt auf dem Gerät, das den Dienst bereitstellt (z. B. Server, Datenbank).
	    - Alternativ als Proxy-Sensor, der den Service-Traffic analysiert.
	- **Zweck:**
	    - Überwachung von Service-spezifischen Protokollen und Diensten (z. B. HTTP, FTP, SMTP).
	    - Analyse des Dienststatus, der Verfügbarkeit und der Performance.
	    - Erkennung von Dienst-spezifischen Bedrohungen, z. B. SQL-Injection oder Brute-Force-Angriffe auf FTP-Server.
	- **Beispiele:**
	    - **Webserver-Sensor:** Überwacht HTTP- und HTTPS-Traffic auf einem Webserver.
	    - **Mailserver-Sensor:** Analysiert SMTP- und IMAP-Traffic, um Spam oder Phishing zu erkennen.
	
	- **Netzwerksensoren** geben eine Übersicht über den Verkehr zwischen Netzwerkgeräten.
	- **Hostsensoren** bieten tiefere Einblicke in die Aktivitäten auf einzelnen Endgeräten.
	- **Servicesensoren** fokussieren sich auf die Sicherheit und Performance einzelner Dienste.

| **Kriterium**       | **Netzwerksensor**           | **Hostsensor**                    | **Servicesensor**                                    |
| ------------------- | ---------------------------- | --------------------------------- | ---------------------------------------------------- |
| Platzierung         | Im Netzwerk (Router, Switch) | Auf einem Host (Client, Server)   | Auf einem Dienst (z. B. Webserver)                   |
| Überwachungsbereich | Gesamter Netzwerkverkehr     | Lokale Aktivitäten auf einem Host | Service-spezifische Daten und Protokolle             |
| Einsatzbeispiele    | Traffic-Monitoring, IDS      | Malware-Überwachung, Logs         | Performance- und Sicherheitsüberwachung von Diensten |
| Detailgrad          | Mittel (Protokolle, IPs)     | Hoch (Systemebene, Prozesse)      | Sehr hoch (Dienst-spezifisch)                        |
#### Beschränkung von Daten & Network Flows
- Nutzung eines Rolling Buffer in tcpdump
	- Beispiel: tcpdump soll Dateien von 128 MB schreiben
	- wenn eine Datei voll ist, soll die nächste begonnen werden
	- wenn 32 Dateien geschrieben wurden soll die erste Datei überschrieben werden
	- `tcpdump -i en1 -s 0 result -C 128 -W 32`

	- tcpdump – snaplen (snap length)
	- der -s Parameter bei tcpdump gibt an, wann ein Paket abgeschnitten wird.
	- Beispielsweise -s 68 schneidet immer nach 68 Bytes ab, das heißt es werden alle UDP und TCP Header aufgefangen nicht aber der Payload
### <mark style="background: #FF5582A6;">Netflow</mark>
- Traffic Summarization von Cisco
- liefert Zusammenfassungen von Netzwerk Sessions
- Herz == Flow ist eine Approximation einer zusammenhängenden TCP Session
- Pakete, die gleichen Adressaten haben und einem kurzem Zeitfenster beieinander liegen werden zu einem Flow zusammengefasst

- Flows werden für jedes Protokoll, dass zumindest auf IP basiert angelegt, nicht immer sind alle Felder sinnvoll.
- scraddr, dstaddr, srcport, dstport, prot und tos stammen direkt aus den IP bzw. TCP/UDP Headern und werden aus diesen in den Flow kopiert.
-  srcport und dstport gibt es nur bei TCP/UDP, bei ICMP steht in dstport der type und code

NetFlows werden direkt in Netzwerk Hardware wie Routern oder Switches erhoben. Bei anderen Herstellern hat es aber teilweise andere Namen und wird anders konfiguriert:

Die NetFlow Generierung braucht Zeit und setzt die Performance von Routern / Switches herunter. Hierfür gibt es verschiedene Lösungen wie das droppen von NetFlow bei hoher Last, oder das Auslagern von NetFlow an teure Spezialhardware. Fast alle Router haben als Voreinstellung ein Sampling eingestellt, dies ist für eine Security Analyse ungeeignet. Viele NetFlow Router bieten Aggregation von Ergebnissen an, auch das ist für Security Analyse ungeeignet.

**Anwendungsgebiete von NetFlow:**
- **Netzwerküberwachung & Troubleshooting**: Identifizierung ungewöhnlicher Netzwerkaktivitäten oder Engpässe.
- **Sicherheitsanalysen**: Erkennung von verdächtigem Datenverkehr (z. B. DDoS-Angriffe, Port-Scans).
- **Kapazitätsplanung**: Optimierung der Bandbreitennutzung und Vorhersage von Netzwerkupgrades.
- **Abrechnungszwecke**: NetFlow kann genutzt werden, um den Bandbreitenverbrauch zu überwachen und Kostenmodelle für Cloud- und ISP-Dienste zu erstellen.

**Netflow Sampling (ISP / Internet backbone etc)**
Es wird nur ein Paket von n verarbeitet, wobei n, die Abtastrate, von der
Routerkonfiguration bestimmt wird. Der genaue Auswahlprozess hängt von der Implementierung ab:
- ein Paket pro n Paket in Deterministic NetFlow, wie es auf dem Cisco 12000 verwendet wird.
- ein in einem Intervall von n Paketen ein zufällig ausgewähltes Paket in Random Sampled NetFlow, das auf modernen Cisco-Routern verwendet wird.
- einige Implementierungen verfügen über komplexere Methoden zum Abtasten von Paketen, z. B. das Pro-Flow-Abtasten auf Cisco Martinez Catalysts.
### <mark style="background: #FF5582A6;">Silk</mark>
**Network Flow versus NetFlow**
Network Flow - Ein Oberbegriff für die Zusammenfassung von Paketen,die sich auf denselben Network Flow oder dieselbe Netzwerkverbindung beziehen, in einem einzelnen Datensatz
- **NetFlow** eine von Cisco geschützte Reihe von Formatspezifikationen zum Speichern von Network Flow Informationen in einem digitalen Datensatz. Am häufigsten sind die Versionen 5 und 9.
- **IPFIX** eine Formatspezifikation der IETF für Network Flow Aufzeichnungen, eine Erweiterung von Cisco NetFlow 9
- **SiLK** ein weiterer Satz von Formatspezifikationen für Network Flows und andere zugehörige Daten sowie die Tool-Suite zur Verarbeitung dieser Daten

SiLK ist eine Suite mit Tools um in NetFlow Daten zu suchen und diese zu analysieren. SiLK ermöglicht es auch sehr große Datenbestände effektiv auszuwerten. 56 Im Wesentlichen ist SiLK eine kommandozeilenbasierte (old-school unixartige) Datenbank. Sie besteht aus vielen kleine Applikationen, die genau eine Sache tut, viele Aufrufparameter auswerten, um diese danach mit Pipes zusammenzubinden.

**Vorteile der Analyse von Network Flows mit SiLK**
Network Flows bilden ein Protokoll der Netzwerkaktivität ab. Die Flow Analyse kann viele Fragen beantworten, ohne Inhalte zu speichern. Flow Records sind äußerst kompakt.
Vorteile sind:
- lange Aufbewahrung
- schnellere Verarbeitung
- geringere Datenschutzbedenken
- Verschlüsselung ist kein Hindernis
SiLK verwendet unidirektionale Flows - Uniflows.
![[Pasted image 20250201225223.png]]

Network Monitoring mit SiLK
![[Pasted image 20250201225803.png]]

Sequenzdiagramm:
![[Pasted image 20250201225945.png]]

#### Befehle:
**rwcut:**
SiLK speichert die Dateien in einem Binärformat, dass nicht direkt gelesen werden kann.
- Der Befehl um auf Daten zuzugreifen ist: `rwcut {Datei}` mit `--field` kann die Ausgabe beschränkt werden
(rwcut) output:
![[Pasted image 20250201230041.png]]
Beispiele:
- rwcut datei1 --all-fields -> zeigt alles an
- rwcut datei1 --fields=1,2,3,4 | head -n 5 -> zeigt sip,dip,sport,dport und die ersten 4 zeilen + 1 Kopfzeile
- rwcut datei1 --fields=dport -> zeigt alle dport in datei1 an

**rwfilter:**
lässt sich über eine Pipe mit rwcut verknüpfen, um die Ausgabe zu filtern.
- rwfilter --dport=80 {Datei} --pass=stout | rwcut --field=1-2 -> zeigt alle Verbindungen von Port 80 

Beispiele für Filtertypen:
--sport=79-83
--dport=21,22,80
--sadress=141.55.192.49
--dadress=141.55.193.x
--stime=2015/12/08T00:00:00-2015/12/08T23:59:59

Beispiele für Kombinationen von Filtern:
rwfilter --dport=80 --pass=stdout |
rwfilter --input-pipe=stdin --sport=1024-65535 --pass=stdout

![[Pasted image 20250201230750.png]]
![[Pasted image 20250201230805.png]]

**rwfileinfo & rwcount:**
- rfileinfo gibt Informationen über SiLK Files aus.
- rwcount fasst SilK-Daten zu Zeitreihen zusammen, indem er Records in bins gleicher Zeitdauer einsortiert. (--bin-size=1800)

- schnelle und einfache Möglichkeit, Volumes als Zeitreihen zusammenzufassen
- einfache Ausgabe und Erstellung eines Diagramms mit grafischer Darstellung
- ideal für einfache Bandbreitenstudien

**rwset** 
extrahiert eine Sammlung aller IP-Adressen aus einem Flow und
gehört zu den nützlichsten Tools von SiLK. Es werden bis zu vier Dateien erzeugt:
--sip-file: Mit allen Source IPs
--dip-file: Mit allen Destination IPs
--any-file: Mit allen IPs
--nhip-file: Mit allen next hop Adressen

**rwsetbuild**
ist ein Befehl mit dem sich IP Sets aus eigenen Quellen erstellen lassen. Der folgende Befehl baut aus einer einfachen Textdatei ein IP-Set:
- rwsetbuild {Datei.txt} {Datei.set}

**rwuniq**
erlaubt es dem Analysten einen Key mit einem oder mehreren Feldern zu definieren und zählt dann die verschiedenen Werte.
- rwuniq datei1 --fields=dport -> um mehrere redundante Einträge zusammenzufassen und zu zählen

**Konvertierungstool YAF**
YAF kann pcap Daten einlesen, die z.B. mit tcpdump oder Wireshark aufgenommen wurden, oder selbst Flows aufnehmen. YAF wird komplett über Kommandzeilenparameter konfiguriert.
**Beispiel**:
- sudo yaf --in en1 –live pcap --out /tmp/packets.yaf
### Praktikum SiLK
- NetFlow Aufgezeichnete Metadaten von IP basiertem Netzwerkverkehr
- möglich Quellen und Ziele von Netzwerkdatenverkehr ausfindig zu machen 
- Flow beyteht aus einer Reihe zusammenhängender gerichteten Paketen, die eine unidirektionale Verbindung zwischen zwei Netzwerkteilnehmenr beschreiben 
- Ausswahl der Pakete anhand gemeinsamer Attributte

![[Pasted image 20250202130606.png]]

-  Netflow-Daten in Binär Form abgelegt -> deswegen aufbereiten, dazu diese Werkzeuge und Datenstrukturen
![[Pasted image 20250202131102.png]]

**Befehle:**
![[Pasted image 20250202131211.png]]
- können mittels Pipe | verkettet werden

- rwfilter datei1 --dport=80,443 --pass=stdout | rwuniq --fields=dport -> zählt die http und https verbindungen 
- rwfilter datei1 --daddress=10.1.60.0/24 --pass=stdout | rwuniq --fields=dip -> anzahl der Ziel IP-Addressen in denen Flow-Einträge vorhanden sind 
- rwfilter datei2 --protocol=1,58 --pass=stdout | rwuniq --fields=protocol -> anzahl der ipv4 und ipv6 Verbindungen
- rwfilter datei2 --protocol=1,58, --pass=stdout | rwcut --fields=icode,protocol,itype -> anzeigen von  ICMP-Typ und Code 
- rwcount datei2 --bin-size=30 | head -n 4 -> anzahl der Records(einträge) inherhalb von 30 sekunden

- rwset datei2 --sip-file=datei2.sipset -> erstellt eine Liste aller Quell-IPs in datei2.sipset
- rwsetcat datei2.sipset -> zeigt die datei an
- rwsetcat datei2.sipset --count-ips -> zählt die vorhandenen ip-addressen

- rwsetcat datei2.sipset --ip-ranges -> gibt die Range an
- rwsetcat datei2,sipset --cidr-blocks -> Zeigt die in der Datei enthaltenen IP-Adressen als **zusammenhängende Subnetze in CIDR-Notation** an.

- rwfilter datei3 --saddress=10.1.60.73 --sport=5269 --pass=stdout | rwuniq --fields=sip,sport --bytes -> anzahl bytes von Quell-IP an Quell-Port versendet
- rwfilter datei3 --dport=0-1023 --pass=stdout | rwuniq --fields=4,5 -> welche ports zwischen 0-1023 werden als Ziele von Verbindungen verwendet
- rwfilter datei -dport=0-1023 --pass=stdout | rwuniq --fields=2,4 --flows --packets --bytes -> zeigt Ziel-IPs und Standartports von 0-1023 an mit Anzahl der einträge(records --flows), Pakete (--packets)vund --bytes
- rwfilter dump.rwf --proto=1 --pass=stdout | rwcut --fields=1,2,25 -> alle Einträge ICMP (1)
 



