Use image from docker hub

```bash
docker pull cnsoluciones/mikrotik
```

or

```bash
docker pull cnsoluciones/mikrotik:7.16.2
```

```bash
services:
    routeros:
        container_name: "mikrotik"
        image: cnsoluciones/mikrotik:7.6
        privileged: true
        ports:
            - "21:21" #ftp
            - "22:22" #ssh
            - "23:23" #telnet
            - "80:80" #www
            - "443:443" #www-ssl
            - "1194:1194" #OVPN
            - "1450:1450" #L2TP
            - "8291:8291" #winbox
            - "8728:8728" #api
            - "8729:8729" #api-ssl
            - "13231:13231" #WireGuard
        cap_add: 
            - NET_ADMIN
        devices: 
            - /dev/net/tun
```

List of exposed ports
Default ports of RouterOS: 21, 22, 23, 80, 443, 8291, 8728, 8729

OpenVPN: 1194

L2TP: 1450

WireGuard: 13231
