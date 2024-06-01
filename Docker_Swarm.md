### Docker Swarm Tutorial: From Basics to Real-World Deployment

Docker Swarm is Docker's native clustering and orchestration tool. It turns a pool of Docker hosts into a single, virtual Docker host. In this tutorial, we'll cover the basics of Docker Swarm, set up a Swarm cluster, add a node, deploy a sample application, and demonstrate scaling operations.

---

### Table of Contents

1. [What is Docker Swarm?](#what-is-docker-swarm)
2. [Installing Docker](#installing-docker)
3. [Setting Up a Swarm Cluster](#setting-up-a-swarm-cluster)
4. [Adding a Node to the Swarm](#adding-a-node-to-the-swarm)
5. [Deploying a Sample Application](#deploying-a-sample-application)
6. [Scaling the Application](#scaling-the-application)
7. [Scaling Down the Application](#scaling-down-the-application)
8. [Conclusion](#conclusion)

---

### What is Docker Swarm?

Docker Swarm is a container orchestration tool that allows you to manage multiple Docker hosts as a single virtual host. It enables scaling, load balancing, and high availability for your applications.

### Installing Docker

Before setting up Docker Swarm, ensure Docker is installed on all the machines you want to include in the Swarm cluster.

**For Ubuntu/Debian:**

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

**For CentOS:**

```bash
sudo yum update
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
```

**For macOS and Windows:**

Download and install Docker Desktop from the [Docker website](https://www.docker.com/products/docker-desktop).

### Setting Up a Swarm Cluster

1. **Initialize the Swarm Manager**:

   On the manager node, run:

   ```bash
   docker swarm init --advertise-addr <MANAGER-IP>
   ```

   Replace `<MANAGER-IP>` with the IP address of your manager node. This command will output a command to join worker nodes to the swarm.

2. **Join Worker Nodes**:

   On each worker node, run the command output from the `docker swarm init` command. It looks something like this:

   ```bash
   docker swarm join --token <TOKEN> <MANAGER-IP>:2377
   ```

   Replace `<TOKEN>` with the actual token and `<MANAGER-IP>` with the IP address of your manager node.

3. **Verify the Swarm**:

   On the manager node, run:

   ```bash
   docker node ls
   ```

   This command lists all nodes in the swarm and their status.

### Adding a Node to the Swarm

1. **Get the Join Token**:

   On the manager node, run:

   ```bash
   docker swarm join-token worker
   ```

   This command outputs the join command with the token for worker nodes.

2. **Join the New Node**:

   On the new node, run the join command with the token:

   ```bash
   docker swarm join --token <TOKEN> <MANAGER-IP>:2377
   ```

3. **Verify the New Node**:

   On the manager node, run:

   ```bash
   docker node ls
   ```

   The new node should appear in the list.

### Deploying a Sample Application

We'll deploy a simple Python Flask application using Docker Swarm.

1. **Create a Dockerfile**:

   Create a directory for your application and add a `Dockerfile`:

   ```dockerfile
   FROM python:3.8-slim

   WORKDIR /app

   COPY requirements.txt requirements.txt
   RUN pip install -r requirements.txt

   COPY . .

   CMD ["python", "app.py"]
   ```

2. **Create `app.py`**:

   ```python
   from flask import Flask, jsonify

   app = Flask(__name__)

   @app.route('/')
   def hello():
       return jsonify(message="Hello, Docker Swarm!")

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=5000)
   ```

3. **Create `requirements.txt`**:

   ```txt
   flask
   ```

4. **Build the Docker Image**:

   In the directory containing your Dockerfile, run:

   ```bash
   docker build -t my-python-app .
   ```

5. **Create a Docker Stack File (`docker-compose.yml`)**:

   ```yaml
   version: '3.8'

   services:
     web:
       image: my-python-app
       ports:
         - "5000:5000"
       deploy:
         replicas: 2
         resources:
           limits:
             cpus: "0.1"
             memory: "50M"
         restart_policy:
           condition: on-failure
   ```

6. **Deploy the Stack**:

   On the manager node, run:

   ```bash
   docker stack deploy -c docker-compose.yml myapp
   ```

7. **Verify the Deployment**:

   Run the following command to see the services:

   ```bash
   docker service ls
   ```

   To see the tasks for a specific service, run:

   ```bash
   docker service ps myapp_web
   ```

### Scaling the Application

To scale the application, use the `docker service scale` command:

```bash
docker service scale myapp_web=5
```

This command scales the `web` service to 5 replicas.

### Scaling Down the Application

To scale down the application, use the same command with a lower number of replicas:

```bash
docker service scale myapp_web=2
```

This command scales the `web` service down to 2 replicas.

### Conclusion

In this tutorial, we covered the basics of Docker Swarm, set up a Swarm cluster, added nodes, deployed a sample application, and demonstrated scaling operations. Docker Swarm simplifies the management of containerized applications across multiple Docker hosts, providing high availability and scalability.

By following this tutorial, you now have a foundational understanding of Docker Swarm and how to use it to manage your applications in a clustered environment.
