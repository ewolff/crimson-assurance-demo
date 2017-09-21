# How to Run

This is a step-by-step guide how to run the example:

## Installation

* The example runs in Docker Containers. You need to install Docker
  Community Edition, see https://www.docker.com/community-edition/
  . You should be able to run `docker` after the installation.

* The example need a lot of RAM. You should configure Docker to use 4
  GB of RAM. Otherwise Docker containers might be killed due to lack
  of RAM. On Windows and macOS you can find the RAM setting in the
  Docker application under Preferences/ Advanced.

* After installing Docker you should also be able to run
  `docker-compose`. If this is not possible, you might need to install
  it separately. See https://docs.docker.com/compose/install/ .

## Build the containers

First you need to build the Docker images. Change to the directory
`docker` and run `docker-compose build`. This will download some base
images, install software into Docker images and will therefore take
its time:


Change to the directory `crimson-insurance-demo` and run
`docker-compose build`. This will take a while:

```
[~/crimson-insurance-demo]docker-compose build
....
[INFO] Installing /crimson-postbox/pom.xml to /root/.m2/repository/com/innoq/lvm-las-postbox/0.1.0-SNAPSHOT/lvm-las-postbox-0.1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 52.883 s
[INFO] Finished at: 2017-09-19T07:39:42+00:00
[INFO] Final Memory: 29M/209M
[INFO] ------------------------------------------------------------------------
 ---> b091fcc2b55f
Removing intermediate container f1207111f599
Step 22/23 : CMD cd crimson-postbox && mvn spring-boot:run
 ---> Running in 9318bed1fea6
 ---> 8ef39a0f8e37
Removing intermediate container 9318bed1fea6
Step 23/23 : EXPOSE 3001
 ---> Running in f86f143f8972
 ---> 4a4f63a978eb
Removing intermediate container f86f143f8972
Successfully built 4a4f63a978eb
Successfully tagged crimsoninsurancedemo_crimson-postbox:latest
```


Afterwards the Docker images should have been created. They have the prefix
`crimsoninsurancedemo`:

```
[~/crimson-insurance-demo]docker images 
REPOSITORY                                              TAG                 IMAGE ID            CREATED             SIZE
crimsoninsurancedemo_crimson-postbox                    latest              4a4f63a978eb        9 minutes ago       427MB
crimsoninsurancedemo_crimson-portal                     latest              e3f50765a44b        12 minutes ago      207MB
crimsoninsurancedemo_crimson-damage                     latest              59fead419bfd        14 minutes ago      201MB
crimsoninsurancedemo_crimson-letter                     latest              f8d68ef9ad9b        16 minutes ago      203MB
crimsoninsurancedemo_crimson-backend                    latest              0c1ce5a82033        23 minutes ago      86.2MB
```

## Run the containers

Now you can start the containers using `docker-compose up -d`. The
`-d` option means that the containers will be started in the
background and won't output their stdout to the command line:

```
[~/crimson-insurance-demo]docker-compose up -d
crimsoninsurancedemo_crimson-backend_1 is up-to-date
Recreating crimsoninsurancedemo_crimson-portal_1 ...
Recreating crimsoninsurancedemo_crimson-damage_1 ...
Recreating crimsoninsurancedemo_crimson-letter_1 ...
Recreating crimsoninsurancedemo_crimson-portal_1
Recreating crimsoninsurancedemo_crimson-postbox_1 ...
Recreating crimsoninsurancedemo_crimson-damage_1
Recreating crimsoninsurancedemo_crimson-letter_1
Recreating crimsoninsurancedemo_crimson-portal_1 ... done
```

The Docker containers should run on `localhost`. Otherwise set the
environment variable `CRIMSON_SERVER` to the host the containers run
on.

Check wether all containers are running:

```
[~/crimson-insurance-demo]docker ps
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                                                NAMES
c0f6167fd919        crimsoninsurancedemo_crimson-postbox   "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3001->3001/tcp   crimsoninsurancedemo_crimson-postbox_1
b27142edc51f        crimsoninsurancedemo_crimson-letter    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3002->3002/tcp   crimsoninsurancedemo_crimson-letter_1
4da7ff2e8e5d        crimsoninsurancedemo_crimson-damage    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3003->3003/tcp   crimsoninsurancedemo_crimson-damage_1
f2ab6a5e2695        crimsoninsurancedemo_crimson-portal    "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp   crimsoninsurancedemo_crimson-portal_1
5c47a52e0317        crimsoninsurancedemo_crimson-backend   "/bin/sh -c 'cd cr..."   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   crimsoninsurancedemo_crimson-backend_1
```

`docker ps -a`  also shows the terminated Docker containers. That is
useful to see Docker containers that crashed rigth after they started.

If one of the containers is not running, you can look at its logs using
e.g.  `docker logs crimsoninsurancedemo_crimson-letter_1`. The name of the container is
given in the last column of the output of `docker ps`. Looking at the
logs even works after the container has been
terminated. If the log says that the container has been `killed`, you
need to increase the RAM assigned to Docker to e.g. 4GB. On Windows
and macOS you can find the RAM setting in the Docker application under
Preferences/ Advanced.

If you need to do more trouble shooting open a shell in the container
using e.g. `docker exec -it crimsoninsurancedemo_crimson-letter_1 /bin/sh` or execute
command using `docker exec crimsoninsurancedemo_crimson-letter_1 /bin/ls`.

You can now go to http://localhost:3000/ to use the application.

You can terminate all containers using `docker-compose down`.

