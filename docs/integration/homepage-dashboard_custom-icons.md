# Homepage Dashboard Custom Icons

## Objective

Create custom service icons for infrastructure dashboard services not available in the default Homepage icon repository.

Services:

* Samba
* CP Plus Camera
* Logitech Webcam
* Camera Streams

---

## Directory Structure

Compose Project:

/home/tech/docker/homepage

Icons:

/home/tech/docker/homepage/icons

---

## Docker Mapping

docker-compose.yml

volumes:

* /srv/docker/homepage:/app/config
* /home/tech/docker/homepage/icons:/app/public/icons

---

## Usage

Custom icons referenced using:

icon: /icons/<filename>.png

Examples:

icon: /icons/samba.png
icon: /icons/cpplus.png
icon: /icons/logitech.png

---

## Verification

Open directly:

http://SERVER_IP:3000/icons/samba.png

Successful image display confirms correct icon deployment.
