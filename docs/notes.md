# Development Notes

## 2026-05-20

### Completed

- Installed Ubuntu Server
- Configured SSH remote access
- Installed hardware monitoring tools
- Installed NVIDIA drivers
- Verified CUDA support
- Verified GPU functionality
- Configured static DHCP reservation

### Observations

- BIOS temperatures were initially high on stock cooler
- Linux idle temperatures stabilized properly after installation
- NVIDIA open drivers installed cleanly without MOK enrollment

### Next Steps

- Setup Custom Scripts & Services
- Install Docker
- Configure NVIDIA container runtime
- Deploy Ollama
- Setup Open WebUI


## 2026-05-21

### Completed

- Installed Docker Engine
- Installed Docker Compose plugin
- Configured Docker service
- Enabled Docker startup on boot
- Added non-root Docker permissions

### Learnings

- Docker containers isolate applications from the host system
- Docker Compose allows infrastructure deployment through YAML definitions
- Persistent data must be stored outside containers using mounted volumes


## 2026-05-21

### Completed

- Created Docker Compose based Jellyfin deployment
- Learned Docker container basics
- Learned volume mapping concepts
- Learned port binding concepts
- Configured persistent media storage

### Learnings

- Containers are isolated runtime environments
- Docker Compose acts as infrastructure-as-code
- Persistent data must be bind-mounted outside containers
- Containers can auto-restart independently from the host system


## Jellyfin Deployment Notes

Successfully migrated from native-service mindset to containerized deployment model.

### Key Learnings

- Containers should remain stateless wherever possible
- Persistent application data must exist outside containers
- Docker Compose simplifies infrastructure deployment and recovery
- Port mapping exposes containerized services to the host network

### Current Media Structure

/srv/media/movies
/srv/media/shows
/srv/media/anime


## NVIDIA Docker Runtime

### Completed

- Installed NVIDIA Container Toolkit
- Configured Docker GPU runtime
- Verified GPU passthrough inside containers

### Learnings

- Containers require explicit GPU passthrough configuration
- NVIDIA Container Toolkit bridges Docker and host GPU drivers
- CUDA workloads inside containers use host GPU resources



## Dockge

### Completed

- Installed Dockge container
- Configured Docker socket access
- Configured compose stack management
- Verified Jellyfin stack detection

### Learnings

- Docker socket provides container management access
- Compose stacks can be centrally managed through Dockge
- Infrastructure can be managed visually without relying entirely on CLI



## Uptime Kuma

### Completed

- Installed Uptime Kuma
- Configured infrastructure monitoring
- Added service uptime checks
- Added SSH TCP monitoring

### Learnings

- Monitoring infrastructure is a core DevOps practice
- HTTP monitoring validates service availability
- TCP monitoring validates port accessibility
- Observability improves operational awareness



## Open WebUI

### Completed

- Installed Open WebUI
- Connected frontend to Ollama backend
- Enabled browser-based AI interaction
- Configured persistent AI conversations

### Learnings

- Open WebUI acts as frontend layer for local AI
- Ollama remains the backend inference engine
- Frontend/backend separation improves architecture modularity
- Local AI platforms can fully replicate modern AI chat UX



