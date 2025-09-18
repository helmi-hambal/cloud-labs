# Linux: Processes & Systemd (Lab 3)

## Process Management
ps aux | head -5
top
htop

## Kill Process
sleep 500 &
ps aux | grep sleep
kill <PID>

## Services
systemctl status ssh
sudo systemctl restart ssh
sudo systemctl enable ssh

## Logs
journalctl -u ssh --since "5 min ago"

## Custom Service
loop.service – simple systemd service to run loop.sh and auto-restart
