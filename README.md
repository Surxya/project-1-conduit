# Conduit - Containerized Full-Stack Application

[![CI Pipeline](https://github.com/Surxya/project-1-conduit/actions/workflows/ci.yml/badge.svg)](https://github.com/Surxya/project-1-conduit/actions/workflows/ci.yml)

A production-ready microservices architecture for the Conduit RealWorld application, fully containerized using Docker, Docker Compose, and custom Nginx reverse proxy routing.

---

## 🏗 Architecture Overview

The system consists of three isolated services running within a shared Docker bridge network:

```
[ Client Browser ]
│
▼ (Port 80)
┌──────────────────────────────────────────────────┐
│  Frontend (React + Custom Nginx Proxy)           │
│  - Serves built static assets                    │
│  - Custom try_files fallback handling SPA routes │
└───────────────────────┬──────────────────────────┘
                        │ API Requests (/api)
                        ▼
┌──────────────────────────────────────────────────┐
│  Backend Service (Node.js / Express)             │
│  - RESTful API business logic                    │
│  - Environment-based database connectivity       │
└───────────────────────┬──────────────────────────┘
                        │ TCP / SQL (Port 5432)
                        ▼
┌──────────────────────────────────────────────────┐
│  Database (PostgreSQL)                           │
│  - Persistent data storage via Docker volumes    │
└──────────────────────────────────────────────────┘
```

---

## 🛠 Tech Stack

* **Frontend:** React, Nginx (Alpine)
* **Backend:** Node.js, Express.js
* **Database:** PostgreSQL
* **Orchestration:** Docker, Docker Compose

---

## 🚀 Key Technical Problem & Solution

### **The Challenge: SPA 404s on Hard Refresh**
When hosting single-page applications (SPAs) like React behind web servers, direct navigation or page refreshes on sub-routes return a `404 Not Found` error because the static server looks for a literal file path matching the URL.

### **The Fix**
Configured a custom `nginx.conf` utilizing the `try_files` directive to route all unmatched client requests back to `index.html`, allowing React Router to handle client-side routing seamlessly.

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
}
```

---

## ⚡ Quickstart (Local Deployment)

### Prerequisites
* [Docker Engine](https://docs.docker.com/get-docker/) (v20.10+)
* [Docker Compose](https://docs.docker.com/compose/) (v2.0+)

### Running the Application

1. **Clone the repository:**
```bash
git clone [https://github.com/Surxya/project-1-conduit.git](https://github.com/Surxya/project-1-conduit.git)
cd project-1-conduit
```

2. **Launch the stack:**
```bash
docker compose up --build -d
```

3. **Access the app:**
* **Frontend:** http://localhost
* **Backend API:** http://localhost:3000

4. **Tear down:**
```bash
docker compose down -v
```
