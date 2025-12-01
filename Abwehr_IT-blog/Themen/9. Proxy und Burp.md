#### Die Darstellungsschicht
**Funktion**
- behandelt Syntax und Semantik der übertragenen Informationen
- Kommunikation internen Datenrepräsentationen
- Datenstrukturen zusammen mit Standartcodierungen abstrakt definiert

- Systemabhängige Darstellung Daten (ASCII, EBCDI, ANSI) in unabhänige Form zum Datenaustausch zwischen unterschiedlichen Systemen
- Verwaltung der abstrakten Datenstrukturen
- Definiert und Austauschen übergeortneter Datenstrukturen(z.B Datenrecords, Tabellen)
#### Darstellungsschicht - Normen
**Services**
- Daten Konvertierung
-  Character code translation
- Kompression
- Encryption und Decryption
**Mögliche Angriffsbereiche**
- Decryption Attacks
- Encryption Downgrade Attacks
- Parsing/Character/Uncompress Exploits
- Encoding Attacks
- Type Confusion
#### Proxy Server
- Agiert als Vermittler
- Nimmt Anfragen entgegen um über eigene IP Verbindung herzustellen
**unterscheiden zwischen:**
- Dedicated Proxys (Proxy-Server)
- Circuit-Level-Proxys (Generischer Proxy)
**hinzukommt:**
- Proxy Firewalls
- Transparente Proxys
- Reverse Proxys
#### Dedicated Proxy
- Vermitteln Datenverkehr zwischen Client und Zielsystem
- immer auf Kommunikationsprotokoll spezialisiert
- Kommunikation analysieren und Inhalt manipulieren
- Eigenständige anfragen senden und als zwischenspeicher fungieren
- Auf einem Server können mehrere Proxys mit verschiedenen Protokollen parallel laufen
- Arbeitet aus OSI-Schicht 7 -> Anwendungsschicht
#### Circuit Level Proxy
- Protokollunabhängiger  Filter
- Realisiert Filterung Port -und adressbasiert
- Nutzung: einfache Weiterleitung durch lauschen auf Port eines Netzwerkadapters und weitergeben der Daten auf anderen Netzwerkadapter und Port 
- Kann Komunikation nicht einsehen, beeinflussen oder selbst führen
- Arbeitet Layer 4
#### Proxy-Firewall
- Dedicated oder Circuit Level Proxys als Filtermodule
- Filtermodule -> Regeln, welche Daten weitergeleitet werden 
- Eigenes Netz(Segment) Schutz vor unerlaubten Zugriffen
- Aufgaben: Konvertierung der Daten, Inhalte zwischenspeichern und alle weiteren Funktionen eines Proxy
#### Transparenter Proxy
- Zwei Komponenten 
- Ports der Protokolle abgreifen und an Proxy weiterleiten
#### Reverse Proxys
- Vermeintliche Zielsysteme in Erscheinung
- Adressumsetzung in entgegengesetzter Richtung
- Client wahre Adresse des Zielsystems verborgen
#### Tools
- Burp proxy -> analysieren 

Abwehr von Zugriffen aus Secure Datenübertragungen 
- TSL-Zertifikat eines vertrauenswürdigen Herausgeber belegt nicht, dass kontaktierter Server auch derjenige, für den er sich ausgibt
- Angreifer können Zertifikate besitzen -> die bei BS als vertrauenswürdig gelten
- Trojaner und Analyse Tools nutzen Tertifikate für verschlüsselte Verbindungen 
- Überprüfung auf ein bestimmtes Zertifikat
#### hydra - ein Tool für logins
- Network Login Cracker
- versucht unbekannte Passwörter zu erraten
- Probiert ledlglich Passwörter aus Textdatei aus, die zur Verfügung gestellt werden muss
- -> Wörterbuchangriff
#### SPARTA
- vereinigt nmap, nikto... in einem tool