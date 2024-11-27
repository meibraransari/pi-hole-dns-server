---
Created: 2024-11-27T08:20:58+05:30
Updated: 2024-11-27T08:47:30+05:30
Maintainer: Ibrar Ansari
---
# Beginner's guide to building a Pi-Hole DNS Server

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/meibraransari/pi-hole-dns-server/blob/main/assets/pihole.png">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/meibraransari/pi-hole-dns-server/blob/main/assets/pihole.png">
    <img src="https://github.com/meibraransari/pi-hole-dns-server/blob/main/assets/pihole.png" width="168" height="270" alt="Pi-hole website">
  </picture>
    <br>
    <strong>Network-wide ad blocking via your own Linux hardware</strong>
</p>


### What is Pi-hole?
Pi-hole is free and open-source software that works as a network-wide ad blocker, acting as a DNS sinkhole to block unwanted content. It improves privacy and speeds up browsing by preventing ads and trackers.
Official Documentation: https://pi-hole.net/

### What are the benefits of Pi-hole?
Here are the key benefits of Pi-hole:
- **DNS Server** Use Pi-hole as your DNS server.
- **Ad Blocking**: Blocks ads at the network level, preventing them from loading on any device connected to your network.
- **Privacy Protection**: Reduces tracking by blocking third-party cookies, analytics, and tracking domains.
- **Faster Browsing**: Speeds up page load times by preventing ads and unnecessary content from loading.
- **Network-wide Coverage**: Blocks ads across all devices (smartphones, computers, smart TVs, etc.) without needing individual app configurations.
- **Reduced Bandwidth Usage**: Less data is consumed since ads and unwanted content are blocked before being downloaded.
- **Customizable Filters**: Allows you to add custom blocklists or allow specific domains to suit your needs.
- **Enhanced Security**: Can block malicious domains, protecting your network from malware and phishing attempts.
- **Easy Setup**: Simple installation on a Raspberry Pi or other devices, with a user-friendly web interface for management.
- **Free and Open Source**: No cost and open for community contributions and improvements.

### **Who can use it?**  
Anyone with a home or business network can use Pi-hole, particularly those looking to enhance privacy or reduce internet clutter.

### **Why it must be in everyone's system or server**  
Pi-hole offers improved security, faster browsing, and a cleaner, ad-free experience for all devices connected to the network.

### **Prerequisites**  
- Basic understanding of Docker.
- Docker must be installed on your system.
- Basic knowledge of command-line operations.
- Minimal resources (256MB RAM, 2GB storage).
- Pi-hole runs on devices like Raspberry Pi, Linux servers, or virtual machines.

### Deployment Guide

#### 1. Using Docker run command

```
docker run -d \
    --name c-pihole \
    -p 53:53/tcp -p 53:53/udp \
    -p 67:67/udp \
    -p 8080:80 \
    -v "$(pwd)/pihole/data/pihole:/etc/pihole" \
    -v "$(pwd)/pihole/data/dnsmasq.d/custom-dns.conf:/etc/dnsmasq.d/custom-dns.conf" \
    -e ServerIP="$(ip route get 8.8.8.8 | awk '{for(i=1;i<=NF;i++) if ($i=="src") print $(i+1)}')" \
    -e DNS1=8.8.8.8 \
    -e DNS2=4.2.2.2 \
    -e TZ=Asia/Kolkata \
    -e WEBPASSWORD=Secure_Password \
    --restart=always \
    pihole/pihole:latest
```
#### 2. Using Docker Compose
##### Create compose file
```
nano compose.yml
```

```
services:
  pihole:
    container_name: pihole
    hostname: pihole
    image: pihole/pihole:latest
    restart: unless-stopped
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    cap_add:
      - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    ports:
      - "53:53/tcp" # DNS TCP
      - "53:53/udp" # DNS UDP
      - "67:67/udp" # DNS UDP
      - "8080:80/tcp" # WEB ADMIN GUI
      #- "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
    environment:
      - WEBPASSWORD=Secure_Password
    volumes:
      - ${DOCKER_VOLUME_STORAGE:-/mnt/docker-volumes}/pihole/data/pihole:/etc/pihole
      - ${DOCKER_VOLUME_STORAGE:-/mnt/docker-volumes}/pihole/data/dnsmasq.d/custom-dns.conf:/etc/dnsmasq.d/custom-dns.conf
```

##### Run container
```
docker compose up -d
```


##### Access DNS Server
```
http://your_ip_or_FQDN:8080
```


---
### 💼 Connect with me 👇👇 😊

- 🔥 [**Youtube**](https://www.youtube.com/@DevOpsinAction?sub_confirmation=1)
- ✍ [**Blog**](https://ibraransari.blogspot.com/)
- 💼 [**LinkedIn**](https://www.linkedin.com/in/ansariibrar/)
- 👨‍💻 [**Github**](https://github.com/meibraransari?tab=repositories)
- 💬 [**Telegram**](https://t.me/DevOpsinActionTelegram)
- 🐳 [**Docker**](https://hub.docker.com/u/ibraransaridocker)
