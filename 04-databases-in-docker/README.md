# Databases in Docker

This page encapsulates how to run different databases in Docker since that can be an important use case, standing up a database in a container to do some testing or application development.

- Each example will use a container name of `my-project-db-name`
- Data will be stored in a directory named `C:\docker-data\my-project\db-name`

This might be the way you would orgnize things on your own laptop if say for example, you had three different projects you were using MongoDB for and wanted to keep the containers separate (though you would also need to deal with the port numbers if you wanted to run them at the same time).

## MongoDB

MongoDB official image page at Docker Hub --> [https://hub.docker.com/_/mongo](https://hub.docker.com/_/mongo)

```bash
docker run -d -p 27017:27017 --name my-project-mongodb -v "C:\docker-data\my-project\mongodb:/data/db" mongo:latest
```

## MySQL

MySQL official image page at Docker Hub --> [https://hub.docker.com/_/mysql](https://hub.docker.com/_/mysql)

```bash
docker run -d -p 3306:3306 --name my-project-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -v "C:\docker-data\my-project\mysql:/var/lib/mysql" mysql:latest
```

## PostreSQL

Postgres official image page at Docker Hub --> [https://hub.docker.com/_/postgres](https://hub.docker.com/_/postgres)

```bash
docker run -d -p 5432:5432 --name my-project-postgres -e POSTGRES_PASSWORD=mysecretpassword -e PGDATA=/var/lib/postgresql/data/pgdata -v "C:\docker-data\my-project:/var/lib/postgresql/data" postgres
```

## Microsoft SQL Server

Micrsoft Learn Article [Configure and customize SQL Server Linux containers](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-docker-container-configure?view=sql-server-ver16&pivots=cs1-powershell)

```bash
docker run -d -p 1433:1433 --name my-project-mssql -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<password>" -v "C:\docker-data\my-project\data:/var/opt/mssql/data" -v "C:\docker-data\my-project\log:/var/opt/mssql/log" -v "C:\docker-data\my-project\secrets:/var/opt/mssql/secrets" mcr.microsoft.com/mssql/server:2022-latest
```
