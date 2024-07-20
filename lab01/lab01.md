# Lab01 - Setup MySQL with Docker

Reference: https://www.datacamp.com/tutorial/set-up-and-configure-mysql-in-docker

# Step by Step

## Step 01

* Download MySQL Docker

```shell
docker pull mysql:latest
```

* List docker images on pc

```shell
docker images
```

## Step 02

* Running and Managing MySQL Container

```shell
docker run -d --name db-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=admin123 mysql
```

Command description

* __run__ creates a new container or starts an existing one. 
* __-d__ this tag make the container ruing in the background.
* __--name <CONTAINER_NAME>__ setup container name
* __-p <HOST_PORT>:<DOCKER_PORT>__ define the port mapping from Docker Port to Host (PC) Port. In this case is docker port 3306 (default port of mysql) mapped to host port 3307 (real port in pc).
* __-e [ENV_VARIABLE=value]__ the -e tag creates an environment variable. In this case is MYSQL_ROOT_PASSWORD that is root password of MySQL DB.
* __<IMAGE_NAME>__ is the name of image. In this case is ***mysql***


## Step 03

Access to MySQL Container

```shell
docker exec -it db-mysql bash
```

* __exec__ is command to access to container
* __-it__ is:
    
    * __-i__ Keep STDIN open even if not attached
    * __-t__ Allocate a pseudo-TTY

* __CONTAINER_NAME__ is the name of container which is accessed. In this case is ***db-mysql***
* __bash__ is type of terminal

## Step 04
using bash command to connect mysql-command

* Connect to client as ROOT user of MySQL

```shell
mysql -u root -p
```
Enter password (admin123) after enter above command

Now, we can used MySQL command (query) to access database

## Step 05

Configuring the MySQL Container

* We can edit configure of MySQL by access to the directories in MySQL Docker Container:

> /etc/mysql/

> /etc/mysql/conf.d

> /etc/mysql/mysql.conf.d

After that, remove current MySQL Docker Container and create new with command

* Remove current MySQL Docker Container

```shell
docker container rm db-mysql -f
```

* Create host directory configure for MySQl Configure

```shell
mkdir ./mysql-conf
touch ./mysql-conf/my.cnf
```

* Enter the config to apply for MySQL

```shell
vim ./mysql-conf/my.cnf
[mysqld]
max_connections=200 
```

* Create new MySQL Docker Container with configure directory mounted

```shell
dcoker run \
    --name db-mysql \
    -e MYSQL_ROOT_PASSWORD=admin123 \
    -p 3307:3306
    -v ./mysql-conf:/etc/mysql/conf.d \
    -d mysql
```

## Step 06
Advance command docker to preserve the database store in MySQL Docker Container

* Create a volume

```shell
docker volume create db-mysql-data
```

* Create new MySQL container with the volume mounted

```shell
docker run \
    --name db-mysql \
    -e MYSQL_ROOT_PASSWORD=admin123 \
    -p 3307:3306 \
    -v db-mysql-data:/var/lib/mysql \
    -d mysql
```

## FINALLY

```shell
docker run \
    --name db-mysql \
    -e MYSQL_ROOT_PASSWORD=admin123 \
    -p 3307:3306 \
    -v ./mysql-conf:/etc/mysql/conf.d \
    -v db-mysql-new-data:/var/lib/mysql \
    -d mysql
```

* This command will create new volume db-mysql-new-data and map to MySQL Docker Container

