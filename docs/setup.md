# Ubuntu Server Installation

## Base System
- Ubuntu Server 26.04 LTS
- UEFI installation
- Secure Boot enabled
- CSM disabled
- Power On by PCIe Device - Active
- Static DHCP reservation configured through router

## Hardware
|   Component   |         Model         |
|---------------|-----------------------|
| CPU           | Ryzen 5 3600 6C/12T   |
| GPU           | RTX 3060 Ti 8GB       |
| RAM           | 32GB DDR4 3600MHz     |
| Storage       | 2TB NVMe SSD          |
| Motherboard   | ASUS TUF X570 WiFi    | 
| Networking    | TPLink Gigabit Router |

## Partition Setup
- Single primary partition
- Defaults system partitions


# SSH Configuration
Installed OpenSSH server for remote management and headless operation.

## Verification
SSH service enabled and verified successfully.
Remote access now possible through:
~ ssh user@<SERVER_IP>


# Hardware Monitoring
- lm-sensors
- nvme-cli
- htop
- btop
- nvtop


# Basic Tools and Services     
- fastfetch         // neofetch upgrade
- net-tools         // ip a - used modernly
- traceroute
- iperf3


# NVIDIA Drivers Setup
Installed system recommended driver 
    nvidia-driver-595-open - distro non-free recommended

Completed installation for basic driver with CUDA support.
~ nvidia-smi

Realtime GPU Monitoring through 
~ watch -n 1 nvidia-smi


# Wake On Lan Setup
Magic Packet for powering on the system.
Default system package : ethtools
Enabled WOL and made it boot persistent by modifying the wol.service file.
Enable the custom service to be activated on boot.

Now server can be powered on via magic packet on the network.


# Docker Setup
Installed Docker Engine using the official Docker repository instead of Ubuntu packages.

## Installed Components
- docker-ce
- docker-ce-cli
- containerd.io
- docker-buildx-plugin
- docker-compose-plugin

## Verification
docker run hello-world


# Jellyfin Deployment

## Objective
Deploy Jellyfin as a containerized media streaming service using Docker Compose.

## Directory Structure
Created persistent storage directories for:
- application configuration
- cache
- media libraries

## Paths
/srv/docker/jellyfin/config
/srv/docker/jellyfin/cache
/srv/media/movies
/srv/media/shows
/srv/media/anime


# NVIDIA Docker Runtime Configuration

## Objective
Enable GPU acceleration support inside Docker containers using NVIDIA Container Toolkit.
This allows containerized applications to access:
- NVIDIA GPU
- CUDA runtime
- GPU compute acceleration

Required for:
- Ollama
- AI inference workloads
- hardware transcoding
- GPU accelerated containers

## NVIDIA Container Toolkit
Installed:
- nvidia-container-toolkit
Configured Docker runtime integration using:
sudo nvidia-ctk runtime configure --runtime=docker


# Dockge Deployment

## Objective
Deploy Dockge as a web-based Docker Compose management interface.
Purpose:
- stack management
- compose file editing
- container monitoring
- service lifecycle control

## Deployment Directory
~/docker/dockge


# Homepage Dashboard Deployment

## Objective
Deploy Homepage as a centralized self-hosted infrastructure dashboard.
Purpose:
- service organization
- quick-access dashboard
- container status visualization
- homelab control interface

## Deployment Directory
~/docker/homepage


# Uptime Kuma Deployment

## Objective
Deploy Uptime Kuma for infrastructure availability monitoring and service health tracking.
Purpose:
- service uptime monitoring
- response time tracking
- infrastructure observability
- health monitoring dashboard

## Deployment Directory
~/docker/uptime-kuma


# Ollama Deployment

## Objective
Deploy Ollama as the local AI inference runtime for TITAN.
Purpose:
- local LLM execution
- offline AI inference
- GPU accelerated model serving
- AI API backend

## Deployment Directory
~/docker/ollama


# Open WebUI Deployment

## Objective
Deploy Open WebUI as the frontend conversational interface for TITAN AI infrastructure.
Purpose:
- browser-based AI chat interface
- local ChatGPT-like experience
- persistent conversation management
- model interaction layer

## Deployment Directory
~/docker/open-webui


# NAS Storage Structure Setup
Created centralized storage structure under : 
/srv/storage


# FileBrowser NAS UI Setup

## Objective
Deploy browser-accessible NAS management interface.
Purpose:
- modern filesystem UI
- browser-based storage management
- infrastructure file access
- NAS-style web experience

## Deployment Directory
~/docker/filebrowser


# Netdata Monitoring Setup

## Objective
Deploy real-time infrastructure monitoring platform.
Purpose:
- live infrastructure analytics
- Docker observability
- resource monitoring
- visual performance dashboards

## Deployment Directory
~/docker/netdata


# USB Webcam Setup

## Objective
Setup a USB Web Cam to act as a surveillance camera.
Purpose : 
- live camera feed
- Go2rtc camera access
- recordable and analyzable (later)

## Deployment Directory
~/docker/go2rtc


# IP Camera Setup to GO2RTC

## Objective
Configure IP Cam to show live camera feed on unified go2rtc channel.
Purpose : 
- IP cam feed to go2rtc dashboard
- Easy access with a single click
- Drawback - only 0.33 FPS achievable (due to HA limitation)

## Deployment Method
Current Setup permits 1 frame every 3 seconds.
Frame captured and stored using Python Pipeline.
Python used to simulate a virtual camera.
Virtual camera used to pass stream to go2rtc config file.

## PROGRESS - ONVIF Device Manager

## Improvement 
Allowed for direct RTSP Stream URL identification for the IP Camera

## Deployment
Directly mapped stream URL with credentials into GO2RTC Device listing.


# NVR Setup (for active camera devices)

## Objective
NVR like recording system for assigned cameras, storing chunks of realtime footage.
- Centralized network storage for footage collection
- Chunks of 5min clips ensuring lower footage loss upon power cut / system crash
- Realtime recording as well as live preview through go2rtc

## CP Plus E25A IP Camera
  - Live streaming via Go2RTC
  - RTSP integration
  - Automated recording
  - 5-minute segmented archives

## Logitech C925e USB Camera
  - Live streaming via Go2RTC
  - Automated recording
  - 5-minute segmented archives

## Storage Layout
  /srv/surveillance/<camera>/<date>/

## Automatic startup
  systemd-managed services

## Retention policy design
  90-day archive lifecycle 