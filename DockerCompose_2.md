create a simple microservices application with one service in Python (using Flask) and one service in Java (using Spring Boot). We'll use Docker Compose to deploy both services. Here's the step-by-step guide.

### Architecture

The application will consist of two microservices:
1. **Python Microservice**: A simple Flask application that provides an API endpoint to return a greeting message.
2. **Java Microservice**: A Spring Boot application that provides an API endpoint to return a different greeting message.

Both services will be containerized using Docker and managed using Docker Compose.

### Directory Structure

```
microservices-app/
├── python-service/
│   ├── app.py
│   ├── Dockerfile
│   ├── requirements.txt
├── java-service/
│   ├── src/main/java/com/example/demo/DemoApplication.java
│   ├── src/main/resources/application.properties
│   ├── Dockerfile
│   ├── pom.xml
├── docker-compose.yml
```

### Python Microservice

#### `python-service/app.py`

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/greet', methods=['GET'])
def greet():
    return jsonify(message="Hello from Python service!")

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### `python-service/requirements.txt`

```
Flask
```

#### `python-service/Dockerfile`

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

### Java Microservice

#### `java-service/src/main/java/com/example/demo/DemoApplication.java`

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @RestController
    class GreetingController {

        @GetMapping("/greet")
        public String greet() {
            return "Hello from Java service!";
        }
    }
}
```

#### `java-service/src/main/resources/application.properties`

```properties
server.port=8080
```

#### `java-service/pom.xml`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>demo</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.6</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

#### `java-service/Dockerfile`

```dockerfile
FROM openjdk:11-jre-slim

WORKDIR /app

COPY target/demo-0.0.1-SNAPSHOT.jar demo.jar

CMD ["java", "-jar", "demo.jar"]
```

### Docker Compose File

#### `docker-compose.yml`

```yaml
version: '3.8'

services:
  python-service:
    build: ./python-service
    ports:
      - "5000:5000"
    networks:
      - app-network

  java-service:
    build: ./java-service
    ports:
      - "8080:8080"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

### Steps to Deploy the Application

1. **Build the Java Application**: First, navigate to the `java-service` directory and build the Spring Boot application:

    ```bash
    cd java-service
    ./mvnw package
    ```

    This will create a JAR file in the `target` directory.

2. **Start Docker Compose**: Navigate back to the root directory of the project (`microservices-app`) and run Docker Compose:

    ```bash
    cd ..
    docker-compose up --build
    ```

3. **Access the Services**:
   - Python service: Open a browser and go to `http://localhost:5000/greet` to see the greeting message from the Python service.
   - Java service: Open a browser and go to `http://localhost:8080/greet` to see the greeting message from the Java service.

### Conclusion

In this tutorial, we created a simple microservices application with one service in Python and another in Java. We used Docker Compose to manage and deploy both services. 
This setup demonstrates how you can use Docker Compose to orchestrate multi-container applications, making it easier to develop, test, and deploy microservices.


If in case the `mvnw` (Maven Wrapper) script is not present in your `java-service` directory. The Maven Wrapper is a script that allows you to run Maven commands without having Maven installed globally on your system.

Then you have to fix this by including the Maven Wrapper in your Java project. Follow these steps:

1. **Add the Maven Wrapper to Your Java Project**:
   - Navigate to your `java-service` directory.
   - Run the following command to generate the Maven Wrapper scripts:

   ```bash
   mvn -N io.takari:maven:wrapper
   ```

   This will create the necessary Maven Wrapper files (`mvnw`, `mvnw.cmd`, `.mvn/wrapper`).

2. **Run the Maven Build**:
   - Now, you can use the Maven Wrapper to build your project:

   ```bash
   ./mvnw package
   ```

If you don't have Maven installed, here are the steps to install it:

### Install Maven (If Not Already Installed)

#### On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install maven
```

#### On macOS (Using Homebrew):

```bash
brew install maven
```

#### On Windows:

Download and install Maven from the [Apache Maven website](https://maven.apache.org/download.cgi).

### Complete Steps

1. **Navigate to Your Java Service Directory**:

   ```bash
   cd java-service
   ```

2. **Generate the Maven Wrapper**:

   ```bash
   mvn -N io.takari:maven:wrapper
   ```

3. **Build the Java Application Using the Maven Wrapper**:

   ```bash
   ./mvnw package
   ```

4. **Navigate Back to the Root Directory**:

   ```bash
   cd ..
   ```

5. **Start Docker Compose**:

   ```bash
   docker-compose up --scale web=3 --build
   ```

### Updated Project Directory Structure

After adding the Maven Wrapper, your `java-service` directory should look like this:

```
java-service/
├── .mvn/
│   └── wrapper/
│       ├── maven-wrapper.jar
│       └── maven-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── demo/
│   │   │               └── DemoApplication.java
│   │   └── resources/
│   │       └── application.properties
├── Dockerfile
├── mvnw
├── mvnw.cmd
├── pom.xml
```

With these steps, you should be able to build your Java application using the Maven Wrapper and deploy both your Python and Java microservices using Docker Compose without encountering the previous issues.
