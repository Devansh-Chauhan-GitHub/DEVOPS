## 🔥 Dockerizing Flask with Redis – Documentation

This README documents the steps, configurations, and Docker setup used to containerize a **Flask** application with **Redis** integration using Docker and Docker Compose.

---

### 🧾 Dockerfile

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip3 install -r requirements.txt

COPY . .

CMD ["python3", "app.py"]
```

#### 🛠️ Explanation

- `FROM python:3.10-slim`:

  - Uses a lightweight Python image for a smaller container size.

- `WORKDIR /app`:

  - Sets the working directory inside the container to `/app`.

- `COPY requirements.txt .`:

  - Copies the dependency list.

- `RUN pip3 install -r requirements.txt`:

  - Installs all required Python packages.

- `COPY . .`:

  - Copies the remaining application code into the container.

- `CMD ["python3", "app.py"]`:

  - Launches the Flask app.

---

### 🧱 docker-compose.yaml

```yaml
services:
  redis:
    image: redis:alpine
    ports:
      - 6379:6379
    networks:
      - mynet
    volumes:
      - /home/ubuntu/DEVOPS/flask-redis/data:/data

  flask:
    build:
      context: .
    ports:
      - 8000:8000
    depends_on:
      - redis
    networks:
      - mynet

networks:
  mynet:
     driver: bridge
```

#### 🛠️ Explanation

- **redis**:

  - Uses the official Redis image (alpine variant).
  - Mounts local `data/` directory to container’s `/data` for persistence.

- **flask**:

  - Builds the Flask app using the Dockerfile.
  - Exposes port 8000 for app access.
  - Depends on Redis service to be available before starting.

- **networks**:

  - Defines a user-defined bridge network `mynet` to enable communication between services.

---

### 🐳 Docker Commands

#### Build Image:

```bash
docker compose build --no-cache
```

#### Start Containers:

```bash
docker compose up -d
```

#### Check Running Containers:

```bash
docker ps
```

#### View Logs:

```bash
docker logs flask-redis-flask-1
```

---



### 📁 Project Structure

```
flask-redis/
├── Dockerfile
├── README.md
├── app.py
├── data/
├── docker-compose.yaml
└── requirements.txt
```

---

### 🧪 Testing the Setup

Once everything is up:

- Open browser: `http://<your-ec2-ip>:8000`
- Your Flask app should be connected to Redis and fully functional.

---

Created for: **Flask + Redis Microservice**  |  Dockerized on: `2025-07-13`

