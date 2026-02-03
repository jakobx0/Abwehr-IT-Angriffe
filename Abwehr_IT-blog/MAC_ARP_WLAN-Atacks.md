# MAC/ARP/WLAN – Angriffe (Obsidian)

> [!info] ARP / Layer‑2
> Kurzüberblick der wichtigsten Angriffe auf Switch/ARP.

> [!danger] MAC‑Flooding (Switch als Hub)
> CAM‑Tabelle mit vielen gefälschten MACs füllen → Fail‑Open → Mitschnitt möglich.

> [!danger] ARP‑Spoofing (MITM)
> Gefälschte ARP‑Replies vergiften ARP‑Cache → Angreifer wird als Gateway/Host eingetragen.

> [!danger] ARP‑Flooding (DoS/Abgriff)
> - ARP‑Request‑Flood → Cache/Netz überlastet.
> - ARP‑Response‑Flood → falsche MAC verteilt → Kommunikation gestört/umgeleitet.
> - Variante: Broadcast‑MAC am Gateway → Frames im LAN für Mitschnitt.

> [!info] WLAN / Wi‑Fi
> Wichtige Angriffe auf WLAN‑Verbindungen.

> [!danger] Deauthentication‑Attacke
> Clients trennen → Verfügbarkeit angreifen, Handshake erzwingen, Hidden SSID sichtbar machen.

> [!danger] Packet Injection
> Frames aktiv einspeisen → MitM/DoS/Manipulation.

> [!danger] WEP‑Cracking (ARP‑Replay)
> IV‑Sammlung + Injektion → Schlüssel berechenbar.

> [!danger] WPA/WPA2‑PSK‑Cracking
> 4‑Way‑Handshake mitschneiden → Offline‑Brute‑Force.

> [!danger] KRACK (WPA2 Implementierung)
> Key‑Reinstallation → Wiederverwendung/Key‑Nulling möglich.

> [!danger] WPA2‑Enterprise Evil‑Twin
> Fake‑AP → Challenge‑Response abgreifen → Offline‑Cracking.

> [!danger] WPS‑PIN‑Angriff
> PIN in 2 Teile → effektiv ~11.000 Versuche.

> [!danger] Default‑Passwort‑Berechnung
> Hersteller‑Algorithmus bekannt → Passwort aus SSID/MAC.

> [!danger] Evil‑Twin (offene WLANs)
> Probe‑Requests ausnutzen → Fake‑AP → MitM.

> [!danger] WPA3 / Dragonblood
> SAE sicherer, aber Downgrade/Implementierungsrisiken.
