#### <mark style="background: #ADCCFFA6;">Transportschicht</mark>
**Verbindungslos:**
- UDP
- IPX
**Verbindungsorientiert:**
- TCP
- SPX
#### **Aufteilung der Portnummern**
- zum aufbauen von Verbindungen
- anhand von Portnummern werden Downloads getrennt
Well Known Ports:
	![[Pasted image 20250123121427.png]]

Aufteilung Portnummern
- Ports um zwei Verbindungen gleichzeitig abzuwickeln
- Anhand von Portnummer -> Downloads trennen
Beispiel:
	Ein Client startet 2 Downloads gleichzeitig. Ein weiterer einen Download. Die Clienten wählen unterschiedliche Ports. Beide verbinden sich mit Server Port 80. 
	![[Pasted image 20250123121727.png]]
#### <mark style="background: #BBFABBA6;">UDP</mark>
![[Pasted image 20250123121027.png]]
#### <mark style="background: #BBFABBA6;">Angriffe auf UDP</mark>
**UDP-Flooding:**
	 nutzt die verbindungslose Implementation von UDP. Das UDP Protokoll besitzt keine Möglichkeit, den Empfang eines gesendeten Pakets zu kontrollieren. TCP ist in der Lage, auf eine verzögerte Empfangsbestätigung durch Senken der Sendehäufigkeit zu reagieren, während UDP unverändert weiter sendet. Da UDP-Pakete Vorrang vor TCP-Paketen haben, belegt der UDP-Verkehr nach einiger Zeit die gesamte Bandbreite einer Verbindung und unterbindet damit den TCP-Verkehr. Als Schutz vor UDP-Flooding können unerwünschte UDP-Pakete in der Firewall ausgefiltert werden.

**Beispiel:**
**Chargen-Angriff**
	Dazu wird der Zeichen erzeugende Dienst Chargen eines Rechners mit dem die empfangenen Daten reflektierenden Dienst Echo eines anderen Rechners verbunden. Der Angreifer sendet UDP-Pakete zum chargen-Port (19) seines Opfers und gibt als Quelle den echo-Port (7) und eine ggf. gefälschte Quelladresse an. Der so erzeugte 'UDP Packet Storm' kann bei geeigneter Wahl der IPv4- Adressen ein ganzes Netzwerk lahmlegen.
**Fraggle-Angriff:**
	verwendet statt ICMP, UDP-Echo Pakete.
	Dabei wird ein Paket an eine Broadcast-Adresse geschickt, die es an jeden Rechner im betreffenden Netzwerk weiterleitet. Ein Angreifer kann eine Folge von UDP-Echo-Paketen mit der IPv4- Adresse des gewünschten Opfers als Quelle an die Broadcast-Adresse des Netzwerks des Opfers senden. Alle Rechner im betroffenen Netzwerk antworten darauf mit einem ECHO-REPLY-Paket an das Opfer, das davon überflutet wird.
### <mark style="background: #FF5582A6;">TCP</mark>
![[Pasted image 20250123123211.png]]
Zwei Sequenznummern:
	1. Source Sequence Number
	2. Acknowladgement sequence Number
Es muss nicht nach jedem Paket ein Ack erfolgen. Ein Ack kann sich auch auf mehrere Pakete beziehen.

Beispiel: Start Source Sequence Number ist 101. 
-  Bob sendet 3 Bytes, im Feld Source Sequence Number steht 101 
-  Alice acknowledged mit Acknowledgement Sequence Number 104. 
-  Bob will weitere 3 Byte senden, im Feld Source Sequence Number steht 104
- Alice acknowledged mit Acknowledgement Sequence Number 107.

**Berechnung der neuen Seqenznummer:**
![[Pasted image 20241106143738.png]]
#### **3 Wege Handshake**
![[Pasted image 20250203123606.png]]
![[Pasted image 20250203123647.png]]
![[Pasted image 20250203123710.png]]
![[Pasted image 20250203123724.png]]
### <mark style="background: #FF5582A6;">Angriffe gegen TCP</mark>
- gut für DoS Angriffe geeignet
- Sogar große Server mit Load Balencern, etc. lassen sich mit DDoS Angriffen auf TCP gut in die Knie zwingen 
- aktuelle DoS Angriffe laufen zumeist gegen TCP.

**SYN-Flooding Angriff** (DoS)
	Senden einer großen Anzahl von SYN-Paketen. Diese werden vom Opfer mit einem SYN/ACK-Paket beantwortet und eine TCP-Verbindung wird reserviert. Der Angreifer antwortet auf die SYN/ACK-Pakete nicht, sodass der 3-Wege-Handshake nicht vollendet wird.
		1) Schicke TCP-Session Startup mit SYN flag
		2) Server schickt ACK + SYN
		3) => Angreifer Stoppt den 3 Wege Handshake und sendet kein ACK
	Die beim Opfer erzeugten halboffenen TCP-Verbindungen belegen Ressourcen, sodass nach einiger Zeit keine weiteren Verbindungen mehr angenommen werden können. Der reserviert Speicher für das Window kann vom Angreifer über TCP Options beeinflusst werden.
**Abwehr mittels SYN Cookies:**
	Das Transmission Control Protocol (TCP) macht keine Vorgaben zum initialen Wert der Sequenznummer der SYN/ACK-Pakete. Also kann der Server sie nutzen, um Informationen zu kodieren, die er sonst in einer Tabelle halboffener TCP-Verbindungen speichern müsste. Da es somit keine solche Tabelle gibt, kann sie auch nicht überlaufen, womit ein SYN-Flood-Angriff nicht zu einem DoS führen kann.
	Für die Durchführung einer erfolgreichen SYN Flood DoS Attacke bei aktivierten SYN Cookies, ist es nötig einen kompletten 3-Wege-Handshake auszuführen und über die Anzahl der Zugriffe den TCP Stack zum Überlaufen zu bringen.
	
**RST Angriff (Teardown-Attacke):** 
Senden eines TCP-Paketes mit gesetztem RST Flag, unter Beachtung:
- Pakete müssen zur Verbindung passen
- Absender-Port und Adresse fälschen
- Ziel-Port und Adresse passend wählen
als Schutz vor willkürlichen Resets werden die Sequenznummer der Pakete herangezogen:
- Sequenz-Nummer (ein 32-Bit-Wert) daher Wahrscheinlichkeit von 1 zu 232 die richtige Sequenznummer zu erraten
- Der TCP-Stack akzeptiert Sequenz-Nummern im Bereich der Window Size (maximale Window Size 64k)! -> daher lediglich 65.535 RST-Pakete durch Angreifer nötig

**TCP/IP-Spoofing:**
	Das Spoofen von IP-Adressen alleine stellt noch keinen Angriff dar, sondern ist i.d.R. nur ein Teil eines Angriffs. Es kann z.B. verwendet werden, um den Verursacher eines DoS-Angriffs zu verbergen. Kombiniert mit dem Erraten gültiger Sequenznummern und einem DoS-Angriff kann es von einem Angreifer verwendet werden, um sich in eine auf Basis der IP-Adresse authentifizierte Verbindung einzuschleichen. Statt nur Daten einzuschleusen, kann der Angreifer auch die Kontrolle über eine Verbindung übernehmen.

**TCP-Hijacking:**
	Angreifer Schaltet sich in eine bestehende Verbindung ein. Dazu belauscht (sniffed) er den Datenverkehr und übernimmt in einem geeigneten Moment die Kontrolle über eine ausgewählte Verbindung, indem er entsprechend manipulierte TCP-Pakete an die Kommunikationspartner sendet. Ziel eines solchen Angriffs ist z.B. die komplette Übernahme der Verbindung einschließlich Disconnect eines der Kommunikationspartner, das Einschleusen von Befehlen oder das Umgehen von Schutzmaßnahmen wie Authentifizierungen

 1. Vor dem Angriff: Sniffen
	 Belauschen (Sniffen) der Pakete mindestens eines der Opfer
 2. Verbindung stören:
	 erster Schritt des Angriffs, die Verbindung desynchronisieren. Eine Desynchronisation liegt vor, wenn die Sequenznummer eines empfangenen Pakets nicht mit einer erwarteten Sequenznummer übereinstimmt.
 3. Pakete einschleusen:
	 Nach der Desynchronisation kommt es zu einem 'ACK-Storm': Kommunikationspartner erkennen falsche Sequenznummern, verwerfen empfangenen Pakete, fordern mit ACK-Paketen an.
 4. Der Angreifer kann den ACK-Storm begrenzen, indem er selbst die falschen Pakete bestätigt (acknowledget).
 5. Der Angreifer kann als Vermittler arbeiten, indem er die Pakete 'übersetzt' (also gültige Sequenznummern einträgt) und ansonsten unverändert weiterleitet, oder aktiv in die Kommunikation eingreifen und die Nutzdaten nach seinen Wünschen manipulieren.
Beispiel:
Der Angreifer wartet bis zum gewünschten Zeitpunkt, z.B. nach dem erfolgreichen Telnet- Login, und startet dann den Angriff. Dazu stehen ihm mehrere Möglichkeiten zur Verfügung:
- Desynchronisation durch RST/SYN-Pakete: Angreifer antwortet an Stelle des Opfers auf ein SYN/ACK-Paket mit einem RST- und einem SYN-Paket
	- Desynchronisation durch Infiltration: Angreifer sendet Datenpaket mit gültiger Sequenznummer an Server, der es normal verarbeitet, danach ist die Verbindung desynchronisiert, da nächstes Paket des Opers mit bereits 'verbrauchter' Sequenznummer versehen
- Angriff ohne Rücksicht auf Verluste ("Take-No-Prisoners", "Simple Hijack"): entweder einschleusen ohne Tarnung oder Tarnung durch fingierte Fehlermeldung des angegriffenen Protokolls ('Session closed' o.ä.) und/oder ein FIN- oder RST-Paket an das Opfer. Auch eine Resynchronisierung der Verbindung kann den Angriff vertuschen.
- Übernahme während des Verbindungsaufbaus ("EarlyDesynchronization"): Angreifer antwortet an Stelle des Opfers auf ein SYN/ACK-Paket mit einem RST- und einem SYN-Paket während des 3 Wege Handshake 

**Port-Scanning**
Schicke eine Anfrage(SYN) an alle Ports des Zielsystems
- Antwort: Port ist aktiv (offen/open)
- keine Antwort: Port ist inaktiv (geschlossen/closed, gefiltert oder geblockt)
Selbst für alle 65635 Ports über das Internet in 7  Minuten möglich.
![[Pasted image 20250203124804.png]]

**OS-Fingerprinting**
	Die Erkennung entfernter Betriebssysteme durch OS-Fingerprinting
	geschieht für gewöhnlich durch eine Analyse des TCP/IP-Stacks.
	Dabei werden zunächst verschiedene, speziell vorbereitete
	Nachrichten an das Zielsystem gesendet, die die Options Felder im
	TCP/IP ausnutzen. Anhand des unterschiedlichen Antwort-Verhaltens verschiedener TCP/IP-Implementierungen in verschiedenen Betriebssystemen können Rückschlüsse auf das verwendete Betriebssystem gezogen werden. Durch eine Kombination verschiedener Tests lässt sich die Treffer Wahrscheinlichkeit verbessern.
**Ziele Service-Fingerprinting:**
- Anwendungsprotokoll ermitteln
- Server-Software herausfinden
- Release identifizieren

**Mittels Seqenz Nummer Offset**
 - durch senden doppelter Pakete mit richtiger Seq. an den Host -> Pakete mit Offset einfügen die im TCP Stack abgelegt werden 
Pakete weden im Stack nach Seq. sortiert und warten auf fehlende Pakete -> FIN an Host 

Session Hijacking Tools 
- Juggernaut
- Scapy -> zum erzeugen von Paketen
### Absicherung von Hijacking Angriffen
- Benutzung von Verschlüsselungen (IPSec)
- Benutzung von Verschlüsselungen (SSH)
- Limitierung von eingehenden Verbindunsmöglichkeiten
- Sensiblisierung vom Mitarbeitern (Manipulation von Sessions)

