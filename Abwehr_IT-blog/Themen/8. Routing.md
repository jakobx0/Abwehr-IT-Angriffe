### <mark style="background: #BBFABBA6;">Session Layer</mark> 

### **Grundlagen und wichtige Konzepte des Routings**
#### **Was ist Routing?**
- **Routing** ist der Prozess, bei dem **Datenpakete** von einem Quellnetzwerk zu einem Zielnetzwerk übertragen werden.
- Router oder andere Layer-3-Geräte bestimmen den besten Weg, den ein Paket nehmen soll, basierend auf Routing-Tabellen und Routing-Protokollen.
### **Wichtige Grundlagen und Begriffe**

1. **IP-Adressen und Subnetze**
    - Pakete werden zwischen Netzwerken basierend auf **IP-Adressen** (z. B. IPv4/IPv6) geleitet.
    - Subnetze (definiert durch Subnetzmasken, z. B. `/24`) bestimmen, welche IPs direkt im gleichen Netzwerk liegen.
2. **Routing-Tabelle**
    - Enthält Informationen über die erreichbaren Netzwerke und deren **Next-Hop**.
    - Bestandteile einer Routing-Tabelle:
        - **Zielnetzwerk (Destination)**: Das Netzwerk, zu dem ein Paket gesendet werden soll.
        - **Netzmaske**: Definiert den Bereich des Zielnetzwerks.
        - **Gateway/Next-Hop**: Die nächste Station, an die das Paket weitergeleitet wird.
        - **Schnittstelle (Interface)**: Die physische oder virtuelle Verbindung, die genutzt wird.
3. **Default Gateway**
    - Wenn ein Zielnetzwerk nicht in der Routing-Tabelle steht, wird der Datenverkehr an das **Default Gateway** weitergeleitet.
### **Arten des Routings**
1. **Statisches Routing**
    - Der Administrator konfiguriert Routen manuell.
    - Vorteil: Kontrollierbar, keine unerwarteten Änderungen.
    - Nachteil: Keine automatische Anpassung bei Netzwerkänderungen.
2. **Dynamisches Routing**
    - Router verwenden Routing-Protokolle, um automatisch Routen zu lernen und Änderungen anzupassen.
    - Beispiele für Protokolle:
        - **RIP (Routing Information Protocol):**
            - Distance-Vector-Protokoll, verwendet Hop Count als Metrik.
        - **OSPF (Open Shortest Path First):**
            - Link-State-Protokoll, basiert auf Kosten (Bandbreite, Verzögerung).
        - **BGP (Border Gateway Protocol):**
            - Verwendet für das Routing zwischen autonomen Systemen (z. B. zwischen ISPs).
        - **EIGRP (Enhanced Interior Gateway Routing Protocol):**
            - Hybridprotokoll, kombiniert Distance-Vector- und Link-State-Methoden.
### **Wichtige Konzepte im Routing**
1. **Metrik**
    - Bewertet die Qualität eines Pfades (z. B. Hop Count, Bandbreite, Verzögerung).
    - Der Router wählt den Pfad mit der niedrigsten Metrik.
2. **Next-Hop**
    - Der nächste Router auf dem Weg zum Zielnetzwerk.
3. **Administrative Distance (AD)**
    - Priorisiert Routen basierend auf ihrer Vertrauenswürdigkeit:
        - **Statische Routen**: AD = 1 (höchste Priorität).
        - **OSPF**: AD = 110.
        - **RIP**: AD = 120.
4. **CIDR (Classless Inter-Domain Routing)**
    - Ermöglicht die flexible Zuweisung von IP-Adressbereichen und die Aggregation von Routen (z. B. `192.168.0.0/22` anstelle von vier Subnetzen).
5. **Load Balancing**
    - Verteilung des Datenverkehrs auf mehrere gleichwertige Pfade.
6. **Route Redistribution**
    - Erlaubt das Teilen von Routen zwischen unterschiedlichen Routing-Protokollen (z. B. OSPF und RIP).
7. **Policy-Based Routing (PBR)**
    - Erzwingt Routing basierend auf benutzerdefinierten Regeln (z. B. nach Quelle oder Dienst).
### **Prozesse im Routing**
1. **Empfangen eines Pakets**
    - Der Router überprüft die Ziel-IP-Adresse im Paket.
2. **Entscheidung anhand der Routing-Tabelle**
    - Router prüft, ob das Ziel im gleichen Subnetz liegt:
        - **Ja:** Weiterleitung innerhalb des lokalen Netzwerks.
        - **Nein:** Weiterleitung an das Gateway oder den Next-Hop.
3. **Forwarding**
    - Der Router passt die MAC-Adresse (Layer 2) an und leitet das Paket weiter.
    - Die IP-Adresse (Layer 3) bleibt unverändert.
### **Routing-Sicherheitsaspekte**
1. **Access Control Lists (ACLs)**
    - Regeln, um Datenverkehr basierend auf Quelle, Ziel oder Protokoll zu erlauben oder zu blockieren.
2. **Route Filtering**
    - Bestimmte Routen werden blockiert oder bevorzugt (z. B. in BGP).
3. **VLANs und Routing**
    - Kommunikation zwischen VLANs erfolgt über einen Layer-3-Switch oder Router.
4. **Redundanz**
    - Implementierung von Protokollen wie HSRP oder VRRP, um einen Ausfall des Gateways zu vermeiden.
### **Zusammenfassung:**
- **Routing** ist essenziell, um Datenpakete effizient und sicher zwischen Netzwerken zu leiten.
- **Statisches Routing** bietet Kontrolle, während **dynamisches Routing** Flexibilität und Anpassung an Netzwerkänderungen ermöglicht.
- Routing-Protokolle wie RIP, OSPF, und BGP sind zentrale Werkzeuge in der Netzwerktechnik.
- Sicherheitsmaßnahmen wie ACLs, Route Filtering und Redundanz erhöhen die Verlässlichkeit und Integrität von Routing-Setups.