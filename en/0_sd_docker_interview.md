# Docker — Interview

Docker interview questions.

[← Back to hub](./0_system_design.md)

### Related docs

- [Docker — Interview](./0_sd_docker_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/).

### Docker Fundamentals

<details>
<summary>What is Docker, and how is it different from virtual machines?</summary>

### What is Docker, and how is it different from virtual machines?

**Definition:**

Docker is a containerization platform that simplifies application deployment by ensuring software and its dependencies run uniformly on any infrastructure, from laptops to servers to the cloud.

Using Docker allows you tobundle code and dependencies into a container image you can then run on any Docker-compatible environment. This approach is a significant improvement over traditional virtual machines, which are less efficient and come with higher overheads.

**How it works:**

Key Docker Components

- **Docker Daemon**: A persistent background process that manages and executes containers.
- **Docker Engine**: The CLI and API for interacting with the daemon.
- **Docker Registry**: A repository for Docker images.

Core Building Blocks

- **Dockerfile**: A text document containing commands that assemble a container image.
- **Image**: A standalone, executable package containing everything required to run a piece of software.
- **Container**: A runtime instance of an image.

Virtual Machines vs. Docker Containers

Virtual Machines

Advantages:

- **Isolation**: VMs run separate operating systems, providing strict application isolation.

Inefficiencies:

- **Resource Overhead**: Each VM requires its operating system, consuming RAM, storage, and CPU. Running multiple VMs can lead to redundant resource use.
- **Slow Boot Times**: Booting a VM involves starting an entire OS, slowing down deployment.

Containers

Efficiencies:

- **Resource Optimizations**: As containers share the host OS kernel, they are exceptionally lightweight, requiring minimal RAM and storage.
- **Rapid Deployment**: Containers start almost instantaneously, accelerating both development and production.

Isolation Caveats:

- **Application-Level Isolation**: While Docker ensures the separation of containers from the host and other containers, it relies on the host OS for underlying resources.

Core Unique Features of Docker

- **Layered File System**: Docker images are composed of layers, each representing a set of file changes. This structure aids in minimizing image size and optimizing builds.

- **Container Orchestration**: Technologies such as Kubernetes and Docker Swarm enable the management of clusters of containers, providing features like load balancing, scaling, and automated rollouts and rollbacks.

- **Interoperability**: Docker containers are portable, running consistently across diverse environments. Additionally, Docker complements numerous other tools and platforms, including Jenkins for CI/CD pipelines and AWS for cloud services.

**Examples:**

Code Example: Dockerfile

Here is the Dockerfile:

```dockerfile
FROM python:3.8

WORKDIR /app

COPY requirements.txt requirements.txt

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Can you explain what a Docker image is?</summary>

### Can you explain what a Docker image is?

**Definition:**

A Docker image is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and configuration files.

It provides consistency across environments by ensuring that each instance of an image is identical, a key principle of Docker's build-once-run-anywhere philosophy.

**How it works:**

Image vs. Container

- **Image**: A static package that encompasses everything the application requires to run.
- **Container**: An operating instance of an image, running as a process on the host machine.

Layered File System

Docker images comprise multiple layers, each representing a distinct file system modification. Layers are read-only, and the final container layer is read/write, which allows for efficiency and flexibility.

Key Components

- **Operating System**: Traditional images have a full or bespoke OS tailored for the application's needs. Recent developments like "distroless" images, however, focus solely on application dependencies.
- **Application Code**: Your code and files, which are specified during the image build.

Image Registries

Images are stored in Docker image registries like Docker Hub, which provides a central location for image management and sharing. You can download existing images, modify them, and upload the modified versions, allowing teams to collaborate efficiently.

How to Build an Image

- **Dockerfile**: Describes the steps and actions required to set up the image, from selecting the base OS to copying the application code.
- **Build Command**: Docker's build command uses the Dockerfile as a blueprint to create the image.

Advantages of Docker Images

- **Portability**: Docker images ensure consistent behavior across different environments, from development to production.
- **Reproducibility**: If you're using the same image, you can expect the same application behavior.
- **Efficiency**: The layered filesystem reduces redundancy and accelerates deployment.
- **Security**: Distinct layers permit granular security control.

Best Practices for Dockerfiles

Use the official base image if possible.
Aim for minimal layers for better efficiency.
Regularly update the base image to ensure security and feature updates.
Reduce the number of packages installed to minimize security risks.

**Examples:**

Code Example: Dockerfile

Here is the Dockerfile:

```dockerfile
# Use a base image
FROM ubuntu:latest

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Specify the command to run on container start
CMD ["/bin/bash"]
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>How does a Docker container differ from a Docker image?</summary>

### How does a Docker container differ from a Docker image?

**Definition:**

Docker images serve as templates for containers, whereas Docker containers are running instances of those images.

**How it works:**

Key Distinctions

- **State**: Containers encapsulate both the application code and its runtime environment in a stable and consistent state. In contrast, images are passive and don't change once created.

- **Mutable vs Immutable**: Containers, like any running process, can modify their state. In contrast, images are immutable and do not change once built.

- **Disk Usage**: Containers have both writable layers (such as logs or configuration files) and read-only layers (the image layers), potentially leading to increased disk usage over time. Docker's use of layered storage, however, limits this growth.

Images, on the other hand, are solely read-only, meaning each instance based on the same image doesn't consume additional disk space.

Docker Image vs Container

Practical Demonstration

Building an Image - Use the docker build command to create the image.
```shell
docker build -t myapp .
```

Instantiating Containers - Run the built image with docker run to spawn a container.
Viewing Containers - The docker container ls or docker ps commands display active containers.

Modifying Containers - As an example, you can change the content of a container by entering in via docker exec.

```shell
docker exec -it mycontainer /bin/bash
```

Stopping and Removing Containers - This can be done using the docker stop and docker rm commands or combined with the -f flag.
Cleaning Up Images - Remove any unused images to save storage space.
```shell
docker image prune -a
```

**Examples:**

Here is the code:

Dockerfile - Defines the image:

```dockerfile
# Set the base image
FROM python:3.8

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

```shell
# Run a single command within a new container
docker run myapp python my_script.py
# Run a container in detached mode and enter it to explore the environment
docker run -d -it --name mycontainer myapp /bin/bash
```

```shell
docker stop mycontainer
docker rm mycontainer
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>What is the Docker Hub, and what is it used for?</summary>

### What is the Docker Hub, and what is it used for?

**Definition:**

The Docker Hub is a public cloud-based registry for Docker images. It's a central hub where you can find, manage, and share your Docker container images. Essentially, it is a version control system for Docker containers.

**How it works:**

Key Functions

- **Image Storage**: As a centralized repository, the Hub stores your Docker images, making them easily accessible.

- **Versioning**: It maintains a record of different versions of your images, enabling you to revert to previous iterations if necessary.

- **Collaboration**: It's a collaborative platform where multiple developers can work on a project, each contributing to and pulling from the same image.

- **Link to GitHub**: Docker Hub integrates with the popular code-hosting platform GitHub, allowing you to automatically build images using pre-defined build contexts.

- **Automation**: With automated builds, you can rest assured that your images are up-to-date and built to the latest specifications.

- **Webhooks**: These enable you to trigger external actions, like CI/CD pipelines, when certain events occur, enhancing the automation capabilities of your workflow.

- **Security Scanning**: Docker Hub includes security features to safeguard your containerized applications. It can scan your images for vulnerabilities and security concerns.

Cost and Pricing

- **Free Tier**: Offers one private repository and unlimited public repositories.
- **Pro and Team Tiers**: Both come with advanced features. The Team tier provides collaboration capabilities for organizations.

Key Benefits and Limitations

Benefits:

Centralized Container Distribution

Security Features

Integration with CI/CD Tools

Multi-Architecture Support

Limitations:

Limited Private Repositories in the Free Plan

Might Require Additional Security Measures for Sensitive Workloads

**When to use:**

Use Cases

- **Public Repositories**: These are ideal for sharing your open-source applications with the community. Docker Hub is home to a multitude of public repositories, each extending the functionality of Docker.

- **Private Repositories**: For situations requiring confidentiality, or to ensure compliance in regulated environments, Docker Hub allows you to maintain private repositories.

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Explain the Dockerfile and its significance in Docker.</summary>

### Explain the Dockerfile and its significance in Docker.

**Definition:**

One of the defining features of Docker is its use of Dockerfiles to automate the creation of container images. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

**How it works:**

Common Commands

- **FROM**: Sets the base image for subsequent build stages.
- **RUN**: Executes commands within the image and then commits the changes.
- **EXPOSE**: Informs Docker that the container listens on a specific port.
- **ENV**: Sets environment variables.
- **ADD/COPY**: Adds files from the build context into the image.
- **CMD/ENTRYPOINT**: Specifies what command to run when the container starts.

Multi-Stage Builds

- **FROM**: Allows for multiple build stages in a single Dockerfile.
- **COPY --from=source**: Enables copying from another build stage, useful for extracting build artifacts.

Image Caching

Docker uses caching to speed up build processes. If a layer changes, Docker rebuilds it and all those that depend on it. Often, this results in fortuitous cache misses, making builds slower than anticipated.

To optimize, place commands that change frequently (such as file copying or package installation) toward the end of the file.

Docker Build Accesses a remote repository, the Docker Cloud. The build context is the absolute path or URL to the directory containing the Dockerfile.

Tips for Writing Efficient Dockerfiles

- **Use Specific Base Images**: Start from the most lightweight, appropriate image to keep your build lean.
- **Combine Commands**: Chaining commands with && (where viable) reduces layer count, enhancing efficiency.
- **Remove Unneeded Files**: Eliminate files your application doesn't require, especially temporary build files or cached resources.

**Examples:**

Code Example: Dockerfile for a Node.js Web Server

Here is the Dockerfile:

```dockerfile
# Use a specific version of Node.js as the base
FROM node:14-alpine

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json first to leverage caching when the
# dependencies haven't changed
COPY package*.json ./

# Install NPM dependencies
RUN npm install --only=production

# Copy the rest of the application files
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the Node.js application
CMD ["node", "app.js"]
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>How does Docker use layers to build images?</summary>

### How does Docker use layers to build images?

**Definition:**

Docker follows a Layered File System approach, employing Union File Systems like AUFS, OverlayFS, and Device Mapper to stack image layers.

This structure enhances modularity, storage efficiency, and image-building speed. It also offers read-only layers for image consistency and integrity.

**How it works:**

Union File Systems

Union File Systems permit stacking multiple directories or file systems, presenting them coherently as a single unit. While several such systems are in use, AUFS and OverlayFS are notably popular.

- **AUFS**: A front-runner for a long time, AUFS offers versatile compatibility but is not part of the Linux kernel.
- **OverlayFS**: Now integrated into the Linux kernel, OverlayFS is lightweight and provides backward compatibility with ext4 and XFS.

Image Layering in Docker

When stacking Docker image layers, it's akin to a file system with read-only layers superimposed by a writable layer, the container layer. This setup ensures separation and persistence:

- **Base Image Layer**: This is the foundation, often comprising the operating system and core utilities. It's mostly read-only to safeguard uniformity.

- **Intermediate Layers**: These are interchangeable and encapsulate discrete modifications. Consequently, they are also mostly read-only.

- **Topmost or Container Layer**: This layer records real-time alterations made within the container and is mutable.

Code Overlayers

Here is the code:

Each layer is defined by a Dockerfile instruction.
The base image is ubuntu:latest, and the application code is stored in a file named app.py.

**Examples:**

```dockerfile
# Layer 1: Start from base image
FROM ubuntu:latest

# Layer 2: Set the working directory
WORKDIR /app

# Layer 3: Copy the application code
COPY app.py /app

# Placeholder for Dockerfile
# ...
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>What’s the difference between the `COPY` and `ADD` commands in a Dockerfile?</summary>

### What’s the difference between the `COPY` and `ADD` commands in a Dockerfile?

**Definition:**

Let's look at the subtle distinctions between the COPY and ADD commands within a Dockerfile.

**How it works:**

Purpose

- **COPY**: Designed for straightforward file and directory copying. It's the preferred choice for most use-cases.
- **ADD**: Offers additional features such as URI support. However, since it's more powerful, it's often recommended to stick with COPY unless you specifically need the extra capabilities.

Key Distinctions

- **URI and TAR Extraction**: Only ADD allows you to use URIs (including HTTP URLs) as well as automatically extract local .tar resources. For simple file transfers, COPY is the appropriate choice.
- **Cache Considerations**: Unlike COPY, which respects image build cache, ADD bypasses cache for any resources that differ even slightly from their cache entries. This can lead to slower builds.
- **Security Implications**: Since ADD permits downloading files at build-time, it introduces a potential security risk point. In scenarios where the URL isn't controlled, and the file isn't carefully validated, prefer COPY.
- **File Ownership**: While both COPY and ADD maintain file ownership and permissions during the build process, there might be OS-specific deviations. Consistent behavior is often a critical consideration, making COPY the safer choice.
- **Simplicity and Transparency**: Using COPY exclusively, when possible, ensures clarity and simplifies Dockerfile management. For instance, it's easier for another developer or a CI/CD system to comprehend a straightforward COPY command than to ascertain the intricate details of an ADD command that incorporates URL-based file retrieval or TAR extraction.

Best Practices

- **Avoid Web-Based Transfers**: Steer clear of resource retrieval from untrusted URLs within Dockerfiles. It's safer to copy these resources into your build context, ensuring security and reproducibility.

- **Cache Management**: Because ADD can bypass caching for resources that are even minimally different from their cached versions, it can inadvertently lead to slowed build processes. To avoid this, prefer the deterministic, cache-friendly behavior of COPY whenever plausible.

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>What’s the purpose of the `.dockerignore` file?</summary>

### What’s the purpose of the `.dockerignore` file?

**Definition:**

The .dockerignore file, much like gitignore, is a list of patterns indicating which files and directories should be excluded from image builds.

Using this file, you can optimize the build context, which is the set of files and directories sent to the Docker daemon for image creation.

By excluding unnecessary files, such as build or data files, you can reduce the build duration and optimize the size of the final Docker image. This is important for minimizing container footprint and enhancing overall Docker efficiency.

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>How would you go about creating a Docker image from an existing container?</summary>

### How would you go about creating a Docker image from an existing container?

**Definition:**

Let's look at each of the two main methods:

docker container commit Method:

For simple use cases or quick image creation, this method can be ideal.

Here's a detailed example:

Say you have a running container derived from the ubuntu image and nicknamed 'my-ubuntu'.

Start the container:

```shell
docker run --interactive --tty --name my-ubuntu ubuntu
```

For instance, you decide to customize the my-ubuntu container by adding a package.

You will see output resembling:

```text
CONTAINER ID        IMAGE               COMMAND             ...        NAMES
f2cb54bf4059        ubuntu              "/bin/bash"         ...        my-ubuntu
```

In this output, "f2cb54bf4059" is the Container ID for 'my-ubuntu'.

Now, you have a modified image based on your updated container. You can verify it by running:

```shell
docker run --rm -it my-ubuntu:with-neofetch neofetch
```

Here, "f2cb54bf4059" is the Container ID that you can find using docker ps.

Image Build Process Method:

This method provides more control, especially in intricate scenarios. It generally involves a two-step process where you start by creating a Dockerfile and then build the image using docker build.

Steps:

- **Create A Dockerfile**: Begin by preparing a Dockerfile that includes all your customizations and adjustments.
Subsequently, you can run a container using this new image and verify your modifications:

```shell
docker run --rm -it my-ubuntu:with-neofetch neofetch
```

**Examples:**

It uses the following command:

```shell
docker container commit <CONTAINER_ID> <REPOSITORY:TAG>
```

Make the package change (for this example):

```shell
docker exec -it my-ubuntu bash  # Enter the shell of your 'my-ubuntu' container
apt update
apt install -y neofetch         # Install `neofetch` or another package for illustration
exit                           # Exit the container's shell
```

Take note of the "Container ID" using docker ps command:

```shell
docker ps
```

Use the docker container commit command to create a new image based on changes in the 'my-ubuntu' container:

```shell
docker container commit f2cb54bf4059 my-ubuntu:with-neofetch
```

For our 'my-ubuntu' example, the Dockerfile can be as simple as:

```Dockerfile
FROM my-ubuntu:latest
RUN apt update && apt install -y neofetch
```

Build the Image: Enter the directory where your Dockerfile resides and start the build using the following command:

```shell
docker build -t my-ubuntu:with-neofetch .
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>In practice, how do you reduce the size of Docker images?</summary>

### In practice, how do you reduce the size of Docker images?

**Definition:**

Reducing Docker image sizes is crucial for efficient resource deployment. You can achieve this through various strategies.

**How it works:**

Multi-Stage Builds

Multi-Stage Builds allow you to use multiple Dockerfile stages, segregating different aspects of your build process. This enables a cleaner separation between build-time and run-time libraries, ultimately leading to smaller images.

The --from flag in the COPY and RUN instructions is key here, as it allows you to select artifacts from a previous build stage.

.dockerignore File

Similar to .gitignore, the .dockerignore file excludes files and folders from the Docker build context. This can significantly reduce the size of your build context, leading to slimmer images.

Here is an example of a .dockerignore file:

node_modules
npm-debug.log

Using Smaller Base Images

Selecting a minimalistic base image can lead to significantly smaller containers. For node.js, you can choose a smaller base image such as node:alpine, especially for production use. The alpine version is particularly lightweight as it's built on the Alpine Linux distribution.

Here are images with different sizes:

node:current-slim (about 200MB)
node:alpine (about 90MB)
node:current (about 900MB)

One-Time Execution Commands

Using RUN and multi-line COPY commands within the same Dockerfile layer can lead to image bloat. To mitigate this, leverage a single RUN command that packages multiple operations. This approach reduces additional layer creation, resulting in smaller images.

Ensure that you always combine such commands in a single RUN instruction, separated by logical operators like &&, and clean up any temporary files or caches to keep the layer minimal.

Package Managers and Caching

When using package managers like npm and pip in your images, it's important to use a --production flag.

For pip, you can achieve the same with:

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

This practice significantly reduces the image size by only including necessary runtime dependencies.

Utilize Glob Patterns for COPY

When using the COPY command in your Dockerfile, it's best to introduce .dockerignore syntax to ensure only essential files are copied.

**Examples:**

Here is the dockerfile with the multi-stage build.

```dockerfile
# Use an official Node.js runtime as the base image
FROM node:current-slim AS build

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files to the workspace
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the entire project into the container
COPY . .

# Build the app
RUN npm run build

# Use a smaller base image for the final stage
FROM node:alpine AS runtime

# Set the working directory in the container
WORKDIR /app

# Copy built files and dependency manifest
COPY --from=build /app/package*.json ./
COPY --from=build /app/dist ./dist

# Install production dependencies
RUN npm install --only=production

# Specify the command to start the app
CMD ["node", "dist/main.js"]
```

Here is an example:

```text
RUN apt-get update && apt-get install -y nginx && apt-get clean
```

For npm, running the following command prevents the installation of development dependencies:

```text
RUN npm install --only=production
```

Here is an example:

```text
COPY ["*.json", "*.sh", "config/", "./"]
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>What command is used to run a Docker container from an image?</summary>

### What command is used to run a Docker container from an image?

**Definition:**

The lean, transformed and updated version of the answer includes all the essential points.

**How it works:**

The command docker run combines several actions:

- **Creating**: If the container matching the input name already exists, it will stop and then start again.
- **Running**: Activates the container, starting its process.
- **Linking**: Connects to the necessary network, storage, and system resources.

Basic Usage

Practical Example

- **-d**: The container is detached, running in the background.
-p 5000:5000: The host port 5000 is mapped to the container port 5000.
- **--name myapp**: The container is named myapp.
myimage:latest: The image used is myimage with the latest tag.

Additional Options and Example

Deletes the container after it stops.
Opens an interactive terminal.
Mounts the host's /host/path to the container's /container/path.
Uses the command /bin/bash when starting the container.

**Examples:**

To run a Docker container from an image, you can use the docker run command:

```shell
docker run
```

Here is the generic structure:

```shell
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

```shell
docker run -d -p 5000:5000 --name myapp myimage:latest
In this example:
```

Here is an alternative command:

```shell
docker run --rm -it -v /host/path:/container/path myimage:1.2.3 /bin/bash
This:
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Can you explain what a Docker namespace is and its benefits?</summary>

### Can you explain what a Docker namespace is and its benefits?

**Definition:**

A Docker namespace uniquely identifies Docker objects like containers, images, and volumes. Namespaces streamline resource organization and data isolation, supporting your security and operational requirements.

**How it works:**

Advantages of Docker Namespaces

- **Isolated Environment**: Ensures separation, vital for multi-tenant systems, in-house CI/CD, and staging environments.

- **Resource Segregation**: Every workspace allocates distinct processes, network ports, and filesystem mounts.

- **Multi-Container Management**: You can track related containers across various environments thoroughly.

- **Improved Debugging and Error Control**: Dockers namespace, keep your workstations clean and facilitate accurate error tracking.

- **Enhanced Security**: Reduces the risk of data breaches and system interdependencies.

- **Portability and Adaptability**: Supports a consistent operational model, irrespective of the environment.

Key Namespace Types

- **Image IDs**: Unique identifiers for Docker images.
- **Container Names**: Provides friendly readability to Docker containers.
- **Volume Names**: Simplified references in managing persistent data volumes.

**Examples:**

Code Example: Working with Docker Namespaces

Here is the Python code:

```python
import docker

# Establish connection with Docker daemon
client = docker.from_env()

# Pull a Docker image
client.images.pull('ubuntu:latest')

# List existing Docker images
images = client.images.list()
print(images)

# Note: In a practical Docker environment, you would see more detailed output related to the images.

# Retrieve a container by its name
event_container = client.containers.get('event-container')

# Inspect a specific container to gather detailed information
inspect_data = event_container.attrs
print(inspect_data)

# Create a new Docker volume
client.volumes.create('my-named-volume')
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>What is a Docker volume, and when would you use it?</summary>

### What is a Docker volume, and when would you use it?

**Definition:**

A Docker volume is a directory or file within a Docker Host's writable layer that isn't tied to a specific container. This decoupling allows data persistence, even after containers have been stopped or removed.

**How it works:**

Volume Types

- **Host-Mounted Volumes**: These link a directory on the host machine to the container.
- **Named Volumes**: They have a specific name and are managed by Docker.
- **Anonymous Volumes**: These are generated by Docker and not tied to a specific container or its data.

**When to use:**

Use Cases

Docker volumes are fundamental for data storage and sharing, which is especially beneficial in microservice and stateful applications.

- **File Sharing**: Volume remaps between containers, facilitating file sharing without needing to commit volumes to an image or set up additional systems like NFS.

- **Database Management**: Ensures database consistency by isolating database files within volumes. This makes it simpler to back up and restore databases.

- **Stateful Container Handling**: Volumes assist in preserving stateful container data, like logs or configuration files, ensuring uninterrupted service data delivery and persistence, even in case of container updates or failures.

- **Configuration and Secret Management**: Volumes provide an excellent way to mount configuration files and secrets. This can help you secure sensitive data and reduces the need to build it into the application.

- **Backup and Restore**: By using volumes, you can separate your data from the lifecycle of the container. It becomes easier to back them up and restore them in the event of data loss.

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Explain the use and significance of the `docker-compose` tool.</summary>

### Explain the use and significance of the `docker-compose` tool.

**Definition:**

Docker Compose, a command-line tool, facilitates multi-container Docker applications, using a YAML file to define their architecture and how they interconnect. This is incredibly useful for setting up multi-container environments and facilitates a "one command" startup for all relevant components. For instance, a web application might require a backend database, a message queue, and more. While you can launch these components individually, using docker-compose makes it a seamless single-command operation.

**How it works:**

Core Advantages

- **Simplified Multi-Container Management**: With one predefined configuration, launch and manage multi-container apps effortlessly.
- **Streamlined Environment Sharing**: Consistent setups between teams and environments simplify testing, staging, and development.
- **Automatic Inter-Container Networking**: Defines network configurations such as volume sharing and service linking without added commands.
- **Parallel Service Startup**: Efficiently starts services in parallel, making boot-ups faster.

Core Components

- **Services**: Containers that build off the same image, defined in the compose file. Each is an independent component (e.g., web server, database).
- **Volumes**: For persistent data, decoupled from container lifespan. Useful for databases, among others.
- **Networks**: Virtual networks for isolating different applications or services, keeping them separate or aiding in communication.

YAML Configuration Example

- **Services**: web and db are the components mentioned. They define an image to be used, port settings, volumes for data persistence, and dependency structures (like how web depends on db).

- **Volumes**: The db service has a volume specified for persistent storage.

- **Networks**: The web and db services are part of the backend network, defined at the bottom. This assures consistent networking, even when services get linked or containers restarted.

**Examples:**

Here is the YAML configuration:

```yaml
version: '3.3'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - "/path/to/html:/usr/share/nginx/html"
    depends_on:
      - db

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: dbname
    volumes:
      - /my/own/datadir:/var/lib/postgresql/data

networks:
  backend:
    driver: bridge
```

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Can Docker containers running on the same host communicate with each other by default? If so, how?</summary>

### Can Docker containers running on the same host communicate with each other by default? If so, how?

**Definition:**

Yes, Docker containers on the same host can communicate with each other by default. This is because, when you run a Docker container, it's on a single network namespace of the host, and Docker uses that network namespace to manage communication between containers.

**How it works:**

Default Network Configuration

By default, Docker provides each container with its own network stack. The configuration includes:

- **IP Address**: Obtained from the Docker network.

- **Network Interfaces**: Namespaced within the container.

Default Docker Bridge Network

A Docker bridge network, such as docker0, serves as the default network type. Containers within the same bridge network can communicate with each other by their container names or IP addresses.

Custom Networks

Containers can also be part of user-defined bridge networks or other network types. In such configurations, containers belonging to the same network can communicate with each other.

Configuring Communication

Direct container-to-container communication is straightforward. Once a container knows the other's IP address, it can initiate communication.

Here are two key methods to configure container communication:

1. By Container IP
```shell
docker inspect -f '{{.NetworkSettings.IPAddress}}' <container_id>
```

2. By Container Name
Containers within the same Docker network can reach each other by their names. Use docker network inspect <your_network_name> to see container IP addresses and ensure proper network setup.

**Sources:**

- [Devinterview.io — Docker](https://devinterview.io/questions/software-architecture-and-system-design/docker-interview-questions/)
</details>

<details>
<summary>Describe the different types of networks in Docker.</summary>

### Describe the different types of networks in Docker.

**Definition:**

Docker’s network subsystem is pluggable. The drivers that ship with the engine are `bridge`, `host`, `overlay`, `ipvlan`, `macvlan`, and `none`. You can also install a network plugin.

**How it works:**

- `bridge` is the default. Containers on the same bridge on one host can communicate. Traffic from outside reaches them through published ports. If you omit `--driver`, `docker network create` uses bridge.
- `host` removes the network namespace between the container and the host. The container uses the host’s network stack.
- `overlay` connects daemons so Swarm services and containers can talk across nodes without you configuring OS routes between those nodes.
- `ipvlan` gives control of IPv4 and IPv6 addresses. The VLAN driver adds layer-2 VLAN tags and L3 routing for an underlay.
- `macvlan` assigns a MAC address so the container looks like a physical device. The daemon routes by MAC. The docs suggest it for legacy apps that expect to sit on the physical network.
- `none` isolates the container from the host and from other containers. It is not available for Swarm services.

**When to use:**

- `bridge` for containers on one machine.
- `overlay` for a Swarm that spans hosts.
- `host` when you accept sharing the host’s network stack.
- `none` when the container should have no network.

**Examples:**

```console
docker network create -d overlay --attachable my-attachable-overlay
```

`--attachable` lets standalone containers join that overlay, not only Swarm services.

**Sources:**

- [Docker — Network drivers](https://docs.docker.com/engine/network/drivers/)
- [Docker — Overlay driver](https://docs.docker.com/engine/network/drivers/overlay/)

</details>

<details>
<summary>How do you create a Docker network?</summary>

### How do you create a Docker network?

**Definition:**

`docker network create` creates a network. Without `-d` / `--driver`, the driver is `bridge`.

**How it works:**

- `docker network create my-net` creates a user-defined bridge.
- `docker network create --driver overlay my-network` on a Swarm manager creates an overlay. Every manager can see it.
- `docker network create -d macvlan --subnet=172.16.86.0/24 --gateway=172.16.86.1 -o parent=eth0 pub_net` bridges a macvlan network to a host interface.
- A user-defined bridge can be configured on its own (MTU, masquerade, and the other bridge options). The default `bridge` network is shared by every container that does not set a network.

**Examples:**

```console
docker network create --driver overlay my-network
```

**Sources:**

- [Docker — Overlay network driver](https://docs.docker.com/engine/network/drivers/overlay/)
- [Docker — Macvlan network driver](https://docs.docker.com/engine/network/drivers/macvlan/)
- [Docker — Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)

</details>

<details>
<summary>How could you connect a container to a specific network?</summary>

### How could you connect a container to a specific network?

**Definition:**

`docker network connect NETWORK CONTAINER` attaches a running container. `docker run --network` attaches it at start.

**How it works:**

- Connect by name or id. After that, the container can reach other containers on that network by IP or name.
- `--ip` and `--ip6` set the address. `--alias` adds a network-scoped DNS name.
- A container may join more than one network, including a bridge and an overlay.
- On overlay networks and multi-host plugins, containers on different engines that share the network can communicate by IP or name.
- If you pin `--ip` and that address is taken when the container restarts, the container fails to start. The connect reference suggests an `--ip-range` on `docker network create` and a static address outside that range.
- `docker network disconnect` removes the container from the network. `docker network inspect` shows who is connected.
- You can connect and disconnect a user-defined network while the container runs. Leaving the default bridge means recreating the container.

**Examples:**

```console
docker network connect multi-host-network container1
docker run -itd --network=multi-host-network busybox
```

**Sources:**

- [Docker — docker network connect](https://docs.docker.com/reference/cli/docker/network/connect/)
- [Docker — Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)

</details>

<details>
<summary>Can you explain how Docker’s default bridge network differs from a user-defined bridge network?</summary>

### Can you explain how Docker’s default bridge network differs from a user-defined bridge network?

**Definition:**

Starting the daemon creates a default bridge named `bridge`. New containers join it unless you set a network. User-defined bridges are networks you create, and the docs say they are the better default.

**How it works:**

- On a user-defined bridge, containers resolve each other by name or alias. On the default bridge they reach each other by IP, unless you use the legacy `--link` flag.
- Every container without `--network` shares the default bridge, so unrelated stacks can talk. A user-defined network limits that to containers you attached.
- You can connect or disconnect a user-defined network during the container’s life. Removing a container from the default bridge means stopping it and recreating it with different network options.
- The default bridge is one configuration (MTU, iptables) for every container on it, and changing it is done outside Docker and needs a daemon restart. Each user-defined bridge is configured with `docker network create`.
- Legacy `--link` on the default bridge shares environment variables. User-defined networks do not. Share config with a volume, a Compose file, or Swarm secrets and configs.
- Containers on the same user-defined bridge see each other’s ports without publishing. A port must be published with `-p` / `--publish` to be reachable from another network or from outside Docker.

**When to use:**

- A user-defined bridge for any app that has more than one container.
- The default bridge only when you have a reason to stay on `docker0`.

**Sources:**

- [Docker — Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)

</details>

<details>
<summary>How would you enable communication between Docker containers on different hosts?</summary>

### How would you enable communication between Docker containers on different hosts?

**Definition:**

A bridge only covers one daemon. The bridge driver docs say cross-host traffic is either routed by the operating system or carried by an overlay network.

**How it works:**

- `docker network create --driver overlay my-network` on a Swarm manager creates an overlay the swarm can use. `--attachable` also lets standalone containers join.
- Containers on that overlay, including ones started on different engines, communicate by IP or name. That is the connect command’s multi-host note.
- `docker stack deploy` creates the stack’s network and services. The stack deploy guide shows the routing mesh: a published port works on any node and is routed to the service.
- OS-level routing is the other option the bridge page names. You then manage addresses and routes yourself.

**When to use:**

- Overlay when the containers are Swarm services or attachable overlay members.
- Host routing when you are not using Swarm and you control the network between the machines.

**Examples:**

```console
docker network create --driver overlay my-network
```

**Sources:**

- [Docker — Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)
- [Docker — docker network connect](https://docs.docker.com/reference/cli/docker/network/connect/)
- [Docker — Deploy a stack](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

### Docker Performance & Security

<details>
<summary>How can resource constraints be applied to containers in Docker?</summary>

### How can resource constraints be applied to containers in Docker?

**Definition:**

By default a container can use as much memory and CPU as the kernel scheduler allows. `docker run` flags set cgroup limits. Compose `deploy.resources` sets limits and reservations for a service.

**How it works:**

- `-m` / `--memory` is a hard maximum. The minimum you may set is `6m`.
- `--memory-swap` is the total of memory plus swap, and it only means something together with `--memory`. Setting both to the same value disables swap. Unset `--memory-swap` with `--memory` set lets the container use as much swap as the memory limit, if the host has swap. `-1` means unlimited swap up to the host.
- `--memory-reservation` is a soft limit and must be lower than `--memory`. `--memory-swappiness` is 0–100. `0` disables anonymous page swap. The default is the host’s value.
- `--cpus=0.5` caps the container at half a CPU. That equals `--cpu-period=100000` and `--cpu-quota=50000`. `--cpu-shares` (default 1024) is a weight and applies only when the CPU is busy.
- Do not set `--oom-kill-disable` unless `--memory` is also set. The kernel’s OOM killer is what stops one container from taking the host down. Docker lowers the daemon’s OOM priority so the daemon is less likely to be killed than a container.
- Compose `deploy.resources.limits` uses `cpus` and `memory` for the same idea on a single host.

**Examples:**

```console
docker run -it --cpus=".5" --memory="300m" ubuntu /bin/bash
```

**Sources:**

- [Docker — Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)
- [Docker — Compose deploy](https://docs.docker.com/reference/compose-file/deploy/)

</details>

<details>
<summary>What are Docker security profiles, and how do they work?</summary>

### What are Docker security profiles, and how do they work?

**Definition:**

Seccomp is a Linux kernel feature Docker uses to limit system calls inside a container. The default profile is an allowlist. The docs say not to change it unless you have a reason.

**How it works:**

- The default profile’s `defaultAction` is `SCMP_ACT_ERRNO` (permission denied), then specific calls are set to `SCMP_ACT_ALLOW`. It disables on the order of 44 system calls out of 300+.
- A container uses that profile unless you pass `--security-opt seccomp=/path/to/profile.json`. The CLI reads the file from the client machine.
- `--security-opt seccomp=unconfined` turns the default profile off. The docs show that so a process can `unshare` a user namespace. That is a wider syscall surface.
- The kernel must be built with `CONFIG_SECCOMP`. Check `/boot/config-$(uname -r)`.
- The seccomp page also notes the default AppArmor profile and the SELinux policy as the other mandatory-access checks around address families such as `AF_VSOCK`.

**When to use:**

- Leave the default profile on.
- A custom profile only when an application needs a call the default blocks, and you can name that call.
- `unconfined` only for a short-lived diagnosis, not as a production setting.

**Examples:**

```console
docker run --rm -it --security-opt seccomp=/path/to/seccomp/profile.json hello-world
```

**Sources:**

- [Docker — Seccomp security profiles](https://docs.docker.com/engine/security/seccomp/)

</details>

<details>
<summary>Explain how you would scan a Docker image for vulnerabilities.</summary>

### Explain how you would scan a Docker image for vulnerabilities.

**Definition:**

Image scanning compares the packages in an image with a vulnerability database. Docker’s product for that is Docker Scout. The scan result is a list of known CVEs in the image, not a proof the image is safe.

**How it works:**

- Build the image, then run `docker scout cves`. By default Scout analyzes the image you built most recently. `--only-package` limits the report to one package. Fix by updating the base image or the package, then rebuild. Scanning a running container does not change the image.
- A scan is one moment in time. A new advisory can appear after you shipped the tag.
- Do not bake secrets into the image you scan. A CVE report does not look for passwords in layers. Use Swarm secrets or a runtime mount for those.

**Examples:**

```console
docker scout cves --only-package express
```

**Sources:**

- [Docker Scout quickstart](https://docs.docker.com/scout/quickstart/)

</details>

<details>
<summary>What is the purpose of a Docker Healthcheck?</summary>

### What is the purpose of a Docker Healthcheck?

**Definition:**

`HEALTHCHECK` tells the daemon how to test that the process inside the container is still doing useful work. A running PID is not enough. A web server can be stuck and still be process 1.

**How it works:**

- `HEALTHCHECK [OPTIONS] CMD command` runs a command in the container. `HEALTHCHECK NONE` disables a check inherited from the base image. Only the last `HEALTHCHECK` in the Dockerfile is kept.
- Options: `--interval` (default `30s`), `--timeout` (default `30s`), `--start-period` (default `0s`), `--start-interval` (default `5s`, Engine 25.0+), `--retries` (default `3`).
- Exit `0` is healthy, `1` is unhealthy, `2` is reserved. Status moves from `starting` to `healthy`, and to `unhealthy` after `--retries` consecutive failures. Failures during the start period do not count toward retries unless a check has already succeeded.
- The first 4096 bytes of stdout and stderr are stored and shown by `docker inspect`. A status change emits a `health_status` event.

**Examples:**

```dockerfile
HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

**Sources:**

- [Docker — Dockerfile HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck)

</details>

<details>
<summary>Describe how you would handle sensitive data with Docker.</summary>

### Describe how you would handle sensitive data with Docker.

**Definition:**

Do not put passwords, keys, or certificates in the image or the Dockerfile. For Swarm services, Docker secrets store that data encrypted and mount it only into services you grant.

**How it works:**

- A secret is sent to a manager over mutual TLS and stored in the encrypted Raft log. The log is replicated to other managers.
- When a task is allowed to use the secret, the decrypted value is mounted on an in-memory filesystem. On Linux the default path is `/run/secrets/<secret_name>`. On Windows it is `C:\ProgramData\Docker\secrets`.
- Only managers, and nodes running a task that was granted the secret, hold it. When the task stops, the secret is unmounted and flushed from the node’s memory.
- Secrets are for Swarm services, not standalone containers. A single replica (`scale` 1) is the docs’ path if you need a secret without rewriting the container as a multi-replica service.
- Size limit in the docs: 500 KB. Non-sensitive files can be configs, which are mounted as normal files rather than a RAM disk.
- `docker secret rm` cannot remove a secret a running service still uses.

**When to use:**

- Swarm secrets for passwords and TLS keys a service needs at runtime.
- A user-defined config for non-secret files.

**Sources:**

- [Docker — Manage sensitive data with Docker secrets](https://docs.docker.com/engine/swarm/secrets/)

</details>

### Docker Orchestration

<details>
<summary>What is Docker Swarm, and when would you use it?</summary>

### What is Docker Swarm, and when would you use it?

**Definition:**

Swarm mode is Docker Engine’s built-in orchestrator. You turn it on with `docker swarm init`. Services are scheduled across the nodes that joined the swarm.

**How it works:**

- A manager accepts `docker service` and `docker stack` commands. `docker stack deploy` reads a Compose file and creates services. The stack deploy guide warns that this command uses the legacy Compose file version 3 format, not the latest Compose specification.
- Images must be in a registry the nodes can pull. The guide uses Docker Hub or a local registry service.
- The routing mesh publishes a service port on every node and routes to a task.
- Secrets and overlay networks are Swarm features. `none` is not a Swarm network driver.
- Leave a local test swarm with `docker swarm leave --force`.

**When to use:**

- You want several Docker hosts to run one application and you are staying inside Docker Engine.
- A single host does not need Swarm. `docker compose up` runs that stack on one engine. The stack guide says Compose does not schedule across swarm nodes.

**Examples:**

```console
docker swarm init
docker stack deploy --compose-file compose.yaml stackdemo
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>Can you explain the difference between Docker Swarm and Kubernetes?</summary>

### Can you explain the difference between Docker Swarm and Kubernetes?

**Definition:**

Swarm mode is part of Docker Engine. Kubernetes is a separate orchestrator with its own API and control plane. Both schedule containers across machines. They are not the same command set.

**How it works:**

- Swarm: `docker swarm init`, then `docker service` and `docker stack deploy`. The deploy command expects a legacy Compose v3 file. Networking across nodes is an overlay. Published ports use the routing mesh.
- Kubernetes: you apply manifests to an API server. Pods, Services, and Deployments are Kubernetes objects. Docker Engine can run the containers, but `docker stack deploy` does not talk to that API.
- Swarm secrets are the Docker feature documented for swarm services. Kubernetes has its own Secret object. Do not assume one store is the other.
- Pick Swarm when the cluster should stay on Docker’s CLI. Pick Kubernetes when the platform you deploy to is a Kubernetes cluster.

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Kubernetes — Overview](https://kubernetes.io/docs/concepts/overview/)

</details>

<details>
<summary>What is a Docker Stack?</summary>

### What is a Docker Stack?

**Definition:**

A stack is a group of services deployed together from a Compose file onto a swarm. `docker stack deploy` creates the services and a network whose names are prefixed with the stack name.

**How it works:**

- The file in the guide is `compose.yaml` with a `services` map. `docker stack deploy --compose-file compose.yaml stackdemo` is the command. The last argument is the stack name.
- The guide’s deploy prints `Ignoring unsupported options: build`. Build the image and push it to a registry first. Swarm nodes pull. They do not build from your laptop’s context.
- `docker stack services STACK` lists the services and their replica counts. `docker stack rm STACK` removes the services and the stack network.
- Compose `docker compose up` is not a stack deploy. On a swarm engine it still schedules every container on the current node.

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>How do you deploy an application stack in Docker Swarm?</summary>

### How do you deploy an application stack in Docker Swarm?

**Definition:**

Initialize swarm mode, push images to a registry, then run `docker stack deploy` from a manager.

**How it works:**

1. `docker swarm init` on the manager. Other engines join that swarm.
2. Put images in a registry. The guide’s example tags the web image `127.0.0.1:5000/stackdemo` and runs `docker compose push`.
3. From a manager: `docker stack deploy --compose-file compose.yaml stackdemo`.
4. `docker stack services stackdemo` should show replicas such as `1/1` after the nodes pull the images.
5. A published port is reachable on any node through the routing mesh.
6. `docker stack rm stackdemo` removes it.

**Examples:**

```console
docker stack deploy --compose-file compose.yaml stackdemo
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>How does Docker manage service replication and load balancing?</summary>

### How does Docker manage service replication and load balancing?

**Definition:**

A Swarm service runs a set number of tasks. The routing mesh sends traffic that arrives on a published port to those tasks.

**How it works:**

- Compose `deploy.mode` for a durable service is `replicated` (the default, a set number of tasks) or `global` (one task on each node). Jobs use `replicated-job` or `global-job` and exit when they finish.
- `docker stack services` prints `REPLICAS` as running/desired, for example `1/1`.
- In the stack guide, port `8000` published by the web service answers on every node. A request to another node’s address is routed to the service. That is the routing mesh.
- `deploy.resources.limits` caps CPUs and memory for those tasks. Limits are enforced. Reservations are what the scheduler should be able to grant.

**Examples:**

```yaml
services:
  web:
    image: 127.0.0.1:5000/stackdemo
    ports:
      - "8000:8000"
    deploy:
      mode: replicated
      replicas: 3
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Docker — Compose deploy](https://docs.docker.com/reference/compose-file/deploy/)

</details>

### Docker CLI & API

<details>
<summary>What command would you use to list all running containers?</summary>

### What command would you use to list all running containers?

**Definition:**

`docker ps` lists running containers. It is the alias of `docker container ls`.

**How it works:**

- The default table includes container id, image, command, created time, status, ports, and name.
- Stopped containers are omitted until you ask for all containers. The exec reference shows a paused container still listed by `docker ps` with status `Paused`.
- `docker ps -q` prints only ids. That output is what you pass to other commands.

**Examples:**

```console
docker ps
```

**Sources:**

- [Docker — docker container ls](https://docs.docker.com/reference/cli/docker/container/ls/)

</details>

<details>
<summary>How can you stop all containers running on a host using a single command?</summary>

### How can you stop all containers running on a host using a single command?

**Definition:**

`docker stop` stops one or more containers. Give it every running id from `docker ps -q`.

**How it works:**

- `docker ps -q` prints ids of running containers, one per line.
- `docker stop` sends those containers the stop signal and waits, then kills them if they do not exit.
- This affects every running container on that daemon, including ones you did not start for this project.

**Examples:**

```console
docker stop $(docker ps -q)
```

**Sources:**

- [Docker — docker container ls](https://docs.docker.com/reference/cli/docker/container/ls/)
- [Docker — docker container stop](https://docs.docker.com/reference/cli/docker/container/stop/)

</details>

<details>
<summary>Can you describe how you would execute a command inside a running container?</summary>

### Can you describe how you would execute a command inside a running container?

**Definition:**

`docker exec` starts a new process inside a running container. The container’s PID 1 must be running. The exec process is not restarted if the container restarts.

**How it works:**

- The command must be an executable. `docker exec -it my_container sh -c "echo a && echo b"` works. Quoting the whole chain as the executable does not.
- `-i` keeps STDIN open. `-t` allocates a TTY. `-d` runs in the background. `-w` sets the working directory. `-e` sets an environment variable for that process only.
- `-u` sets the user. `--privileged` extends privileges. The exec docs point that flag at `docker run --privileged`.
- `docker exec` fails if the container is paused. Unpause it first.
- The working directory defaults to the one set when the container was created.

**Examples:**

```console
docker exec -it mycontainer sh
```

**Sources:**

- [Docker — docker container exec](https://docs.docker.com/reference/cli/docker/container/exec/)

</details>

<details>
<summary>What command would you use to display the logs of a container?</summary>

### What command would you use to display the logs of a container?

**Definition:**

`docker logs CONTAINER` shows that container’s STDOUT and STDERR. `docker service logs` shows the logs of every task in a Swarm service.

**How it works:**

- The text is whatever the main process wrote. A server that logs only to a file inside the container will not appear until those files are redirected to stdout and stderr. The nginx official image links its log files to `/dev/stdout` and `/dev/stderr`. The httpd image writes to `/proc/self/fd/1` and `/proc/self/fd/2`.
- If the logging driver ships logs elsewhere and dual logging is off, `docker logs` can be empty. Read the driver backend in that case.
- Healthcheck output is separate. `docker inspect` stores the first 4096 bytes of a health command’s stdout and stderr.

**Examples:**

```console
docker logs CONTAINER
```

**Sources:**

- [Docker — View container logs](https://docs.docker.com/engine/logging/)

</details>

<details>
<summary>Explain how to copy files from a container to the local file system.</summary>

### Explain how to copy files from a container to the local file system.

**Definition:**

`docker cp` copies files between the local filesystem and a container’s filesystem. The container path is prefixed with the container name and a colon.

**How it works:**

- From the container to the host: `docker cp CONTAINER:SRC DEST`.
- The other direction is `docker cp SRC CONTAINER:DEST`.
- This copies the file bytes. It does not create a volume. A copy is a snapshot. Later writes in the container are not in the file you already copied.
- For data you need to keep, mount a volume instead of copying out of the writable layer.

**Examples:**

```console
docker cp mycontainer:/var/log/app.log ./app.log
```

**Sources:**

- [Docker — docker container cp](https://docs.docker.com/reference/cli/docker/container/cp/)
- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)

</details>

<details>
<summary>How can you inspect the details of a specific container via the CLI?</summary>

### How can you inspect the details of a specific container via the CLI?

**Definition:**

`docker inspect CONTAINER` prints the daemon’s JSON for that container: state, mounts, network settings, and health.

**How it works:**

- Identify the container by name or id.
- Healthcheck stdout and stderr, up to 4096 bytes, are on that JSON. The Dockerfile reference says to query them with `docker inspect`.
- A format template prints one field. The network connect guide uses `docker network inspect` for the network object, which lists attached containers. That is a different object from `docker inspect` on the container.

**Examples:**

```console
docker inspect mycontainer
```

**Sources:**

- [Docker — docker container inspect](https://docs.docker.com/reference/cli/docker/container/inspect/)
- [Docker — Dockerfile HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck)

</details>

<details>
<summary>Describe how to use Docker’s REST API to start a container.</summary>

### Describe how to use Docker’s REST API to start a container.

**Definition:**

The Docker CLI talks to the Engine over the Engine API, a HTTP API on the daemon socket. Starting a container is a POST after the container exists.

**How it works:**

- The local socket is `/var/run/docker.sock`. A request uses that socket and an HTTP path on `localhost`.
- Create the container first, then start it. The start call does not build an image and does not pull.
- `docker start` is the CLI for the same operation. Use the API when another program must drive the daemon.
- Do not expose the socket to the network. Whoever can call the API can start privileged containers.

**Examples:**

```console
curl --unix-socket /var/run/docker.sock \
  -X POST "http://localhost/containers/CONTAINER_ID/start"
```

**Sources:**

- [Docker Engine API](https://docs.docker.com/reference/api/engine/)

</details>

### Docker Best Practices

<details>
<summary>Explain the importance of minimizing the number of layers in a Dockerfile.</summary>

### Explain the importance of minimizing the number of layers in a Dockerfile.

**Definition:**

Each instruction that changes the filesystem adds a layer. Extra layers add size and cache entries. The reason to combine commands is smaller images and fewer stale layers, not a contest to use one line.

**How it works:**

- `RUN`, `COPY`, and `ADD` create layers. `ENV` and metadata instructions also affect the image, but the size problem is usually files left behind in a `RUN`.
- A `RUN` that installs packages and deletes the package cache in a later instruction keeps the cache in the earlier layer. Delete it in the same `RUN`.
- Ordering still matters for the build cache. Put files that change often in a later `COPY` so earlier layers stay cached.
- A multi-stage build avoids shipping the compiler layer at all. The final stage copies only the artifact. That reduces size more than merging two `RUN` lines.

**Examples:**

```dockerfile
RUN apt-get update && apt-get install -y curl \
  && rm -rf /var/lib/apt/lists/*
```

**Sources:**

- [Docker — Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Docker — Building best practices](https://docs.docker.com/build/building/best-practices/)

</details>

<details>
<summary>Discuss the best practices for tagging Docker images.</summary>

### Discuss the best practices for tagging Docker images.

**Definition:**

A tag is a name that points at an image. A digest (`sha256:…`) is the immutable id. The stack deploy guide pulls by digest in `docker stack services` output even when the Compose file used a tag.

**How it works:**

- `latest` moves. A node that already pulled `latest` will not notice your new build. Pin a version tag, and for production record the digest.
- The registry host is part of the name when the image is not on Docker Hub. The stack guide tags `127.0.0.1:5000/stackdemo` so every swarm node can pull it.
- `docker stack deploy` ignores `build`. The tag you deploy must already exist in the registry.
- Do not reuse one tag for two different contents if another environment may have cached the old tag.

**Examples:**

```console
docker compose push
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>When should you use a `.dockerignore` file?</summary>

### When should you use a `.dockerignore` file?

**Definition:**

Use a `.dockerignore` whenever `docker build` would otherwise send files the image does not need. The build context is the directory sent to the daemon.

**How it works:**

- Patterns work like gitignore. Secrets (`.env`, keys), local `node_modules`, and VCS directories should be listed so they are not in the context and cannot be `COPY`’d by accident.
- A smaller context makes the transfer to the daemon shorter. It does not by itself delete files already copied by a `COPY` instruction.
- Put the file in the context root, next to the Dockerfile you build.

**When to use:**

- Any build whose directory contains dependencies, logs, or secrets.
- You can skip it only when the context directory is already just the files the Dockerfile copies.

**Examples:**

```text
node_modules
npm-debug.log
.env
```

**Sources:**

- [Docker — Dockerfile reference](https://docs.docker.com/reference/dockerfile/)
- [Docker — Building best practices](https://docs.docker.com/build/building/best-practices/)

</details>

<details>
<summary>Why is it advised to run only one process per container?</summary>

### Why is it advised to run only one process per container?

**Definition:**

A container’s lifecycle follows PID 1. `docker stop`, healthchecks, and logs are defined for that process. Several unrelated servers in one container hide which one failed.

**How it works:**

- `HEALTHCHECK` tests one command. If a sidecar process inside the same container dies, the health command may still exit 0.
- `docker logs` shows PID 1’s stdout and stderr. A second process writing to a file is invisible until you redirect it.
- `docker exec` and restart policies act on the container, not on an extra daemon you started beside PID 1.
- Scale by running more containers. Swarm `replicas` and Compose services are the unit of scale, not extra processes in one container.
- A process that must share a network namespace with another is the exception. Prefer two containers on one user-defined bridge.

**Sources:**

- [Docker — Dockerfile HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck)
- [Docker — View container logs](https://docs.docker.com/engine/logging/)
- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>How should you handle logging in Docker containers?</summary>

### How should you handle logging in Docker containers?

**Definition:**

Write application logs to STDOUT and STDERR. `docker logs` and `docker service logs` read those streams.

**How it works:**

- A logging driver can forward those streams to a file, a host, or a database. If dual logging is disabled, `docker logs` may show nothing. Read the driver in that case.
- Official images that were written for files on disk either symlink the log file to `/dev/stdout` and `/dev/stderr` (nginx) or write to `/proc/self/fd/1` and `/proc/self/fd/2` (httpd).
- Do not log secrets. Healthcheck text is stored in `docker inspect` (first 4096 bytes) and is not a substitute for application logs.

**Examples:**

```console
docker logs CONTAINER
docker service logs SERVICE
```

**Sources:**

- [Docker — View container logs](https://docs.docker.com/engine/logging/)

</details>

### Docker Troubleshooting & Optimization

<details>
<summary>How would you diagnose high CPU or memory usage in a container?</summary>

### How would you diagnose high CPU or memory usage in a container?

**Definition:**

Compare what the container is allowed to use with what it is using. Limits come from `docker run` (`--memory`, `--cpus`) or Compose `deploy.resources.limits`. Usage is what you observe while it runs.

**How it works:**

- `docker inspect` shows the host config, including memory and CPU settings you passed.
- `docker stats` prints a live CPU percentage and memory line per running container.
- If the kernel OOM-kills the container, the host was out of memory or the container hit its limit. The resource guide says not to disable the OOM killer unless `--memory` is set. A kill in `docker inspect` state means the limit or the host was too small.
- `--cpus` throttles. The container still runs. It just cannot take more than that fraction.
- Fix the application or raise the limit. Raising the limit without a host that has the RAM brings down other processes. The guide says the daemon’s OOM score is adjusted so a container is more likely to be killed than the daemon.

**Examples:**

```console
docker stats
docker run -it --cpus=".5" --memory="300m" ubuntu /bin/bash
```

**Sources:**

- [Docker — Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)
- [Docker — docker container stats](https://docs.docker.com/reference/cli/docker/container/stats/)

</details>

<details>
<summary>What steps would you take if a container consistently fails shortly after starting?</summary>

### What steps would you take if a container consistently fails shortly after starting?

**Definition:**

Read why PID 1 exited, then the health status if a check is configured. The container is restarting or staying exited because that process stopped.

**How it works:**

- `docker ps -a` includes exited containers. `docker logs CONTAINER` shows STDOUT and STDERR from the failed process.
- `docker inspect` shows `State.ExitCode` and, when a `HEALTHCHECK` exists, the health log. Exit `1` from the health command marks the check unhealthy after `--retries` failures (default 3).
- A paused container is not crashed. `docker exec` fails until you unpause. The exec docs show that error.
- If the process cannot bind a port or resolve a name, check the network. On a user-defined bridge the other container is reached by name. On the default bridge it is not.
- A missing required file or a bad command in `CMD` exits immediately. The Dockerfile `CMD` is PID 1.

**Examples:**

```console
docker logs CONTAINER
docker inspect CONTAINER
```

**Sources:**

- [Docker — View container logs](https://docs.docker.com/engine/logging/)
- [Docker — docker container exec](https://docs.docker.com/reference/cli/docker/container/exec/)
- [Docker — Dockerfile HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck)

</details>

<details>
<summary>In what scenarios would you prune Docker objects, and how?</summary>

### In what scenarios would you prune Docker objects, and how?

**Definition:**

`docker system prune` deletes unused data: stopped containers, networks with no container, dangling images, and unused build cache. Use it when disk is full of leftovers from builds, not as a way to remove a volume you still need.

**How it works:**

- Without flags it asks for confirmation. `-f` / `--force` skips the prompt.
- Volumes are kept unless you pass `--volumes`. The docs say that is so a volume with no container is not deleted by accident. `--volumes` prunes anonymous volumes not used by a container.
- `-a` / `--all` also removes unused images that are not merely dangling. An image with no container is eligible.
- `--filter until=24h` keeps objects newer than that. `--filter label=foo` selects by label. Different filter keys are AND. Repeated keys are OR.
- Build cache, including BuildKit `RUN --mount=type=cache`, is always eligible for `docker system prune`. `docker builder prune` clears only build cache.
- `docker volume prune` removes unused volumes and leaves a volume that a container still references. The volumes guide says a volume is not deleted when its container is removed.

**Examples:**

```console
docker system prune
```

**Sources:**

- [Docker — docker system prune](https://docs.docker.com/reference/cli/docker/system/prune/)
- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)

</details>

<details>
<summary>How can you monitor the performance of Docker containers?</summary>

### How can you monitor the performance of Docker containers?

**Definition:**

`docker stats` streams CPU, memory, and I/O for running containers. Health status and restart counts come from `docker inspect`. Logs come from `docker logs`.

**How it works:**

- Stats are a point in time on that daemon. They are not a long-term metrics store.
- A `HEALTHCHECK` adds `healthy` or `unhealthy` beside the running status and emits `health_status` events.
- Limits you set with `--memory` and `--cpus` are the budget. Stats show whether the container is near that budget.
- For a Swarm service, `docker service logs` aggregates task logs. `docker stack services` shows replica counts, not CPU.

**Examples:**

```console
docker stats
```

**Sources:**

- [Docker — Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)
- [Docker — View container logs](https://docs.docker.com/engine/logging/)
- [Docker — Dockerfile HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck)

</details>

<details>
<summary>What common issues might arise when working with Docker in a CI/CD pipeline?</summary>

### What common issues might arise when working with Docker in a CI/CD pipeline?

**Definition:**

CI builds an image and pushes it. The failures that show up are context size, cache, tags that move, and a swarm that cannot pull what you just built.

**How it works:**

- The build context is everything not excluded by `.dockerignore`. Secrets and `node_modules` in that directory slow the build and can end up in a layer.
- `docker stack deploy` ignores `build`. The pipeline must push the image. Nodes pull it. A tag that only exists on the CI runner is invisible to the swarm.
- `latest` is a moving tag. Pin a version or deploy the digest printed after the push.
- `docker system prune` on a shared runner deletes unused images and build cache. The next job rebuilds more. Do not pass `--volumes` on a runner that stores data you still need.
- Layer cache helps only when the instructions above the change stay the same. A `COPY` of the whole source early in the Dockerfile invalidates every later step.

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Docker — docker system prune](https://docs.docker.com/reference/cli/docker/system/prune/)
- [Docker — Building best practices](https://docs.docker.com/build/building/best-practices/)

</details>

### Docker Deployment & Integration

<details>
<summary>How do you define a multi-container application with `docker-compose`?</summary>

### How do you define a multi-container application with `docker-compose`?

**Definition:**

A Compose file lists services. `docker compose up` creates the containers, and a default network, on one engine. The stack guide’s `compose.yaml` is that file.

**How it works:**

- Each key under `services` is a service. `image` selects an image. `build` builds from a Dockerfile. `ports` publishes. The guide’s web service uses the hostname `redis` because Compose puts both on one network and DNS uses the service name.
- `docker compose up -d` starts them. `docker compose ps` lists them. `docker compose down --volumes` stops them and removes that project’s volumes.
- On an engine that is also in swarm mode, `docker compose up` still runs every container on the current node. The warning in the guide says to use `docker stack deploy` to spread across the swarm, and that command does not understand `build`.
- `deploy.resources` in the Compose spec sets CPU and memory limits. `docker stack deploy` uses the legacy Compose v3 file, not every key in the current spec.

**Examples:**

```yaml
services:
  web:
    image: 127.0.0.1:5000/stackdemo
    build: .
    ports:
      - "8000:8000"
  redis:
    image: redis:alpine
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Docker — Compose deploy](https://docs.docker.com/reference/compose-file/deploy/)

</details>

<details>
<summary>What considerations must be made when deploying Docker containers in a production environment?</summary>

### What considerations must be made when deploying Docker containers in a production environment?

**Definition:**

Production means a registry, pinned images, resource limits, logs on stdout, secrets outside the image, and a healthcheck. A laptop `docker run` has none of those by default.

**How it works:**

- Push the image. Swarm nodes pull. Do not rely on a local build.
- Set `--memory` and `--cpus`, or Compose `deploy.resources.limits`. The minimum memory limit is `6m`.
- Leave the default seccomp profile on. Do not pass `seccomp=unconfined` in production.
- Mount secrets with Swarm secrets (`/run/secrets/<name>`), not `ENV` passwords in the Dockerfile.
- Add a `HEALTHCHECK`. Send logs to stdout. Prune dangling images on the host when disk fills, and do not prune volumes unless you mean to delete that data.
- Several replicas need an overlay and the routing mesh, or another load balancer you operate.

**Sources:**

- [Docker — Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)
- [Docker — Seccomp](https://docs.docker.com/engine/security/seccomp/)
- [Docker — Secrets](https://docs.docker.com/engine/swarm/secrets/)
- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>

<details>
<summary>Explain the process of integrating Docker with a continuous integration system.</summary>

### Explain the process of integrating Docker with a continuous integration system.

**Definition:**

The CI job runs `docker build`, tags the image with the commit or version, and pushes it to a registry. Deploy is a later job that pulls that tag.

**How it works:**

- The daemon must be available to the job. The context is the workspace minus `.dockerignore`.
- `docker compose push` in the stack guide publishes the image the Compose file names. A swarm then `docker stack deploy`s a file whose `image` is that registry name.
- Record the digest from the push. Deploy that, not a floating `latest`.
- Fail the job if the build fails. A healthcheck belongs in the Dockerfile so the orchestrator can restart an unhealthy task after deploy.
- Do not store the registry password in the Dockerfile. Pass it to `docker login` from the CI secret store.

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Docker — Building best practices](https://docs.docker.com/build/building/best-practices/)

</details>

<details>
<summary>How would you migrate a traditional on-premise application to a Dockerized environment?</summary>

### How would you migrate a traditional on-premise application to a Dockerized environment?

**Definition:**

Put the process in an image, move its state to a volume or a database, and put configuration in the environment or a secret. Then run it with the same ports the old host used.

**How it works:**

- One process per container. A machine that ran a web server and a database becomes two services, as in the stack guide’s web and Redis example. They meet by service name on a user-defined bridge or an overlay.
- Data on local disk moves to a volume. A volume outlives the container. A bind mount is only when a host path must stay visible.
- Do not copy passwords into the image. Use a Swarm secret or an env var injected at run time.
- Publish the port the clients already use (`ports: ["8000:8000"]` in the guide).
- Add a `HEALTHCHECK` so a stuck process is visible. Confirm logs are on stdout before you turn off the old host.

**Sources:**

- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker — Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)
- [Docker — Secrets](https://docs.docker.com/engine/swarm/secrets/)

</details>

<details>
<summary>Describe how to automate the deployment of Docker containers using a CI/CD pipeline.</summary>

### Describe how to automate the deployment of Docker containers using a CI/CD pipeline.

**Definition:**

After the image is in the registry, the deploy job runs `docker stack deploy` from a Swarm manager, or `docker compose up` on a single host. The pipeline should not build on the production node.

**How it works:**

- Build and push in CI. The stack guide’s `docker compose push` is that step. `docker stack deploy --compose-file compose.yaml STACK` is the deploy step, and it ignores `build`.
- The Compose file’s `image` must be the registry name the nodes can pull.
- Roll forward by pushing a new tag or digest and deploying again. `docker stack services` shows whether replicas reached the desired count.
- Roll back by deploying the previous digest. Pruning images on the node does not roll back a running service.
- Gate the deploy on tests against the built image. A vulnerability scan such as `docker scout cves` can be one of those gates.

**Examples:**

```console
docker stack deploy --compose-file compose.yaml stackdemo
```

**Sources:**

- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)
- [Docker Scout](https://docs.docker.com/scout/)

</details>

### Docker Storage & Persistence

<details>
<summary>When would you use a bind mount over a volume?</summary>

### When would you use a bind mount over a volume?

**Definition:**

A volume is a directory Docker manages. A bind mount is a host path you choose, mounted into the container. The volumes guide says volumes are the default for persistent data, and bind mounts are for when the host and the container must see the same files.

**How it works:**

- Volumes are easier to back up and migrate, work on Linux and Windows containers, and can be shared by several containers. They do not follow the host’s directory layout.
- Bind mounts depend on the host path and OS. Use one when you are editing source on the host, or when a program outside Docker must read that directory.
- A non-empty volume mounted onto a directory hides the files that were in the image at that path. An empty volume receives a copy of those files unless you set `volume-nocopy`.
- Volumes use `rprivate` propagation. You cannot reconfigure that.

**When to use:**

- Volume for databases and application data.
- Bind mount for a local source tree or a host file you must share.

**Sources:**

- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)

</details>

<details>
<summary>How do you create a Docker volume, and what is its lifecycle?</summary>

### How do you create a Docker volume, and what is its lifecycle?

**Definition:**

`docker volume create` makes a named volume. Docker can also create one when you start a container or a service. The volume stays after the container is removed.

**How it works:**

- The volume is a directory on the host, managed by Docker, and mounted into the container. It is not the container’s writable layer. Writing to the writable layer is slower because of the storage driver’s union filesystem.
- Destroying the container destroys the writable layer, not the volume. A volume with no container is still there until you prune it.
- `docker volume prune` removes unused volumes. `docker system prune` does not, unless you pass `--volumes`, and that flag prunes anonymous volumes.
- An anonymous volume gets a random name. A named volume is the one you pass to `docker volume create`.
- Mounting a non-empty volume hides existing files in that container path. Mounting an empty volume copies the image’s files into the volume unless `volume-nocopy` is set.

**Examples:**

```console
docker volume create app-data
```

**Sources:**

- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker — docker system prune](https://docs.docker.com/reference/cli/docker/system/prune/)

</details>

<details>
<summary>What strategies would you use for data persistence in Docker clusters?</summary>

### What strategies would you use for data persistence in Docker clusters?

**Definition:**

A container’s writable layer is local to that task and dies with it. Cluster data has to live on a volume the replacement task can mount, or in a database that already replicates.

**How it works:**

- Named volumes outlive a container on one node. A Swarm task rescheduled onto another node does not see that node-local volume. Pin the task to the node that has the data, or use a volume driver that is shared storage.
- The volumes guide prefers volumes over bind mounts for data Docker should manage, and bind mounts when a specific host path is required.
- Do not store passwords in the volume’s image layer. Swarm secrets are mounted at `/run/secrets/<name>` and are removed from the node when the task stops. They are not a database.
- `docker system prune --volumes` deletes unused anonymous volumes. Do not run that against the disks that hold the cluster’s data.
- For a single-node Compose project, `docker compose down --volumes` removes the project’s volumes. Leave that flag off when the data must stay.

**Sources:**

- [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/)
- [Docker — Secrets](https://docs.docker.com/engine/swarm/secrets/)
- [Docker — Deploy a stack to a swarm](https://docs.docker.com/engine/swarm/stack-deploy/)

</details>
