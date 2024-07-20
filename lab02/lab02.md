# Lab01 - Access MySQL with command

## Step 01

Start ***db-mysql*** Docker Container

```shell
docker container start db-mysql
```

Or use alias docker command

```shell
docker start db-mysql
```

## Step 02

* Access to MySQl Docker Container

```shell
docker exec -it db-mysql bash
```

## Step 03

* Log into MySQL Database

```shell
mysql -u root -p
```
Enter root-password (admin123) after enter above command

* Create new Database User 

```sql
CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'user_pass';
```

__db_user__ is database user which is used for access to database
__user_pass__ is password of database user

* Set permission (PRIVILEGES) for ***db_user*** to access all databases

```sql
GRANT ALL PRIVILEGES ON *.* TO 'db_user'@'localhost';
```

* List all users on MySQL

```sql
SELECT user, host FROM mysql.user;
```

* Log out MySQL Database (with root user)

```sql
\q
```

* Log into new user database (db_user)

```shell
mysql -u db_user -p
```
Enter db_user password is ***user_pass***

* Show current user information login to MySQL

```sql
SELECT USER();
```
or
```sql
SELECT CURRENT_USER();
```

* List all user on MySQL

```sql
SELECT host, user FROM mysql.user; 
```

## To change user password

* Connect Database with ***root***

```shell
mysql -u root -p
```

Enter root password (admin123)

* Change db_user password to "new_pass"


```sql
ALTER USER 'db_user'@'localhost' IDENTIFIED BY 'new_pass';
```

* Reload Grant Table

```sql
FLUSH PRIVILEGES;
```

* When I want to run MySQL Docker Container

```shell
docker container start db-mysql
```

with __db-mysql__ is docker container name or container ID