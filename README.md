# T.I.T.A.N.
### Trusted Infrastructure for Technology & Advanced Networking

> Self-hosted AI-powered homelab infrastructure built on Linux, Docker, GPU acceleration, and local-first services.

---

# Overview

TITAN is a personal infrastructure and AI experimentation platform designed to combine:

- Self-hosted cloud services
- Local AI inference
- Media streaming
- Remote infrastructure management
- GPU accelerated workloads
- Linux systems engineering
- Automation and monitoring

The project is focused on building a fully containerized, remotely accessible, privacy-oriented server environment capable of hosting both traditional services and modern AI workloads.

---

# Current Infrastructure

## Hardware

| Component | Specification |
|---|---|
| CPU | AMD Ryzen 5 3600 |
| GPU | NVIDIA RTX 3060 Ti |
| RAM | 32GB DDR4 3600 |
| Storage | 2TB NVMe SSD |
| Motherboard | ASUS TUF X570 WiFi |

---

# Software Stack

## Operating System
- Ubuntu Server 26.04 LTS

## Core Infrastructure
- OpenSSH Server
- NVIDIA Driver 595 Open
- CUDA 13.2
- Wake-on-LAN
- Static DHCP networking

## Monitoring & Utilities
- lm-sensors
- nvme-cli
- fastfetch
- htop
- btop
- traceroute
- iperf3

---

# Planned Services

## Infrastructure
- Docker Engine
- Docker Compose
- NVIDIA Container Toolkit
- Cloudflare Tunnel
- Reverse Proxy

## Self Hosted Services
- Jellyfin
- Nextcloud
- Samba NAS
- Homepage Dashboard
- Uptime Kuma

## AI Stack
- Ollama
- Open WebUI
- Whisper
- Local LLM inference
- Voice assistant framework

---

# Features

- Remote headless server management through SSH
- GPU accelerated compute environment
- Local-first AI infrastructure
- Wake-on-LAN remote boot support
- Hardware monitoring and diagnostics
- Self-hosted media and cloud ecosystem
- Expandable Docker-based architecture

---

# Project Goals

The primary goal of TITAN is to explore and implement:

- Linux server administration
- Infrastructure engineering
- Containerized deployments
- AI inference workflows
- Networking and remote access
- Monitoring and observability
- Privacy-oriented self-hosting

while maintaining a practical real-world homelab environment.

---

# Repository Structure

```text
Titan/
├── docs/
├── scripts/
├── docker/
├── screenshots/
└── private/