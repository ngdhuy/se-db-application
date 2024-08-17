# Export and Import Data

## 1 - Using terminal

### 1.1 - Export database

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

### 1.2 - Import database

* Duplicate script file db_product_script.sql to db_product_new.sql

* Copy file ***db_product_new.sql*** to Docker Volume (MySQL Docker Container) ***/var/lib/mysql/***

```shell
docker cp ./script/db_product_new.sql db-mysql:/var/lib/mysql/
```

* In MySQL Docker Container, ***CREATE DATABASE*** with database name ***db_product_new***

```sql
CREATE DATABASE IF NOT EXISTS `db_product_new`
CHARACTER SET utf16
COLLATE utf16_unicode_ci;
```
* In MySQL Docker Container, execute ***db_product_new.sql*** to immport data to database ***db_product_new***

```shell
mysql -u root -p db_product_new < ./var/lib/mysql/db_product_new.sql
```

* Connect to database to check data after import

```shell
mysql -u root -p
```
imput root password (*admin123*)

* Show all database

```sql
SHOW DATABASES;
```

* Set ***db_product_new*** is current working database

```sql
use db_product_new;
```

* Show all tables in database ***db_product_new***

```sql
SHOW TABLES;
```

* Execute query to check data

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id;
```
