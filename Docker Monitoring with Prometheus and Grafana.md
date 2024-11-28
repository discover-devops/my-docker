### **Docker Monitoring with Prometheus and Grafana**

---

### **What is Docker Monitoring?**
Docker Monitoring involves tracking the performance, resource usage, and health of Docker containers and the host system. Effective monitoring provides insights into:
- CPU, memory, and network usage.
- Container lifecycle events.
- Service and application performance.

---

### **Tools for Monitoring Containers**

#### **1. Prometheus**
- **What**: Open-source monitoring tool that collects metrics in a time-series format.
- **Why**: Ideal for containerized environments with support for Docker, Kubernetes, and more.
- **How**: Prometheus scrapes metrics from a configured endpoint (e.g., Docker’s `/metrics`).

#### **2. Grafana**
- **What**: Visualization and analytics tool that integrates with Prometheus for metrics.
- **Why**: Provides customizable dashboards for monitoring container and system performance.

---

### **Use Case**
Monitor the resource usage (CPU, memory, and network) of Docker containers using **Prometheus** and visualize the metrics in **Grafana**.

---

### **Step-by-Step Lab: Integrating Docker with Prometheus and Grafana**

---

#### **Step 1: Prerequisites**
1. Install Docker and Docker Compose.
   ```bash
   sudo apt update
   sudo apt install docker.io docker-compose -y
   ```

2. Verify Docker is running:
   ```bash
   docker --version
   ```

---

#### **Step 2: Set Up Prometheus and Grafana**

1. **Create a Working Directory**:
   ```bash
   mkdir docker-monitoring
   cd docker-monitoring
   ```

2. **Create a `docker-compose.yml` File**:
   ```yaml
   version: "3.7"

   services:
     prometheus:
       image: prom/prometheus:latest
       container_name: prometheus
       ports:
         - "9090:9090"
       volumes:
         - ./prometheus.yml:/etc/prometheus/prometheus.yml

     grafana:
       image: grafana/grafana:latest
       container_name: grafana
       ports:
         - "3000:3000"
       environment:
         - GF_SECURITY_ADMIN_USER=admin
         - GF_SECURITY_ADMIN_PASSWORD=admin
       volumes:
         - grafana_data:/var/lib/grafana

     cadvisor:
       image: google/cadvisor:latest
       container_name: cadvisor
       ports:
         - "8080:8080"
       volumes:
         - /var/run:/var/run:ro
         - /sys:/sys:ro
         - /var/lib/docker:/var/lib/docker:ro

   volumes:
     grafana_data:
   ```

3. **Create a `prometheus.yml` File**:
   ```yaml
   global:
     scrape_interval: 15s

   scrape_configs:
     - job_name: "prometheus"
       static_configs:
         - targets: ["localhost:9090"]

     - job_name: "cadvisor"
       static_configs:
         - targets: ["cadvisor:8080"]
   ```

---

#### **Step 3: Start the Monitoring Stack**

1. **Run the Stack**:
   ```bash
   docker-compose up -d
   ```

2. **Verify Containers Are Running**:
   ```bash
   docker-compose ps
   ```
   Output Example:
   ```
   Name               Command               State    Ports
   -------------------------------------------------------------
   prometheus         /bin/prometheus ...   Up       0.0.0.0:9090->9090/tcp
   grafana            /run.sh               Up       0.0.0.0:3000->3000/tcp
   cadvisor           /usr/bin/cadvisor ... Up       0.0.0.0:8080->8080/tcp
   ```

3. **Test Access**:
   - Prometheus: Open `http://localhost:9090`.
   - cAdvisor: Open `http://localhost:8080`.
   - Grafana: Open `http://localhost:3000` (default credentials: `admin` / `admin`).

---

#### **Step 4: Configure Grafana Dashboards**

1. **Access Grafana**:
   - Open Grafana at `http://localhost:3000`.
   - Log in with the default credentials (`admin` / `admin`).

2. **Add Prometheus as a Data Source**:
   - Go to **Configuration > Data Sources**.
   - Add a new Prometheus data source:
     - URL: `http://prometheus:9090`

3. **Import a Pre-Built Docker Monitoring Dashboard**:
   - Go to **Dashboard > Import**.
   - Enter Dashboard ID `893` (Docker and cAdvisor Monitoring).
   - Select the Prometheus data source and click **Import**.

4. **View Metrics**:
   - Access the imported dashboard to monitor Docker containers.

---

### **Monitoring Metrics in the Lab**

1. **Prometheus**:
   - Open `http://localhost:9090`.
   - Use the Prometheus query language (**PromQL**) to fetch metrics:
     - CPU Usage:
       ```
       container_cpu_usage_seconds_total
       ```
     - Memory Usage:
       ```
       container_memory_usage_bytes
       ```

2. **Grafana**:
   - Access the Docker Monitoring dashboard to visualize:
     - CPU, memory, and network usage for each container.
     - Real-time metrics and trends.

---

### **Best Practices for Docker Monitoring**

1. **Set Alerts**:
   - Use Prometheus to configure alerts for critical metrics (e.g., high CPU usage).

2. **Retain Metrics**:
   - Use long-term storage backends (e.g., Thanos) for Prometheus to retain metrics.

3. **Secure Monitoring**:
   - Protect Grafana and Prometheus endpoints with authentication and TLS.

4. **Monitor the Docker Host**:
   - Add node exporters for detailed host-level monitoring.

---

### **Conclusion**

- **Prometheus** scrapes metrics from Docker and other systems like cAdvisor.
- **Grafana** provides rich, customizable dashboards for visualizing container performance.
- This setup ensures comprehensive monitoring of Docker containers and hosts.
