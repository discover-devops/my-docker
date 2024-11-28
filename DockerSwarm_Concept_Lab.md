### **Step-by-Step Guide to Setting Up Docker Swarm**

---

### **What is Docker Swarm?**

Docker Swarm is Docker’s native container orchestration tool. It allows you to:
- Manage a cluster of Docker nodes as a single virtual system.
- Deploy, scale, and manage containerized applications across multiple nodes.

#### **Key Features:**
1. **High Availability**: Automatically replicates services across multiple nodes for failover.
2. **Scaling**: Easily scale services up or down.
3. **Load Balancing**: Distributes traffic across containers.
4. **Declarative Service Model**: Desired state configuration ensures consistency.

---

### **Use Case for Docker Swarm**
- **Small to Medium-Scale Deployments**: Ideal for applications requiring simple orchestration without the complexity of Kubernetes.
- **High Availability**: Ensures services are available even if some nodes go down.
- **Easy Multi-Node Management**: Simplifies the process of deploying containers across multiple nodes.

---

### **Lab: Setting Up Docker Swarm**

#### **Prerequisites**
1. **Docker Installed**: Ensure Docker is installed on all nodes. Install it using the following command if not already installed:
   ```bash
   sudo apt update
   sudo apt install docker.io -y
   ```
2. **Multiple Hosts**: You need at least **two nodes** (one manager and one worker). These can be physical machines, VMs, or cloud instances.

---

#### **Step 1: Initialize the Docker Swarm**
1. **Choose a Manager Node**:
   - On the manager node, initialize the Swarm:
     ```bash
     docker swarm init
     ```
   - Output example:
     ```
     Swarm initialized: current node (abc123xyz) is now a manager.
     To add a worker to this swarm, run the following command:
         docker swarm join --token <token> <manager-ip>:2377
     ```
   - Note the `join-token` and `manager IP`. You’ll use this on the worker nodes.

---

#### **Step 2: Add Worker Nodes**
1. **Run the Join Command on Worker Nodes**:
   - On each worker node, execute the `docker swarm join` command provided in the output of the `docker swarm init` command:
     ```bash
     docker swarm join --token <token> <manager-ip>:2377
     ```
   - Example:
     ```bash
     docker swarm join --token SWMTKN-1-0nqax <manager-ip>:2377
     ```

2. **Verify the Swarm Setup**:
   - On the manager node, list all nodes in the Swarm:
     ```bash
     docker node ls
     ```
   - Output example:
     ```
     ID                           HOSTNAME      STATUS  AVAILABILITY  MANAGER STATUS
     abc123xyz                    manager       Ready   Active        Leader
     def456uvw                    worker1       Ready   Active
     ghi789rst                    worker2       Ready   Active
     ```

---

#### **Step 3: Deploy a Service**
1. **Deploy a Service**:
   - Deploy a simple NGINX service with 3 replicas:
     ```bash
     docker service create --name nginx --replicas 3 -p 80:80 nginx
     ```
   - This creates 3 NGINX containers distributed across the Swarm.

2. **Check the Service Status**:
   - Verify the status of the service:
     ```bash
     docker service ls
     ```
   - Output example:
     ```
     ID            NAME   MODE        REPLICAS  IMAGE
     xyz987abc     nginx  replicated  3/3       nginx:latest
     ```

3. **Inspect Service Details**:
   - Get detailed information about the service:
     ```bash
     docker service ps nginx
     ```

---

#### **Step 4: Scale the Service**
1. **Scale Up the Service**:
   - Increase the replicas to 5:
     ```bash
     docker service scale nginx=5
     ```

2. **Verify Scaling**:
   - Check the updated status:
     ```bash
     docker service ls
     ```

---

#### **Step 5: Test Load Balancing**
1. **Access the Service**:
   - Open a browser or use `curl` to access the NGINX service:
     ```
     http://<manager-ip>
     ```

2. **Verify Load Balancing**:
   - Make multiple requests and observe that the requests are balanced across replicas.

---

#### **Step 6: Monitor Swarm**
1. **Check Node Status**:
   - On the manager node:
     ```bash
     docker node ls
     ```

2. **Inspect Logs**:
   - View logs for the service:
     ```bash
     docker service logs nginx
     ```

---

#### **Step 7: Remove the Service**
1. **Remove the Service**:
   ```bash
   docker service rm nginx
   ```

---

### **Optional: Promoting/Demoting Nodes**

1. **Promote a Worker to Manager**:
   ```bash
   docker node promote <worker-node-id>
   ```

2. **Demote a Manager to Worker**:
   ```bash
   docker node demote <manager-node-id>
   ```

---

### **Summary**
1. Docker Swarm simplifies deploying and managing containerized applications across multiple nodes.
2. In this lab:
   - **1 Manager Node** and **2 Worker Nodes** were set up.
   - An NGINX service was deployed and scaled across the Swarm.
3. Swarm provides **load balancing**, **high availability**, and **service orchestration** with minimal setup and effort.

Refrence: https://docs.docker.com/engine/swarm/
