# TITAN - Phase 1 Infrastructure Foundation

## Objective

Build the foundational infrastructure layer for a self-hosted AI-enabled homelab platform.

Phase 1 focused on:
- Linux server deployment
- GPU enablement
- Docker infrastructure
- service orchestration
- monitoring
- dashboard systems
- observability setup



# Base Operating System

## Installed

- Ubuntu Server 26.04 LTS

## System Configuration

- UEFI boot mode
- Secure Boot enabled
- CSM disabled
- Wake-on-LAN enabled
- Static DHCP reservation configured through router



# Hardware Platform

| Component     | Specification      |
|---------------|--------------------|
| CPU           | AMD Ryzen 5 3600   |
| GPU           | NVIDIA RTX 3060 Ti |
| RAM           | 32GB DDR4 3600     |
| Storage       | 2TB NVMe SSD       |
| Motherboard   | ASUS TUF X570 WiFi |



# Core Linux Packages Installed

## Networking & Remote Access

- openssh-server
- net-tools
- traceroute
- iperf3

## Monitoring & Diagnostics

- lm-sensors
- nvme-cli
- htop
- btop
- fastfetch

## Utilities

- curl
- wget
- git
- nano



# Infrastructure Features Configured

## SSH Remote Management

Configured headless remote access through:
- OpenSSH Server
- local network SSH connectivity



# Wake-on-LAN

Configured:
- BIOS WoL support
- Linux WoL persistence
- remote power-on functionality



# Hardware Monitoring

Configured monitoring for:
- CPU temperature
- GPU temperature
- NVMe temperature
- system health diagnostics



# NVIDIA GPU Stack

## Installed

- NVIDIA Driver 595 Open
- CUDA 13.2
- NVIDIA Container Toolkit

## Verified

- GPU detection
- CUDA runtime
- Docker GPU passthrough
- container GPU acceleration



# Docker Infrastructure

## Installed

- Docker Engine
- Docker Compose Plugin
- containerd
- Docker Buildx

## Configured

- Docker service auto-start
- non-root Docker permissions
- GPU container runtime support



# Containerized Services

## Jellyfin

Purpose:
- self-hosted media streaming platform

Features:
- persistent storage
- Docker Compose deployment
- media library organization
- isolated container runtime



# Dockge

Purpose:
- Docker Compose stack management UI

Features:
- stack management
- compose editing
- container lifecycle management
- centralized Docker administration



# Homepage

Purpose:
- centralized homelab dashboard

Features:
- service launcher
- infrastructure organization
- Docker integration
- quick-access dashboard



# Uptime Kuma

Purpose:
- infrastructure monitoring and observability

Features:
- service uptime tracking
- latency monitoring
- HTTP monitoring
- TCP monitoring
- infrastructure health visibility



# Infrastructure Monitoring Targets

Configured monitoring for:
- Homepage
- Dockge
- Jellyfin
- SSH service



# Docker Concepts Learned

- containerization
- images
- volumes
- bind mounts
- port mapping
- restart policies
- Docker Compose
- GPU passthrough
- infrastructure-as-code



# Infrastructure Architecture Achieved

TITAN now supports:
- headless operation
- containerized deployments
- GPU accelerated workloads
- centralized dashboards
- infrastructure monitoring
- remote administration
- modular service deployment



# Current Service Ports

| Service   |   Port     |
|-----------|------------|
| Jellyfin  |   8096     |
| Dockge    |   5001     |
| Homepage  |   3000     |
| UptimeKuma|   3001     |
| SSH       |     22     |



# Current Project State

Phase 1 completed successfully.

TITAN now operates as:
- a self-hosted Linux infrastructure platform
- a GPU-capable AI-ready compute server
- a monitored containerized homelab environment



# Next Phase

## AI Infrastructure Layer

Planned:
- Ollama
- local LLM deployment
- Open WebUI
- GPU accelerated AI inference
- local conversational AI platform