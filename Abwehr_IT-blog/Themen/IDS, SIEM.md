Während ein Logger, wie NetFlow, einfach alles protokolliert, wird ein IDS nur auf solche Ereignisse reagieren, die als gefährlich eingestuft werden.
#### Unterteilung:
#### Stantort:
- host based IDS
- Networkbased
- log-file monitoring
#### Arbeitsweise:
- **signature based IDS:**
	- **Pattern Matching:** Viele Angriffe sind in ihrer Signatur bekannt. Daher kann man eine Datenbank bekannter Angriffe mit dem fließenden Traffic vergleichen und reagieren, wenn ein Vergleich positiv ist.
	Vorteile:
	- Deutlich weniger Fehlalarme 
	- Zuverlässiges erkennen bekannter Angriffe
	- Schnellere Installation und Einarbeitung
	Nachteile:
	- Einspielen neuer Signaturen notwendig
	- Erkennt keine neuen Angriffe
	- Leichte Unterschiede in den Angriffspaketen können zur Nichterkennung führen
- **Anomaly based:**
	- **Anomaly Detection:** Netzwerktraffic ist im allgemeinen gleichmäßig. Wenn man den normalen Traffic eines Netzes zugrunde legt, kann jede Abweichung ein Angriff sein. So führt jede Anomalie im Netz zu einer Reaktion.
	Vorteile:
	-  Unbekannte Angriffe können erkannt werden
	- Ungewöhnliches Verhalten von Usern wird erkannt
	- Keine riesige Angriffsdatenbank notwendig
	Nachteile:
	- Großer Aufwand beim Installieren, Lernphase
	- Viele Fehlalarme
	- Nur in gleichmäßigen Umgebungen möglich
	- Resourcen fressend

**NIDS(Network-based Intrusion Detection System):**
	Analysiert den Traffic auf dem Netz und stellt mittels Anomaly Detection oder Pattern Matching fest, ob ein Angriff stattfindet. Es handelt sich also um Netzwerksensoren
Funktionen:
- Netzwerksniffer -> sammelt Rohdaten
- Paketdecoder ->  Rohdaten in Datenstrukturen umwandeln
- Detection Engine -> Muster Erkennung/Anomaly, Entscheidung Alarm, Logeintrag, Reaktion
- Reaktives System -> Ausführung der Entscheidung

**HIDS(Host-based Intrusion Detection System):**
	(System Integrity Verifier): monitort kontinuierlich bestimmte Systemparameter am Host (crond, registry,...) und speichert sie. Bei Parameteränderung ist ein Alarm möglich. Es handelt sich also um Hostsensoren
**Log File Monitor:** 
	Analysiert die Logfiles verschiedener Hosts, ähnlich einem NIDS, speichert sie und alarmiert gegebenenfalls

**Spezial "Honeypot"**
- System wird als Köder benutzt, zur Ablenkung oder Beobachtung 
- wird von Sensoren/IDS Überwacht
##### **Einsatzgebiete von IDS**
Ein **Intrusion Detection System (IDS)** überwacht den Netzwerk- und Systemverkehr, um verdächtige Aktivitäten zu erkennen. Es greift jedoch nicht aktiv ein, sondern alarmiert Administratoren über potenzielle Angriffe.

1. **Unternehmensnetzwerke:**
    - Überwachung des ein- und ausgehenden Netzwerkverkehrs auf verdächtige Aktivitäten.
    - Erkennen von ungewöhnlichen Datenströmen oder Angriffsversuchen.
2. **SCADA- und industrielle Steuerungssysteme (ICS):**
    - Schutz kritischer Infrastrukturen (z. B. Energieversorger, Wasserwerke) vor Cyberbedrohungen.
    - Erkennung von Angriffen auf industrielle Netzwerke.
##### **Einsatzgebiete von IPS**
Ein **Intrusion Prevention System (IPS)** geht einen Schritt weiter als IDS: Es erkennt Bedrohungen und blockiert automatisch verdächtigen Netzwerkverkehr.

1. **Cloud-Sicherheit & Virtual Private Networks (VPNs):**
    - Schutz von Cloud-Diensten vor unautorisierten Zugriffen.
    - Absicherung von VPN-Zugängen gegen Angriffe.
2. **E-Commerce & Webserver:**
    - Schutz von Online-Shops und Webseiten vor SQL-Injections, XSS und DDoS-Angriffen.
    - Erkennung und Blockierung von bösartigen Bots.

Tools:
Pattern Matching YARA:
Pattern Matching:  SIGMA
#### Security Information und Event Management
Die Grundidee eines SIEM ist alle für die IT-Sicherheit relevanten Daten an einer zentralen Stelle zu sammeln und durch Analysen Muster und Trends zu erkennen, die auf gefährliche Aktivitäten schließen lassen. Das Sammeln und die Interpretation der Daten erfolgen in Echtzeit

- SIEM = Security Information Management + Security Event Management 
- In Echtzeit Reaktionsfähig
- Sammeln der Daten durch Software Agenten
- Analyse über Regel Korrelationsmodelle maschinelles Lernen und KI
	- Durch das Korrelieren der Datensätze ist es beispielsweise möglich, Einbruchsversuche auf Grund fehlerhafter Anmeldeversuche und/oder unerlaubter Zugriffe der Firewall zu erkennen.







