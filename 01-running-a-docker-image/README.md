# Running a Docker Image

You can create and run a Docker container from an image by using the `docker run` command.

## Running a Container Interactively

One of the easiest things to do is to just pull an image of a Linux distro (like Alpine) and run it interactively.  

```bash
docker run -it alpine:3.20.1
```

In this example:

- `-i` tells Docker to run the container interactively
- `-t` allocates a pseudo-TTY
- `alpine:3.20.1` is the tag of the image to run

Together, the result is you will have a shell prompt in the container where you can run commands.  This is useful if you need to check things out in the container or test how different commands work.

## Running a Container With an App

A frequent use case is to containerize an application with Docker.  For example, you can containerize a web application so all of the dependencies it needs are within the container.

This can be demonstrated with the [`docker/getting-started`](https://hub.docker.com/r/docker/getting-started) container from Docker

```bash
docker run -d -p 3333:80 docker/getting-started
```

In this example:

- `-d` tells Docker to run the container in detached mode (the background)
- `-p` provides the port mapping in the form of *\<host-port\>:\<container-port\>*.  In this case, port 80 in the container is being mapped to port 3333 on my localhost.
- `docker/getting-started` is the name of the container to run

After running this command, you can browse to `http://localhost:3333` and you should see a web page of the app that is running inside the Docker container (its a tutorial on Docker).

When you are finished, you will want to shut down the conatiner.  This can be done in the Docker Desktop app or from the command line using the following commands.

First, list the containers that are running on your system using the `docker container ls` command.

```bash
docker container ls
```

You will see output like the following.

```text
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                  NAMES
5e270259f23c   docker/getting-started   "/docker-entrypoint.…"   27 seconds ago   Up 26 seconds   0.0.0.0:3333->80/tcp   serene_poincare
```

Use the `docker stop` command with the container id (the first colum) to stop the container

```bash
docker stop 5e270259f23c
```
