# Lab03 - Create and Access database on MySQL Docker Container

References: 

## Step 1 

Install Client GUI Tool to access Database

* MySQL Workbench: https://dev.mysql.com/downloads/workbench/
* Azure Data Studio: https://azure.microsoft.com/en-us/products/data-studio
    *   Install Extension:
        * MySQL
        * SQL Database project

## Step 2

Connect to Database with Terminal

### Step 2.1

Start MySQL Docker Container

```shell
docker container start db-mysql
```

Check status of MySQL Docker Container

```shell
docker ps
```

### Step 2.2

Access to MySQL Docker Container

```shell
docker exec -it db-mysql bash
```

### Step 2.3

Connect to Database on MySQL Docker Container

```shell
mysql -u db_user -p
```

using db_user password is ***user_pass***