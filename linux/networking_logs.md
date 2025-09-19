# Linux: Networking & Logs (Lab 4)

## Network Basics
- IP & routes:
  - `hostname -I`
  - `ip addr show`
  - `ip -4 route`
- DNS:
  - `resolvectl status`
  - `/etc/resolv.conf`

## Connectivity Tests
- `ping -c 4 1.1.1.1`
- `ping -c 4 google.com`
- `traceroute google.com`

## Listening Sockets
- `ss -tulpen` (expect: sshd on :22)

## Mini HTTP Test (8080)
- `python3 -m http.server 8080 &`
- `ss -tulpen | grep 8080`
- `curl -I http://127.0.0.1:8080`
- `pkill -f "http.server 8080"`

## UFW Firewall
- `sudo ufw allow OpenSSH`
- `sudo ufw enable`
- `sudo ufw allow 8080/tcp`
- `sudo ufw status numbered`
> Note: Azure NSG + UFW kedua-dua perlu allow untuk public inbound.

## Logs & Troubleshooting
- Journal:
  - `journalctl --since "-10 min"`
  - `journalctl -u ssh --since "-30 min"`
  - `journalctl -p warning --since today`
- Auth log:
  - `sudo tail -n 50 /var/log/auth.log` (bruteforce attempts noted)
- Kernel:
  - `dmesg -T | less`
  - `journalctl -k -b`
- Custom log:
  - `logger -p user.info "helmi lab test"`
  - `journalctl -t logger --since "-1 min"`

## Logrotate (reference)
- `/etc/logrotate.conf`
- `/etc/logrotate.d/`
