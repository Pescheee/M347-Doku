# Lernziele 1. Prüfung Modul 347

## Thema: Container Grundlagen, `docker run`, Docker-Images mit Dockerfile

### Lösungen und Antworten zu den Lernzielen

#### **Grundlagen**
- **Containerisierung:** Eine Technik zur Virtualisierung auf Betriebssystemebene, bei der Anwendungen in isolierten Umgebungen („Containern“) ausgeführt werden.
- **Image:** Eine unveränderliche Vorlage für einen Container, die alle notwendigen Abhängigkeiten enthält.
- **Layer:** Images bestehen aus mehreren Schichten (Layers), die übereinander liegen und gemeinsam ein vollständiges Image bilden.
- **Container:** Eine laufende Instanz eines Images.
- **Repository:** Eine Sammlung von Images mit verschiedenen Versionen (Tags).
- **Registry:** Eine Plattform zum Speichern und Verteilen von Container-Images (z. B. Docker Hub).
- **Dockerfile:** Eine Textdatei mit Anweisungen zur Erstellung eines Docker-Images.

#### **Docker-Engine Architektur**
- **Client:** Nimmt Befehle entgegen und kommuniziert mit dem Docker-Daemon.
- **Docker-Daemon:** Verwaltet Container, Images, Netzwerke und Volumes.
- **REST API:** Schnittstelle zwischen Client und Daemon.
- **Container Runtime:** Führt Container aus (z. B. runc für Docker).

#### **Container Status**
- **Created:** Container wurde erstellt, aber nicht gestartet.
- **Running:** Container läuft.
- **Paused:** Container ist eingefroren.
- **Stopped:** Container wurde gestoppt.
- **Exited:** Container wurde beendet.
- **Removing:** Container wird gelöscht.

---

### **Praktische Lernziele und Lösungen**

#### **docker run**
- **Container erzeugen:** `docker run ubuntu`
- **Container starten:** `docker start container_id`
- **Container stoppen:** `docker stop container_id`
- **Container löschen:** `docker rm container_id`
- **Wichtige `docker run` Optionen:**
    - `--name=mycontainer` → Setzt einen Namen für den Container.
    - `--rm` → Löscht den Container nach Beenden automatisch.
    - `--network=my_network` → Verbindet den Container mit einem bestimmten Netzwerk.
    - `--ip=192.168.1.100` → Setzt eine feste IP-Adresse.
    - `-d` → Startet den Container im Hintergrund (detached mode).
    - `-it` → Interaktiver Modus mit Terminal.
    - `-p 8080:80` → Leitet Port 8080 des Hosts auf Port 80 im Container weiter.
    - `-v /host/path:/container/path` → Bind-Mount eines Volumes.
    - `-e VAR=value` → Setzt Umgebungsvariablen.

- **Befehl im laufenden Container ausführen:**  
  `docker exec -it container_id bash`

- **Container mit bestimmtem Image-Tag starten:**  
  `docker run nginx:1.21`

- **Unbenannte, benannte und gemountete Volumes nutzen:**
    - Unbenannt: `docker run -v /data ubuntu`
    - Benannt: `docker run -v mein_volume:/data ubuntu`
    - Gemountet: `docker run -v /pfad/auf/host:/pfad/im/container ubuntu`

- **Docker-Netzwerke einrichten und Container zuweisen:**
  ```sh
  docker network create mein_net
  docker run --network=mein_net --name=container1 ubuntu
  docker run --network=mein_net --name=container2 nginx
  ```

- **Unbekannte Images anhand der offiziellen Dokumentation verwenden:**
    1. `docker search <image_name>` → Image suchen
    2. `docker pull <image_name>` → Image herunterladen
    3. Offizielle Dokumentation auf Docker Hub lesen
    4. `docker run <image_name>` mit den benötigten Optionen ausführen

---

### **Docker-Images und Dockerfiles**

- **12 Dockerfile-Anweisungen:**
    1. `FROM` → Basis-Image
    2. `RUN` → Befehl ausführen
    3. `CMD` → Standard-Befehl für Containerstart
    4. `ENTRYPOINT` → Alternative zu CMD mit festen Argumenten
    5. `COPY` → Dateien ins Image kopieren
    6. `ADD` → Wie COPY, aber mit zusätzlichen Features (z. B. Entpacken von Archiven)
    7. `ENV` → Umgebungsvariablen setzen
    8. `EXPOSE` → Port im Container öffnen
    9. `VOLUME` → Datenvolumen definieren
    10. `WORKDIR` → Arbeitsverzeichnis setzen
    11. `USER` → Benutzer für Containerprozesse setzen
    12. `LABEL` → Metadaten hinzufügen

- **Unterschied `ENTRYPOINT` vs. `CMD`**
    - `CMD` → Standardbefehl, kann durch `docker run` überschrieben werden.
    - `ENTRYPOINT` → Setzt festen Startbefehl, Parameter können ergänzt werden.

- **Unterschied `COPY` vs. `ADD`**
    - `COPY` → Kopiert Dateien ins Image.
    - `ADD` → Kopiert und entpackt Archive, kann URLs als Quelle nutzen.

- **Eigenes Docker-Image erstellen:**
    1. Dockerfile erstellen:
       ```dockerfile
       FROM ubuntu
       RUN apt-get update && apt-get install -y nginx
       CMD ["nginx", "-g", "daemon off;"]
       ```
    2. Image bauen: `docker build -t mein_image .`
    3. Container starten: `docker run -d -p 8080:80 mein_image`

---
