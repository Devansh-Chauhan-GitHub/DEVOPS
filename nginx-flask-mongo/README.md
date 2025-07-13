## 🚀 NGINX + Flask + MongoDB Microservice - Dockerized Setup

This documentation explains the full setup, configuration, and purpose of each component in a **multi-container Docker application** combining **Flask**, **MongoDB**, and **NGINX**.

---

### 📁 Project Structure

```
nginx-flask-mongo/
├── docker-compose.yaml
├── README.md
├── flask/
│   ├── Dockerfile
│   ├── server.py
│   └── requirements.txt
└── nginx/
    └── nginx.conf
```

---

## 📦 flask/Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip3 install -r requirements.txt

COPY . .

CMD ["python3", "server.py"]
```

### 🔍 Explanation

- **FROM python:3.10-slim**: Uses a lightweight Python base image to minimize image size.
- **WORKDIR /app**: Sets `/app` as the working directory for all subsequent commands.
- **COPY requirements.txt .**: Copies only the dependency list first (Docker layer optimization).
- **RUN pip3 install -r requirements.txt**: Installs all Python libraries listed.
- **COPY . .**: Copies all Flask app files (e.g., `server.py`) into the container.
- **CMD ["python3", "server.py"]**: Default command to start the Flask server.

---

## ⚙ docker-compose.yaml

```yaml
services:
  mongodb:
    image: mongo:8.0.12-rc0
    networks:
      - mynet

  flask:
    build:
      context: flask
    depends_on:
      - mongodb
    environment:
      - FLASK_SERVER_PORT=9090
    networks:
      - mynet

  nginx:
    image: nginx:stable-perl
    ports:
      - 80:80
    environment:
      - FLASK_SERVER_ADDR=backend:9091
    depends_on:
      - flask

networks:
  mynet:
    driver: bridge
```

### 🔍 Explanation

#### 📌 `mongodb` service

- **image: mongo:8.0.12-rc0**: Uses an official MongoDB release candidate image.
- **networks**: Joins the custom `mynet` network to allow internal communication.

#### 📌 `flask` service

- **build.context: flask**: Builds Flask image using the Dockerfile in `flask/` folder.
- **depends\_on: mongodb**: Ensures MongoDB container starts before Flask.
- **environment**: Sets `FLASK_SERVER_PORT` as an environment variable (for internal use).
- **networks**: Same network to talk to MongoDB.

#### 📌 `nginx` service

- **image: nginx****:stable-perl**: Stable NGINX image with Perl module support.
- **ports**: Maps container port 80 to host port 80 to serve browser traffic.
- **environment**: Sets backend Flask address (optionally used in nginx.conf).
- **depends\_on: flask**: Starts only after Flask is up.

#### 📌 `networks`

- **mynet**: A bridge network for Docker containers to communicate using service names.

---

## 🌐 nginx/nginx.conf (Example)

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://flask:9090;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 🔍 Explanation

- **listen 80**: Accept HTTP requests on port 80.
- **location /**: Forward all traffic to the Flask container.
- **proxy\_pass **[**http://flask:9090**](http://flask:9090): Proxy to the Flask app using the service name and port.
- **proxy\_set\_header**: Forward client headers correctly.

---

## 🐳 Docker Commands

### 🔧 Build and Start

```bash
docker compose build --no-cache
docker compose up -d
```

### 📋 View Containers

```bash
docker ps
```

### 🧾 View Logs

```bash
docker logs nginx-flask-mongo-flask-1
```

---

## ✅ Test Setup

- Access your app at: `http://<your-ec2-public-ip>`
- Flask talks to MongoDB internally.
- NGINX serves as a reverse proxy to Flask.

---

Created on: **2025-07-13**

Author: **Devansh Chauhan** | Stack: **NGINX + Flask + MongoDB** | Containerized using **Docker & Compose**

