# 🚀 eNiGo Labs DevOps Task

## 📌 Task Overview
This project demonstrates a **Dockerized web application** proxied by **Traefik** as a reverse proxy. The setup includes:
- A **static web application** served using NginX.
- **Traefik** as a dynamic reverse proxy.
- **Docker Compose** for easy deployment.
- Domain-based routing (`app.localhost`).

## 🏗️ Project Structure
```
├── webapp/
│   ├── index.html  # Static web page
│   ├── Dockerfile  # Dockerfile for web app
├── traefik/
│   ├── traefik.yml  # Traefik configuration
├── docker-compose.yml  # Defines all services
└── README.md  # Documentation
```

---

## ⚙️ Setup Instructions

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/enigo-devops-task.git
cd enigo-devops-task
```

### 2️⃣ Add `app.localhost` to Your Hosts File
```bash
sudo nano /etc/hosts
```
Add this line:
```
127.0.0.1 app.localhost
```
Save and exit.

### 3️⃣ Start the Services
```bash
docker-compose up -d
```

### 4️⃣ Access the Web App
- Open **http://app.localhost** in your browser.
- The static web app should load successfully.

### 5️⃣ Access the Traefik Dashboard
- Open **http://localhost:8080/dashboard/** to view the proxy status.

### 6️⃣ Stop Services
```bash
docker-compose down
```

---

## 📝 Configuration Details

### 📄 `webapp/Dockerfile`
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

### 📄 `traefik/traefik.yml`
```yaml
entryPoints:
  web:
    address: ":80"

providers:
  docker:
    exposedByDefault: false

api:
  dashboard: true
```

### 📄 `docker-compose.yml`
```yaml
version: '3'

services:
  traefik:
    image: traefik:v2.9
    command:
      - "--configFile=/etc/traefik/traefik.yml"
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - "./traefik/traefik.yml:/etc/traefik/traefik.yml"
      - "/var/run/docker.sock:/var/run/docker.sock"
  
  webapp:
    build: ./webapp
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.webapp.rule=Host(`app.localhost`)"
      - "traefik.http.services.webapp.loadbalancer.server.port=80"
```

---

## 🚀 Deployment Process
1. Push the latest changes to GitHub:
   ```bash
   git add .
   git commit -m "Updated project files"
   git push origin main
   ```
2. Ensure the setup runs smoothly by following the **Setup Instructions**.
3. If required, extend Traefik's configuration to support HTTPS (Let’s Encrypt integration).

---

## 🛠 Troubleshooting
- **Web app not loading?**
  - Check if `app.localhost` is correctly mapped in `/etc/hosts`.
  - Run `docker-compose logs webapp` to check for errors.
- **Traefik dashboard not accessible?**
  - Ensure Traefik is running with `docker ps`.
  - Restart Traefik: `docker-compose restart traefik`.

---

## 🤝 Contributing
Feel free to fork this repo and improve the project. Submit a PR with any enhancements.

---

## 📜 License
This project is licensed under the MIT License.

---

## 📩 Contact
For any questions, feel free to reach out:
- **Email:** karimikuriah@gmail.com
- **GitHub:** [Your GitHub Profile](https://github.com/YOUR_GITHUB_USERNAME)

---

### 🎉 Thank you for reviewing my submission! Looking forward to your feedback. 🚀

