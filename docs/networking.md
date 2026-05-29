# Networking

## Hardware Structure

Networking Tools Used : 
- Optical Network Unit  : Syrotech SY-GPON-1000R-DONT Gigabit ONU
- Wireless Router       : TP-Link Archer AC1200 Archer C6 Dual Band Gigabit Router
- Secondary Router Ext. : TP-Link Archer AC1200 Archer C6U Dual Band Gigabit Router
- Onboard Networking    : Realtek® L8200A Gigabit Ethernet with TUF LANGuard and TurboLAN technology
- Personal System       : intel Killer™ E3000 2.5 Gbps LAN Controller
- Network Plan          : 150Mbps truly unlimited with dynamic IP assignment

## Static IP

Configured static DHCP lease through router.

## SSH Remote Access

OpenSSH Server installed and enabled.

Server accessible remotely on local network through:

ssh username@<SERVER_IP>



# Infrastructure Addressing

| Service           | Address       |
|-------------------|---------------|
| Client PC         | x.x.x.3       |
| Client PC         | x.x.x.4       |
| TITAN Server      | x.x.x.5       |
| JARVIS Pi         | x.x.x.6       |
| Client PC         | x.x.x.7       |
| Homepage          | x.x.x.x:3000  |
| Uptime Kuma       | x.x.x.x:3001  |
| Open WebUI        | x.x.x.x:3002  |
| FileBrowser       | x.x.x.x:8080  |
| Jellyfin          | x.x.x.x:8096  |
| Ollama            | x.x.x.x:11434 |
| Netdata           | x.x.x.x:19999 |
| Go2RTC            | x.x.x.x:1984  |
| Home Assistant    | x.x.x.x:8123  |
| TAPO Smart Plug   | x.x.x.201     |
| Halonix Bulb      | x.x.x.202     |
| CP Plus IP Camera | x.x.x.203     |



# Remote Power Control

Wake On LAN enabled.

Server may remain powered off and be remotely started using magic packets.

Requirements:
- Router powered
- Network path active
- Ethernet connected

Use Cases:
- Remote maintenance
- Energy saving
- On-demand AI server activation



Internet
   │
 ONU
   │
 Archer C6 Router
   │
 ├── TITAN Server
 │
 ├── IP Cameras
 │
 ├── Home Assistant
 │
 └── Client Devices