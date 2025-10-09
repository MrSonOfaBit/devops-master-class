# Course Docker Notes

- Docker is a software for isolating applications with container virtualization
- Docker Container run by default inside a bridge network. Every container has its own isolated IP address

## Docker Advantage

### Standardized Packaging
Once you pull or create a docker image, you can alway execute the image regardless of the content of the image. There are no requirements or packages that you have to install. Everything the application needs is inside the image.

### Multi Platform Support
Docker is supported on multiple platforms. You run Docker images on your local machine or in Data Centers and Cloud Provider. AWS, Azure and GCP support Docker.

### Light-Weight & Isolation
Virtual Machines are one way to use applications inside a isolated environment. But a VM needs Hardware, Host OS, Hypervisor and for every VM a Client Os, Software and Application is needed. Because of that VM's are Heavy-Weight.

For docker you need Hardware or Cloud-Infrastructure, a Host Os and a Docker Engine. Inside the Docker Engine you can run any kind of Container. E
- very container shares the kernel of the Host-System
- Containers images are made of different layers. If multiple container use the same base-image it will be shared
- Every container could be described as a isolated linux process. You don't need an extra booting process or initializing for every new container 


## Docker Architecture

```
┌────────────────────────────────────┐
│             Docker CLI             │
└────────────────────────────────────┘
               │
               ▼
┌────────────────────────────────────┐
│          Docker Daemon             │
│          (dockerd Service)         │
└────────────────────────────────────┘
               │
               ▼
┌────────────────────────────────────┐
│      Docker Objects (Images,       │
│      Containers, Volumes, ... )    │
└────────────────────────────────────┘
```

### Docker Client 
This component is the interface for the user. Every Command will be send via API and every information will be received by the Docker Daemon.
You can access different Docker Remote Clients too.

### Docker Daemon
This Component will process everything. Build your images, start and stop your containers, use local images or pull images from the docker registry

## Docker Commands
Hints
> When selecting a container/image ID you can only select the first unique digits to use the ID

> "docker run" is a shortcut for "docker container run" Same shortcut goes for similar commands

Run image inside a docker container
```bash
docker container run <DockerImage>
```
---
Publish Docker container on a host port
```bash
docker run -p 8080:80 <imageName>:<Tag>
docker run -p 8081:80 <imageName>:<Tag>
```
The -p or -publish flag is used to link a container port with a port from the host system.
The left port is assigned to the host system and the right one to the container.

> Docker makes a NAT-Rule inside a IP-Table. The example above would looks like that:
```bash
Host 0.0.0.0:8080 → 172.17.0.2:80
Host 0.0.0.0:8081 → 172.17.0.3:80
```
---
Run multiple container inside one terminal
```bash
docker run -d -p 8080:80 <imageName>:<Tag>
docker run -d -p 8081:80 <imageName>:<Tag>
docker run -d -p 8082:80 <imageName>:<Tag>
```
---
See the logs of a container
```bash
docker logs <DockerID>
```
---
Attach container log to your terminal
```bash
docker logs -f <DockerID>
```
Close container
```bash
docker stop <ContainerID>
```
Pause container
```bash
docker pause <ContainerID>
```
Unpause container
```bash
docker unpause <ContainerID>
```
---
Kill container
```bash
docker kill <ContainerID>
```
> Whats the difference between stop and kill container? 
> The command "docker stop" will shut down the container. The application also have the chance to shutdown its connections, end tasks and services before the container goes down.
> The Command "docker kill" will immediately kill the application without giving it the chance to "gracefully shutting down"
---
Show all running container
```bash
docker ps
```
---
Show every launched container
```bash
docker ps -a
```
---
Show detailed image info
```bash
docker inspect <DockerImage>
```
---
Show all local docker images
```bash
docker images
```
---
Pull latest image if no tag is provided from docker registry
```bash
docker pull <ImageName>
```
---
Search images docker registry based on given name
```bash
docker search <ImageName>
```
---
Display every Layer of the image
```bash
docker image history <ImageName>
```
---
Remove container (container needs to be stopped first)
```bash
docker container remove <ContainerID>
```
---
Remove image
```bash
docker image remove <ImageName>
```
---