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
ssh helmi@10.0.0.5```        # test login ke VM2 private IP```

✅ Confirmed SSH cross-VM success.

