# Services & Deployments

---

# Jellyfin Deployment

## Purpose
Self-hosted media streaming platform.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/jellyfin
/srv/media

## Features
- media streaming
- anime/movie library hosting
- browser playback
- persistent Docker deployment



# Dockage Deployment

## Purpose
Docker Compose stack management UI.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/dockge

## Features
- stack management
- compose editing
- container lifecycle management
- centralized Docker administration



# Homepage Deployment

## Purpose
Centralized homelab dashboard.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/homepage

## Features
- infrastructure dashboard
- service launcher
- centralized quick-access UI
- organized service grouping



# Uptime Kuma Deployment

## Purpose
Infrastructure monitoring and uptime observability.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/uptime-kuma

## Features
- HTTP monitoring
- TCP monitoring
- latency tracking
- infrastructure observability
- service health tracking

## Monitoring Targets
- Jellyfin
- Dockge
- Homepage
- SSH
- Open WebUI
- Ollama



# Ollama Deployment

## Purpose
Local AI inference runtime backend.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/ollama

## Features
- local LLM inference
- GPU accelerated AI workloads
- CUDA runtime integration
- persistent model storage
- local AI API backend

## Installed Model
- Gemma 3 4B

## GPU Support
- NVIDIA Container Toolkit
- CUDA acceleration
- Docker GPU passthrough



# Open WebUI Deployment

## Purpose
Browser-based conversational AI frontend.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/open-webui

## Features
- ChatGPT-style interface
- persistent conversations
- multimodal image understanding
- AI image uploads
- local AI chat platform
- web retrieval integration
- model selection

## Backend Integration
Connected to:
- Ollama



# FileBrowser Deployment

## Purpose
Browser-based NAS and filesystem management interface.

## Deployment Method
Docker Compose deployment.

## Storage Layout
/srv/docker/filebrowser



# Home Assistant Deployment

## Purpose

Central smart home management platform.

## Features

- Device discovery
- ONVIF camera integration
- Automation engine
- Sensor aggregation
- Unified smart home dashboard

## Storage Layout

/srv/docker/homeassistant

## Current Integrations

- CP Plus IP Camera



# Go2RTC Deployment

## Purpose

Unified camera streaming platform.

## Deployment Method

Docker Compose deployment.

## Storage Layout

/ srv/docker/go2rtc

## Features

- RTSP restreaming
- WebRTC streaming
- Browser viewing
- Multi-camera dashboard
- Low latency camera access

## Current Cameras

### Logitech C925e

Source:
USB Webcam

Resolution:
1920x1080

Frame Rate:
30 FPS

### CP Plus E25A

Source:
RTSP

URL:
rtsp://<SERVER_IP>:<PORT>/live/channel0

Frame Rate:
Native camera stream

Discovery Method:
ONVIF Device Manager



# NAS Infrastructure

## Storage Root

/srv/storage

## Access Methods

- SMB
- FileBrowser
- Local Linux Filesystem

## Shares

- Public
- Tech
- Home
- Srv

## Purpose

Centralized storage for:

- Media
- Documents
- Backups
- AI Datasets
- Future Camera Recordings



