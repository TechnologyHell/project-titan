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
~ ssh user@192.186.x.x


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


