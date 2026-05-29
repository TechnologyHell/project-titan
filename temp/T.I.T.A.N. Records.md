# T.I.T.A.N.

Trusted Infrastructure for Technology & Advanced Networking









**Hardware Configuration (last updated 24.11.2025)**

Processor 	: AMD Ryzen 5 2600 6c/12t Processor
Motherboard 	: MSI B450m Pro M2 MAX
RAM 		: T-Force XTREEM 8GB x2 16GB DDR4 4500MHz
Storage 	: WD Blue SN550e 2TB Gen3x4 NVME SSD
Graphics Card 	: Zotac GTX 1050ti 4GB GDDR5
Power Supply 	: Gigabyte P450B Bronze SMPS



BIOS Changes :

* UEFI Mode
* Secure Boot Enabled
* Trusted Platform Module Enabled
* Restore on Power Loss - Last State
* Wake Up Event - PCIe Device









**Operating System Installation (28.10.2025)**



Image : ubuntu-24.04.3-live-server-amd64
Created bootable USB via Rufus, clean install over the system



Partition Setup :
DEVICE						TYPE		SIZE

Western Digital SN550E\_20290L640012\_1		local disk	1.819T

partition 1 , mounted at /boot/efi		fat32		1.049G

partition 2 , mounted at /boot			ext4		2.000G

partition 3 					swap		16.000G

partition 4 , mounted at /			ext4		50.000G

partition 5 , mounted at /srv			ext4		1.751T









**Configured SSH for Remote Access (05.11.2025)**



1. Installed OpenSSH Server Package

 	sudo apt update

 	sudo apt install openssh-server

2\. Start and Enable the SSH Service

 	sudo systemctl start ssh

 	sudo systemctl enable ssh



Now accessible from remote PC through terminal
Command to access : ssh <user\_name>@<ip\_address>









**Configured WakeOnLAN for Remote Power ON (12.11.2025)**  && ESP32 Remote Trigger



1. Install ethtools for WOL feature

 	sudo apt install ethtool

 		Check network interface name "ip link" (enp37s0)

2\. Enable WOL for the interface

 	sudo ethtool -s <interface\_name> wol g



3\. Make WOL persistent on every boot

 	sudo nano /etc/systemd/system/wol.service



\[Unit]

Description=Enable Wake On Lan

After=network-online.target

Wants=network-online.target



\[Service]

Type=oneshot

ExecStart=/sbin/ethtool -s enp3s0 wol g



\[Install]

WantedBy=multi-user.target



4\. Enable the custom service

 	sudo systemctl daemon-reload

 	sudo systemctl enable wol

 	sudo systemctl start wol



Now can wake the server from any device on the network

* Android - via WakeOnLAN apps
* Linux - sudo apt install wakeonlan  ->  wakeonlan <MAC\_ADDRESS>
* Windows :

  1. Allow module installation \& NuGet
     Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force

 	Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force

   2. Install WOL Module from PSGallery
Install-Module -Name WakeOnLan -Force

   3. Import the module (not always needed)

 	Import-Module WakeOnLan

   4. Send WOL Packet

 	Invoke-WakeOnLan -MacAddress "xx:xx:xx:xx:xx:xx"





DONE DAY 1*******************************************



**Setting up of Samba - Root server access via Mapped Network Drive**



1. Installation : 		sudo apt install samba
2. Created Shared Folder : 	sudo mkdir -p /srv/shared
3. Set Folder Ownership : 	sudo chown tech:tech /srv
4. Set Permissions : 		sudo chmod 770 /srv
5. Create UsedID \& PWD : 	sudo smbpasswd -a tech
6. Enable User Profile : 	sudo smbpasswd -e tech
7. Edit Samba Conf File : 	sudo nano /etc/samba/smb.conf

\[srv]

&nbsp;	comment = TITAN main share

&nbsp;	path = /srv

&nbsp; 	browsable = yes

&nbsp; 	read only = no

&nbsp;	writable = yes

&nbsp;	valid users = tech

&nbsp;	force user = tech

&nbsp;	force group = tech

&nbsp;	create mask = 0660

&nbsp;	directory mask = 0770


### Additional shared locations : 
[home]
   comment = TITAN Home
   path = /home/tech
   browsable = yes
   read only = no
   writable = yes
   valid users = tech
   create mask = 0660
   directory mask = 0770

[server]
   comment = TITAN Root (READ ONLY)
   path = /
   browsable = yes
   read only = yes
   writable = no
   valid users = tech

   # Hard safety rails
   veto files = /boot/* /dev/* /proc/* /sys/* /run/* /tmp/* /var/tmp/*
   hide files = /boot /dev /proc /sys /run /tmp /var/tmp


8\. Samba Restart : 	sudo systemctl restart smbd nmbd

9\. Enable Samba : 	sudo systemctl enable smbd nmbd



Mapping it as a network drive on Windows : 
1. Map Network Drive

2\. \\\\<SERVER_IP>\\srv

3\. Enter Credentials

4\. Mount to empty Drive Letter



Now the shared folder is accessible to all devices as a network storage.
Secured by user credentials.







**Setting up of Jellyfin Media Server**



1. Create folder, and assign permissions

sudo mkdir -p jellyfin/movies

sudo chown -R tech:tech /srv/jellyfin

sudo chmod -R 770 /srv/jellyfin



2\. Install Jellyfin from official repo

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://repo.jellyfin.org/jellyfin\_team.gpg.key \\

&nbsp;| sudo gpg --dearmor -o /etc/apt/keyrings/jellyfin.gpg



echo "deb \[arch=$( dpkg --print-architecture ) signed-by=/etc/apt/keyrings/jellyfin.gpg] \\

&nbsp;https://repo.jellyfin.org/ubuntu $(lsb\_release -cs) main" \\

&nbsp;| sudo tee /etc/apt/sources.list.d/jellyfin.list



sudo apt update

sudo apt install jellyfin



3\. Allow access to folders

sudo apt install acl

sudo setfacl -m u:jellyfin:rx /srv

sudo setfacl -R -m u:jellyfin:rx /srv/jellyfin



4\. Reconfig access permissions

ls -ld /srv /srv/jellyfin /srv/jellyfin/movies

ls -l /srv/jellyfin/movies | head

sudo usermod -aG tech jellyfin

sudo chgrp -R tech /srv

sudo chmod -R 775 /srv









**Setting up NextCloud**



1\. Prepare the Nextcloud data directory

sudo mkdir -p /srv/nextcloud

sudo chown -R www-data:www-data /srv/nextcloud

sudo chmod 770 /srv/nextcloud





2\. Install Nextcloud (standard LAMP style)

Install webstack:	sudo apt update

&nbsp;			sudo apt install apache2 mariadb-server libapache2-mod-php \\

&nbsp; php php-gd php-mbstring php-xml php-zip php-curl php-mysql php-intl php-bz2 php-imagick



3\. Create a database and user in MariaDB:

sudo mysql



CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4\_general\_ci;

&nbsp;CREATE USER 'tech'@'localhost' IDENTIFIED BY '5624';

&nbsp;GRANT ALL PRIVILEGES ON nextcloud.\* TO 'tech'@'localhost';

FLUSH PRIVILEGES;

EXIT;





4.Download and place Nextcloud:

cd /tmp

wget https://download.nextcloud.com/server/releases/latest.tar.bz2

tar -xjf latest.tar.bz2

sudo mv nextcloud /var/www/

sudo chown -R www-data:www-data /var/www/nextcloud





5.Configure Apache virtual host (basic):

sudo nano /etc/apache2/sites-available/nextcloud.conf



<VirtualHost \*:80>

&nbsp;   ServerName <SERVER_IP>



&nbsp;   DocumentRoot /var/www/nextcloud



&nbsp;   <Directory /var/www/nextcloud>

&nbsp;       Require all granted

&nbsp;       AllowOverride All

&nbsp;       Options FollowSymLinks MultiViews

&nbsp;   </Directory>

</VirtualHost>





5.1 Enable site and required modules: 

sudo a2ensite nextcloud.conf

sudo a2enmod rewrite headers env dir mime

sudo systemctl reload apache2





Now open http://<SERVER_IP> in your browser to start the web installer. 








Installing NVIDIA Drivers


0️⃣ First: Check current GPU visibility (baseline)

Before installing anything, confirm the system sees the GPU.

lspci | grep -i nvidia


You should see something like:

01:00.0 VGA compatible controller: NVIDIA Corporation GP107 [GeForce GTX 1050 Ti]




1️⃣ Remove any broken / partial NVIDIA installs (important)

Do this even if you think nothing is installed.

sudo apt purge 'nvidia*'
sudo apt autoremove
sudo apt autoclean
sudo reboot


After reboot, confirm nouveau is active (expected at this stage):

lsmod | grep nouveau



2️⃣ Enable Ubuntu restricted drivers (REQUIRED)
sudo add-apt-repository restricted
sudo apt update

3️⃣ Identify the correct driver for GTX 1050 Ti

Run:

ubuntu-drivers devices


You should see output similar to:

driver   : nvidia-driver-535 - distro non-free recommended
driver   : nvidia-driver-470 - distro non-free



4️⃣ Install NVIDIA driver (server-safe method)
✅ Recommended (no GUI needed)
sudo apt install nvidia-driver-535


(Replace 535 with whatever Ubuntu recommends)





0️⃣ Sanity checks (do these first)
CUDA available?
nvcc --version


If not:

sudo apt install nvidia-cuda-toolkit




CREATION OF CUSTOM COMMANDS


TITAN-SPECS
sudo nano /usr/local/bin/titan-specs

#!/bin/bash

CPU_MODEL=$(lscpu | awk -F: '/Model name/ {print $2}' | xargs)
CORES=$(lscpu | awk '/^CPU\(s\)/ {print $2}')
THREADS=$(lscpu | awk '/Thread\(s\) per core/ {print $4}')
CORES_PER_SOCKET=$(lscpu | awk '/Core\(s\) per socket/ {print $4}')
THREADS_TOTAL=$((CORES_PER_SOCKET * THREADS))

GPU_NAME=$(nvidia-smi --query-gpu=name --format=csv,noheader 2>/dev/null)
GPU_VRAM=$(nvidia-smi --query-gpu=memory.total --format=csv,noheader 2>/dev/null | awk '{printf "%.0f GB", $1/1024}')

RAM_TOTAL=$(free -h | awk '/Mem:/ {print $2}')
RAM_SPEED=$(dmidecode -t memory | awk '/Speed:/ && $2!="Unknown" {print $2; exit}')
RAM_STICKS=$(dmidecode -t memory | awk '/Size:/ && $2!="No" {c++} END {print c}')

SSD=$(lsblk -d -o MODEL,SIZE,TRAN | awk 'NR==2')
MB=$(dmidecode -t baseboard | awk -F: '/Product Name/ {print $2}' | xargs)

OS=$(lsb_release -ds)
KERNEL=$(uname -r)

echo ""
echo "SYSTEM SPECIFICATIONS"
echo ""
echo "CPU         : $CPU_MODEL ($CORES cores / $THREADS_TOTAL threads)"
echo "GPU         : $GPU_NAME ($GPU_VRAM VRAM)"
echo "RAM         : $RAM_TOTAL DDR4 @ ${RAM_SPEED:-Unknown} MHz ($RAM_STICKS sticks)"
echo "Storage     : WD Blue SN550E 2TB NVMe (PCIe Gen3 x4)"
echo "Motherboard : $MB"
echo "OS          : $OS (kernel $KERNEL)"
echo ""


Make Executable : sudo chmod +x /usr/local/bin/titan-specs




TITAN-TOP
sudo nano /usr/local/bin/titan-top

#!/bin/bash

# ---------- SETUP ----------
trap "stty sane; echo; exit" INT TERM
stty -echo -icanon time 0 min 0

IFACE=$(ip route | awk '/default/ {print $5; exit}')

prev_rx=$(cat /sys/class/net/$IFACE/statistics/rx_bytes)
prev_tx=$(cat /sys/class/net/$IFACE/statistics/tx_bytes)

# ---------- FUNCTIONS ----------
fmt_speed() {
  local bytes=$1
  local kb=$((bytes / 1024))
  if [ "$kb" -lt 1024 ]; then
    echo "${kb} KB/s"
  else
    awk "BEGIN {printf \"%.2f MB/s\", $kb/1024}"
  fi
}

# ---------- STATIC LAYOUT ----------
clear
echo "TITAN REALTIME TASK MANAGER  (press any key to exit)"
echo ""
echo "CPU Load        :"
echo "CPU Temperature :"
echo ""
echo "GPU Load        :"
echo "GPU Temperature :"
echo ""
echo "Memory Used     :"
echo "Memory Avail    :"
echo ""
echo "Disk Used (/)   :"
echo "Disk Free (/)   :"
echo ""
echo "Network ($IFACE)"
echo "RX Speed        :"
echo "TX Speed        :"

# ---------- LOOP ----------
while true; do
  # CPU
  CPU_LOAD=$(awk '{print $1}' /proc/loadavg)
  CPU_TEMP=$(awk '{printf "%.1f", $1/1000}' /sys/class/hwmon/hwmon*/temp1_input 2>/dev/null | head -n1)

  # GPU
  GPU_LOAD=$(nvidia-smi --query-gpu=utilization.gpu --format=csv,noheader,nounits 2>/dev/null)
  GPU_TEMP=$(nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader 2>/dev/null)

  # Memory
  MEM_USED=$(free -h | awk '/Mem:/ {print $3}')
  MEM_AVAIL=$(free -h | awk '/Mem:/ {print $7}')

  # Disk
  DISK_USED=$(df -h / | awk 'NR==2 {print $3}')
  DISK_FREE=$(df -h / | awk 'NR==2 {print $4}')

  # Network speed
  rx_now=$(cat /sys/class/net/$IFACE/statistics/rx_bytes)
  tx_now=$(cat /sys/class/net/$IFACE/statistics/tx_bytes)

  rx_speed=$((rx_now - prev_rx))
  tx_speed=$((tx_now - prev_tx))

  prev_rx=$rx_now
  prev_tx=$tx_now

  RX_FMT=$(fmt_speed $rx_speed)
  TX_FMT=$(fmt_speed $tx_speed)

  # ---------- UPDATE VALUES INLINE ----------
  tput cup 2 20;  printf "%-10s" "$CPU_LOAD"
  tput cup 3 20;  printf "%-10s °C" "${CPU_TEMP:-N/A}"

  tput cup 5 20;  printf "%-10s %%" "${GPU_LOAD:-N/A}"
  tput cup 6 20;  printf "%-10s °C" "${GPU_TEMP:-N/A}"

  tput cup 8 20;  printf "%-15s" "$MEM_USED"
  tput cup 9 20;  printf "%-15s" "$MEM_AVAIL"

  tput cup 11 20; printf "%-15s" "$DISK_USED"
  tput cup 12 20; printf "%-15s" "$DISK_FREE"

  tput cup 15 20; printf "%-15s" "$RX_FMT"
  tput cup 16 20; printf "%-15s" "$TX_FMT"

  # Exit on keypress
  read -n1 -t1 key && break
done

stty sane
echo ""



Make Executable : sudo chmod +x /usr/local/bin/titan-top


TITAN-SERVICES
sudo nano /usr/local/bin/titan-services

#!/bin/bash

svc() {
  systemctl is-active --quiet "$1" && echo "running" || echo "stopped"
}

JELLYFIN=$(svc jellyfin)
NEXTCLOUD=$(svc apache2 || svc nginx)
SAMBA=$(svc smbd)
TUNNEL=$(svc cloudflared)
DOCKER=$(svc docker)

WHISPER=$(pgrep -f whisper >/dev/null && echo "running" || echo "stopped")
JARVIS=$(pgrep -f jarvis >/dev/null && echo "running" || echo "stopped")

echo ""
echo "SERVICES"
echo ""
printf "Jellyfin     : %-10s Nextcloud   : %s\n" "$JELLYFIN" "$NEXTCLOUD"
printf "NAS (Samba)  : %-10s Tunnel      : %s\n" "$SAMBA" "$TUNNEL"
printf "Whisper      : %-10s JARVIS      : %s\n" "$WHISPER" "$JARVIS"
printf "Docker       : %-10s\n" "$DOCKER"
echo ""


Make Executable : sudo chmod +x /usr/local/bin/titan-services

















\-

