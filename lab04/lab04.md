# Lab04 - Basic Query on MySQL

Reference: https://dev.mysql.com/doc/refman/8.4/en/sql-data-manipulation-statements.html

## 1 - Set working database

```sql
USE db_product;
```

## 2 - Insert data to Database

* Insert data to ***tb_categories***

```sql
INSERT INTO tb_categories(id, name) VALUES
(1, 'mobile'),
(2, 'tablet'), 
(3, 'laptop'), 
(4, 'smart watch');
```

* Insert data to ***tb_products***

```sql
INSERT INTO tb_products(id, name, price, cat_id) VALUES
(1, 'iPhone X', 200, 1), 
(2, 'iPhone XS MAX', 300, 1), 
(3, 'iPhone 11 Pro', 350, 1), 
(4, 'iPhone 12 Pro Max', 400, 1), 
(5, 'iPhone 13 Pro Max', 500, 1), 
(6, 'iPhone 14 Pro', 550, 1), 
(7, 'iPhone 15 Pro Max', 600, 1), 
(8, 'iPad gen 10', 400, 2), 
(9, 'iPad Air 4', 250, 2),
(10, 'iPad Pro M1', 500, 2), 
(11, 'Galaxy S24', 1200, 1), 
(12, 'Galaxy Z Fold 6', 2500, 1), 
(13, 'Galaxy Flip 6', 2400, 1), 
(14, 'Galaxy Tab S9', 2000, 2),
(15, 'macBook Pro M2 16 inch', 2500, 3), 
(16, 'macBook Pro M3 14 inch', 2200, 3), 
(17, 'macBook Pro M3 16 inch', 3000, 3), 
(18, 'Apple Watch 9', 500, 4), 
(19, 'Apple Watch Ultra 2', 800, 4), 
(20, 'Galaxy Watch Ultra', 800, 4);
```

## 3 - Query data

* Using WHERE to map data between tb_categories and tb_products by column cat_id

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p, tb_categories c
WHERE p.cat_id = c.id;
```

* Using JOIN command with foreign key relationship

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id;
```

* Limit num of row result from query (skip 5 first rows and get 10 next rows)

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
LIMIT 5, 10;
```

* Get 5 first rows

```sql
SELECT p.id, p.name, p.price, c.name
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
LIMIT 5;
```

* using ***group by*** command to group data

```sql
SELECT c.name, COUNT(p.id) AS num_of_product
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
GROUP BY p.cat_id;
```

* using ***having*** to set condition filter of result data

```sql
SELECT c.name, COUNT(p.id) AS num_of_product
FROM tb_products p LEFT JOIN tb_categories c ON p.cat_id = c.id
GROUP BY p.cat_id
HAVING num_of_product > 5;
```

## 4 - Delete data

* Remove all of products from ***Smart Watch***

```sql 
DELETE FROM tb_products p
WHERE p.cat_id = (SELECT id 
                    FROM tb_categories 
                    WHERE name = 'smart watch'
                );
```

## 5 - Update data

* Rename (update name) of product which name is ***iPhone 12 Pro Max*** to ***"***Galaxy S25 Ultra***

```sql
UPDATE tb_products
SET name = 'Galaxy S25 Ultra'
WHERE name like '%12 Pro Max';
```