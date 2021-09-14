# Nordvpn
Nordvpn docker settings

# Nordvpn with Transmission Docker stack

```version: "2"
services:
  vpn:
    image: ghcr.io/bubuntux/nordvpn
    container_name: vpn
    cap_add:
      - NET_ADMIN               # Required
    environment:                # Review https://github.com/bubuntux/nordvpn#environment-variables
      - USER=user@mail.com	# Required
      - "PASS=password"         # Required
      - CONNECT=Hong_Kong
      - TECHNOLOGY=NordLynx
      - NETWORK=192.168.0.0/24  # So it can be accessed within the local network
      - TZ= Asia/Hong_kong
    ports:
      #- 8080:80
      - 9091:9091
      - 51413:51413
      - 51413:51413/udp
    restart: unless-stopped

  transmission:
    image: ghcr.io/linuxserver/transmission
    container_name: transmission
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_kong
    volumes:
      - <Path to config>:/config
      - <Path to downloads>:/downloads
      - <Path to watch>:/watch
    restart: unless-stopped
    network_mode: service:vpn	# Network via the above vpn service
```
