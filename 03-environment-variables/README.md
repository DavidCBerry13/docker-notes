# Environment Variables

Many Docker containers allow you to pass in environment variables to configure certain aspects of the container.

An example of this is the [BaGetter NuGet Server](https://www.bagetter.com/docs/Installation/docker).  BaGetter is an open-source NuGet server that can be used as a private repository for NuGet or Chocolatey packages.

If you install BaGetter on a Virtual Machine or other host, you can edit the `appsettings.json` directory to set various configuration parameters.  However, in a containerized environment, you are not able to edit the `appsettings.json` file, so you instead set environment variables to configure BaGetter.

Typically, the documentation for the container will tell you what environment variables are available to configure the container.  For BaGetter, available environment variables include:

| **Environment Variable**     | **Description**                                                                           |
|------------------------------|-------------------------------------------------------------------------------------------|
| `ApiKey`                     | Sets the API Key used for uploading packages to the NuGet (BaGetter) server               |
| `Storage__Type`              | How the actual packages are stored.  `FileSystem` for this example                        |
| `Storage__Path`              | The path where the packages will be stored (and Sqlite database file if `Sqlite` is used) |
| `Database__Type`             | The type of database to use. `Sqlite`, `MySql`, `SqlServer`, `PostgreSql`, `AzureTable`   |
| `Database__ConnectionString` | Connection string for the database engine                                                 |
| `Search__Type`               | Type of search to use when searching packages. `Database` for this example                |

## Setting Environment Variables Individually on the Command Line

We are only going to set the `Storage__Type` and `Storage__Path` varaibles and let the rest use their default values.

If you only have a couple of environment variables to pass to the container, the easiest way is to use the `-e` command line parameter

- Environment varaibles are passed in the form of `VARIABLE_NAME='value'`
- Use a separate `-e` switch for each environment variable

This will start a BaGetter container with its data directory at `/data` (which is in turn mounted to `C:\data\bagetter\bagetter-data` through use of a Docker volume)

```bash
docker run -d --rm --name nuget-server -p 5000:8080 -v "C:\data\bagetter\bagetter-data:/data" -e Storage__Type="FileSystem" -e Storage__Path="/data" bagetter/bagetter:latest
```

Here is what all of these parameters do:

- `-d` - Runs the container in detached (background) mode
- `--rm` - Removes the stopped container once the container is stopped
- `--name nuget-server` - Gives this container the name *nuget-server*
- `-p 5000:8080` - Maps port 8080 in the container to port 5000 on the host (so we can access the server at *http://localhost:5000*)
- `-v "C:\data\bagetter\bagetter-data:/data"` - Creates a volume mount at `/data` on the container to the `C:\data\bagetter\bagetter-data` directory on the host server
- `-e Storage__Type="FileSystem` - Assigns the environment varaible *Storage__Type* the value of *FileSystem*
- `-e Storage__Path="/data"` - Assignes the environment variable *Storage__Path* the value of */data*
- `bagetter/bagetter:latest` - This is the name and version of the container image to use from Docker Hub - See [https://hub.docker.com/r/bagetter/bagetter/tags](https://hub.docker.com/r/bagetter/bagetter/tags)

## Setting Environment Variables using a file

If you have multiple environment variables, it makes more sense to set them in a file using the `--env-file` parameter.  This keeps your command line cleaner and makes sure you don't accidently mistype a value.

It is typical for the file containing the environment variables to have an extension of `.env`.  In this case, we'll name the followng file `bagetter.env`

```text
Storage__Type=FileSystem
Storage__Path=/data
Database__Type=Sqlite
Database__ConnectionString=Data Source=/data/db/bagetter.db
Search__Type=Database
```

Then, provide the path to the `.env` file to the `docker run` command using the `--env-file` parameter

```bash
docker run -d --rm --name nuget-server -p 5000:8080 -v "C:\data\bagetter\bagetter-data:/data" -env-file "C:\data\bagetter\bagetter.env" bagetter/bagetter:latest
```
