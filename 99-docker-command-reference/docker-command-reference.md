# Docker CLI Command Reference

A list of useful commands for the Docker CLI.

## Image Management

| Command                                  | Description                                       | Example                                                  |
|------------------------------------------|---------------------------------------------------|----------------------------------------------------------|
| `docker pull \<image-name\>:\<tag\>`     | Pulls an image from the registry to your machine. If you omit the `\<tag\>`, the latest tag will be pulled | `docker pull ubuntu:latest` |
| `docker images ls`                       | Lists all of the images on your machine. This commmand will also show the size of the images |               |
| `docker image rm \<image-name\>:\<tag\>` | Removes an image from your machine                | `docker image rm ubuntu:latest`                          |
| `docker image rm $(docker image ls -q)`  | Remove all images from your machine               |                                                          |

## Container Management

| Command                                  | Description                                       | Example                                                  |
|------------------------------------------|---------------------------------------------------|----------------------------------------------------------|
| `docker container ls`                    | Lists all *running* containers                    |                                                          |
| `docker container ls -a`                 | Lists all containers, both running and stopped    |                                                          |
| `docker container stats`                 | Displays a live table of running container resource usage statistics |                                       |
| `docker stop \<container-id\>`           | Stops the specified container.  `<container-id`> is the first column in the `docker container ls` output | `docker stop d7dfe2a0604a`  |
| `docker remove \<container-id\>`         | Remove the specified container. `<container-id`> is the first column in the `docker container ls` output | `docker remove d7dfe2a0604a`  |

## Running Docker Images

Containers are run using the [`docker run`](https://docs.docker.com/reference/cli/docker/container/run/) command.  

| Command                                 | Description                                                               | Example                            |
|-----------------------------------------|---------------------------------------------------------------------------|------------------------------------|
| `docker run -it \<image-name\>:\<tag\>` | Run an image interactively. Typically used if you want to land at a command prompt in the image | `docker run -it ubuntu:latest` |
| `docker run -d -p 3333:80 \<image-name\>:\<tag\>` | Run an image in detached (`-d`) mode (in the background).  Typically used when the image is running a service like a web server or the database and you want it to keep running in the background.  The `-p` parameter maps the ports | `docker run -d -p 3333:80 docker/getting-started` |

### Common `docker run` Parameters

You can see all of the parameters by running `docker run --help`.  They can also be viewed in the [`docker run` reference documentation](https://docs.docker.com/reference/cli/docker/container/run/).  This is an abbreviated list of some of the most common.

| Parameter                             | Usage                                                                                                                            |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `-it`                                 | Run the container in interactive mode.  This typically lands you are a command prompt in the container                           |
| `-d`                                  | Run the container in detached mode (in the background).  Typically used with a web server or database or other service           |
| `-p \<host-port\>:\<container-port\>` | Maps the container port to the host port.  For example, `-p 80:8080` would map port 8080 on the container to port 80 on the host |
| `-e NAME=value`                       | Sets and environment variable                                                                                                    |
| `-env-file \<filename\>`              | Reads in a file of environment variables                                                                                         |

## Building and Registering Images

| Command                                                                               | Description                                                                                                          | Example                             |
|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|-------------------------------------|
| `docker build -t \<docker-user-name\>:\<app-name\>:\<version-number\> \<PATH\>`       | Builds and image for the Dockerfile in the specified PATH.  Path is typically passed as `.` (the current directory). | `docker build -t my-docker-image .` |
| `docker push \<registry-host\>/\<app-name\>:\<version-number\>`                       | Push the image to the provided registry host                                                                         |                                     |
