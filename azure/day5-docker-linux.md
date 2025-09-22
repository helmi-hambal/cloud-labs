# Day 5 – Docker on Linux VM

## 🎯 Objective
Belajar asas Docker pada Linux VM di Azure → run container, expose port, deploy WordPress stack.

---

## 🖥️ Setup Environment
- VM: **Ubuntu 24.04 LTS** (Standard B2s).
- RG:
  - `RG-Linux` → simpan VM.
  - `RG-Network` → simpan NIC, Public IP, NSG, VNet.
- Authentication: **SSH key (.pem)**.

---

## 🔧 Steps

### 1. Install Docker
```bash
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker
docker version
```
### 2. Test Hello World
```bash
docker run hello-world
```
### 3. Run Nginx
```bash
docker run -d -p 8080:80 nginx
```
- Access: http://<public-ip>:8080

---

🗂️ Multi-container (WordPress + MySQL)
Create Network
```bash
docker network create wp-net
```
Run MySQL
```bash
docker run -d \
  --name wp-mysql \
  --network wp-net \
  -e MYSQL_ROOT_PASSWORD=StrongPass123 \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wpuser \
  -e MYSQL_PASSWORD=StrongPass123 \
  mysql:5.7
```
Run WordPress
```bash
docker run -d \
  --name wordpress \
  --network wp-net \
  -e WORDPRESS_DB_HOST=wp-mysql:3306 \
  -e WORDPRESS_DB_USER=wpuser \
  -e WORDPRESS_DB_PASSWORD=StrongPass123 \
  -e WORDPRESS_DB_NAME=wordpress \
  -p 8080:80 \
  wordpress:latest
```
- Access: http://<public-ip>:8080/wp-admin/install.php

---

📝 Notes
- NSG rules perlu open port 22 (SSH) dan 8080 (HTTP test).

- Bila container stop, app unavailable.

- Docker Compose boleh simplify multi-container setup.

---

✅ Outcome

- Docker successfully installed on Linux VM.

- Able to run single (nginx) and multi-container (WordPress + MySQL) apps.

- Learned basic networking & port mapping in Docker.

## 🐞 Troubleshooting / Challenges faced
- Port conflict: dulu ada container `nginx-web` occupy port `8080`, jadi WordPress tak boleh start sampai `nginx-web` di-stop dan di-remove.  
- Naming conflict: container name `wp-mysql` dan `wordpress` dah ada dari eksperimen pertama, menyebabkan `docker-compose up` gagal → perlu `docker rm` lama dulu.  
- `.pem` file issue: Cloud Shell ephemeral, `.pem` hilang, kena upload semula atau reset SSH key.

## 🛠 Debug techniques used
- `docker ps -a` untuk senaraikan semua containers (termasuk exited).  
- `docker logs <container_name>` untuk tengok detail kenapa salah satu service tak start.  
- `curl ifconfig.me` untuk confirm IP semasa untuk NSG SSH rule.

## 🔒 Security & Best Practices Notes
- Port 8080 dibuka, tapi hanya HTTP untuk lab sahaja. Untuk production, gunakan HTTPS + domain.  
- DB port 3306 tidak diekspos ke internet. Container DB hanya boleh diakses melalui network `wp-net`.  
- SSH rule awalnya dikunci ke IP tertentu, tapi dibuka untuk testing. Penting untuk set semula hanya IP kau untuk security.

## 🚀 What’s next
- Build `Dockerfile` untuk custom WordPress image atau plugin.  
- Guna `nginx` sebagai reverse proxy untuk WordPress site.  
- Setup Docker Compose dengan file `.env` supaya password / credentials tidak hard-coded.  
- Automate deployment + backup (cron job atau Azure Automation).  

