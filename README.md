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
-	Proxmox (PVE)
Vergleich Proxmox vs OpenStack

### Harware 
-	Server aus dem Labor aus der AFBB
-	Extra Festplatte falls nicht genug vorhanden im Server (ggf. eigene mitbringen)
-	Wenn benötigt als cluster Setup, laptop zu Serber umbauen

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

### Umgebung
-	Proxmox (PVE)
Vergleich Proxmox vs OpenStack

### Harware
-	Server aus dem Labor aus der AFBB
-	Extra Festplatte falls nicht genug vorhanden im Server (ggf. eigene mitbringen)
-	Wenn benötigt als cluster Setup, laptop zu Serber umbauen

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

## Welche Recovery Time Objective (RTO) soll erreicht werden?
Eine geringe RTO sollte sollte gegeben sein, durch die regelmäßigen Backups sollte in einem Fall der unterbrechung der Resourcen und corruption der Daten innerhalb der Zeit die es braucht zur Wiederherstellung recht gering sein.
(Auch möglich wäre ein HA System, da ist die RTO sehr gering)


## Welche Systemeigenschaften sollen vom Monitoring überwacht werden?
### Hardware
-	CPU-Auslastung (gesamt und pro Prozess)
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
-	AI wie ChatGPT, Gemini, Cloud Sonnet, und änhliches