You can reduce the size of a Docker image using multi-stage builds. Multi-stage builds allow you to use multiple `FROM` statements in a single Dockerfile, creating multiple "stages" in the build process. Each stage can be used to perform a specific set of tasks, and only the necessary artifacts are carried over to the final image.

Here's an example of how you can refactor the Dockerfile for the Python Flask application using multi-stage builds:

```Dockerfile
# Stage 1: Build Stage
FROM python:3.8-slim as builder

WORKDIR /app

COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Production Stage
FROM python:3.8-slim

WORKDIR /app

# Copy only necessary files from the build stage
COPY --from=builder /usr/local/lib/python3.8/site-packages /usr/local/lib/python3.8/site-packages
COPY --from=builder /usr/local/bin/gunicorn /usr/local/bin/gunicorn
COPY app.py .

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define the command to run the application
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

Explanation:

1. **Build Stage:**
   - Use the `python:3.8-slim` image as the build stage.
   - Set the working directory and copy only the `requirements.txt`.
   - Install dependencies in this stage.

2. **Production Stage:**
   - Use another `python:3.8-slim` image for the final stage.
   - Set the working directory and copy only the necessary files from the build stage.
   - The `EXPOSE`, `CMD`, and other instructions follow.

This approach reduces the size of the final image because it only includes the necessary artifacts from the build stage and omits unnecessary build dependencies. The resulting image is more streamlined and typically smaller.

To build and run the Docker image with multi-stage build:

```bash
docker build -t simple-web-app-multistage .
docker run -p 5000:5000 simple-web-app-multistage
```

This should provide you with a smaller Docker image while still having all the necessary components to run your Python Flask application.
