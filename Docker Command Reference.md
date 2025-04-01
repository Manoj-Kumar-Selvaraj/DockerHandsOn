# Docker Command Reference (A-Z)

## 1. Docker System Management

### 1.1 Check Docker Version
```bash
docker version  # Displays Docker client and server version details
docker info     # Displays system-wide information about Docker
```

### 1.2 Manage Docker System
```bash
docker system df         # Show disk usage of Docker

docker system df -v      # Show detailed disk usage

docker system prune      # Remove all unused containers, networks, and images

docker system prune -a   # Remove all unused images as well
```

## 2. Working with Images

### 2.1 Listing Images
```bash
docker images            # List all locally available images
docker images -a         # List all images including intermediate ones
docker images --digests  # Show digests for images
docker images --no-trunc # Show full image details
```

### 2.2 Pulling Images
```bash
docker pull <image>:<tag>     # Pull an image from Docker Hub
docker pull ubuntu:latest     # Example: Pull latest Ubuntu image
```

### 2.3 Removing Images
```bash
docker rmi <image_id>     # Remove an image
docker rmi $(docker images -q)  # Remove all images
```

## 3. Containers Management

### 3.1 Running Containers
```bash
docker run -it <image> /bin/bash    # Start a container with interactive mode
docker run -d <image>               # Run container in detached mode
docker run --rm <image>             # Remove container after exit
docker run -p 8080:80 <image>       # Map host port to container port
```

### 3.2 Listing Containers
```bash
docker ps            # List running containers
docker ps -a         # List all containers (including stopped ones)
docker ps -q         # Show only container IDs
docker ps --format "{{.ID}}: {{.Image}}"  # Custom format
```

### 3.3 Stopping & Removing Containers
```bash
docker stop <container_id>  # Stop a running container
docker rm <container_id>    # Remove a container
docker rm $(docker ps -aq)  # Remove all stopped containers
```

### 3.4 Viewing Container Logs
```bash
docker logs <container_id>             # Show logs of a container
docker logs -f <container_id>          # Follow logs
docker logs --tail 50 <container_id>   # Show last 50 log lines
```

### 3.5 Inspecting Containers
```bash
docker inspect <container_id>         # Show detailed info about a container
docker inspect --format='{{.State.Running}}' <container_id> # Check running status
```

## 4. Networking

### 4.1 List Networks
```bash
docker network ls    # List all networks
```

### 4.2 Create & Remove Networks
```bash
docker network create my_network     # Create a new network
docker network rm my_network         # Remove a network
```

### 4.3 Connect & Disconnect Containers
```bash
docker network connect my_network <container_id>   # Connect a container to a network
docker network disconnect my_network <container_id> # Disconnect container
```

## 5. Docker Volumes

### 5.1 Creating and Listing Volumes
```bash
docker volume create my_volume   # Create a named volume
docker volume ls                 # List all volumes
```

### 5.2 Inspect and Remove Volumes
```bash
docker volume inspect my_volume  # Show details of a volume
docker volume rm my_volume       # Remove a volume
```

## 6. Docker Compose

### 6.1 Managing Services
```bash
docker-compose up -d       # Start services in detached mode
docker-compose down        # Stop and remove containers
```

### 6.2 Viewing Logs
```bash
docker-compose logs -f     # View logs for all services
```

## 7. Security & User Management

### 7.1 Manage User Groups
```bash
sudo usermod -aG docker <username>  # Add a user to the docker group
```

---

This is a complete **A-Z guide** covering all essential **Docker commands with options**.


