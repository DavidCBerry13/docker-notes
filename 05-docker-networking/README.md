## Docker Networking

When you install Docker (on Windows), three Docker networks are creted by default.  They are:

- **bridge** - A bridge network with a separate IP address space.  This is where containers are placed by default and this network can talk to the internet
- **host** - The container uses teh hosts network directly and therefore has the same IP as the host. Containers are able to talk to the internet
- **none** - An isolated network where containers have no network access.

You can list the docker networks with the `docker network` command.  

```bash
docker network ls
```

To view the configuration of a network (like its IP address range), use the `docker network inspect` command.

```bash
docker network inspect \<network-name\>
```

## Bridge Networks

Typically you are going to be using a bridge network with a docker container.  This will be either the default bridge network (named *bridge*) or a bridge network you create.

- A bridge network will get its own IP range.
- Containers in the same bridge network can route to each other
- Consequently, it often makes sense to create a bridge network per project so containers in the same project can talk to each other but not to containers in other projects
- If you use a user-defined bridge network, then you get DNS resolution between containers in the network.

This last point is important.  In the default bridge network, there is no DNS resolution, so to talk to another container, you need to know the IP address that was assigned to the container.  However, in a user-defined bridge network, the container name becomes the DNS name for the container.  For example, if you have a container named *mongo-database-server*, now in the connection string in the container for your app server, you can use the DNS name *mongo-database-server* in your connection string.  

To create a user-defined bridge network, use the `docker network create` command:

```bash
docker network create my-net
```

Running `docker network create` creates a user-defined bridge network by default with a default IP range.

You can also specify the IP range and other options if you want:

```bash
docker network create --driver=bridge --subnet=172.28.0.0/16 --ip-range=172.28.5.0/24 --gateway=172.28.5.254 my-net
```

To attach a container to a specific network, use the `--network` option in the `docker create` or `docker run` commands.  

```bash
docker run -d -p 3333:80 --network my-net docker/getting-started
```

## IP Address assignment

Each docker network (except `none`) has a DHCP server which will assign a container an IP address automatically to a container.  If you want to assign a specific IP address to a container, use the `--ip` parameter in the `docker create` or `docker run` command (but make sure the IP address is a valid address on the docker network the container will be on)

```bash
docker run -d -p 3333:80 --network my-net --ip 172.28.5.100 docker/getting-started
```
