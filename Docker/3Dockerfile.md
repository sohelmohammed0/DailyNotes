# 📘 Dockerfile - Complete Notes

## 1. What is a Dockerfile?

-   A **Dockerfile** is a plain text file that contains a **set of
    instructions** to build a Docker image.\
-   It is like a **recipe** that Docker follows to create an image.\
-   Each line in a Dockerfile represents a step.\
-   The Docker engine reads the file **top to bottom** and executes
    instructions one by one.

🔹 Example:

``` dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y nginx
CMD ["echo", "Hello Dockerfile!"]
```

This Dockerfile: 1. Uses `ubuntu:20.04` as the base image.\
2. Installs nginx.\
3. Sets a default command.

------------------------------------------------------------------------

## 2. Why use a Dockerfile?

-   To **automate** image creation.\
-   To ensure **consistency** across environments.\
-   To version control infrastructure along with code.\
-   To save time (no need to manually install dependencies each time).

------------------------------------------------------------------------

## 3. How to Write/Create a Dockerfile?

1.  Create a new file named `Dockerfile` (no extension).\

2.  Write instructions inside the file.\

3.  Build the image:

    ``` bash
    docker build -t myimage:1.0 .
    ```

    (`-t` → tag, `.` → path to Dockerfile)\

4.  Run a container from the image:

    ``` bash
    docker run myimage:1.0
    ```

------------------------------------------------------------------------

## 4. Dockerfile Instructions (with Examples)

Here are the **most common instructions** you'll use:

### 🟦 1. `FROM`

-   Sets the **base image**. Every Dockerfile must start with it.\

-   Example:

    ``` dockerfile
    FROM ubuntu:20.04
    ```

------------------------------------------------------------------------

### 🟦 2. `RUN`

-   Executes commands **inside the image** during build.\

-   Used to install software or configure environment.\

-   Example:

    ``` dockerfile
    RUN apt-get update && apt-get install -y python3
    ```

------------------------------------------------------------------------

### 🟦 3. `COPY`

-   Copies files/folders from host → image.\

-   Example:

    ``` dockerfile
    COPY app.py /usr/src/app/
    ```

------------------------------------------------------------------------

### 🟦 4. `ADD`

-   Similar to `COPY` but with extra features:

    -   Can extract compressed files (`.tar`, `.gz`).\
    -   Can download files from URLs.\

-   Example:

    ``` dockerfile
    ADD app.tar.gz /usr/src/app/
    ```

👉 **Best practice**: Use `COPY` unless you specifically need `ADD`.

------------------------------------------------------------------------

### 🟦 5. `WORKDIR`

-   Sets the **working directory** inside the container.\

-   Example:

    ``` dockerfile
    WORKDIR /usr/src/app
    ```

------------------------------------------------------------------------

### 🟦 6. `CMD`

-   Provides default **command to run when the container starts**.\

-   Only **one CMD** is allowed (last one overrides previous).\

-   Example:

    ``` dockerfile
    CMD ["python3", "app.py"]
    ```

------------------------------------------------------------------------

### 🟦 7. `ENTRYPOINT`

-   Defines a container's **main executable**.\

-   Unlike CMD, ENTRYPOINT **cannot be overridden easily** when running
    `docker run`.\

-   Example:

    ``` dockerfile
    ENTRYPOINT ["python3", "app.py"]
    ```

------------------------------------------------------------------------

### 🟦 8. `ENV`

-   Sets environment variables inside container.\

-   Example:

    ``` dockerfile
    ENV APP_ENV=production
    ```

------------------------------------------------------------------------

### 🟦 9. `EXPOSE`

-   Documents the **port** the container listens on.\

-   Example:

    ``` dockerfile
    EXPOSE 8080
    ```

------------------------------------------------------------------------

### 🟦 10. `VOLUME`

-   Creates a mount point for volumes.\

-   Example:

    ``` dockerfile
    VOLUME /data
    ```

------------------------------------------------------------------------

### 🟦 11. `USER`

-   Specifies which user runs inside the container.\

-   Example:

    ``` dockerfile
    USER 1000
    ```

------------------------------------------------------------------------

### 🟦 12. `LABEL`

-   Adds metadata to the image.\

-   Example:

    ``` dockerfile
    LABEL maintainer="sohel@example.com"
    ```

------------------------------------------------------------------------

## 5. Difference Between `CMD` and `ENTRYPOINT`

  ------------------------------------------------------------------------
  Feature                  CMD            ENTRYPOINT
  ------------------------ -------------- --------------------------------
  Purpose                  Provides       Defines **main executable**
                           default        
                           **command**    

  Overridable?             Yes, by        No (but can add args)
                           arguments      
                           passed to      
                           `docker run`   

  Use case                 When you want  When container should always run
                           a default      a fixed program
                           command but    
                           allow override 
  ------------------------------------------------------------------------

🔹 Example:

``` dockerfile
# Using CMD
FROM ubuntu
CMD ["echo", "Hello from CMD"]
```

Run:

``` bash
docker run myimage
# Output: Hello from CMD
docker run myimage echo "Hi"
# Output: Hi   (overridden)
```

------------------------------------------------------------------------

``` dockerfile
# Using ENTRYPOINT
FROM ubuntu
ENTRYPOINT ["echo", "Hello from ENTRYPOINT"]
```

Run:

``` bash
docker run myimage
# Output: Hello from ENTRYPOINT
docker run myimage Hi
# Output: Hello from ENTRYPOINT Hi  (cannot override base command)
```

------------------------------------------------------------------------

## 6. Best Practices for Writing Dockerfiles

-   Use **small base images** (e.g., `alpine`) to reduce size.\
-   Minimize layers → combine commands with `&&`.\
-   Use `.dockerignore` to avoid copying unnecessary files.\
-   Prefer `COPY` over `ADD`.\
-   Pin version numbers to avoid unexpected updates.\
-   Always specify `ENTRYPOINT` for critical apps.
