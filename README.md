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

## Docker Image Commands
```
docker images --> to see all images
docker build -t <url>/<username>/<image>:<tag> . --> to tag image with name and version
docker push <url>/<username>/<image>:<tag> --> to push image to ECR or Dockerhub
docker pull <url>/<username>/<image>:<tag> --> to pull image
docker login -u <username> -p<password> --> to login to the dockerhub
docker inspect <imagename> --> to inspect image
docker rmi <image/ID> --> to delete images
docker images -q --> to get only the ID's of all images
docker rmi $(docker images -q) --> to delete all images using ID's
docker tag <imageID> <imagename> --> to tag the image after buidling
```

## Container commands
```
docker ps --> to see the list of running containers
docker ps -a --> to see all stopped, exited and running containers
docker create nginx:latest --> to create the container
docker start nginx:latest --> to start the container
docker run nginx --> this will pull image, creates and starts container
docker stop <ID/name> --> to stop container
docker rm nginx --> to remove stopped container
docker rm -f nginx --> to remove running container
docker rm $(docker ps -aq -f status=exited) --> to remove all sxited container
docker run -d --name <container-name> -p <hostport>:<containerport> imagename --> to start container
-d -> detach mode
-p -> port mapping
--name -> to define name for container or else daemon generates random name to container
docker rename <oldname> <newname>
docker restart <ID/name>
docker kill <ID/name>
docker stop <ID/name>
docker exec -it <ID/name> /bin/bash
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
docker rmi -f <image_id> - this will just untag the image but it wont delete image
```

## command to see docker image layers?
```
docker history <image>:<tag> is used to view the image layers, the commands used to create them, and their respective sizes.
```
```
docker history <image>:<tag>

IMAGE          CREATED        CREATED BY                     SIZE
abc123         2 days ago     RUN apt-get install nginx      45MB
def456         2 days ago     COPY app /app                 10MB
ghi789         2 days ago     FROM ubuntu:22.04            77MB
```

## what are dangling images?
```
Dangling images are untagged Docker images that appear as <none>:<none>. 
They are commonly created when an image is rebuilt with the same repository name and tag, causing the old image to lose its reference. 
We can list them using docker images -f dangling=true and 
remove them using docker image prune.
```

## docker system prune
```
docker system prune command is used to delete all dangling images, all dangling build cache, all stopped containers, all networks not used by ateast one container.
```

## How can you copy image from one server to another server?
```
Lets say I want to copy image from one server to another without using repos:
we can use commands to do this:

docker save <Image> -o <file>.tar

since image is not a file, it is collection of multiple image layers. Hence we are zipping the file.
Now we can copy the tar file between servers using WINSCP tool
Once after copied, then we can run

docker load -i <file>.tar

to load the docker image

-i --> stands for input
-0 --> stands for output
```

## Difference between docker stop and docker kill
```
docker stop gracefully stops a container by sending SIGTERM and allowing the application to perform cleanup before shutting down. 
docker kill sends SIGKILL immediately and forcefully terminates the container without allowing cleanup. 
In production, docker stop is preferred, while docker kill is used when a container becomes unresponsive."
```

## Can we create image out of a running container
```
Yes we can create an image from a running container using command
docker commit <ID/name> imagename
```

## can we copy files or dir from host to running container and vice versa?
```
docker cp is used to copy files or directories between a Docker container and the host system. 
It is commonly used to retrieve logs, configuration files, or application artifacts from containers without logging into them.

docker cp <src path> <containerID/name>:<TargetPath> --> copy from Host to Container
docker cp <containerID/name>:<src path> <TargetPath> --> copy from container to host
```