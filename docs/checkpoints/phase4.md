# TITAN - Phase 4 Automation, Surveillance & Remote Infrastructure Management

## Objective

Transform TITAN from a self-hosted infrastructure platform into an intelligent remotely manageable homelab by integrating:

- remote infrastructure control
- surveillance subsystem
- continuous camera recording
- centralized infrastructure automation
- intelligent service orchestration

---

## Phase 4 focused on

- CCTV infrastructure
- surveillance recording
- camera stream management
- Telegram remote administration
- infrastructure automation
- system service orchestration
- centralized monitoring

---

# Surveillance Infrastructure

## Objective

Deploy a lightweight Network Video Recorder (NVR) architecture capable of continuously recording camera feeds while remaining independent of AI workloads.

---

## Camera Sources

| Camera | Source |
|---------|--------|
| CP Plus E25A | RTSP IP Camera |
| Logitech C925e | USB Webcam |

---

# go2rtc Deployment

## Purpose

Provide a centralized camera stream broker capable of serving multiple applications simultaneously.

### Features

- RTSP proxy
- USB webcam virtualization
- multi-client camera access
- centralized camera endpoints
- stream abstraction layer

### Result

Applications no longer access camera hardware directly.

All camera streams are served through go2rtc.

---

# Continuous Recording System

## Objective

Create a lightweight always-on recording system independent of Frigate or other NVR platforms.

Implemented using custom FFmpeg recording services.

### Features

- continuous recording while server is online
- 5-minute recording segments
- clock-aligned segmentation
- date-wise folder organization
- zero transcoding
- direct stream copy
- automatic recovery after interruptions

---

## Recording Structure

```
/srv/surveillance/

    CPPlus_E25A/
        YYYY-MM-DD/
            HH_000.mp4
            HH_001.mp4

    Logitech_C925e/
        YYYY-MM-DD/
            HH_000.mp4
            HH_001.mp4
```

---

# Recorder Services

Dedicated systemd services created for:

- cpplus-recorder
- logitech-recorder

### Features

- automatic startup during boot
- automatic restart on failure
- background execution
- independent recording services

---

# Recording Retention

Implemented automated cleanup policy.

### Features

- recordings older than 90 days automatically removed
- scheduled cleanup using systemd timer
- storage lifecycle management

---

# Telegram Remote Administration

## Objective

Enable secure remote management of TITAN infrastructure from anywhere using Telegram.

---

## Security

- access restricted to authorized Telegram user ID
- unauthorized users cannot execute commands

---

## Implemented Commands

- /ping
- /help
- /status
- /docker
- /services
- /camera
- /gpu
- /restart
- /reboot

---

## Capabilities

### Infrastructure Monitoring

- system status
- Docker container status
- GPU utilization
- recorder service status
- camera subsystem health

### Remote Administration

- restart Docker containers
- restart infrastructure services
- remotely reboot server

---

## Architecture

```
Telegram

        ↓

Python Telegram Bot

        ↓

Command Router

        ↓

Infrastructure Modules

        ↓

Docker
Systemd
Linux
```

---

# Homepage Expansion

Homepage dashboard expanded with additional services including:

- go2rtc
- Home Assistant
- Netdata
- Surveillance
- Infrastructure utilities

Providing centralized access to the complete homelab environment.

---

# Infrastructure Organization

Automation scripts standardized under:

```
/srv/scripts/
    surveillance/
```

Centralized script management improves maintainability and scalability.

---

# Result

TITAN now supports:

- centralized camera management
- continuous surveillance recording
- automated recording retention
- Telegram-based remote administration
- infrastructure service control
- centralized surveillance architecture
- modular automation framework

---

# TITAN now operates as

- AI inference platform
- NAS environment
- media server
- infrastructure monitoring platform
- surveillance recording server
- remotely managed homelab
- scalable self-hosted infrastructure ecosystem

---

# Next Phase

## Intelligent Human-Computer Interaction Layer

Planned:

- Jarvis voice assistant
- speech recognition
- text-to-speech
- AI conversation engine
- voice-controlled infrastructure management
- natural language command execution
- AI-powered camera scene understanding
- intelligent automation workflows