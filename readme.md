# Docker Services Repository

This repository contains individual Docker Compose configurations for running services in separate Docker containers. Each service is configured to use volumes mounted from local directories, ensuring data persistence. All services are connected to the same Docker network to enable communication between them.

## Getting Started

Before starting any service, ensure that a Docker network named `docker_services_network` is created. This network enables communication between the containers.

To create the Docker network, run the following command:

```
docker network create docker_services_network
```

## How to Use Services

To use a specific service, navigate to its directory using the command line. For example, if you want to use PostgreSQL, navigate to the `postgresql` directory:

```
cd postgresql
```
Then, run the following command to start the service:
```
docker-compose up -d
```



