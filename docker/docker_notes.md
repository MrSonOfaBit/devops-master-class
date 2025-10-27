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

For docker you need Hardware or Cloud-Infrastructure, a Host Os and a Docker Engine. Inside the Docker Engine you can run any kind of Container.
- Every container shares the kernel of the Host-System
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

## Build Docker Image
Your Dockerfile will contain everything that docker needs to know, to build a image based on your config. When building a image, docker saves the different steps inside a cache. So when you build your image on different versions, you can optimize your config files structure to build more efficient images.  

### Dockerfile Examples

```Dockerfile
from python:3.12-slim # define base-image
WORKDIR /app # define working directory for the image
COPY . /app # copy local data for the image
RUN pip install -r requirements.txt # execute commands while building the image
EXPOSE 5000 # inform docker which port will be exposed
CMD python ./launch.py # command that is executed at the start of the container
```

Local directory for the example above:
```python
your_image/
├─ Dockerfile # file for building image
├─ launch.py # application code
└─ requirements.txt # requirements for your project
```
Multi-Stage optimized build
```Dockerfile
# Build-Stage
FROM node:20 AS build
WORKDIR /app
COPY package.json /app # copy only package.json so docker can cache this step
RUN npm install
COPY . /app
RUN npm run build

# Finale Image
FROM nginx:stable-alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
This **Multi-Stage** Dockerfile has multiple advantages. 

### Docker Cache

At first the use of the **cache** can speed up the process of building the image because the **dependency installation** will be executed and copied separately from the code. If the code changes but the package.json stays the same, docker uses the cache to skip the installation process and only change the code in the image building. Pushing your image to the docker hub will also take less time if you use the cache efficient.

The **size** of the image will also be reduced because in the first stage the application will be compromised into a **build folder**. The build folder is then used for the image to build and not the whole application code.

## Docker Commands
Hints
> ⓘ When selecting a container/image ID you only have to select the first unique digits to use the container/image

> ⓘ "docker run" is a shortcut for "docker container run" Same shortcut goes for similar commands

Run image inside a docker container
```bash
docker container run <DockerImage>
```
---
Publish Docker container on a host port
```bash
docker run -p 8080:80 <ImageName>:<Tag>
docker run -p 8081:80 <ImageName>:<Tag>
```
The -p or -publish flag is used to link a container port with a port from the host system.
The left port is assigned to the host system and the right one to the container.

> ⓘ Docker makes a NAT-Rule inside a IP-Table. The example above would looks like that:
```bash
Host 0.0.0.0:8080 → 172.17.0.2:80
Host 0.0.0.0:8081 → 172.17.0.3:80
```
---
Run multiple container inside one terminal
```bash
docker run -d -p 8080:80 <ImageName>:<Tag>
docker run -d -p 8081:80 <ImageName>:<Tag>
docker run -d -p 8082:80 <ImageName>:<Tag>
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
> ⓘ Whats the difference between stop and kill container? 
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
docker inspect <ImageID>
```
---
Show detailed container info
```bash
docker container inspect <ContainerID>
```
---
Show stats of a specific container
```bash
docker stats <ContainerID>
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
Deletes all stopped container
```bash
docker container prune <ContainerID>
```
---
Remove image
```bash
docker image remove <ImageName>
```
---
Show docker disk usage
```bash
docker system df
```
> ⓘ Active images from the output means the amount of images that is referenced with a container 
---
Show realtime events from the server
```bash
docker system events
```
---
Display system-wide information
```bash
docker system info
```
---
Remove unused data on the server
```bash
docker system prune
```
---
Remove all unused data on the server
```bash
docker system prune -a
```
> ⓘ WARNING! This will remove 
> - all stopped containers
> - all networks not used by at least one container
> - all images without at least one container associated to them
> - all build cache
---
Assign limited memory to a started container
```bash
docker run -d -p 8080:80 -m 512m <ImageName>:<Tag>
```
---
Assign limited cpu use to a started container
```bash
docker run -d -p 8080:80 --cpu-quota=50000 <ImageName>:<Tag>
```
---
Build Docker Image 
```bash
docker build -t <ImageName>:<Tag> .
```
> ⓘ The . at the end of the command is for the build-context. It tells docker the directory where it can use all files that are defined in the Dockerfile like COPY or ADD

> ⓘ If your not using a tag docker will use the :latest tag

> ⓘ Add your docker ID before the ImageName so you can push it to your docker hub account. Example: 
> 
> ```docker build -t myDockerID/myImage:0.0.1.RELEASE``` 
---
Login to docker hub
```bash
docker login
```
---
After ```docker login``` push image to your docker hub account 
```bash
docker push <ImageName>:<Tag>
```
---