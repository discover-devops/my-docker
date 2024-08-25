Here are some commonly used Docker commands:

### **Basic Commands:**
- **`docker version`**: Show Docker version information.
- **`docker info`**: Display system-wide information about Docker.
- **`docker images`**: List all Docker images on the system.
- **`docker ps`**: List running containers.
- **`docker ps -a`**: List all containers (running and stopped).
- **`docker pull [image-name]`**: Pull an image from a Docker registry.
- **`docker run [image-name]`**: Run a container from an image.
- **`docker stop [container-id]`**: Stop a running container.
- **`docker start [container-id]`**: Start a stopped container.
- **`docker restart [container-id]`**: Restart a container.
- **`docker rm [container-id]`**: Remove a stopped container.
- **`docker rmi [image-name]`**: Remove an image from the system.
- **`docker exec -it [container-id] /bin/bash`**: Access a running container interactively.
  
### **Image Management:**
- **`docker build -t [image-name] .`**: Build a Docker image from a Dockerfile in the current directory.
- **`docker tag [image-id] [new-name]`**: Tag an image with a new name.
- **`docker push [image-name]`**: Push an image to a Docker registry.

### **Container Management:**
- **`docker logs [container-id]`**: View logs from a container.
- **`docker inspect [container-id]`**: Display detailed information about a container.
- **`docker stats [container-id]`**: Display resource usage statistics for a container.
- **`docker cp [container-id]:/path/to/file .`**: Copy a file from a container to the host system.
  
### **Networking:**
- **`docker network ls`**: List all Docker networks.
- **`docker network create [network-name]`**: Create a new Docker network.
- **`docker network inspect [network-name]`**: Display details about a specific network.
- **`docker network connect [network-name] [container-id]`**: Connect a container to a network.
- **`docker network disconnect [network-name] [container-id]`**: Disconnect a container from a network.

### **Volume Management:**
- **`docker volume ls`**: List all Docker volumes.
- **`docker volume create [volume-name]`**: Create a new Docker volume.
- **`docker volume inspect [volume-name]`**: Display detailed information about a volume.
- **`docker volume rm [volume-name]`**: Remove a Docker volume.

These commands cover a wide range of Docker operations, from basic container management to advanced networking and storage configurations.
