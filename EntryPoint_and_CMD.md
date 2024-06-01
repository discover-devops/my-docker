In Docker, both `ENTRYPOINT` and `CMD` are instructions used to define the default commands and arguments that will be executed when a container starts. They serve different purposes and have distinct behaviors. Here's a detailed explanation with examples to illustrate their differences.

### CMD
The `CMD` instruction provides default arguments for the `ENTRYPOINT` instruction or can define a command to be executed when a container starts. If the container is run with a different command specified in `docker run`, the `CMD` will be overridden.

**Example:**

**Dockerfile:**
```Dockerfile
FROM ubuntu:latest
CMD ["echo", "Hello, World!"]
```

If you build and run this Docker image, it will execute the command specified in `CMD`.

**Build and run the container:**
```sh
docker build -t myimage .
docker run myimage
```

**Output:**
```
Hello, World!
```

However, if you specify a different command in `docker run`, it will override the `CMD` instruction:

**Run with a different command:**
```sh
docker run myimage echo "Goodbye, World!"
```

**Output:**
```
Goodbye, World!
```

### ENTRYPOINT
The `ENTRYPOINT` instruction configures a container to run as an executable. It won't be overridden by the arguments provided in `docker run` command, but those arguments will be passed as additional parameters to the `ENTRYPOINT`.

**Example:**

**Dockerfile:**
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
```

If you build and run this Docker image, it will execute the command specified in `ENTRYPOINT`.

**Build and run the container:**
```sh
docker build -t myentryimage .
docker run myentryimage "Hello, World!"
```

**Output:**
```
Hello, World!
```

The `ENTRYPOINT` command will always run, and any additional arguments provided in `docker run` will be appended to it:

**Run with different arguments:**
```sh
docker run myentryimage "Goodbye, World!"
```

**Output:**
```
Goodbye, World!
```

### Combining ENTRYPOINT and CMD
You can combine both `ENTRYPOINT` and `CMD` in a Dockerfile. In this case, `CMD` provides default arguments to the `ENTRYPOINT` instruction.

**Example:**

**Dockerfile:**
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
CMD ["Hello, World!"]
```

**Build and run the container:**
```sh
docker build -t mycombinedimage .
docker run mycombinedimage
```

**Output:**
```
Hello, World!
```

If you specify different arguments in `docker run`, it will override the `CMD` arguments but not the `ENTRYPOINT`:

**Run with different arguments:**
```sh
docker run mycombinedimage "Goodbye, World!"
```

**Output:**
```
Goodbye, World!
```

### Summary
- **CMD:** Sets default command and/or parameters that can be overridden from the `docker run` command line.
- **ENTRYPOINT:** Sets the command to be executed, which is not overridden but can have its arguments appended from the `docker run` command line.
- **Combined:** You can use `ENTRYPOINT` to define the executable and `CMD` to provide default arguments to it, which can be overridden by `docker run`.

By understanding the roles and interactions of `ENTRYPOINT` and `CMD`, you can better control the behavior of your Docker containers.
