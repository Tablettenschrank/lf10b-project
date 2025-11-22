# Checkmk 



### Dokumentation: Checkmk Docker Container (LXC-basiert)

#### 1. Einführung

Dieses Dokument beschreibt die Einrichtung und grundlegende Konfiguration eines Checkmk Monitoring-Servers, der in einem Docker Container betrieben wird. Der Docker Container selbst läuft innerhalb eines LXC (Linux Containers) auf dem Hostsystem. Diese Struktur ermöglicht eine gute Isolation und Portabilität der Checkmk-Instanz.

* **Checkmk Version:** ("Checkmk Raw Edition 2.4.0p15" (:latest))
* **Zugriffs-IP:** `192.168.88.28:8080`
* **Architektur:** Checkmk Docker Container ➡️ LXC Container ➡️ Host System

#### 2. Voraussetzungen

Stelle sicher, dass die folgenden Komponenten auf dem Host-System und im LXC-Container installiert und konfiguriert sind:

* **Host-System:**
  * Betriebssystem: (z.B. Proxmox PVE 9.1 - Debian 13)
* **LXC Container:**
  * Betriebssystem: (Debian 12)
  * Docker Engine und Docker Compose installiert
  * Ausreichend Ressourcen zugewiesen:

    **CPU:** 2 Cores

    **RAM:** 2048 MB - 2 GIB

    **Speicher:** (z.B. 30 GB SSD)
  * Netzwerkkonfiguration für den Zugriff auf Port `8080` (ggf. Port-Weiterleitung im LXC oder Firewall-Regeln)

#### 3. Docker Container Setup

Der Checkmk Docker Container wird mittels Docker Compose im LXC Container gestartet.

##### 3.1 Docker Compose Datei (`docker-compose.yml`)

Erstelle oder bearbeite die `docker-compose.yml` Datei im entsprechenden Verzeichnis des LXC Containers (`~/docker-stuff/checkmk/docker-compose.yml`): /

```yaml
services:
  checkmk:
    image: "checkmk/check-mk-raw:2.4.0-latest"
    container_name: "monitoring"
    env_file:
      - .env
    environment:
      - CMK_PASSWORD=${CMK_PASSWORD}
      - TZ=Europe/Berlin
    volumes:
      - ./monitoring:/omd/sites
    tmpfs:
      - /opt/omd/sites/cmk/tmp:uid=1000,gid=1000
    ports:
      - 8080:5000
      - 8000:8000
    restart: always

volumes:
  monitoring:
```

Environment File: `~/docker-stuff/checkmk/.env`

```yaml
CMK_PASSWORD=Bier123*
```

* **`image`**: Verwende das offizielle Checkmk Raw Edition Image. **Passe die Version an deine Bedürfnisse an!**
* **`ports`**: Der Standard-Webserver-Port von Checkmk im Container ist `5000`. Dieser wird hier auf den Host-Port `8080` gemappt, der extern über `192.168.88.28:8080` erreichbar ist.
* **`volumes`**: Wichtig für die Persistenz der Checkmk-Daten. Alle Konfigurationen, Sites und Monitoring-Daten werden im Verzeichnis `~/host_adm/docker-stuff/checkmk/monitoring` auf dem LXC-Dateisystem gespeichert.

##### 3.2 Starten des Containers

Navigiere im LXC Container zu dem Verzeichnis, in dem die `docker-compose.yml` liegt, und starte den Container:

```shellscript
docker compose up -d
```

Überprüfe den Status des Containers:

```shellscript
docker compose ps
```

#### 4. Checkmk Konfiguration

Nach dem Start des Containers ist Checkmk verfügbar, muss aber initial eingerichtet werden.

##### 4.1 Grundlegende Einstellungen

Nach der Anmeldung kannst du die grundlegenden Einstellungen für deine Monitoring-Site vornehmen:

* **Host hinzufügen:** Füge die ersten Hosts hinzu, die du überwachen möchtest.
* **Agenteninstallation:** Installiere die Checkmk-Agenten auf den zu überwachenden Systemen.
* **Regelwerk:** Konfiguriere die Monitoring-Regeln entsprechend deinen Anforderungen.

#### 5. Zugriff auf Checkmk

Der Checkmk Web-Interface ist erreichbar unter:
`http://192.168.88.28:8080/`

**Login**:  
**User**: cmkadmin  
**Passwort**: `Bier123*`
