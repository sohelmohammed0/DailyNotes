What is the difference between a docker image and docker container?

1. Docker image:
    - A read only template that defines what goes inside a container. It includes the 
    1. application code, 
    2. runtime, 
    3. libraries, 
    4. environment variables, 
    5. configuration files and 
    6. required dependencies


2. Docker container:
    - A runtime instance of Docker image. It adds a writable layer on top of the image, where changes (like logs, temp files and configs) happen during execution. Container is isolated light weight environments that can run packaged applications

    *Tip*: We can run multiple containers using a single image

What is the purpose of Dockerfile?

1. Docker file: A text file with instructions that define how to build a docker image step by step. Instructions create a new layer in the image

2. COMMON INSTRUCTIONS:
    1. FROM: Spccifies the base image (Ex: FROM ubuntu:22.04) Every dockerfile must start with this
    2. LABEL: Adds metadata like meintainer info, version and description (LABEL maintainer="sohel_mohammed")
    3. RUN: Executes commands at build time  (RUN apt update -y (This installs packages inside the image))
    4. COPY: Copies files/directories from local system to image (COPY ./app /usr/src/app)
    5. ADD: similar to copy but with extra faetures (can copy from remote URLs and automaticall extract compressed archives)
    6. CMD: Provides default commands to run when a container starts. It can be over ridden at container run with docker run
    7. ENTRY POINT: Defines a fixed command that always run It can be easily over ridden 

3. Docker Volumes: 
    - Managed by docker itself (lives under /var/lib/docker/volumes by default)
    - Created using 'docker volume create'
    - Decouples data from the host file system layout
    - Portable, easier to backup and recomended for production 

    - Example
        docker run -v mydata:/app/data imagename


    - Bind Mounts:
        - Directly map a host directory/ file in to a container
        - Useful in development when you want live sync between local code and the container
        - More control, but tightly coupled to host machine paths
        
