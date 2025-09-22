# LF10B-Project - Serverdienste bereitstellen und Administrationsaufgaben automatisieren

## Projektbeschreibung
Im Rahmen unseres Ausbildungsprojekts planen wir die Einrichtung einer Virtualisierungsumgebung mit Proxmox VE, die dauerhaft auf den Servern bestehen bleiben soll. Ziel ist es, eine stabile, skalierbare und wartbare Infrastruktur zu schaffen, die verschiedene Dienste wie Backup, Monitoring und Netzwerksegmente integriert. Die Umgebung soll nicht nur für die aktuelle Projektphase genutzt werden, sondern auch zukünftigen Jahrgängen als Grundlage dienen. So entsteht eine nachhaltige Lernplattform, die kontinuierlich erweitert und verbessert werden kann. Das Projekt wird umfassend dokumentiert und so gestaltet, dass spätere Automatisierung und Wartung problemlos möglich sind.

## Projektziele
Ziel des Projekts ist es, ein PVE (Proxmox Virtual Environment) auf einem Server zu installieren, um dessen Hardware durch Virtualisierung und Containerisierung optimal zu nutzen:
- Vertiefung der Kenntnisse in Virtualisierung
- Praktische Anwendung von Linux
- Aufbau einer eigenen IT-Infrastruktur zu Übungszwecken
- Verständnis für Monitoring-Systeme und Backup-Strategien
- Aufbau und Umgang mit Proxmox und ggf. Container-Technologien
- Verbesserung der Fähigkeiten in Projektplanung und Dokumentation
- Erlernen von Automatisierungen mit Tools wie Ansible oder Bash
- Vorbereitung auf zukünftige Abschlussprojekte
- Schaffung einer nachhaltigen Infrastruktur für zukünftige Jahrgänge

## Welche Dienste sollen als Teil des Projekts integriert werden?

### Primäre Aufgaben
- Backup-System (externe Storage-Box oder Festplatte)
- Monitoring-System (CheckMK und optional Grafana)

### Sekundäre Aufgaben
- Kubernetes-Cluster (TalosOS oder Alternative)
- Cloudflare Tunnel (externe Erreichbarkeit, habe noch eine Domain rumliegen)
- Automatisierung via Ansible
- High Availability (HA)

### Umgebung
- Proxmox VE (PVE)  
(Geplanter Vergleich Proxmox vs. OpenStack)

## Vergleich zwischen Proxmox VE und OpenStack
| Software    | Funktion | Vorteile | Nachteile | Einsatzbereich |
| :---        | :---     | :---     | :---      | :--- |
| OpenStack   | Cloud-Management-Plattform (IaaS): Compute, Storage, Netzwerk über viele Hosts | Sehr skalierbar, modular, für Public/Private Cloud, Open Source | Hohe Komplexität, hoher Ressourcenbedarf | Große Rechenzentren, Service Provider, Enterprise Clouds |
| Proxmox VE  | Virtualisierungsplattform mit Cluster, Storage, Backups | Einfach zu installieren, integrierte GUI, gute Container-/VM-Verwaltung, Open Source | Weniger skalierbar, nicht für Public-Cloud-Betrieb gedacht | Kleine bis mittlere Umgebungen, Labs, Unternehmens-IT |

### Hardware
- Server aus dem Labor (AFBB)
- Zusätzliche Festplatte, falls Kapazität fehlt
- Falls benötigt für Cluster-Setup: Umbau eines Laptops zum Server

### Betriebssysteme
- Basis: Debian 12 (Bookworm) oder Debian 13 (Trixie)

## Grobe Übersicht über das Projekt

### Monitoring
Proxmox bietet die Möglichkeit, VMs und LXC-Container zu erstellen; diese werden automatisch in das integrierte Basis-Monitoring eingebunden. Dieses kann Parameter wie CPU, RAM, Speicher, Netzwerk, Swap und mehr anzeigen. Laut Anforderungsanalyse reicht die integrierte Lösung jedoch nicht vollständig aus. Daher wird zusätzlich ein dediziertes Monitoring-System (z. B. CheckMK, optional mit Grafana) in einem Container bereitgestellt. Das integrierte Monitoring hilft dennoch bei ersten Einschätzungen (z. B. Lastspitzen von CPU oder RAM).

### Backups

Proxmox bietet die Möglichkeit Backups zu machen, und diese direkt mit unterschiedlichen Methoden zu verkleinern nach erfolgreichen Backup Job.
Beispiele.: 
LZO, GZIP, ZSTD
Auch werden unterschiedliche Methoden geboten wie:
Snapshot, Suspend, Stop
Die Backups werden via einer “Datacenter” Gruppe verwaltet in der man ganz einfach Backups für unterschiedliche LXC Container oder VM’s verwalten kann. Diese Funktion bietet auch an, dass eine gewisse Anzahl an Backups nur behalten werden soll und die älteren immer gelöscht werden. Damit kann sicher gestellt werden, dass die Backup-Location nicht ausversehen voll läuft. Die Backup Jobs können direkt in unterschiedlichen Zeiten geplant und ausgeführt werden.
Das sollte die Anforderungen des Backups Thema vollständig erfüllen.
Falls noch Zeit ist, wird eine weitere manuelle Backup Methode eingebaut die Daten via SMB/CIFS in eine Storage Box von Hetzner in ein Rechenzentrum sendet und lagert.

### Betriebssysteme 
-	Grundsystem Debian 12 bookworm oder Debian 13 trixie

## Mit welcher Software sollen die Dienste betrieben werden?
### Software 
-	Tailscale (externe Verbindung)
-	Cloudflare tunnel (Fernwartung)
-	Docker (Containerisierung)
-	TalosOS – Kubernetes (K8S Cluster)
-	OpnSense (Firewall)
-	Windows Server (Admin access auf interne Ressourcen)

### Monitoring
- CheckMK
- Grafana (optional in Kombination mit CheckMK)

### Backups
- Integrierte Proxmox-Backup-Funktionen

## Vergleich ausgewählter Software / Alternativen
| Software | Alternative | Zweck / Funktion |
| :---     | :---        | :--- |
| Tailscale | ZeroTier | Tailscale: Mesh-VPN (WireGuard, Identity-basiert); ZeroTier: virtuelles Layer-2/3-Netzwerk |
| Cloudflare Tunnel | ngrok | Cloudflare: dauerhafte, sichere Zero-Trust-Tunnel; ngrok: temporäre Tunnels für Tests |
| Docker | Podman | Docker: großes Ökosystem; Podman: rootless, daemonlos |
| TalosOS (K8s) | k3s | TalosOS: spezialisiertes Minimal-OS für sichere K8s-Cluster; k3s: schlanke K8s-Variante |
| OPNsense | pfSense | Beide: Open-Source-Firewalls mit Routing, VPN, IDS/IPS |
| Windows Server | Univention Corporate Server | Windows: AD / zentrale Dienste; UCS: Linux-basiert, AD-kompatibel |

## Recovery Time Objective (RTO)
Durch regelmäßige Backups soll eine möglichst geringe RTO erreicht werden. Optional kann ein HA-Setup die Ausfallzeit weiter minimieren.

## Welche Systemeigenschaften sollen vom Monitoring überwacht werden?
### Hardware
-	CPU-Auslastung 
-	Arbeitsspeicher (Nutzung, verfügbarer Speicher, Cache)
-	Festplatten (Auslastung, freier Speicher, I/O-Operationen)
-	Netzwerk (Bandbreite, Verbindung, Latenz)
-	Temperaturen (CPU ggf. GPU)

### Systemstatus
-	Systemlast (Load)
-	Uptime (Laufzeit seit letztem Neustart)
-	Benutzeraktivität (eingeloggte Benutzer, SSH-Verbindungen)

### Netzwerk / Kommunikation
- Offene Ports
- Firewall-Status
- DNS- und DHCP-Aktivität

## Umsetzung der Automatisierung
Automatisierung durch:
- Shell-Skripte
- Ansible (optional)
- Cronjobs
- Integrierte Proxmox-Automatisierungen

## Zeitplan
- 25.09. - Server aufsetzen
- 29.09. - Monitoring 
- 30.09. - Backup
- 17.11. - Automatisierungen
- 18.11. - Testen und Optimierung

## Aufgabenverteilung
- Server aufsetzen – alle gemeinsam
- Monitoring – [Nezzard22](https://github.com/Nezzard22) mit Unterstützung von [Tablette](https://github.com/Tablettenschrank)
- Backup – [KikeRicci](https://github.com/KikeRicci) mit Unterstützung von [Tablette](https://github.com/Tablettenschrank)
- Automatisierungen – alle gemeinsam
- Testen und Optimieren – alle gemeinsam

## Anleitung/Tutorials/Dokumentationen
-	Die Docu’s der jeweiligen Softwares
-	Google als Suchmaschine
-	AI wie ChatGPT, Gemini, Cloude Sonnet, und änhliches
