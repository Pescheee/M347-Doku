
# Grafiksammlung

## GBSSG Gitlab:
https://gbssg.gitlab.io/m347/
https://gbssg.gitlab.io/m169/
## 🔥 Wichtige Docker-Befehle
| Befehl                                                 | Beschreibung                                                    |
| ------------------------------------------------------ | --------------------------------------------------------------- |
| `docker ps`                                            | 📋 Laufende Container anzeigen                                  |
| `docker ps -a`                                         | 📋 Alle Container (auch gestoppte) anzeigen                     |
| `docker images`                                        | 🖼️ Lokale Images auflisten                                     |
| `docker pull <image>`                                  | ⬇️ Ein Image aus der Docker-Registry herunterladen              |
| `docker run -d -p 8080:80 --name my-container <image>` | 🚀 Container starten (Port 80 im Container → 8080 auf dem Host) |
| `docker exec -it <container> bash`                     | 🔧 In einen laufenden Container springen                        |
| `docker logs <container>`                              | 📜 Logs eines Containers anzeigen                               |
| `docker stop <container>`                              | ⏹️ Container stoppen                                            |
| `docker start <container>`                             | ▶️ Gestoppten Container wieder starten                          |
| `docker restart <container>`                           | 🔄 Container neu starten                                        |
| `docker rm <container>`                                | ❌ Container löschen                                             |
| `docker rmi <image>`                                   | ❌ Image löschen                                                 |
| `docker system prune`                                  | 🧹 Unbenutzte Ressourcen (Container, Images, Volumes) aufräumen |
---
### 💾 **Docker Volumes (Daten persistent speichern)**
| Befehl                                                | Beschreibung                                             |
| ----------------------------------------------------- | -------------------------------------------------------- |
| `docker volume create my-volume`                      | 🆕 Neues Volume erstellen                                |
| `docker volume ls`                                    | 📋 Alle Volumes anzeigen                                 |
| `docker volume inspect my-volume`                     | 🔍 Details eines Volumes anzeigen                        |
| `docker volume rm my-volume`                          | ❌ Ein Volume löschen                                     |
| `docker run -d -v my-volume:/app/data <image>`        | 📂 Container mit Volume starten (Daten bleiben erhalten) |
| `docker run -d -v /host/path:/container/path <image>` | 📂 Lokalen Ordner als Volume einbinden                   |
---
### 🌐 **Docker Netzwerke (Kommunikation zwischen Containern steuern)**
|Befehl|Beschreibung|
|---|---|
|`docker network ls`|📋 Alle Netzwerke anzeigen|
|`docker network create my-net`|🌍 Eigenes Docker-Netzwerk erstellen|
|`docker network inspect my-net`|🔍 Details eines Netzwerks anzeigen|
|`docker network connect my-net <container>`|🔗 Container mit Netzwerk verbinden|
|`docker network disconnect my-net <container>`|❌ Container vom Netzwerk trennen|
|`docker network rm my-net`|❌ Netzwerk löschen|
---
### 🔌 **Ports & Exponierte Services**
| Befehl                             | Beschreibung                                      |
| ---------------------------------- | ------------------------------------------------- |
| `docker port <container>`          | 🌍 Zeigt die zugeordneten Ports eines Containers  |
| `docker run -d -p 8080:80 <image>` | 🚀 Port 80 (Container) → 8080 (Host) weiterleiten |
### 🚀 **Parameter**
| Flag        | Bedeutung                                                                                             | Beispiel                                         |
| ----------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| `-e`        | Setzt eine Umgebungsvariable im Container                                                             | `docker run -e "MY_VAR=value" image`             |
| `-p`        | Mappt Ports zwischen dem Container und dem Host-System                                                | `docker run -p 8080:80 image`                    |
| `-v`        | Bindet ein Volume (Datenordner) zwischen Container und Host                                           | `docker run -v /host/path:/container/path image` |
| `-d`        | Führt den Container im Hintergrund aus (detached mode)                                                | `docker run -d image`                            |
| `-it`       | Kombiniert `-i` (interaktiv) und `-t` (Terminal), um einen Container im interaktiven Modus zu starten | `docker run -it image bash`                      |
| `--rm`      | Entfernt den Container automatisch nach dem Stoppen                                                   | `docker run --rm image`                          |
| `--name`    | Gibt einen benutzerdefinierten Namen für den Container an                                             | `docker run --name my-container image`           |
| `--network` | Legt das Netzwerk fest, zu dem der Container gehört                                                   | `docker run --network host image`                |
| `-l`        | Listet Container anhand eines Labels                                                                  | `docker ps -l`                                   |
## Alle Commands der Aufgabe MariaDB:
Root Shell wechseln: ``sudo su
### Portainer installieren
```
sudo docker run -d --name jositest-portainer -p 9443:9443 portainer
```
### Container Mariadb starten
```Shell
sudo docker run -d --name jositest-mariadb -e "MYSQL_ROOT_PASSWORD=Riethuesli>12345" mariadb
```
```Shell
sudo docker inspect jositest-mariadb
```

## Mariadb mit PHPMyAdmin
```Shell
sudo docker run -d --name jositest-mariadb -e "MYSQL_ROOT_PASSWORD=Riethuesli>12345" mariadb
```
```Shell
sudo docker run -d --name jositest-phpmyadmin --link jositest-mariadb:db -p 9444:80 phpmyadmin
```

### Mariadb mit PHPMyAdmin & Eigenem Volume (Benanntes Volume)
```Shell
sudo docker run -d --name jositest-mariadb -v jositest-db:/var/lib/mysql -e "MYSQL_ROOT_PASSWORD=Riethuesli>12345" mariadb
```
```Shell
sudo docker run -d --name jositest-phpmyadmin --link jositest-mariadb:db -p 9444:80 phpmyadmin
```

### Mariadb in ein eigenes Verzeichnis legen (Bind-Mount)
```Shell
sudo docker run -d --name jositest-mariadb -v /home/jonas-sieber/docker/mariadb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD='Riethuesli>12345' mariadb
```
### **Eigenes Docker-Netzwerk mit MariaDB und PHPMyAdmin**
1. **Erstelle ein benanntes Docker-Netzwerk:**

```Shell
sudo docker network create jositest-net
```

2. **Starte MariaDB im benutzerdefinierten Netzwerk:**

```Shell
sudo docker run -d --name jositest-mariadb --network jositest-net -v jositest-db:/var/lib/mysql -e "MYSQL_ROOT_PASSWORD=Riethuesli>12345" mariadb
```

3. **Starte PHPMyAdmin im selben Netzwerk:**

```Shell
sudo docker run -d --name jositest-phpmyadmin --network jositest-net -p 9444:80 -e PMA_HOST=jositest-mariadb phpmyadmin
```
 
 