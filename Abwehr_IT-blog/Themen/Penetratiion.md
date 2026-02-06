> [!abstract] Kurzüberblick
> Pentesting ist das **legale Eindringen** in Systeme/Netze, um **Schwachstellen zu identifizieren, zu bewerten und nachzuweisen**. Im Fokus: klarer Prozess von Vorbereitung bis Abschlussanalyse.

> [!warning] Legalität & Rahmen
> Ein Pentest braucht **explizite Genehmigung**, klare **Rahmenbedingungen**, definierte **Zeiträume** und ein **Notfall-/Störungsprozedere**.

## Begriffe & Rollen
> [!note] Begriffsklärung
> **Penetration Tester / Ethical Hacker** testen Systeme kontrolliert im Auftrag. Im Team-Setup: **Red Team** (Angriff), **Blue Team** (Verteidigung).
## Klassifikation von Pentests
> [!summary] Kategorien
> - Informationsbasis: Black-Box, White-Box, Grey-Box
> - Aggressivität: passiv, vorsichtig, abwägend, aggressiv
> - Umfang: fokussiert, begrenzt, vollständig
> - Vorgehensweise: verdeckt, offensichtlich
> - Technik: Netzwerk, physisch, Social Engineering
> - Ausgangspunkt: von innen, von außen
## Phasenmodell
> [!summary] Phasen
> 1. Vorbereitung
> 2. Informationsbeschaffung (Pre-Exploitation)
> 3. Bewertung der Informationen
> 4. Aktive Angriffe (Exploitation)
> 5. Abschlussanalyse (Post-Exploitation)
## Vorbereitung
> [!important] Rahmenbedingungen
> - Betroffene Systeme und Personen klären
> - Genehmigungen sichern
> - Zeitfenster und Vorlauf planen
> - Zugangsberechtigungen organisieren
> - Vorgehen bei Störungen/Notfällen definieren

> [!tip] Ziele & Aufwand
> - Infrastruktur- vs. Einzeltests
> - Module nach Aufgabe (z.B. Web App Tests)
> - Schutzbedarfsfeststellung
## Informationsbeschaffung (Footprinting / OSINT)
> [!info] Typische Quellen
> - Suchmaschinen, Google Hacking
> - Shodan, Netcraft, Robtex
> - Internet Archive
> - Soziale Netze
> - Social-Engineer Toolkit (SET)

> [!note] Tool
> **Maltego**: Visualisiert Beziehungen zwischen Personen, Systemen und Netzen.
## Scanning & Enumeration (Pre-Exploitation)
> [!warning] Typische Prüfungen
> - ARP-Scans
> - Port-Scans
> - SNMP-Scans (UDP 161)
> - Unsichere Dienste (z.B. VNC ohne Auth)
> - Windows-Infos über NetBIOS/SMB
> - SSH-Identifikation (Port 22, Versionen/Optionen)

> [!example] Typische Ergebnisse
> - IP-Liste der Systeme
> - Service- und Versionsinfos
> - Lockout-Mechanismen
> - Auffällige Reaktionszeiten
> - OS-Abschätzung

> [!tip] Zusatz
> Unbekannte Ports werden manuell verifiziert (z.B. Netcat). SSL-Dienste separat prüfen.

![[pdf/14_pentesting_gekuerzt/slide-08.png]]
![[pdf/14_pentesting_gekuerzt/slide-09.png]]
![[pdf/14_pentesting_gekuerzt/slide-10.png]]
![[pdf/14_pentesting_gekuerzt/slide-11.png]]
![[pdf/14_pentesting_gekuerzt/slide-12.png]]

## Automatisierung: Security-Scanner
> [!info] Zweck
> Automatisierte Prüfung auf bekannte Schwachstellen.

> [!note] Beispiele
> Nessus, Saint, OpenVAS, Nexpose.

> [!note] OpenVAS
> Liefert **Severity** (CVSS) und **QoD** (Quality of Detection).

![[pdf/14_pentesting_gekuerzt/slide-13.png]]
![[pdf/14_pentesting_gekuerzt/slide-14.png]]

## CVSS – Schwachstellenbewertung
> [!summary] Kernaussagen
> - Wertebereich **0.0–10.0**
> - Ab **7.0**: hohe Gefahr
> - Ratings: None, Low, Medium, High, Critical

> [!example] CVSS-Formel (aus der Folie)
> $$\text{Exploitability} = 20 * Zugriff * Schwierigkeit * Anmeldung$$
> $$\text{Impact} = 10.41 * (1 - (1-C) * (1-I) * (1-A))$$
> $$f(Impact) = 0 \text{ wenn Impact=0, sonst } 1.176$$
> $$\text{BaseScore} = \text{auf 1 Nachkommastelle runden}(((0.6*Impact)+(0.4*Exploitability)-1.5) * f(Impact))$$

> [!info] CVSS v3.1 Parameter
> Angriffsvektor, Angriffskomplexität, Privilegien, Nutzerinteraktion, Vertraulichkeit/Integrität/Verfügbarkeit, Scope.

![[pdf/14_pentesting_gekuerzt/slide-15.png]]
![[pdf/14_pentesting_gekuerzt/slide-16.png]]
![[pdf/14_pentesting_gekuerzt/slide-17.png]]
![[pdf/14_pentesting_gekuerzt/slide-18.png]]

## Schwachstellen-Kataloge
> [!note] Datenbanken
> - **CVE**: eindeutige IDs für bekannte Schwachstellen
> - **CWE**: Kategorien von Schwachstellen (Weakness Types)
> - **CAPEC**: Angriffsmuster (Mechanismen und Domänen)

![[pdf/14_pentesting_gekuerzt/slide-19.png]]
![[pdf/14_pentesting_gekuerzt/slide-20.png]]
![[pdf/14_pentesting_gekuerzt/slide-21.png]]

## Exploitation-Phase
> [!important] Grundidee
> Exploits nutzen identifizierte Schwachstellen aus. **Zero-Days** sind ungepatchte Schwachstellen.

> [!info] Frameworks
> - Metasploit (Rapid7)
> - Canvas (Immunity)
> - Core Impact (Core Security)

> [!example] Ablauf (hochlevel)
> 1. Findings aus Vulnerability-Scan analysieren
> 2. Exploit-Möglichkeiten identifizieren
> 3. Exploit/Payload wählen und testen
> 4. Zugriff nachweisen

![[pdf/14_pentesting_gekuerzt/slide-22.png]]
![[pdf/14_pentesting_gekuerzt/slide-23.png]]
![[pdf/14_pentesting_gekuerzt/slide-24.png]]

## Post-Exploitation / Lateral Movement
> [!warning] Ziel
> Nach Erstzugriff **Seitwärtsbewegung** im Netzwerk und Ausweitung der Kontrolle.

> [!note] Tools / Konzepte
> - Metasploit (Meterpreter)
> - Empire Framework: Listener, Stager, Launcher, Agent
> - Persistente, verschlüsselte Kommunikation

![[pdf/14_pentesting_gekuerzt/slide-25.png]]
![[pdf/14_pentesting_gekuerzt/slide-26.png]]
![[pdf/14_pentesting_gekuerzt/slide-27.png]]
![[pdf/14_pentesting_gekuerzt/slide-28.png]]

## Abschluss
> [!summary] Quintessenz
> Pentests verbinden manuelle und automatisierte Methoden, um **Schwachstellen systematisch aufzudecken** und **Risiken sichtbar** zu machen.

![[pdf/14_pentesting_gekuerzt/slide-40.png]]
