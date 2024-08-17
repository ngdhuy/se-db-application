# Export and Import Data

## 1 - Using terminal

* Connect to MySQL Docker Container

```shell
docker exec -it db_product bash
```

* Navigate to directory ***/var/lib/mysql*** (because docker volume is mapped here)

```shell
cd /var/lib/mysql
```

* Dump database to Script SQL file with name db_product_script.sql

```shell
mysqldump -u root -p db_product > db_product_script.sql
```

* Copy script file ***db_product_script.sql*** from docker volume or docker container ***db-mysql-new-data*** to host (PC)

```shell
docker cp db-mysql:/var/lib/mysql/db_product_script.sql ./scripts/db_product_script.sql
```

