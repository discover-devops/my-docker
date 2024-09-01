To ensure that your Docker container is running as expected, you can use several Docker commands to check its status, view logs, and interact with it. Here’s a step-by-step guide to monitoring and troubleshooting your Docker container:

### **1. Check Container Status**

You can see the status of your containers using the `docker ps` command:

```bash
docker ps
```

This command lists all running containers, showing their status, names, and other details.

To see all containers, including those that are stopped:

```bash
docker ps -a
```

### **2. View Container Logs**

To view the logs of a specific container, use the `docker logs` command:

```bash
docker logs <container_name_or_id>
```

This command will show you the standard output and standard error from the container. You can use flags to customize the output:

- **Follow logs in real-time:**

  ```bash
  docker logs -f <container_name_or_id>
  ```

- **Show only the latest `n` lines:**

  ```bash
  docker logs --tail n <container_name_or_id>
  ```

- **Show timestamps:**

  ```bash
  docker logs -t <container_name_or_id>
  ```

### **3. Access the Container’s Shell**

If you need to debug your container from the inside, you can access its shell using the `docker exec` command:

```bash
docker exec -it <container_name_or_id> /bin/bash
```

This command opens an interactive bash shell inside the running container, allowing you to inspect files, run commands, and troubleshoot directly within the container.

### **4. Check Container Resource Usage**

To monitor the resource usage (CPU, memory, etc.) of your container, you can use the `docker stats` command:

```bash
docker stats <container_name_or_id>
```

This command provides a real-time view of the container’s resource usage.

### **5. Inspect Container Details**

The `docker inspect` command gives you detailed information about the container, including its configuration, network settings, mounted volumes, and more:

```bash
docker inspect <container_name_or_id>
```

This is useful for verifying the container's configuration and identifying potential issues.

### **6. Testing Application Connectivity**

To test if your application inside the container is responding as expected, you can use `curl` or other networking tools:

```bash
curl http://localhost:<host_port>
```

Replace `<host_port>` with the port that you’ve mapped to the container's application port. This is useful for testing web servers or API services running inside the container.

### **7. Check Container’s Health Status**

If your Docker container has a `HEALTHCHECK` defined, you can see the health status by running:

```bash
docker inspect --format='{{.State.Health.Status}}' <container_name_or_id>
```

### **8. Restart the Container**

If your container is not behaving as expected, sometimes a restart might help:

```bash
docker restart <container_name_or_id>
```

### **9. Investigate Container Exit Codes**

If a container stops unexpectedly, you can check its exit code to understand why it stopped:

```bash
docker inspect <container_name_or_id> --format='{{.State.ExitCode}}'
```

An exit code of `0` usually indicates that the container exited normally. Non-zero exit codes indicate errors or issues.

### **Summary**

- **View logs**: `docker logs <container_name_or_id>`
- **Check status**: `docker ps`
- **Access shell**: `docker exec -it <container_name_or_id> /bin/bash`
- **Monitor resources**: `docker stats <container_name_or_id>`
- **Inspect details**: `docker inspect <container_name_or_id>`
- **Test connectivity**: Use `curl` or similar tools.
- **Check health status**: If applicable, use `docker inspect --format='{{.State.Health.Status}}' <container_name_or_id>`.
- **Restart**: `docker restart <container_name_or_id>`

Using these commands, you can effectively monitor and troubleshoot your Docker containers to ensure they are functioning as expected.
