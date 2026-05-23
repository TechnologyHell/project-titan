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



#