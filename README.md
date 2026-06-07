## what is Docker?
```
Docker is an open-source containerization platform used to package an application along with its dependencies, libraries, and runtime into a portable unit called a container. 
This ensures the application runs consistently across different environments such as development, testing, and production.
```

## what is Dockerfile?
```
A Dockerfile is a text file that contains a set of instructions used to build a Docker image. 
It defines the base image, application code, dependencies, environment variables, and startup commands. 
During the build process, Docker executes these instructions sequentially and creates image layers. Following Docker best practices such as layer optimization, multi-stage builds, and using lightweight base images helps create secure and efficient container images.
```

## What is Docker image?
```
A Docker image is a lightweight, standalone, and immutable package that contains the application code, runtime, libraries, dependencies, and configuration required to run an application. 
It serves as a template from which Docker containers are created, ensuring consistent application execution across different environments.
```

## what is a Docker Container?
```
A Docker container is a running instance of a Docker image. 
When we execute docker run, Docker first checks for the image locally and pulls it from a registry if needed. 
It then creates a container, adds a thin writable layer on top of the read-only image layers, configures networking and storage, and finally executes the CMD or ENTRYPOINT process. 
The container remains running as long as the main process inside it is running.
```

## what happens when you run docker build command?
```
When I run docker build -t image:version ., the Docker daemon reads the Dockerfile and executes the instructions from top to bottom. 
For instructions such as RUN, COPY, and ADD, Docker creates immutable image layers. 
During the build process, Docker uses intermediate containers to execute commands and commits the changes as layers. 
Once all instructions are processed, Docker combines these layers to create the final image and removes the temporary containers. 
If a layer already exists in cache and hasn't changed, Docker reuses it to speed up the build process.
```

## what happens when you run docker run?
```
When I run docker run -d image:version, Docker first checks whether the image exists locally. 
If not, it pulls it from the configured registry such as Docker Hub or ECR. 
Docker then creates a container from the image, adds a writable layer on top of the image layers, configures networking and storage, and starts the process defined in CMD or ENTRYPOINT. 
The container remains running as long as the main process inside the container is running; if that process exits, the container stops.
```

## can we delete the image of a running container?
```
Docker does not allow deleting an image that is being used by a running or stopped container.
The image acts as the parent template for containers, and its layers may be shared across multiple containers.
To remove the image, we must first stop and remove the dependent containers, unless we force-remove the image.
```
```
docker rmi nginx
```
```
Error response from daemon:
conflict: unable to remove repository reference
(image is being used by running container)
```
```
docker rmi -f <image_id>
```