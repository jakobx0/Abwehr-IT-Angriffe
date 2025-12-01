#### Grundlagen
- Datenübertragung von Web und Dienste über http in Layer7
- zentrale Bestandteile des Internets
- Webanwendungen nicht auf Gerät installiert
- interagieren ausschließlich mit Server
#### http-Protokoll
- Übertragung von daten über ein Netz
- basiert auf TCP/IP -> Layer 7
- Anfrage Antwort (ASCII-format)
- Nachichtenstruktur
	- Header -> mit Metainfo
	- Payload -> mit eigentlichen Inhalt
#### Kommunikation
1. **http-Connect:** Client baut TCP/IP-Verbindung zu HTTP- Server über dessen TCP-Port 80 auf
2. **http-Request:** Client sendet Anfrage an Server und fordert über URL adressierte HTML-Datei an.
3. **http-Response:**  Server empfängt eingehenden HTTP- Request und überträgt adressierte HTML-Datei an Client oder antwortet mit Fehlermeldung.
4. **http-close:** Falls weder Client (HTTP-Request) noch Server (HTTP-Response) Aufrechterhaltung TCP/IP- Verbindung erbeten, z.B. um weitere Bilder der Webseite herunterzuladen, baut Server Verbindung zum Client ab.

![[Pasted image 20250201164400.png]]
#### Anfragemethode
- *GET*: Abfrage von Daten
- *POST*: Datenänderung/Übermittlung
- ...
### Bausteine von Webanwendungen
- **Unterteilung in:**
	- Cient -> ruft auf
	- Server -> stellt zurferügung
- **Frondend**:
	- Zugriff auf Anwendung bereit gestellten code (bsp. js)
	- beliebig veränder und ergänzbar
- **Serverseitiges Backend:**
	- zur Verwaltung und Administration der Awendung und Bereitstellung einer Anbindung an Dateisysteme, DB und Dienste
	- *Servrseitiger Code (PHP) nur vom Server usgeführt*
- Drei Schichten-Architektur
	- View layer
		- HTML, CSS ...
	- Geschäftslogik / Business Logic Layer
		- Verarbeitung von Client- und Serverdaten mit serverseitiger Programmiersprache wie bspw. PHP, Ruby, Perl, JAVA
	- Datenhaltung/ Data Access Layer
		- DBs -> MySQL, PostregsSQL...
### Document Object Model
- Spetzifikation -> W3C-Standart
- Dastellung in Baumstruktur
![[Pasted image 20241211143111.png]]
### Document Object Model und Same Origin Policy (SOP) 
 SOP verhindert Inhalte von Anderen Servern nachzuladen!
- sonst -> <frames.bank_frame.document.getElementById("Kontostand").value>

## <mark style="background: #FF5582A6;">Zustandsbasierte angriffe</mark>
- Übertragungs Protokoll http -> zustandslos
- Seite wird ohne Informationen, von wo der Benutzer kam und wohin er gehen kann
- Nachverfolgung der Aktionen eines Benutzers und deren Verknüpfung (Zustandsinformationen) durch Webanwendung selbst
- Zusammenfassung mehrerer Requests eines Benutzers als Session (Sitzung)
- Identifizierung eins Benutzer mit einer Session-ID (Sitzungs-ID), zur Wiedererkennung nach mehreren Request

**Verwaltung der Zustandsinfo durch Webanwendung**
- Server speichert Identifikation Benutzer über Session-ID
- auf dem Client gespeichert

Unabhängig vom Ansatz müssen Informationen auf Client gespeichert werden – entweder Zustandsinformationen oder Session-ID

**Methoden:**
- versteckte Formularfelder -> ``<input name="id" value="1234" type="hidden">
- Parameter in der URL -> http://www.webanwendung.example/editprofil.cgi?id=456
- Cookies

Nutzung von Zustandsinformationen um Code einzuschleusen oder Sessions übernehmen:
#### Cookie Poisoning
- Änderungen von Cookie Inhalten zur Manipulation von Session ID‘s oder auch das Verfallsdatum
- Bei fehlender Plausibilitätsprüfung werden geänderten Einträge genutzt
#### URL Jumping
- Angreifer verlässt vorgesehene Reihenfolge der Web-Seiten und nutzt Funktionen, die noch garnicht zugänglich sein sollten
- Beispiel: Ändern von Bestellinformationen (Bestellmengen) nach dem Bezahlvorgang
#### Session Hijacking
- Ausspähen Session ID eines Benutzers und danach als dieser Benutzer auszugeben (Cookie, URL ID, verstecktes Feld)
- Spezialfall des Session Hijacking ist „Authorization bypass“ 
#### Session Fixation
- Angreifer erstellt eigene Session mit eigener Session ID
- Session ID des Angreifers wird von einem anderen Benutzer authentifiziert
- Angreifer hat Zugriff auf Daten des Benutzers in eigener Session (Bsp: Bezahlvorgänge) und „arbeitet“ in seinem Namen
### <mark style="background: #FF5582A6;">SQL Injection</mark>
SQL-Injection : Einschleusen von eigenen Datenbankstatements in eine Webanwendung

- Basierend auf dynamisch zusammengesetzten SQL-Anweisungen aus Benutzereingaben und vorgefertigten Statements
- Überprüfung mittels JavaScript getätigte Eingaben vor Weitergabe an Datenbank
- Oft Vergessen auch die Serverseite vor unerwünschten Abfragen zu schützen
- Abfangen von Übertragungen an den Web Server und in verfälschter Form weitergeben

Besonderheit bei Verarbeitung einer SQL-Anweisung: **SQL Interpreter** erhält Eingaben direkt und führt entsprechende Anweisung aus .
Beispiel:
			```
			String query = "SELECT name FROM kunden WHERE kNr = $_GET['input']";
			```
Sucht Benutzer nach Kunden mit Kundennummer 142, sieht erstellte SQL-Anweisung so aus:
			```
			String query = "SELECT name FROM kunden WHERE kNr = 142";```

Webanwendung sendet Query in EINER Stringvariablen gespeichert zur Verarbeitung an den SQL Interpreter.
#### Angriffsmethoden
Setzen meistens innerhalb der WHERE-Klausel an:
- Platzierung immer wahrer Anweisungen, können Datenextrahieren
- Häufig in Kombination, in der hinderliche Befehle für die Ausführung der SQL Injektion auskommentiert werden.
	![[Pasted image 20250201172314.png]]
#### Tautlogies
- Tautologien sind immer wahr Aussagen. Zusammensetzung so, dass ihre Bedingungen immer   werden, unabhängig von nicht erfüllten Teilbefehlen (auch SQL-Tautologien genannt).
- Angriffsziele: Datenextraktion, Umgehung der Authentifizierung, Schwachstellenidentifizierung
#### Illegal/Logical Incorrect Queries
- Anhand zurückgegebener Fehlermeldungen lassen sich Informationen über Datenbank und Struktur abgreifen. Können potentiellen Angreifer helfen, verwundbare Stellen innerhalb Datenbanksystems zu ermitteln.
- Angriffsziele: Datenbankschema und Schwachstellen identifizier
#### Union Queries
- Verkettung Ergebnisse schadhafter und normaler SQL-Abfragen mit Hilfe UNION, um Daten anderer Tabellen zu erhalten. Beide Anweisungen benötigen exakt selbe Struktur Tatsächliche Tabellennamen müssen bekannt sein, sowie die Datentyp und Anzahl der Originalabfrage zurückgegebenen Spalten.
- Angriffsziele: Datenextraktion, Umgehung der Authentifizierung
#### Stored Procedures
- Zusätzliche Abstraktionsschicht, die es Anwendern erlaubt, eigene, komplexe SQL-Anweisungen im Datenbanksystem vorzuhalten. IdR. aus mehreren Anweisungen bestehend, nach Speicherung, genauso abrufbar wie eingebaute Systemfunktionen. Nach Herausfinden des Datenbanktypen auch versuchen Stored Procedures zu ermitteln.
- Angriffsziele: Remote-Code-Ausführung, Privilegien-Eskalation
#### Alternative Encoding Obfuscation
- Methode zur Erschwerung der Entdeckung einer Injektionsattacke durch Verwendung einer alternativen Kodierung (hexadezimal, ASCII, Unicode) durch Erkennungsregeln von Web Application Firewalls (WAF). Kombinierbar mit den verschiedensten Angriffsarten.
- Angriffsziele: Verschleierung, WAF-Täuschung
#### Piggy-backed Queries
- Ursprüngliche, erste SQL-Anweisung nicht verändert, sondern hinter Query-Demilimiter „;“ durch weitere Anweisung ergänzt. Beide Anweisungen können auf Datenbank ausgeführt. Angriff nur möglich, wenn mehrere Abfragen innerhalb einer Anweisung erlaubt
- Angriffsziele: Datenextraktion, Datenmanipulation, Remote-Code-Ausführung
#### Schutzmaßnahmen
- **Escaping:** -> Steuerzeichen maskieren
- **Input Validation**: Validierung von Benutzer Eingaben
- **Prepared Stratments:** Parametrisierter Aufruf vorbereiteter SQL-Anweisungen
- **Stored Procedures**: Gespeicherte Prozeduren
Ein guter Grundsatz ist, dass mindestens zwei und im besten Fall alle der hier genannten
Ansätze implementiert werden sollten
### Cross Site Scripting (XSS)
- Einschleusen schadhafter Skripts in Webanwendungen.

**Charakteristik:**
- Ausführung Skripte auf Nutzerseite und Ausnutzung von Sicherheitslücken, um fremden Code ungeprüft zu übernehmen
- Richten sich gegen Clienten der Webanwendung als Grundlage für weitere Angriffe
- Dienen oft Übernahme digitaler Identitäten (Cookies, Tokens), um an sensible Informationen zu gelangen.
- Je nach Rechten der Identität kann gesamte Webanwendung manipuliert werden
**Angriffsvektor:**
- Wie bei SQL Injections nicht oder unzureichend gefilterte Benutzereingaben
**Ziel:**
- Angreifer auf Clientseite wird in Lage versetzt, schadhaften Code in Webseite einzuschleusen, der durch anderen Client wieder ausgeführt wird
	- Angriffswege nicht auf klassische Webseiten und Eingabefelder beschränkt
	- Schädliche Links auch in Onlineforen oder Apps
	- Schadcode ebenso in Emails einbettbar

**Formen:**
- **Persistentes XSS:** serverseitige Cross-Site-Scripting-Variante Schadcode permanent auf Server (gewöhnlich in Datenbank) der Webanwendung)
- **Reflektiertes XSS:** Schadcode nicht auf Server der Webanwendung, sondern Request mit Schadcode nach Linkaufruf an Webserver gesendet und ausgeführt
- **DOM-basiertes XSS / lokales XSS**: clientbasierter nicht-persistenter Angriff, Manipulation URL-Parameters und Einschleusung direkt in Browser des Clients ohne Nutzung Webserver. Eingeschleuste Code ändert in schadhafter Weise Struktur des DOM.

**Schutzmaßnamen**
Schwierigkeit Implementierung geeigneter Schutzmaßnahmen liegt nicht in den
Schutzmaßnahmen selbst. Bei Entwicklung von Webanwendungen gilt es alle Stellen zu
identifizieren, an denen sich Schwachstellen befinden die ausgenutzt werden können
Die meistverwendeten Schutzmaßnahmen sind:
- Eingabefilterung: Benutzereingaben filtern und unerwünschte Eingabedaten säubern
- Ausgabekodierung: Benutzerinhalte, die in HTTP-Antworten ausgeliefert werden, so kodieren, dass sie beim Empfänger nicht ausgeführt werden
- Escaping: Potenziellen Schadcode maskieren, sodass er nicht ausgeführt wird
- Security Header: Überprüfung der übertragenen Daten durch den Browser
- Content Security Policy: Verwendung von Skripten beschränken
### Cross Site Request Forgery
- Angreifer nutzt harmlos aussehende website um Browser des Benutzers anfragen an verwundbare Webanwendungen senden zu lassen
Vorraussetzungen:
- verwundbare Webanwendung
- bösartige Website für Weiterleitung
- Authe. Benutzer in verwundbaren Webanwendungen
- Angreifer tritt nicht mit eigener Indentität in erscheinung 