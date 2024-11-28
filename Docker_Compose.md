### **Module 3: Docker Compose**

---

### **1. Introduction to Docker Compose**

#### **Concept**
Docker Compose is a tool for defining and running multi-container applications using a YAML configuration file. It simplifies managing multiple services, networks, and volumes for your application.

#### **Use Case**
For applications requiring multiple services, such as a web server, database, and caching system, Docker Compose provides a single file to orchestrate their configuration and deployment.

---

### **2. What is Docker Compose?**

#### **Concept**
- Docker Compose uses a YAML file (`docker-compose.yml`) to define services, networks, and volumes.
- It automates starting, stopping, scaling, and managing multiple containers as a single application.

---

### **3. Why Use Docker Compose?**

#### **Concept**
- Simplifies multi-container setups.
- Provides consistent environments for development, testing, and production.
- Easy scaling and service management.

#### **Use Case**
A Python web application using Flask and PostgreSQL:
- Flask app container handles HTTP requests.
- PostgreSQL container stores application data.
- Docker Compose manages both containers, their networking, and volumes.

---

### **4. Writing a Docker Compose File**

#### **Concept**
A `docker-compose.yml` file defines:
1. **Services**: Containers in the application (e.g., web, db).
2. **Networks**: Communication paths between services.
3. **Volumes**: Shared storage for persistent data.

#### **YAML Basics**
- Indentation matters.
- Use `:` to define key-value pairs.
- Use `-` for lists.

---

### **5. Step-by-Step LAB: Writing a Compose File**

#### **Scenario: A Flask Application with PostgreSQL**

1. **Project Structure:**
   ```
   project/
   ├── app/
   │   ├── app.py
   │   ├── requirements.txt
   └── docker-compose.yml
   ```

2. **Create Application Code (`app/app.py`):**
   ```python
   from flask import Flask
   import psycopg2

   app = Flask(__name__)

   @app.route("/")
   def hello_world():
       return "Hello, Docker Compose!"

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=5000)
   ```

3. **Create Requirements File (`app/requirements.txt`):**
   ```
   flask
   psycopg2-binary
   ```

4. **Write `docker-compose.yml`:**
   ```yaml
   version: "3.9"

   services:
     web:
       build:
         context: ./app
       ports:
         - "5000:5000"
       depends_on:
         - db
       volumes:
         - ./app:/app

     db:
       image: postgres:13
       environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
         POSTGRES_DB: mydb
       volumes:
         - db_data:/var/lib/postgresql/data

   volumes:
     db_data:
   ```

5. **Add a Dockerfile for the Web Service (`app/Dockerfile`):**
   ```Dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   COPY . .
   CMD ["python", "app.py"]
   ```

---

### **6. Running and Managing Applications with Docker Compose**

1. **Build and Start the Application:**
   ```bash
   docker-compose up -d
   ```

2. **Verify Services:**
   ```bash
   docker-compose ps
   ```

3. **Access the Application:**
   - Open a browser or use `curl`:
     ```bash
     curl http://localhost:5000
     ```
   - Output: `Hello, Docker Compose!`

4. **Stop and Remove Services:**
   ```bash
   docker-compose down
   ```

---

### **7. Scaling Services**

#### **Concept**
- Docker Compose allows scaling services horizontally.
- Use the `docker-compose up --scale` command to scale any service.

#### **LAB: Scaling the Web Service**
1. **Scale the Web Service to 3 Instances:**
   ```bash
   docker-compose up -d --scale web=3
   ```

2. **Verify Running Containers:**
   ```bash
   docker ps
   ```

3. **Test Load Balancing:**
   - If using Docker's default bridge network, requests to `http://localhost:5000` will hit a single container. Use a reverse proxy (e.g., NGINX) for proper load balancing.

---

### **8. Docker Compose Commands**

#### **Common Commands**
1. **Start Services:**
   ```bash
   docker-compose up
   ```

2. **Stop Services:**
   ```bash
   docker-compose down
   ```

3. **View Logs:**
   ```bash
   docker-compose logs
   ```

4. **Restart Services:**
   ```bash
   docker-compose restart
   ```

---

### **9. Using Overrides with Compose Files**

#### **Concept**
- Docker Compose supports overriding the `docker-compose.yml` with additional configuration files like `docker-compose.override.yml`.

#### **Use Case**
- Development and production environments with different configurations.

#### **LAB: Create an Override File**
1. **Write `docker-compose.override.yml`:**
   ```yaml
   version: "3.9"

   services:
     web:
       environment:
         FLASK_ENV: development
   ```

2. **Start Services with Overrides:**
   ```bash
   docker-compose up -d
   ```

3. **Verify Override:**
   - Check the environment variable in the `web` container:
     ```bash
     docker exec -it <web-container-id> env | grep FLASK_ENV
     ```
   - Output: `FLASK_ENV=development`

---

### **Summary**

| **Feature**                 | **Command/Explanation**                                       |
|-----------------------------|-------------------------------------------------------------|
| Start application           | `docker-compose up`                                         |
| Scale services              | `docker-compose up --scale web=3`                          |
| Stop application            | `docker-compose down`                                       |
| View logs                   | `docker-compose logs`                                       |
| Override configurations     | Add `docker-compose.override.yml` for environment-specific setups. |

Docker Compose simplifies managing multi-container applications, enabling you to focus on development and scaling. 
