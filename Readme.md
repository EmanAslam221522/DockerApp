

# 🐳 Dockerized Frontend & Backend Application

This project demonstrates a simple **two-tier web application** using **Docker Compose** — featuring a Python Flask **backend API** and an Nginx **frontend**. Both are containerized and managed together using Docker.

---

## 📘 Table of Contents

1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Technologies Used](#technologies-used)
4. [Backend (Flask App)](#backend-flask-app)
5. [Frontend (Nginx + HTML)](#frontend-nginx--html)
6. [Docker Setup](#docker-setup)
7. [Docker Compose Configuration](#docker-compose-configuration)
8. [How to Run the Project](#how-to-run-the-project)
9. [Testing in Browser](#testing-in-browser)
10. [Screenshots (for submission)](#screenshots-for-submission)
11. [Common Issues & Fixes](#common-issues--fixes)
12. [Conclusion](#conclusion)

---

## 🔍 Overview

This project is a simple example of how Docker can be used to containerize a **web application with separate frontend and backend services**.
The backend provides an API endpoint using Flask, and the frontend is served using Nginx.

Both services are defined in a single **docker-compose.yml** file for easy deployment.

---

## 📁 Project Structure

```
dockerapp/
│
├── backend/
│   ├── app.py
│   ├── Dockerfile
│   ├── requirements.txt
│
├── frontend/
│   ├── index.html
│   ├── Dockerfile
│
├── docker-compose.yml
│
└── README.md
```

---

## ⚙️ Technologies Used

| Component            | Technology                         |
| -------------------- | ---------------------------------- |
| **Frontend**         | HTML + Nginx (web server)          |
| **Backend**          | Python Flask                       |
| **Containerization** | Docker                             |
| **Orchestration**    | Docker Compose                     |
| **Base Images**      | `nginx:alpine`, `python:3.11-slim` |

---

## 🧩 Backend (Flask App)

The backend is a simple **Flask API** that returns a message when accessed at `/`.

### `backend/app.py`

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Backend!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### `backend/requirements.txt`

```
Flask==3.0.3
```

### `backend/Dockerfile`

```Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

---

## 🌐 Frontend (Nginx + HTML)

The frontend is a simple HTML page served using Nginx.

### `frontend/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker Frontend</title>
</head>
<body>
    <h1>Welcome to the Docker Frontend</h1>
    <p>This frontend is served by Nginx inside a Docker container.</p>
    <p>Backend runs on Flask at port 5000.</p>
</body>
</html>
```

### `frontend/Dockerfile`

```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

## 🐳 Docker Setup

Each component (frontend and backend) is containerized separately using their Dockerfiles.
Docker Compose brings both containers up together and manages their networking.

---

## ⚙️ Docker Compose Configuration

### `docker-compose.yml`

```yaml
version: '3'
services:
  frontend:
    build: ./frontend
    ports:
      - "8080:80"
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "5000:5000"
```

---

## 🚀 How to Run the Project

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/dockerapp.git
cd dockerapp
```

### Step 2: Build and Run Containers

```bash
docker-compose up --build
```

This command will:

* Build both frontend and backend images.
* Start both containers.
* Connect them on a shared Docker network.

---

## 🌍 Testing in Browser

After containers are running, open your browser:

* **Frontend:** [http://localhost:8080](http://localhost:8080)
  → Shows your `index.html` page.

* **Backend:** [http://localhost:5000](http://localhost:5000)
  → Shows `Hello from Backend!`

You can confirm running containers with:

```bash
docker ps
```

Example output:

```
CONTAINER ID   IMAGE               PORTS                    NAMES
ab12cd34ef56   dockerapp-frontend  0.0.0.0:8080->80/tcp     frontend
cd78ef90gh12   dockerapp-backend   0.0.0.0:5000->5000/tcp   backend
```

---

## 📸 Screenshots for Submission

✅ Take these screenshots for your report:

1. `docker ps` showing both containers running
2. `docker-compose up` terminal output
3. Frontend in browser → `http://localhost:8080`
4. Backend in browser → `http://localhost:5000`

---

## ⚠️ Common Issues & Fixes

| Issue                                 | Cause                                 | Solution                                                     |
| ------------------------------------- | ------------------------------------- | ------------------------------------------------------------ |
| **Frontend not loading on port 8080** | Port mapping missing                  | Check `ports: "8080:80"` in `docker-compose.yml`             |
| **Backend shows “Not Found”**         | Flask route `/` not defined           | Add `@app.route('/')` to `app.py`                            |
| **Address already in use**            | Containers from last run still active | Run `docker-compose down` before `docker-compose up --build` |

---

## 🧾 Conclusion

This project successfully demonstrates:

* How to build and run multiple Docker containers for frontend and backend.
* How to use **Docker Compose** for simplified orchestration.
* How containerization ensures isolated, repeatable environments for development.

You now have a clean, reusable Docker-based full stack setup. 🚀

