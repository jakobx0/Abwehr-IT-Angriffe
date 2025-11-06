>[!info] **Vermittlungsschicht (Network Layer)**
>- Die Vermittlungsschicht (Layer 3) bildet die Grundlage für die Kommunikation über Netzwerke.
>- Wichtige Protokolle:
>	- **IP (Internet Protokoll):** Grundlage für die Datenübertragung.
>	- **ICMP (Internet Control Message Protocol):** Diagnose- und Fehlermeldungsprotokoll.
>	- **Routing-Protokolle:** RIP, OSPF, BGP (zur Wegfindung in Netzwerken).
>
>![[Pasted image 20251103182130.png]]

>[!info] **Das Internet Protokoll (IP)**
>**Eigenschaften**
>- **Verbindungslos:** Kein Zustand zwischen Sender und Empfänger.
>- **eindeutiges Adressschema:** Ermöglicht eindeutige Identifikation der Geräte.
>- kann mit Fragmentierung, Quantity of Service ind IP-Header Fehlerprüfung umgehen.
>- **Fehleranfälligkeiten:**
>	- da Verbindungsloser  Dienst → anfällig für Spoofing und DoS-Angriffe.

 > [!info] **Der IPv4-Header:**
 > -  Enthält wichtige Felder wie:
 > 	- **IHL (Internet Header Length):** Header-Länge in 32-Bit-Words (min. 5, max. 20).
 > 	- **TOS (Type of Service):** nie implementiert
 > 	- **Total Length:** (2 Byte) Gesamtlänge des Paketes Daten + Header
 > 	- **Identification:** (2 Byte) Nummer, die nach Fragmentierung das Zusammensetzen erlaubt.
 > 	- **Fragment Offtset: (Fragmentation (3Bit)):** 1 -> Reserviert, 2 -> Do not Fragment, 3 -> Fragment follows, **Danach Fragment Offtset (13Bit)**
 > 	- **TTL (Time to Live) (1 Byte):** Maximale Anzahl an Hops (wird bei jedem Router verringert).
 > 	- **Protokoll:** Gibt das Transportprotokoll an z. B.:
 > 		- 0x01 ICMP
 > 		- 0x06 TCP
 > 		- 0x08 EGP
 > 		- 0x11 UDP
 > 		- 0x58 IGRP
 > 	- Gibt das Protokoll aus dem höherem Transportschichtlayer an, an welches das Datenteil weiteregegeben werden soll 
 > 	- **Checksum (2 Byte):** Prüft auf Bitfehler, wird bei jedem Hop neu berechnet wergen TLL-Wert.
 > 	- IP-Adressen
 > 		- Sender (Quell-IP)
 > 		- Empfänger (Ziel-IP)
 > 	- Options
 > 		- Optional aber haben Auswirkung auf IP-Datagramm Header Länge
 > 		- Optionsfelder erweitern den IP Header und sollten selten genutzt werden in IPv6 abgeschafft
>
![[Pasted image 20251104105429.png]]

>[!question] **Vergleich IPv4 vs. IPv6**
 >- IPv6 entfernt die Fragmentierung, CRC-Prüfungen und Optionsfelder zur Optimierung des Routings.
 >	- Bei IPv6 ist es Routern nicht mehr erlaubt, Pakete zu fragmentieren. Der Absender wird bei Fragmentierungsbedarf immer mit einer ICMPv6-Nachricht vom Typ 2 (Packet Too Big) informiert. Dieser kann daraufhin seine Paketgrößen dadurch senken, dass die kommunizierende Anwendung kleinere, unfragmentierte Pakete erzeugt, oder dass über einen Fragment Extension Header (Protokoll 44) außerhalb des IPv6-Header realisiert wird
 >- IPv6-Adressen können Rückschlüsse auf MAC-Adressen zulassen. (letzten Bytes -> MAC addresse)
 >
 >![[Pasted image 20251104123930.png]]

>[!abstract] IPv6 & MAC Adresse
>
>![[Pasted image 20251104124130.png]]

>[!abstract] **Internet Control Message Protocol (ICMP)**
 Ist ein weiteres Protokoll auf dem Network Layer. Es unterscheidet sich von vielen anderen Protokollen und Anwendungen darin, dass es normalerweise nicht von Netzwerkanwendungen gesteuert wird. Es wird für logische Fehler und zur Diagnose verwendet. Es kann auch von Angreifern missbraucht werden, wie etwa für Smurf Attacken und kann beim Port Scan hilfreich sein.
>
>![[Pasted image 20251104124704.png]]
>
>![[Pasted image 20251104124312.png]]
>
>**Einsatzbereiche**
>- Diagnose und Fehlermeldungen im Netzwerk (z. B. Ping, Traceroute).
>	- Wichtige ICMP-Typen:
>		- Typ 8: Echo Request.
>		- Typ 0: Echo Reply.
>		- Typ 3: Destination Unreachable.
>		- Typ 11: Time Exceeded.
 >
 >>[!example] **Traceroute (ICMP)**
 >>Werkzeug zur Verfolgung von Netzwerkrouten.
 >>![[Pasted image 20241204165943.png]]
 >>Funktionsweise:
 >>- Für jeden Hop neues UDP Paket
 >>- Sendet Pakete mit schrittweise erhöhter TTL.
 >>- Jeder Router antwortet mit **ICMP TLL Exceeded**, bis das Ziel erreicht wird (Antwort: **ICMP Port Unreachable**).
 >>
 >>![[Pasted image 20251104125152.png]]
 >
 >>[!example] ICMP MTU Discovery
 >>Feststellen der Netzwerk MTU durch Senden einer Naricht ICMP Type 3 Code 4 und Anpassen der Paketgröße:
 >>
 >>![[Pasted image 20251105125431.png]]
 >
 >>[!example] ICMP Redirect
 >>Mitteilung der besseren IP Route an Sender:
 >>
 >>![[Pasted image 20251105130024.png]]
 
 >[!abstract] Angriffe auf IPv4 und ICMP-Typen
 >Schwächen des IP
 >- Das Internet Protokoll ist vor allem verwundbar weil es
 >	- verbindungslos ist
 >	- keine Authentisierung bietet
 >
 >>[!danger] IP Spoofing:
 >>IP Spoofing erfolgt durch das Fälschen der Absenderadresse eines IP Pakets in zwei Arten:
 >>- **Lokal Spoofing:**
 >>	- Angreifer befindet sich im selben Subnetz, durch Packet-Analyzer Trustet IP herausfinden. Bleibt das Problem der Packetnumbers.
 >>- **BlindSpoofing:**
 >>	- Angreifer befindet sich nicht im selben Subnetz. Er hat keine Informationen über die Pakete und muss alle Parameter raten. Bei modernen Betriebssystemen sehr schwierig wegen Randomized TCP Sequence Nummern. Dazumal der Empfänger immer eine Rückmeldung an das Opfer gibt und nicht den MitM der die Anfragen fälscht.
 >>
 >>Fragmentierung bei IPv4 und deren Wiederherstellung 
 >>
 >>![[Pasted image 20251106101856.png]]
 >>
 >>Ausnutzen der Fragmentierung:
 >>- Pakete die ein System sieht aber ein anderes nicht.
 >>- Fehlerhafte fragmentierung
 >>- Ausnutzen von Schawachstellen der NIDS (Network Intrusion Detection Systeme)
 >>- 
 


 Beispiel:
 >>- Die Signatur  des PHP-Angriffs könnte so ähnlich sein:
 >> 	 - `GET /cgi-bin/phf`
 >> - Wir können zusätzlich Pakete einfügen, so dass das IDS die Pakete als:
 >> 	- `GET /cgi-bin/pleasedontdetectthisforme?`, erkennt.
 >> - Während der Endpunktserver folgendes ließt
 >> 	- `GET /cgi-bin/phf`
 >>
 >>**Techniken:**
 >>- **Falsche Sequenznummern:** Pakete mit ungültigen Sequenznummern werden vom Endpunkt zurückgewiesen, können aber vom IDS verarbeitet werden.
 >>- **Falsche Prüfsummen:** IDS überprüfen TCP-Prüfsummen oft nicht, Endpunkte hingegen schon. Pakete mit falschen Prüfsummen werden ignoriert, können jedoch das IDS täuschen.
 >>- **Manipulierte TTL / MTU-Werte:** Ein Angreifer kann mithilfe von unterschiedlichen TTL und MTU, Pakete der IDS einfügen, die beim Ziel-System verworfen werden.
 >
 >>[!danger] Evasion Attacke:
 >>Ein Endsystem kann ein Paket akzeptieren, das ein NIDS ablehnt. Ein NIDS, das ein solches Paket fälschlicherweise ablehnt, erkennt seinen Inhalt nicht vollständig.
 >>
 >>![[Pasted image 20251106095747.png]]
 >>
 >>Beispiel: Folgendes Paket
 >>- `GET /cgi-bin/phf?` kann als 
 >>- `GET /gin/f ` bei der NIDS Erkennung erkannt werden, während das vollständige
 >>- `GET /cgi-bin/phf?` beim Endpunkt ankommt 
 >>
 >>Techniken:
 >>- Fragmentpakete nicht in der richtigen Reihenfolge versenden. Einige IDS gehen davon aus, dass die Fragmentpakete in der richtigen Reihenfolge ankommen. Sie setzen die Daten einfach sequentiell wieder zusammen, sobald das markierte letzte Fragment eintrifft. Das Versenden von Fragmentpaketen in falscher Reihenfolge kann die IDS nicht korrekt zusammensetzen.
 >>- Einige IDS können jeweils nur eine Host- / Port-Verbindung verfolgen. Überfluten Sie zuerst den Zielport mit nicht vorhandenem SYN- Paketen, damit diese IDS anschließend die attackierende Verbindung ignoriert.
 >
 >>[!danger] Teardrop Attacke:
 >>Angeifer nutzt falsche Offsets bei Fragmenten die zu Überschneidunge
 
 



 

#### **Insertion Attack**
- **Beschreibung:**  
    Bei einer Insertion-Attacke fügt der Angreifer Pakete ein, die vom Endpunkt ignoriert werden, aber von Intrusion Detection Systemen (IDS) als gültig interpretiert werden. Ziel ist es, die Analyse des IDS zu umgehen, während der Endpunkt die eigentlichen Daten korrekt empfängt.
- **Techniken:**
    - **Falsche Sequenznummern:** Pakete mit ungültigen Sequenznummern werden vom Endpunkt verworfen, können aber vom IDS verarbeitet werden.
    - **Falsche Prüfsummen:** IDS überprüfen TCP-Prüfsummen oft nicht, Endpunkte hingegen schon. Pakete mit falschen Prüfsummen werden ignoriert, können jedoch das IDS täuschen.
    - **Manipulierte TTL-Werte:** Pakete mit einem zu niedrigen TTL-Wert erreichen den Endpunkt nicht, werden jedoch vom IDS registriert.
- **Beispiel:**  
    Ein Angriff könnte zusätzliche Daten in ein Paket einfügen, um eine Signatur zu verschleiern:
    - IDS sieht: `GET /cgi-bin/pleasedontdetectthisforme`
    - Endpunkt empfängt: `GET /cgi-bin/phf`
#### **Evasion Attack**
- **Beschreibung:**  
    Bei Evasion-Angriffen werden Pakete so manipuliert, dass sie vom Endpunkt akzeptiert, aber vom IDS abgelehnt oder falsch interpretiert werden. Ziel ist es, die Erkennung durch das IDS zu umgehen.
- **Techniken:**
    - **Fragmentierung in falscher Reihenfolge:** Das IDS setzt Fragmente in der falschen Reihenfolge zusammen, während der Endpunkt die richtige Reihenfolge verwendet.
    - **Flutung von SYN-Paketen:** Überfluten des Zielports mit nicht existierenden SYN-Paketen, damit das IDS die echte Verbindung nicht mehr verfolgen kann.
    - **Ungewöhnliche Paketgrößen:** Versenden von Fragmenten, die das IDS nicht korrekt verarbeitet.
- **Beispiel:**  
    Das IDS erkennt nur einen Teil eines Pakets:
    - IDS sieht: `GET /gin/f`
    - Endpunkt empfängt: `GET /cgi-bin/phf`
#### **Teardrop Attack**
- **Beschreibung:**  
    Eine Teardrop-Attacke nutzt die Fragmentierung von IP-Paketen aus. Fragmente werden mit fehlerhaften oder überlappenden Offsets gesendet, sodass Systeme, die diese Pakete nicht korrekt zusammensetzen können, abstürzen.
- **Funktionsweise:**
    - Pakete mit falschen Fragmentierungsinformationen (Offsets) führen zu Konflikten beim Zusammensetzen.
    - Einige Systeme reagieren darauf mit Abstürzen oder hängen sich auf (z. B. ältere Versionen von Windows oder Linux).
- **Ziele:**
    - **Denial of Service (DoS):** Systeme wie Windows 95, NT oder Linux 2.1.63 können durch überlappende Fragmente überlastet und zum Absturz gebracht werden.
    - **Verwirrung von IDS:** Überlappende Fragmente können dazu führen, dass IDS und Endpunkte unterschiedliche Pakete zusammensetzen.
- **Beispiel:**  
    Fragmente wie:
    - Fragment 1: `GET /cgi-bin/ph`
    - Fragment 2: `bin/phf`
    - Ergebnis beim IDS: `GET /cgi-bin/phbin/phf`
    - Ergebnis beim Endpunkt: `GET /cgi-bin/phf`
#### **Schutzmaßnahmen gegen diese Angriffe**
1. **Insertion Attack:**
    - IDS mit **Prüfsummenvalidierung** und **Sequenznummerprüfung** einsetzen.
    - Paketfilter konfigurieren, die ungültige Pakete ablehnen.
2. **Evasion Attack:**
    - IDS aktualisieren, um auch ungewöhnliche Fragmentierungs- und Sequenzierungsverhalten zu erkennen.
    - Netzwerküberwachung auf ungewöhnliche Paketgrößen und Fragmentierungsarten.
3. **Teardrop Attack:**
    - Aktualisierung des Betriebssystems und der Netzwerktreiber.
    - Verwendung moderner Systeme, die fehlerhafte oder überlappende Fragmente korrekt behandeln.
    - Einsatz von Firewalls, die überlappende Fragmente ablehnen.
#### **ICMP-Angriffe**
- **Smurf Attack(ICMP Echo Attacks):** Missbraucht ICMP Broadcasts, um Opfer mit Antworten zu überfluten. Gefälschter Absender, Opfer als Ziel erzeugen und als Brodcast absetzen. Gepingte Server Antworten alle dem Opfer -> DDoS
- **ICMP Nukes:** Gefälschte „Destination Unreachable“-Nachrichten, um Verbindungen zu trennen.
- **ICMP Redirect Angriff:** Schicke gefälschte ICMP Nachricht an ein Opfer mit einer ICMP Redirect Nachricht um den Netzwerkverkehr des Opfers über ein kompromittiertes System umzulenken.
- **Covert Channels:** Nutzung von ICMP-Paketen zum Übertragen verdeckter Nachrichten.
- **Ping of Death:** Ping max 64K Daten -> Fragmentierung mehr als 64K -> Buffer Overflow
- **Land Angriff:** (DoS) Angreifer -> SYN bei dem Quell und Zieladresse sowie -Port identisch sind, an offenen Port von Opfer, dieser antwortet mit SYN-ACK an Quelle, also sich selbst. -> Endlosschleife 
#### **Schutzmaßnahmen**
- **Firewall-Regeln:** Blockieren gefälschter IP-Adressen und unerwünschter ICMP-Nachrichten.
- **IPSec:** Verschlüsselt Datenpakete zur Sicherung der Kommunikation.
- **Routing-Protokolle:** Nicht benötigte Protokolle deaktivieren.
- **Paketfilter:** Zur gezielten Filterung von ICMP-Nachrichten an Netzwerkgrenzen. 
#### **Zusammenfassung**
- Das IP-Protokoll bietet die Grundlage für weltweite Kommunikation, ist jedoch anfällig für Spoofing und DoS-Angriffe.
- ICMP ist sowohl für Diagnosen als auch für Angriffe geeignet (z. B. Smurf Attack, Ping of Death).
- IPv6 bietet größere Sicherheit und Effizienz, ist aber nicht vollständig gegen Angriffe immun.