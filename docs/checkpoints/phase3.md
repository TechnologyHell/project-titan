# TITAN - Phase 3 Storage, Monitoring & Infrastructure Expansion

## Objective

Expand TITAN beyond core AI infrastructure into a fully featured self-hosted platform with:
- centralized storage
- NAS-style access
- infrastructure observability
- service dashboards
- filesystem management
- persistent storage architecture
- intelligent infrastructure preparation

### Phase 3 focused on:
- NAS architecture
- Samba integration
- browser-based filesystem management
- real-time observability
- infrastructure organization
- service centralization
- operational refinement
- Storage Infrastructure Layer
- Centralized Storage Architecture

## Standardized persistent storage hierarchy under:
- /srv
- Storage Layout
- Path	Purpose
- /srv/docker	container persistent data
- /srv/media	Jellyfin media libraries
- /srv/storage/public	shared storage
- /srv/storage/users	personal storage
- /srv/storage/backups	backup storage

# Architecture Goal
Establish:
- centralized infrastructure storage
- persistent Docker data separation
- scalable NAS hierarchy
- organized homelab filesystem design
- Samba NAS Integration

# Objective
Provide:
- Windows network drive compatibility
- SMB file sharing
- LAN-based storage access
- authenticated network storage

## Installed
- Samba
- Configured Shares :
    Share	Purpose
    Public	shared network storage
    Tech	personal authenticated storage
    Home	Linux user workspace
    Srv	    infrastructure filesystem access

# Features Achieved
- authenticated SMB access
- LAN file sharing
- Windows Explorer compatibility
- Linux storage interoperability
- centralized infrastructure access

# Verified
Access successfully tested through:
- \\<SERVER_IP>
from Windows systems.

## FileBrowser Deployment
Purpose
Provide:
- browser-based NAS management
- modern filesystem UI
- infrastructure file browsing
- drag-and-drop file management
- lightweight web storage interface

### Deployment Method
Docker Compose deployment.
Deployment Directory    : ~/docker/filebrowser
Persistent Storage      : /srv/docker/filebrowser

## Infrastructure Access

Mapped  : /srv
into container for centralized filesystem visibility.

# Features Verified
- browser file management
- uploads/downloads
- infrastructure browsing
- filesystem navigation
- authenticated access
- persistent configuration

## Access   : http://SERVER_IP:8080

# Homepage Infrastructure Expansion
- Dashboard Organization
- Expanded Homepage into a centralized operational dashboard.

## Service Categories Added
Category	    Services
Media	        Jellyfin
Infrastructure	Dockge, Uptime Kuma
Storage	        FileBrowser, Samba
Intelligence	Ollama, Open WebUI


# Result
Homepage evolved into:
- centralized homelab launcher
- infrastructure navigation panel
- operational service dashboard
- Uptime Kuma Infrastructure Monitoring Expansion

## Additional Monitoring Targets
Extended monitoring coverage for:
- FileBrowser
- AI infrastructure
- Docker services
- infrastructure dashboards

# Result
TITAN now supports:
- centralized uptime observability
- infrastructure health tracking
- multi-service monitoring
- Netdata Real-Time Observability Deployment

## Objective
Deploy advanced real-time infrastructure telemetry and monitoring platform.

Purpose:
- live infrastructure analytics
- Docker observability
- real-time performance metrics
- resource monitoring
- infrastructure telemetry

# Deployment Method
Docker Compose deployment.
Deployment Directory    : ~/docker/netdata
Persistent Storage      : /srv/docker/netdata
Host Resources Mapped into container:
    /proc
    /sys
    Docker socket
for:
- host-level observability
- container telemetry
- infrastructure visibility
- Features Verified
    CPU monitoring
    RAM monitoring
    disk analytics
    Docker telemetry
    network analytics
    real-time infrastructure visualization

## Access   : http://SERVER_IP:19999


# TITAN now additionally supports:
- NAS-style storage access
- SMB interoperability
- browser filesystem management
- real-time telemetry
- infrastructure observability
- centralized storage architecture
- persistent operational dashboards


# TITAN now operates as:
a self-hosted AI infrastructure platform
a centralized NAS environment
a monitored observability platform
a browser-manageable infrastructure system
a scalable homelab ecosystem


# Next Phase
Smart Features & Intelligent Automation Layer

Planned:
- Home Assistant
- smart device integration
- Telegram infrastructure control
- AI-assisted command execution
- CCTV monitoring
- webcam-based computer vision
- intelligent automation orchestration
- distributed device control
- Jarvis-style interaction layer


#