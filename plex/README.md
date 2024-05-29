# :tv: Plex Media Services

Plex is a media delivery system that can be used to serve video content for the home network and beyond the walls of the home.

# Mapping

The docker container maps to a local folder set which contains the media files, database and metadata consumed by the Plex Media service. 

Operating in a docker container, the config is set for host networking to utilise the host machine's IP address exposing a single port, 32400.

# Construction

## Pre-build items

Mount the secondary data disk on the host system which holds the media files:

```
sudo mount /dev/sdb1 /media/plexdrive/
```

### Install and Configure Docker

**Install Docker**
```
apt install docker-compose -y
```

**Enable Service**
Launch the service and set it to run on boot
```
systemctl start docker
systemctl enable docker
```

**User Permissions**
Set up user and group for the docker container 

```
sudo usermod -aG docker $USER
newgrp docker
```

## Docker Compose file

Description:
- The volumes in this compose file map to the host filesystem volumes. The networking is set to host mode, so it acts as if it were the host machine ( rather than requesting further IP addresses from DHCP )
- The PUID and PGID are obtained from the /etc/password file on the host with the groupid for your current user or the plex user ( likely 1000 ).
- The Plex Claim must be obtained from [The Plex Website](https://plex.tx/claim). This has a 4 minute timeout on the claim so you must launch the docker container quickly after adding the claim ID to the compose file.

```
services:
  plex:
    image: plexinc/pms-docker:latest
    container_name: plex
    network_mode: host
    environment:
      - PUID=999
      - PGID=999
      - TZ=Europe/London
      - PLEX_CLAIM=claim-id-here
    volumes:
      - /media/plexdrive/database:/config
      - /media/plexdrive/transcode:/transcode
      - /media/plexdrive/media:/data
    restart: unless-stopped
```


# Run the container service

Launch the docker service on your machine.

Head to the directory of the docker compose yaml file and run:

```
docker-compose up -d
```

