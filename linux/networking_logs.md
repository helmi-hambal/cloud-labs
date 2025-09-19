## Network Configuration
```bash
hostname -I
ip addr show
ip -4 route
ip -6 route

- Confirmed VM IP: 172.16.0.4
- Default gateway: 172.16.0.1
- DNS via Azure internal: 168.63.129.16

## DNS & Resolving
```bash
```resolvectl status```
```cat /etc/resolv.conf```

- Local stub resolver (127.0.0.53)
- Azure-assigned DNS domain: *.internal.cloudapp.net


