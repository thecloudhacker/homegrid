# :bar_chart: Monitoring System

# Description
Various monitoring services and options for the local network.


----

## Nagios : Extendable Monitoring Test System
A test installation of System Monitoring with Nagios on Docker.

A Dockerised version of Nagios Core monitoring. This is the base installation system allowing the user to build-upon and extend a base Nagios system.

Included in this directory are the [dockerfile](dockerfile) and [docker-compose.yml](docker-compose.yml) files utilised in the construction of the service.

If you build the service locally to run in bridged mode, you can use the docker-compose.yml file as-is.

## Building the Nagios Core container

```
docker build -t nagios .
```

## Launching the container
```
docker compose up -d
```

## Accessing the web front-end

In your browser head to http://localhost:8080/nagios and enter the authentication details:

```
Username: nagiosadmin
Password: nagadmin123
``` 