Docker commands are used to interact with Docker, a popular platform for developing, shipping, and running applications using containerization. 
Here's a list of some useful Docker commands along with their usage and examples:

1. **docker run**: Run a command in a new container.
   ```
   docker run <image_name>
   ```
   Example: `docker run ubuntu`

2. **docker pull**: Pull an image or a repository from a registry.
   ```
   docker pull <image_name>
   ```
   Example: `docker pull nginx`

3. **docker build**: Build an image from a Dockerfile.
   ```
   docker build -t <image_name> <path_to_Dockerfile>
   ```
   Example: `docker build -t my_image .`

4. **docker ps**: List running containers.
   ```
   docker ps
   ```
   Example: `docker ps`

5. **docker images**: List images.
   ```
   docker images
   ```
   Example: `docker images`

6. **docker stop**: Stop one or more running containers.
   ```
   docker stop <container_id>
   ```
   Example: `docker stop abc123def456`

7. **docker rm**: Remove one or more containers.
   ```
   docker rm <container_id>
   ```
   Example: `docker rm abc123def456`

8. **docker rmi**: Remove one or more images.
   ```
   docker rmi <image_name>
   ```
   Example: `docker rmi my_image`

9. **docker exec**: Run a command in a running container.
   ```
   docker exec -it <container_id> <command>
   ```
   Example: `docker exec -it abc123def456 bash`

10. **docker-compose**: Define and run multi-container Docker applications with a YAML file.
   ```
   docker-compose up
   ```
   Example: `docker-compose up`

11. **docker logs**: Fetch the logs of a container.
   ```
   docker logs <container_id>
   ```
   Example: `docker logs abc123def456`

12. **docker inspect**: Return low-level information on Docker objects.
   ```
   docker inspect <object_id>


====================================================

Common Docker commands along with examples to help you understand their usage:

### 1. `docker --version`
Displays the installed Docker version.

docker --version


### 2. `docker info`
Displays system-wide information about Docker.

docker info


### 3. `docker pull`
Pulls an image from a Docker registry (e.g., Docker Hub).

docker pull nginx


### 4. `docker run`
Runs a container from a Docker image.

docker run -d -p 80:80 --name mynginx nginx

- `-d`: Run container in detached mode.
- `-p 80:80`: Map port 80 on the host to port 80 in the container.
- `--name mynginx`: Assign a name to the container.

### 5. `docker ps`
Lists running containers.

docker ps

- `docker ps -a`: Lists all containers, including stopped ones.

### 6. `docker stop`
Stops a running container.

docker stop mynginx


### 7. `docker start`
Starts a stopped container.

docker start mynginx


### 8. `docker restart`
Restarts a running container.

docker restart mynginx


### 9. `docker rm`
Removes a stopped container.

docker rm mynginx


### 10. `docker rmi`
Removes a Docker image.

docker rmi nginx


### 11. `docker images`
Lists all Docker images on the local system.

docker images


### 12. `docker logs`
Fetches logs of a container.

docker logs mynginx


### 13. `docker exec`
Runs a command in a running container.

docker exec -it mynginx /bin/bash

- `-it`: Runs in interactive mode with a terminal.

### 14. `docker build`
Builds an image from a Dockerfile.

docker build -t myapp:latest .

- `-t myapp:latest`: Tags the image with the name `myapp` and tag `latest`.
- `.`: Context path (current directory).

### 15. `docker-compose up`
Starts services defined in a `docker-compose.yml` file.

docker-compose up

- `docker-compose up -d`: Runs in detached mode.

### 16. `docker-compose down`
Stops and removes containers, networks, volumes, and images created by `docker-compose up`.

docker-compose down


### 17. `docker network ls`
Lists all Docker networks.

docker network ls


### 18. `docker network create`
Creates a new Docker network.

docker network create mynetwork


### 19. `docker volume ls`
Lists all Docker volumes.

docker volume ls


### 20. `docker volume create`
Creates a new Docker volume.

docker volume create myvolume


### Example Workflow

1. Pull an image:
   
   docker pull nginx
   

2. Run a container:
   
   docker run -d -p 8080:80 --name webserver nginx
   

3. List running containers:
   
   docker ps
   

4. Check logs of the container:
   
   docker logs webserver
   

5. Execute a command inside the container:
   
   docker exec -it webserver /bin/bash
   

6. Stop the container:
   
   docker stop webserver
   

7. Start the container again:
   
   docker start webserver
   

8. Remove the container:
   
   docker rm webserver
   

9. Remove the image:
   
   docker rmi nginx
   

These commands cover the basics of using Docker for managing images, containers, networks, and volumes, and provide a solid foundation for more advanced Docker usage.
   ```
   Example: `docker inspect abc123def456`

These are just a few commonly used Docker commands. There are many more commands and options available, which you can explore further in the Docker documentation or by using the `docker --help` command.
