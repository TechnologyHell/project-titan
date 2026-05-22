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