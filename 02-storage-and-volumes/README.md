# Storage and Volumes

Every container gets its own storage space, even if two different containers use the same image.  This means that every container is basically a fresh version of the image.  This can be very useful for testing and other times you want to start from a fresh, known starting point.

There are times though when you do want to persist data in a container.  One common example is running a database in a container.  Running a database in a container is attractive because:

- You do not need to install the database on your local system.  You just pull and run the container, keeping your local system clean
- You can run different versions of a database for different projects.  You avoid issues with multiple versions of the database being installed side by side and potentially conflicting with one another
- Many databases set themselves to start up automatically when the system is booted.  This can slow down bootup times and consume a lot of system resources when the database is not in use.  With Docker, you just run the database when you need it to be running.

This document covers two different ways in which data can be persisted for a container such that it is available on subsequent executions of the container.  This document uses MongoDB as an example, but the concepts apply to any database or any container that needs to persist data.

## Named Containers and Container Storage

When a container is created and run using the `docker run` command, you can specify a name for the container using the `--name` parameter.  If no name is specified, then Docker will automatically generate a name for the container.

In this example, we will create and build a container named `example-mongo` from the `mongo:latest` image.

```bash
docker run -d -p 27017:27017 --name example-mongo mongo:latest
```

Once running, you can connect to this image via MongoDB Compass (the GUI admin tool for Mongo) and add a database, a collection, and some data.

When finished, you can shut down this container instance with the `docker stop` command.  In this case, you can specify the name rather than the container id.

```bash
docker stop example-mongo
```

Now, restart the container with the `docker start` command again specifying the container name with the command

```bash
docker start example-mongo
```

Open up MongoDB Express and you will find that your data is still there.  That is because this data is stored as part of the container, and in this case, you stopped and restarted the same container.

However, if you started a *new* container instance, either by specifying a different name or not specifying a name at all, then that *new* container instance would be empty.  The reason why is because it is a different container (instance), and containers do not share storage.

Also, if you delete the `example-mongo` container, then all of your data is gone and lost forever.

Assigning a container a name and persisting data in that container can be useful for simple development and trying things out, but is not a solution for anything beyond that.  This is where the next technique comes into play.

## Docker Volumes and Bind Mounts

Docker has the ability to mount a local directory from your workstation into the contianer at a specified mount point.  In this way, the files that you want persisted are stored on your local workstation and are not tied to the lifecycle of the container.  Then any container instance can mount these files and use them.

For our database example, this means our database files would be stored outside of our Docker contianer, so we could have access to the same data every time we start the container.

The MongoDB image is configured to store its data in the `/data/db` folder in the image.  By mounting this directory to the `C:\data\docker-mongodb` , our mongodb files will be persisted in that directory.

```bash
docker run -d -p 27017:27017 --name mongo-mount-dir -v "C:\data\docker-mongodb:/data/db" mongo:latest
```

This provides a much more robus solution for persisting data outside of a container.  It also means that now the only thing we need to persist on our system is the data.  The database runtime can all be inside of a container.

## References

- [Mounting folders to a Windows Docker container from a Windows host](https://sarcasticcoder.com/docker/mounting-folders-to-a-windows-docker-container-from-a-windows-host/)
- [How to Run MongoDB in a Docker Container](https://www.howtogeek.com/devops/how-to-run-mongodb-in-a-docker-container/)