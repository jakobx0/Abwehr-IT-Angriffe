## Grundlagen
> [!question] Was ist Netzwerkforensik?
>> [!success] Untersuchung von Netzwerkdaten, um Sicherheitsvorfälle zu erkennen, zu rekonstruieren und zu dokumentieren.

> [!question] Was bedeutet Beweissicherung in Netzwerken?
>> [!success] Rechtssichere Sammlung, Integritätssicherung und Dokumentation von Netzwerkdaten (z. B. Logs, PCAPs).

> [!question] Welche Rolle spielen ARP und MAC im Ethernet?
>> [!success] ARP löst IP → MAC auf; MAC-Adressen identifizieren Geräte im lokalen Netz.

> [!question] Wofür steht IP?
>> [!success] Vermittlung/Adressierung von Paketen zwischen Netzen (Layer 3).

> [!question] Wofür steht ICMP?
>> [!success] Diagnose- und Fehlernachrichten (z. B. ping, unreachable).

> [!question] Wofür steht TCP?
>> [!success] Zuverlässiger, verbindungsorientierter Transport mit Sequenzierung und ACKs.

> [!question] Wofür steht DNS?
>> [!success] Namensauflösung von Domains zu IP-Adressen.

> [!question] Was versteht man unter Routing?
>>[!success] Weiterleitung von Paketen zwischen Netzen über Router.

## Besondere Inhalte
> [!question] Was untersucht man bei Netzwerkdatenverkehr?
>> [!success] Protokolle, Kommunikationsbeziehungen, Anomalien, Angriffsindikatoren.

> [!question] Wofür nutzt man Wireshark in der Forensik?
>>[!success] Paketmitschnitt, Filterung und Analyse einzelner Verbindungen/Protokolle.

> [!question] Unterschied Capture- vs. Display-Filter?
>> [!success] Capture-Filter begrenzen den Mitschnitt; Display-Filter filtern nur die Anzeige.

> [!question] Was ist ein MitM-Angriff?
>> [!success] Angreifer sitzt zwischen zwei Kommunikationspartnern und kann Daten lesen/manipulieren.

> [!question] Was ist DoS/DDoS?
>> [!success] Überlastung eines Dienstes/Netzes durch massenhaften Traffic; DDoS verteilt auf viele Quellen.

> [!question] Nenne typische TCP-Angriffe.
>>[!success] SYN-Flood, Session Hijacking, Reset-Angriffe, MitM.

> [!question] Was ist SQL Injection?
>> [!success] Einschleusen von SQL in Eingaben zur Manipulation von Datenbankabfragen.

> [!question] Was sind zustandsbasierte Angriffe auf Web Apps?
>> [!success] Angriffe auf Sessions/State-Handling (z. B. Session Fixation, CSRF).

## Abwehr von IT-Angriffen
> [!question] Was sind Netzwerksensoren?
>> [!success] Systeme zur Überwachung/Erkennung von Angriffen (z. B. IDS, NetFlow-Sensoren).

> [!question] Worauf kommt es bei der Platzierung von Sensoren an?
>> [!success] Sichtbarkeit kritischer Segmente, Nähe zu Engpässen/Übergängen, minimale Blind Spots.

> [!question] Was bedeutet Ausgestaltung von Log-Dateien?
>> [!success] Welche Events protokolliert werden, Zeitstempel, Kontext, Integrität, Aufbewahrung.

> [!question] Wozu dienen Flow-Daten (NetFlow)?
>> [!success] Zusammenfassung von Kommunikationsflüssen zur Analyse von Verkehrsmustern.

> [!question] Was ist SiLK?
> [!success] Tool-/Framework zur Sammlung, Speicherung und Analyse von NetFlow-Daten.

> [!question] Typische Anwendungsgebiete von Flow-Daten?
>> [!success] Erkennung von Scans, DDoS, Datenabfluss, ungewöhnliche Verbindungen.

> [!question] Was ist IDS vs. IPS?
>> [!success] IDS erkennt und meldet; IPS blockiert/unterbindet aktiv.

> [!question] Einsatzgebiete von IDS/IPS?
>> [!success] Netzwerkperimeter, interne Segmente, Rechenzentrum, kritische Servernetze.

> [!question] Was bedeutet Regelerstellung bei SNORT?
>> [!success] Definition von Signaturen/Regeln, die bestimmten Traffic/Angriffe erkennen.

> [!question] Wichtige Bestandteile einer SNORT-Regel?
>> [!success] Header (Aktion/Protokoll/Ports) + Optionen (Content, Flags, SID).
