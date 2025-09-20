# Lab 4 – Cross-VM Networking (Azure Subnet)

## Objective
Test komunikasi antara dua VM dalam subnet yang sama menggunakan private IP.  
Confirm service dari VM2 boleh diakses dari VM1.

---

## Steps

### 1. SSH Key Setup
On **VM1 (10.0.0.4)**, copy SSH public key ke VM2 authorized_keys:

```bash
ssh helmi@85.211.180.46   # masuk VM1
ssh helmi@10.0.0.5        # test login ke VM2 private IP
```

✅ Confirmed SSH cross-VM success.

---

### 2. Start Simple Service on VM2
On VM2 (10.0.0.5):

```bash
nohup python3 -m http.server 8080 &
```

Service running di port 8080, output log ada dalam nohup.out.

---

### 3. Test from VM1
On VM1 (10.0.0.4):

```bash
curl http://10.0.0.5:8080
```

✅ Output expected → Directory listing (e.g. .bashrc, .profile, .ssh, nohup.out).

---

### 4. Cleanup
Stop service pada VM2:

```bash
pkill -f "python3 -m http.server 8080"
```

---

### Results

*SSH cross-VM (private IP) → Success

*HTTP service test (VM2 → VM1) → Success

*Verified intra-subnet communication allowed by default in Azure VNet.
