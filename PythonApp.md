This is a  simple Python program that uses Flask to create a web application. We'll also create a Dockerfile to containerize this application.

**Python Program (`app.py`):**
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, this is a simple web application!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Dockerfile:**
```Dockerfile
# Use an official Python runtime as a base image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]
```

Make sure to have a `requirements.txt` file with the following content:
```plaintext
Flask==2.0.1
Werkzeug==2.0.1
```

Now, let's build and run the Docker container:

1. Build the Docker image:
   ```bash
   docker build -t simple-web-app .
   ```

2. Run the Docker container and expose port 5000:
   ```bash
   docker run -p 5000:5000 simple-web-app
   ```

3. Access the application in your web browser or using a tool like `curl`:
   - If you are running Docker locally, access it at `http://localhost:5000`.

Now, the Flask web application is running inside a Docker container, and you can access it by visiting the specified host and port. In this case, it's `http://localhost:5000`. Adjust the URL accordingly if you are running Docker on a remote server or in a different environment.
