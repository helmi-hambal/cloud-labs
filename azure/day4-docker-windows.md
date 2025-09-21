# Day 4 - Docker on Windows VM (Azure)

## VM Setup
```powershell
az vm create `
  -g rg-lab-net `
  -n win-docker `
  --image Win2022Datacenter `
  --size Standard_D4s_v5 `
  --admin-username helmi `
  --admin-password '********' `
  --public-ip-sku Standard
```
NSG rule untuk RDP:
```powershell
az vm open-port -g rg-lab-net -n win-docker --port 3389
```
Docker Installation
```powershell
Invoke-WebRequest https://download.docker.com/win/static/stable/x86_64/docker-20.10.24.zip -OutFile docker.zip
Expand-Archive docker.zip -DestinationPath 'C:\Program Files\docker'
$env:Path += ";C:\Program Files\docker"
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\docker", [EnvironmentVariableTarget]::Machine)
dockerd --register-service
Start-Service docker
```
Testing
Hello-world:
```powershell
docker run hello-world
```
Windows IIS container:
```powershell
docker run -d -p 8080:80 mcr.microsoft.com/windows/servercore/iis
```
Output: buka browser → http://<public-ip>:8080 → nampak default IIS welcome page.
