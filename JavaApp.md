This is a simple Java program using the Spring Boot framework to create a web application. We'll also create a Dockerfile to containerize this Java application.

**Java Program (`Application.java`):**
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
class HomeController {

    @GetMapping("/")
    public String hello() {
        return "Hello, this is a simple web application!";
    }
}
```

**Dockerfile:**
```Dockerfile
# Use an official OpenJDK runtime as a base image
FROM openjdk:11-slim

# Set the working directory in the container
WORKDIR /app

# Copy the JAR file into the container at /app
COPY target/simple-web-app.jar /app

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Define the command to run the application
CMD ["java", "-jar", "simple-web-app.jar"]
```

To build and run this Java application in a Docker container:

1. Build the Docker image (make sure to run this command in the directory where your `Dockerfile` is located):
   ```bash
   docker build -t simple-web-app-java .
   ```

2. Run the Docker container and expose port 8080:
   ```bash
   docker run -p 8080:8080 simple-web-app-java
   ```

3. Access the application in your web browser or using a tool like `curl`:
   - If you are running Docker locally, access it at `http://localhost:8080`.

Now, the Spring Boot web application is running inside a Docker container, and you can access it by visiting the specified host and port. In this case, it's `http://localhost:8080`. Adjust the URL accordingly if you are running Docker on a remote server or in a different environment.
