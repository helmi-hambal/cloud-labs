# Day 4 – Docker on Windows VM

## 🎯 Objektif
Eksperimen penggunaan **Docker dalam Windows Server 2022 VM** di Azure.  
Fokus: faham limitation Windows container vs Linux container.

---

## 🛠️ Langkah-langkah

### 1. Sediakan VM Windows
```bash
az vm create \
  -g rg-lab-net \
  -n win-docker \
  --image Win2022Datacenter \
  --size Standard_D4s_v5 \
  --admin-username helmi \
  --admin-password '********' \
  --public-ip-sku Standard
```
### 2. Buka port RDP (3389)
```bash
az vm open-port -g rg-lab-net -n win-docker --port 3389
```
Login VM guna Remote Desktop.

---

#### 3. Install Dokcer (manual setup)
```powershell
Invoke-WebRequest https://download.docker.com/win/static/stable/x86_64/docker-20.10.24.zip -OutFile docker.zip
Expand-Archive docker.zip -DestinationPath 'C:\Program Files\docker'
Move-Item "C:\Program Files\docker\docker\*" "C:\Program Files\docker" -Force
$env:Path += ";C:\Program Files\docker"
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\docker", [EnvironmentVariableTarget]::Machine)

dockerd --register-service
Start-Service docker
```
Verify:
```powershell
docker --version
```

---

### 4. Test container
Hello World
```powershell
docker run hello-world
```
Nginx (Linux) → ❌ Failed
```powershell
docker run -d -p 8080:80 nginx
```
Error:
```bash
no matching manifest for windows/amd64 10.0.20348
```
💡 Sebab: Windows Docker hanya support Windows container, bukan Linux image.

---

### 5. IIS Container (Windows)
```powershell
docker run -d -p 8080:80 mcr.microsoft.com/windows/servercore/iis
```
Check:

- Buka browser → http://<Public_IP_VM>:8080

- Nampak IIS default page (“Internet Information Services – Welcome”).

---

⚠️ Masalah & Solusi
| Masalah                                    | Solusi                                                                                    |
| ------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Nginx container tak jalan                  | Guna Windows-based image (contoh: IIS)                                                    |
| Download Docker via PowerShell kadang slow | Pastikan VM ada bandwidth cukup, atau gunakan Storage Blob untuk upload installer sendiri |
| VM terasa berat                            | Guna size lebih besar (contoh: `Standard_D4s_v5`) dengan RAM lebih tinggi                 |
