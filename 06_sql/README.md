# 本周作业（第5次作业）

考虑关系模式`product(product_no, name, price)`，完成下面的题目：

## 题目一（4分）

在数据库中创建该关系，并自建上面关系的txt数据文件：

1. 使用`COPY`命令导入数据库（PostgreSQL）；或使用`LOAD DATA`命令导入数据库（MySQL）。
2. 将该关系导出为任意文件（如SQL、Txt、CSV、JSON等）。
   
CREATE TABLE product (
    product_no INTEGER,
    name VARCHAR(255),
    price NUMERIC(10,2)
);

\copy experiment.public.product FROM 'C:/Users/Air/Desktop/product.txt' DELIMITER ',' CSV
\copy product TO 'C:/Users/Air/Desktop/output.csv' WITH CSV HEADER;

## 题目二（6分）

1. 添加一个新的商品，编号为`666`，名字为`cake`，价格不详。
DELETE FROM product WHERE price > (SELECT AVG(price) FROM product);
2. 使用一条SQL语句同时添加3个商品，内容自拟。
INSERT INTO product (product_no, name, price) VALUES
(667, 'Milk', 4.99),
(668, 'Bread', 2.50),
(669, 'Eggs', 3.00);
3. 将商品价格统一打8折。
UPDATE product SET price = price * 0.8;
4. 将价格大于100的商品上涨2%，其余上涨4%。
UPDATE product
SET price = CASE
    WHEN price > 100 THEN price * 1.02
    ELSE price * 1.04
    END;
5. 将名字包含`cake`的商品删除。
DELETE FROM product WHERE name ILIKE '%cake%'; 
6. 将价格高于平均价格的商品删除。
DELETE FROM product WHERE price > (SELECT AVG(price) FROM product);

## 题目三（5分）

### 针对PostgreSQL

使用参考下面的语句添加10万条商品，

```sql
-- PostgreSQL Only
INSERT INTO product (name, price)
SELECT
    'Product' || generate_series, -- 生成名称 Product1, Product2, ...
    ROUND((random() * 1000)::numeric, 2) -- 生成0到1000之间的随机价格，保留2位小数
FROM generate_series(1, 100000);
```
CREATE TABLE product (
    product_no SERIAL PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(10, 2)
);
INSERT INTO product (name, price)
SELECT
    'Product' || generate_series,
    ROUND((random() * 1000)::numeric, 2)
FROM generate_series(1, 100000);

比较`DELETE`和`TRUNCATE`的性能差异。
DELETE FROM product;
experiment.public> DELETE FROM product
[2025-04-13 16:35:12] 45 ms 中有 100,000 行受到影响

TRUNCATE TABLE product;
experiment.public> TRUNCATE TABLE product
[2025-04-13 16:36:11] 在 14 ms 内完成

TRUNCATE 的性能远高于 DELETE，尤其在处理大量数据时，因 TRUNCATE 直接操作数据页而非逐行处理。
### 针对MySQL

参考`generate_data.py`生成数据，在MySQL比较`LOAD DATA`和[SELECT INTO](https://dev.mysql.com/doc/refman/8.0/en/select-into.html)的性能差异。

