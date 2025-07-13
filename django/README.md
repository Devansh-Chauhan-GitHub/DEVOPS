## 🐍 Dockerizing Django App – Documentation

This README documents the steps, configuration, and decisions behind containerizing a Django application using Docker and Docker Compose.

---

### 🧾 Dockerfile

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt ./

RUN pip3 install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python3","manage.py","runserver","0.0.0.0:8000"]
```

#### 🛠️ Explanation

- `FROM python:3.10-slim`:

  - Uses a slim Python 3.10 image to keep image size small.

- `WORKDIR /app`:

  - Sets the working directory inside the container.

- `COPY requirements.txt ./`:

  - Copies only the requirements file first (to leverage Docker layer caching).

- `RUN pip3 install -r requirements.txt`:

  - Installs Django and other dependencies listed in `requirements.txt`.

- `COPY . .`:

  - Copies the rest of the project files (like `manage.py`, `example/`, etc.).

- `EXPOSE 8000`:

  - Informs Docker that the app will run on port 8000 (useful for mapping ports).

- `CMD [...]`:

  - Runs the Django development server bound to all interfaces so it's accessible externally.

> 🧠 Tip: The reason for `CMD` using `python3 manage.py runserver 0.0.0.0:8000` is because this is the default command to launch Django's development server. We bind to `0.0.0.0` so it can receive requests from outside the container.

---

### 🧱 docker-compose.yaml

```yaml
services:
  django:
    build:
      context: .
    ports:
      - 8000:8000
```

#### 🛠️ Explanation

- **services > django**:

  - Names the container `django`.

- **build > context: .**:

  - Builds the image using the Dockerfile in the current directory.

- **ports**:

  - Maps host port `8000` to container port `8000` so it's accessible via `http://<EC2-IP>:8000`


---

### 🐳 Docker Commands

#### Build Image:

```bash
docker build -t django .
```

#### Run Container:

```bash
docker run -d -p 8000:8000 --name django django
```

#### OR use Docker Compose:

```bash
docker compose up -d
```

Then open:

```
http://<your-ec2-ip>:8000
```

---

### 📌 Notes

- Make sure `index.py` or `manage.py` is correctly located.
- If you use environment variables (`environs`), ensure the modules are listed in `requirements.txt`.
- If using external packages (like `marshmallow`), ensure they're compatible.

---

Created for: **Django App in Docker**  |  Python: `3.10` | Dockerized on: `2025-07-13`

