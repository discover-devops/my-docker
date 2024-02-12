`ENTRYPOINT` and `CMD` are both instructions in a Dockerfile used to specify the default command that should be executed when a container starts. However, they have some key differences:

1. **CMD:**
   - `CMD` is used to provide default arguments for the entry point of the container.
   - You can override the command specified in `CMD` by providing arguments when running the container.
   - If a Dockerfile has multiple `CMD` instructions, only the last one will take effect.
   - If the user provides a command when running the container, it will override the `CMD` instruction.

   **Example:**
   ```Dockerfile
   FROM ubuntu:20.04

   # CMD is used to provide default arguments for ENTRYPOINT
   CMD ["echo", "Hello, CMD!"]
   ```

   To run the container:
   ```bash
   docker run my-container  # This will run 'echo Hello, CMD!'
   ```

2. **ENTRYPOINT:**
   - `ENTRYPOINT` is used to set the main command and any default parameters to be passed to the command.
   - The command specified in `ENTRYPOINT` cannot be overridden by providing arguments when running the container.
   - If the Dockerfile has both `ENTRYPOINT` and `CMD` instructions, the `CMD` arguments will be passed as arguments to the `ENTRYPOINT` command.

   **Example:**
   ```Dockerfile
   FROM ubuntu:20.04

   # ENTRYPOINT sets the main command
   ENTRYPOINT ["echo", "Hello, ENTRYPOINT!"]

   # CMD provides default arguments for the ENTRYPOINT command
   CMD ["CMD default argument"]
   ```

   To run the container:
   ```bash
   docker run my-container  # This will run 'echo Hello, ENTRYPOINT! CMD default argument'
   ```

In summary:
- Use `CMD` when you want to provide default arguments to the command specified in `ENTRYPOINT` or when you want the user to easily override the command.
- Use `ENTRYPOINT` when you want to set the main command and potentially include default arguments that cannot be overridden by the user.

It's common to see both `ENTRYPOINT` and `CMD` used together in a Dockerfile, with `ENTRYPOINT` providing the main command and `CMD` providing default arguments. This combination allows users to easily customize the container's behavior while still providing sensible defaults.
