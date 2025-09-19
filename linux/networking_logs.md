# Linux: Networking & Logs (Lab 4)

## Network Configuration
hostname -I
ip addr show
ip -4 route
ip -6 route
# VM IP: 172.16.0.4 | Gateway: 172.16.0.1 | DNS: 168.63.129.16

## DNS & Resolving
resolvectl status
cat /etc/resolv.conf
# Stub resolver 127.0.0.53 | Azure domain *.internal.cloudapp.net

## Connectivity Test
ping -c 4 1.1.1.1
ping -c 4 google.com
# ICMP & DNS working

## Traceroute
sudo apt install -y traceroute
traceroute google.com
# Azure blocks ICMP Time Exceeded → output `* * *`

## Open Ports & Services
ss -tulpen
python3 -m http.server 8080 &
curl -I http://127.0.0.1:8080
pkill -f "http.server 8080"

## Firewall (UFW)
sudo ufw allow OpenSSH
sudo ufw allow 8080/tcp
sudo ufw enable
sudo ufw status numbered
# ⚠️ Jangan delete OpenSSH rule kalau UFW enable (kalau reboot → lock out SSH)

## Logs
sudo journalctl --since "-10 min"
sudo journalctl -u ssh --since "-30 min"
sudo journalctl -p warning --since today
sudo tail -n 50 /var/log/auth.log
dmesg -T | less
sudo journalctl -k -b

## Logger Test
logger -p user.info "helmi lab: networking log test $(date)"
sudo journalctl -t logger --since "-1 min"
sudo tail -f /var/log/syslog
# Note: logger entry masuk syslog, bukan journalctl

## Log Rotation
cat /etc/logrotate.conf
ls -l /etc/logrotate.d/
# Weekly rotation, keep 4 weeks, service configs under /etc/logrotate.d
