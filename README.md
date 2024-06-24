# docker-notes

A repository containing notes and examples using Docker

## Reasons to use Docker

- You package up exactly what your app needs to run and it can run in any environment
- Run apps in isolation (each app has its exact dependencies)
- You can specify dependenies in the docker file.  Another dev can just pull your project from source and it runs without needing to manually install those dependencies
- Faster to start than a VM.  Containers run on top of the OS instead of running a completely separate copy of the OS.  Therefore containers are more lightweight

## Useful Tutorials

- [Docker in Seven Easy Steps](https://www.youtube.com/watch?v=gAkwW2tuIqE) YouTube video by Fireship IO
- [Docker Full Course](https://www.youtube.com/watch?v=fqMOX6JJhGo) YouTube video by freecodecamp.org
- [Docker Tutorial for Beginners](https://www.youtube.com/watch?v=pTFZFxd4hOI&t=1656s) YouTube video by Mosh
- [Inro to Docker](https://www.youtube.com/watch?v=WcQ3-M4-jik) YouTube video by Tim Corey
- [Reduce WSL 2 Memory Usage](https://blog.nillsf.com/index.php/2021/05/09/reduce-windows-subsystem-for-linux-2-memory-usage/) blog post by NillsF
- [Dev Containers in VS Code are Awesome](https://blog.nillsf.com/index.php/2021/05/19/development-containers-in-visual-studio-code-are-awesome/) blog post by NillsF

## Docker Reference Docs

| **Command**                                                                 | **Description**                                       |
|-----------------------------------------------------------------------------|-------------------------------------------------------|
| [docker build](https://docs.docker.com/reference/cli/docker/image/build/)   | Build an image from a Dockerfile                      |
| [docker init](https://docs.docker.com/reference/cli/docker/init/)           | Creates Docker-related starter files for your project |
| [docker login](https://docs.docker.com/reference/cli/docker/login/)         | Login to a registry                                   |
| [docker pull](https://docs.docker.com/reference/cli/docker/image/pull/)     | Download an image from a registry                     |
| [docker push](https://docs.docker.com/reference/cli/docker/image/push/)     | Upload an image to a registry                         |
| [docker run](https://docs.docker.com/reference/cli/docker/container/run/)   | Create and run a new container from a Docker image    |
| [docker start](https://docs.docker.com/reference/cli/docker/container/run/) | Starts a container with the provided name             |
