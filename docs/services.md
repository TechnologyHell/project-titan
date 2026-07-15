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



# Netdata Deployment

## Purpose

Real-time infrastructure monitoring platform.

## Deployment Method

Docker Compose deployment.

## Storage Layout

/srv/docker/netdata

## Features

- CPU monitoring
- GPU monitoring
- RAM monitoring
- Disk monitoring
- Network monitoring
- Docker monitoring
- Historical performance graphs



# JARVIS Voice Assistant

## Purpose

Provide a completely local conversational AI assistant.

## Deployment Method

Native Python application.

## Storage Layout

/srv/friday

## Core Components

- OpenWakeWord
- Faster Whisper
- Ollama
- Gemma 3 4B
- Piper TTS

## Features

- Wake word activation
- Speech-to-text
- Local LLM inference
- Text-to-speech
- Multi-turn conversations
- Offline operation



# Surveillance Recording

## Purpose

Centralized NVR-style recording for connected cameras.

## Recording Root

/srv/surveillance

## Features

- Automatic recording
- 5-minute segmented clips
- Daily retention cleanup
- Camera-wise storage organization



# Samba Deployment

## Purpose

Provide network file sharing across local devices.

## Access Protocol

SMB / CIFS

## Shared Directories

- Public
- Home
- Tech
- Srv

## Features

- Windows compatibility
- Linux compatibility
- Persistent network storage
- Centralized file sharing



# Infrastructure Summary

| Service | Purpose |
|----------|---------|
| Jellyfin | Media Streaming |
| Dockge | Docker Stack Management |
| Homepage | Infrastructure Dashboard |
| Uptime Kuma | Service Monitoring |
| Ollama | Local AI Runtime |
| Open WebUI | AI Chat Interface |
| FileBrowser | NAS Web UI |
| Home Assistant | Smart Home Platform |
| Go2RTC | Camera Streaming |
| Netdata | Infrastructure Monitoring |
| Samba | Network Storage |
| JARVIS | Local Voice Assistant |



# Deployment Architecture

User
↓
Homepage Dashboard
↓
Docker Services
├── Jellyfin
├── Dockge
├── Ollama
├── Open WebUI
├── Home Assistant
├── Go2RTC
├── FileBrowser
├── Netdata
├── Uptime Kuma
└── Samba
↓
GPU + Storage + Cameras



# Current Deployment Statistics

Current Environment

- Ubuntu Server 26.04 LTS
- Docker Compose Infrastructure
- GPU Accelerated AI
- 10+ Self-hosted Services
- Local AI Platform
- NAS Infrastructure
- NVR Surveillance
- Smart Home Platform
- Voice Assistant