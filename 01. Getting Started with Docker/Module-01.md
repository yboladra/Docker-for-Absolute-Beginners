# 01.Getting Started with Docker

## Introduction

Docker is an open-source platform that simplifies the process of developing, shipping, and running applications in containers. Containers are lightweight, portable, and isolated environments that package an application along with its dependencies, libraries, system tools, system libraries, and settings, ensuring consistency across different environments.

- **Faster Development**: Consistent environments across machines eliminate setup hassles.
- **Effortless Deployment**: Package your application once and run it anywhere with Docker.
- **Enhanced Scalability**: Easily create more containers to handle increased workload.
- **Resource Efficiency**: Containers are lightweight, consuming fewer resources compared to virtual machines.
- **Improved Isolation**: Applications in separate containers don't interact with each other.

## Key Docker Components

### Containers
- **Definition**: Lightweight, isolated environments that package an application's code, runtime, system tools, system libraries, and settings.
- **Best Practice**: Aim for single responsibility principle, encapsulating each service or application in its own container for easier management and scalability.

### Docker Engine
- **Definition**: The core engine responsible for building and running containers.
- **Best Practice**: Stay updated with the latest Docker Engine releases to benefit from performance enhancements, bug fixes, and security patches.

### Dockerfile
- **Definition**: A text file that contains instructions for building a Docker image. It specifies the environment and commands needed to run an application.
- **Best Practice**: Optimize Dockerfiles by leveraging multi-stage builds and caching mechanisms to reduce image size and build times. Follow best practices for security and maintainability, such as minimizing layers and avoiding unnecessary dependencies.

### Docker Image
- **Definition**: A lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, environment variables, and configuration files.
- **Best Practice**: Regularly audit Docker images for vulnerabilities and use trusted base images from reputable sources to enhance security posture. Ensure images are versioned, tagged, and stored in a secure registry to enable reproducible builds and deployments.

### Docker Container
- **Definition**: An executable instance of a Docker image. It encapsulates the application and its dependencies, running in an isolated environment.
- **Best Practice**: Use containers for packaging and deploying applications consistently across different environments. Implement container orchestration tools like Kubernetes or Docker Swarm for automated deployment, scaling, and management of containers in production environments.

### Docker Compose
- **Definition**: A tool for defining and running multi-container Docker applications. It uses a YAML file to configure application services and their dependencies.
- **Best Practice**: Utilize Docker Compose to define application environments in a version-controlled YAML format, promoting consistency and reproducibility across different environments.

### Docker Volumes
- **Definition**: Persistent data storage for containers, decoupled from the container's lifecycle. Volumes allow data to persist between container restarts and migrations.
- **Best Practice**: Prefer using Docker volumes over host bind mounts for managing persistent data, ensuring better scalability, portability, and data integrity.

### Docker Networks
- **Definition**: Mechanism for container isolation and communication. Docker networks enable containers to communicate with each other and external networks while providing network segmentation and access control.
- **Best Practice**: Implement network segmentation to isolate application components and enforce access control between containers for enhanced security.

### Docker Registry
- **Definition**: A repository for sharing and distributing Docker images. It allows users to store, manage, and distribute Docker images within an organization or across multiple environments.
- **Best Practice**: Employ a private Docker registry for storing sensitive or proprietary images, enabling tighter control over image access and distribution.

### Docker Hub
- **Definition**: A public repository of Docker images, providing a centralized location for discovering, sharing, and collaborating on Docker images.
- **Best Practice**: Leverage Docker Hub's automated build features and image scanning capabilities to ensure image quality and security compliance before deployment.

## Setting Up Docker

1. **Check System Requirements**: Ensure your system meets Docker's minimum requirements (details available on the Docker website).
2. **Install Docker**:
   - **Windows/macOS**: Download and install Docker Desktop from [Docker's official website](https://docs.docker.com/).
   - **Linux**: Use your package manager (apt-get, yum, etc.) or follow the official installation instructions.
3. **Verify Installation**: Open your terminal and type `docker version`. You should see Docker's version information if successful.

### Understanding Basic Docker Commands

Here are some essential commands to get you started:

### Understanding Basic Docker Commands

Here are some essential commands to get you started:

- `docker pull <image>`: Pull an image from a registry.
- `docker images`: List all locally available images.
- `docker run <image>`: Run a container based on an image.
- `docker ps`: List all running containers.
- `docker ps -a`: List all containers (including stopped ones).
- `docker stop <container>`: Stop a running container.
- `docker start <container>`: Start a stopped container.
- `docker restart <container>`: Restart a running container.
- `docker rm <container>`: Remove a stopped container.
- `docker rmi <image>`: Remove an image.
- `docker exec -it <container> <command>`: Execute a command in a running container.
- `docker logs <container>`: View logs of a container.
- `docker inspect <container>`: Display detailed information about a container.
- `docker history <image>`: Show the history of an image.
- `docker prune`: Remove all stopped containers, dangling images, and unused networks and volumes.

These commands should help you perform basic operations with Docker containers and images.

### Running Containers
1. **Pull an Image**: Use `docker pull` to download an image, like `docker pull ubuntu:latest` (which gets the latest version of Ubuntu).
    ```bash
    docker pull ubuntu:latest
    ```
2. **Run the Container**: Execute `docker run` with the image name to start a container.
    ```bash
    docker run --name webapp -d -t -p 80:80 ubuntu
    ```
3. **Explore the Container**: Run `docker exec` to interact with the container and perform tasks inside it.
    ```bash
    docker exec -it webapp bash
    ```
4. **Exit the Container**: Type `exit` to leave the container's shell (this doesn't stop the container itself).
    ```bash
    exit
    ```
### Creating Custom Images (Later)

Dockerfiles are special text files used to build custom images. We'll cover this in more detail later, but for now, focus on using pre-built images to get started quickly.


## Alright, see you in the next module.