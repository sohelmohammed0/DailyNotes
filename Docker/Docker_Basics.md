# Docker

1. What is docker?
- Docker is an open source containerization platform which allows to package, run, ship and manage applications with all their dependencies in to containers

- Containers are lightweight, portable and consistent across environments

2. Why Docker?
- Enables CI/CD automation
- Provides isolation like VMs but with lower overhead
- Fast deployment and Portability across environments 
- Helps teams achieve microservice architecture

3.  | Feature        | Containers                     | Virtual Machines                     |
    | -------------- | ------------------------------ | ------------------------------------ |
    | OS Sharing     | Share Host OS Kernel           | Full OS per VM                       |
    | Performance    | Lightweight, fast              | Heavy, slow startup                  |
    | Resource Usage | Less CPU/RAM                   | More CPU/RAM                         |
    | Isolation      | Process-level                  | Full OS-level                        |
    | Image Size     | MBs                            | GBs                                  |
    | Use Case       | Microservices, CI/CD pipelines | Legacy apps, complete OS environment |


4. Docker Engine:
- Daemon: Background process (dockerd) managing containers and images
- CLI: Command line tool
- Docker Client: Sends request to docker daemon using REST API

5. Docker Architecture:

+-------------------------+         +--------------------------+
|      Docker CLI         | ---->   |     Docker Daemon        |
| (docker command line)   |         | (dockerd, REST API)      |
+-------------------------+         +-----------+--------------+
                                              |
                                              v
                           +------------------+------------------+
                           |       Docker Image Registry         |
                           |     (Docker Hub or Private Repo)    |
                           +------------------+------------------+
                                              |
                                              v
                             +-----------------------------+
                             |      Docker Images          |
                             +-----------------------------+
                                              |
                                              v
                             +-----------------------------+
                             |     Docker Containers       |
                             +-----------------------------+

6. Container Life Cycle:

    
                                | run
                                v
                    start           pause
Create --> (Created) ---> RUnning   ------>  Paused
        |                           <------
        |                           unpause
        v                       start || stop
        rm              rm
    Deleted   <-----------------    Stopped


7. Images vs Containers

| Aspect     | Docker Image          | Docker Container             |
| ---------- | --------------------- | ---------------------------- |
| Definition | Blueprint or snapshot | Running instance of an image |
| Mutability | Immutable             | Mutable (can change state)   |
| State      | At rest (not running) | Active (running or stopped)  |

8. Name Spaces



