# Running Your First Docker Container

## Prerequisites:
- Docker is installed on your system. If not, refer to the [installation guide](#install-docker-on-windows-linux-mac) for your operating system.

## Steps:

1. Open a terminal or command prompt on your machine.

2. To verify that Docker is installed, run the following command:
   ```bash
   docker --version
   ```

3. Now, run the following command to pull and execute the "hello-world" Docker image:
   ```bash
   docker run hello-world
   ```

4. Docker will check if the "hello-world" image exists locally. If not, it will download the image from Docker Hub.

5. The container will start and display a welcome message, indicating that your Docker installation is working correctly.

   Example output:
   ```
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ```

6. The output will provide information about the Docker version, your system architecture, and a brief explanation of what happened.

7. The container will then stop, and you'll see a message indicating that the container has exited.

   Example output:
   ```
   ...
   ...
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ...
   ...
   ```

8. Congratulations! You've successfully run your first Docker container.

## Troubleshooting Tips:
- If you encounter any issues, make sure Docker is running and that you have the necessary permissions to execute Docker commands. On Linux, you might need to use `sudo` or add your user to the "docker" group.

- Ensure that your internet connection is stable, as Docker needs to download the "hello-world" image from Docker Hub.

- If the container fails to run, check the Docker logs for more details:
  ```bash
  docker logs <container_id>
  ```

Note: The `<container_id>` is the ID of the last executed container, which you can find using the `docker ps -a` command.

