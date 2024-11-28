### **Step-by-Step Guide: Containerizing a Python Application with and Without Multi-Stage Builds**

This guide demonstrates how to containerize a Python application using both a **standard Dockerfile** and a **multi-stage build**. We will compare the sizes of the resulting Docker images to highlight the advantages of a multi-stage build.

---

### **Step 1: Prepare the Python Application**

1. **Create the Application Directory:**
   ```bash
   mkdir python-multistage-app
   cd python-multistage-app
   ```

2. **Create a Python Script:**
   - Create a file named `app.py`:
     ```python
     from flask import Flask

     app = Flask(__name__)

     @app.route('/')
     def hello_world():
         return "Hello, Multi-Stage Build Python Application!"

     if __name__ == "__main__":
         app.run(host="0.0.0.0", port=5000)
     ```

3. **Create the `requirements.txt` File:**
   - List the required Python dependencies:
     ```
     flask
     ```

---

### **Step 2: Write the Dockerfile Without Multi-Stage Build**

1. **Create a `Dockerfile`:**
   - Add the following content:
     ```Dockerfile
     # Use a Python image with build tools (heavier base image)
     FROM python:3.9-slim

     # Set the working directory in the container
     WORKDIR /app

     # Copy dependencies and install them
     COPY requirements.txt .
     RUN pip install --no-cache-dir -r requirements.txt

     # Copy the application code to the container
     COPY . .

     # Expose the port the app runs on
     EXPOSE 5000

     # Define the command to run the application
     CMD ["python", "app.py"]
     ```

---

### **Step 3: Write the Dockerfile With Multi-Stage Build**

1. **Create a `Dockerfile` for Multi-Stage Build:**
   - Add the following content:
     ```Dockerfile
     # Stage 1: Build stage
     FROM python:3.9 AS builder

     # Set the working directory in the builder container
     WORKDIR /app

     # Copy dependencies and install them
     COPY requirements.txt .
     RUN pip install --no-cache-dir -r requirements.txt

     # Copy the application code to the builder
     COPY . .

     # Stage 2: Production stage
     FROM python:3.9-slim

     # Set the working directory in the final container
     WORKDIR /app

     # Copy only the necessary files from the builder stage
     COPY --from=builder /app /app

     # Expose the port the app runs on
     EXPOSE 5000

     # Define the command to run the application
     CMD ["python", "app.py"]
     ```

---

### **Step 4: Build and Compare the Images**

1. **Build the Image Without Multi-Stage Build:**
   ```bash
   docker build -t python-no-multistage .
   ```

2. **Build the Image With Multi-Stage Build:**
   ```bash
   docker build -t python-multistage .
   ```

3. **List the Images to Compare Sizes:**
   ```bash
   docker images
   ```

   **Expected Output:**
   | Repository           | Tag  | Image ID    | Size     |
   |----------------------|------|-------------|----------|
   | python-no-multistage | latest | abc12345 | ~50MB+   |
   | python-multistage    | latest | def67890 | ~30MB    |

---

### **Step 5: Run and Test the Application**

1. **Run the Container Without Multi-Stage Build:**
   ```bash
   docker run -d -p 5000:5000 python-no-multistage
   ```

2. **Run the Container With Multi-Stage Build:**
   ```bash
   docker run -d -p 5001:5000 python-multistage
   ```

3. **Test Both Containers:**
   - For the first container:
     ```bash
     curl http://localhost:5000
     ```
     Output: `Hello, Multi-Stage Build Python Application!`

   - For the second container:
     ```bash
     curl http://localhost:5001
     ```
     Output: `Hello, Multi-Stage Build Python Application!`

---

### **Step 6: Explanation of Multi-Stage Build**

1. **How Multi-Stage Build Works:**
   - The first stage (`builder`) contains all tools and dependencies for building the application.
   - The second stage (`production`) copies only the necessary files (code and dependencies) from the builder stage, discarding build-time dependencies.

2. **Benefits:**
   - Reduces the size of the final image by excluding unnecessary build tools.
   - Improves security by minimizing the attack surface.

---

### **Step 7: Cleanup (Optional)**

1. **Stop and Remove Containers:**
   ```bash
   docker stop $(docker ps -q)
   docker rm $(docker ps -aq)
   ```

2. **Remove Images:**
   ```bash
   docker rmi python-no-multistage python-multistage
   ```

---

### **Summary**

| **Aspect**                  | **Without Multi-Stage Build** | **With Multi-Stage Build** |
|-----------------------------|------------------------------|----------------------------|
| **Build Image Size**         | Larger (~50MB+)             | Smaller (~30MB)           |
| **Includes Build Tools?**    | Yes                         | No                        |
| **Deployment Efficiency**    | Less efficient              | Highly efficient          |
| **Security**                 | Lower                       | Higher                    |

### **Real-World Use Case**
- **Scenario:** Deploying a Flask-based microservice to Kubernetes.
- **Solution:** Use multi-stage builds to create lightweight images, reducing the time and cost for deployments across environments.
