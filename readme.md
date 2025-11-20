# Docker Services Repository

This repository contains individual Docker Compose configurations for running services in separate Docker containers. Each service uses **Docker named volumes** for data persistence, ensuring cross-platform consistency (Windows, Mac, Linux) and better performance. All services are connected to the same Docker network to enable communication between them.

## Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+

## Available Services

| Service | Port | Description | Default Credentials |
|---------|------|-------------|---------------------|
| **MongoDB** | 27017 | NoSQL Database | Username: `root`, Password: `root` |
| **PostgreSQL** | 5432 | Relational Database | Username: `postgres`, Password: `root`, DB: `initialDB` |
| **Redis** | 6379 | In-Memory Cache | No password (configure in redis.conf if needed) |
| **pgAdmin** | 5050 | PostgreSQL GUI | Email: `pgadmin@sonu.info`, Password: `pgadmin` |
| **Redis Insight** | 5540 | Redis GUI | No authentication required |

## Getting Started

### 1. Create Docker Network

Before starting any service, create a Docker network named `docker_services_network`:

```bash
docker network create docker_services_network
```

### 2. Start a Service

Navigate to the service directory and start it:

```bash
cd mongodb
docker-compose up -d
```

### 3. Verify Service is Running

```bash
docker-compose ps
```

### 4. View Service Logs

```bash
docker-compose logs -f
```

### 5. Stop a Service

```bash
docker-compose down
```

## Service-Specific Instructions

### MongoDB
```bash
cd mongodb
docker-compose up -d
```
- Connection string: `mongodb://root:root@localhost:27017`
- Optional: Edit `mongod.conf` for custom configuration

### PostgreSQL
```bash
cd postgresql
docker-compose up -d
```
- Connection: Host: `localhost`, Port: `5432`, User: `postgres`, Password: `root`
- Initial database `initialDB` is created automatically

### Redis
```bash
cd redis
docker-compose up -d
```
- Connection: `localhost:6379`
- Optional: Create `redis.conf` for custom configuration

### pgAdmin (PostgreSQL GUI)
```bash
cd pgadmin
docker-compose up -d
```
1. Access at: http://localhost:5050
2. Login with: `pgadmin@sonu.info` / `pgadmin`
3. Add PostgreSQL server connection:
   - Host: `postgres` (or `host.docker.internal` if PostgreSQL not in same network)
   - Port: `5432`
   - User: `postgres`
   - Password: `root`

### Redis Insight (Redis GUI)
```bash
cd redis-insight
docker-compose up -d
```
1. Access at: http://localhost:5540
2. Add Redis connection:
   - Host: `redis` (or `host.docker.internal` if Redis not in same network)
   - Port: `6379`

## Docker Volume Management

All data is stored in Docker named volumes for better performance and cross-platform consistency.

### List All Volumes
```bash
docker volume ls
```

### Inspect a Volume
```bash
docker volume inspect mongodb_data
docker volume inspect postgres_data
docker volume inspect redis_data
docker volume inspect pgadmin_data
docker volume inspect redisinsight_data
```

### Backup a Volume
```bash
# Example: Backup MongoDB data
docker run --rm -v mongodb_data:/data -v ${PWD}:/backup alpine tar czf /backup/mongodb_backup.tar.gz -C /data .
```

### Restore a Volume
```bash
# Example: Restore MongoDB data
docker run --rm -v mongodb_data:/data -v ${PWD}:/backup alpine tar xzf /backup/mongodb_backup.tar.gz -C /data
```

### Remove a Volume (⚠️ This will delete all data!)
```bash
# Stop the service first
cd mongodb
docker-compose down

# Remove the volume
docker volume rm mongodb_data
```

### Remove All Unused Volumes
```bash
docker volume prune
```

## Starting All Services at Once

You can start multiple services by running docker-compose in each directory:

```bash
# Start all core services
cd mongodb && docker-compose up -d && cd ..
cd postgresql && docker-compose up -d && cd ..
cd redis && docker-compose up -d && cd ..
cd pgadmin && docker-compose up -d && cd ..
cd redis-insight && docker-compose up -d && cd ..
```

## Troubleshooting

### Service won't start
```bash
# Check logs
docker-compose logs

# Check if port is already in use
netstat -an | findstr :27017  # Windows
lsof -i :27017                # Mac/Linux
```

### Network not found
```bash
docker network create docker_services_network
```

### Reset everything
```bash
# Stop all services
docker-compose down

# Remove all volumes (⚠️ deletes all data!)
docker volume rm mongodb_data postgres_data redis_data pgadmin_data redisinsight_data

# Recreate network
docker network create docker_services_network
```

## Configuration Files

- **MongoDB**: Edit `mongodb/mongod.conf` (optional)
- **Redis**: Create `redis/redis.conf` (optional)

Configuration files are mounted as read-only bind mounts and can be edited directly in your file system.

## Notes

- All services use `restart: always` for automatic restart on system reboot
- Data persists across container restarts using Docker named volumes
- Services on the same network can communicate using service names (e.g., `postgres`, `mongo`, `redis`)
- Named volumes provide better performance than bind mounts, especially on Windows and Mac



