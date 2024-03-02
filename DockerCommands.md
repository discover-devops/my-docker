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
   ```
   Example: `docker inspect abc123def456`

These are just a few commonly used Docker commands. There are many more commands and options available, which you can explore further in the Docker documentation or by using the `docker --help` command.
