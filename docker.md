## Check docker version:
```
$ docker --version
Docker version 20.10.1, build dea9396
```

## Download Docker image:
With Docker, you can either create or own your images or use images from the repository. In this case, since you’re using a PostgreSQL Docker image, it can be pulled from Docker Hub using the following command:
```
docker pull postgres
```
This command connects you to the Docker Hub and pulls the PostgreSQL image to your machine. By default, Docker pulls the latest image from the Docker Hub.

## Check Installed Docker image:
Once the PostgreSQL Docker image has been pulled from the Docker Hub, you can verify the image by using the following command:
```
$ docker image ls
REPOSITORY                                 TAG             IMAGE ID       CREATED         SIZE
postgres                                   latest          5cd1494671e9   15 hours ago    376MB
```
This command lists all the images that are installed in your local machine.

## Run the PostgreSQL Docker Container:
Now that you have the PostgreSQL Docker image on your machine, you can start the container. As mentioned above, a container is an instance of a Docker image. In order to start the PostgreSQL container, there are a few parameters that you need to provide Docker, which are explained below:

  `–name`: the name of the PostgreSQL container.
  
  `–rm`: this removes the container when it’s stopped.
  
  `-e`: the only mandatory environment variable is the database password that needs to be provided before creating the container.
  
  `-p`: the port mapping needs to be provided so that the host port on the machine will map to the PostgreSQL container port inside the container.
  
```
docker run \
  --name pgsql-dev \
  –rm \
  -e POSTGRES_PASSWORD=test1234 \
  -p 5432:5432 postgres
```
![](https://earthly.dev/blog/assets/images/postgres-docker/G834nrN.png)

As soon as you run the command above, Docker starts the PostgreSQL container for you and makes it available. Once your container is up and running, you can open another terminal window and connect to the PostgreSQL database running inside the container.

Now, open another terminal window and type in the following command:
```
$ docker exec -it pgsql-dev bash
root@6b7f283ad618:/#
```
This command lets you connect to the PostgreSQL CLI running inside the Docker container. Once the interactive terminal is started, you can connect to the PostgreSQL instance with the following command:
```
psql -h localhost -U postgres -p 5432
```
This command connects you to the PostgreSQL database using the default PostgreSQL user. The configurations are as follows:

- `-h` **Hostname**: you need to provide the hostname on which the PostgreSQL Docker container is running. Usually it’s “localhost” if it’s running locally.

- `-U` **Username**: by default, the PostgreSQL image uses a username “postgres” that you can use to log in to the database.

- `-p` **Port**:

You can now run SQL queries against this database as usual:
```
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)

postgres=#
```

# Use a Persistent Volume to Store Data
