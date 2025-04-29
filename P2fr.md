## Dockerfile

Ein **Dockerfile** ist eine Textdatei, mit der ein Docker-Image automatisiert gebaut werden kann. Es enthält die Anweisungen, wie ein Container aufgebaut werden soll – z. B. welches Basis-Image verwendet wird, welche Software installiert wird und was beim Start ausgeführt wird.

### Beispiel: Einfaches Dockerfile

```Dockerfile
# Basis-Image
FROM node:18

# Arbeitsverzeichnis erstellen
WORKDIR /app

# Abhängigkeiten installieren
COPY package*.json ./
RUN npm install

# Projektdateien kopieren
COPY . .

# Container-Startbefehl
CMD ["node", "index.js"]
```

Ein Dockerfile muss im selben Verzeichnis wie die Projektdateien gespeichert werden.

### Image bauen und Container starten

```bash
docker build -t meine-node-app .
docker run -p 3000:3000 meine-node-app
```

---

## Docker Compose

Mit **Docker Compose** kann man mehrere Container in einer einzigen Konfigurationsdatei (`docker-compose.yml`) definieren und gemeinsam starten. Dies ist besonders hilfreich für komplexe Umgebungen mit mehreren Services (z. B. Webserver + Datenbank).

### Beispiel: docker-compose.yml

```yaml
version: "3.8"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: geheim
      POSTGRES_DB: mydb
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

### Compose starten und stoppen

```bash
docker-compose up -d
docker-compose down
```

---

## Aufbau einer YAML-Datei

Eine YAML-Datei ist eine textbasierte Konfiguration mit **Einrückung durch Leerzeichen**. Sie ist **strukturiert**, **lesbar** und wird in Docker Compose zur Definition von Services verwendet.

Beispiel für einfache YAML-Syntax:

```yaml
service:
  name: web
  ports:
    - "80:80"
```

---

## Geheimnisse in Compose

**Sensible Daten wie Passwörter** sollten nicht direkt in `docker-compose.yml` stehen. Stattdessen verwendet man Umgebungsvariablen oder `.env`-Dateien.

### Variante 1: `.env`-Datei

`.env`:
```env
POSTGRES_USER=admin
POSTGRES_PASSWORD=supersecret
```

`docker-compose.yml`:
```yaml
environment:
  - POSTGRES_USER=${POSTGRES_USER}
  - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
```

Beim Start liest Compose die Variablen automatisch aus der `.env`-Datei im Projektverzeichnis.
