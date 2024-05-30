# :tv: Plex Media Services

Plex is a media delivery system that can be used to serve video content for the home network and beyond the walls of the home.

# Information

The docker container maps to a local folder set which contains the media files, database and metadata consumed by the Plex Media service. 

Operating in a docker container, the config is set for host networking to utilise the host machine's IP address exposing a single port, 32400.

In this configuration we are using a data mount point on the host for all of the plex media content.

----

# Construction

## Pre-build items

### If you have a secondary data disk
If you have to mount the secondary data disk on the host system which holds the media files:

**Local Drive**
Find the drive ID for the unmounted item with a :
```
lsblk
```

```
sudo mount /dev/sdb1 /media/plexdrive/
```

This plex data mount point can be used later in the configuration to map into the container. You could utilise a network mount from a NAS box in a similar way, simply use that mount point as necessary.

**Example Samba Mount from NAS:**

```
sudo mount -t cifs -o username=netadmin //192.168.1.10/plexmedia /media/plexdrive/
```

### Install and Configure Docker

**Install Docker**
```
sudo apt install docker-compose -y
```

**Enable Service**
Launch the service and set it to run on boot
```
sudo systemctl start docker
sudo systemctl enable docker
```

**User Permissions**
Set up user and group for the docker container 

```
sudo usermod -aG docker $USER
sudo newgrp docker
```

**Sign in to Docker**
You must sign in to Docker in order to pull the containers as necessary in the next steps:

```
docker login
```

Enter your details in the command prompt for your Docker Account [Registered for Docker?](https://hub.docker.com)

## Docker Compose file

[View Compose File](docker-compose.yml)

Description:
- The volumes in this compose file map to the host filesystem volumes. The networking is set to host mode, so it acts as if it were the host machine ( rather than requesting further IP addresses from DHCP ). The below code maps the host directories to the container directories, modify yours as appropriate.
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

# Verify Running

Check your docker container is running with:

```
docker ps
```

***Web Interface:***
Launch your web browser and navigate to your machine's name or IP address and add the port number after it, e.g. : https://mymachine:32400

