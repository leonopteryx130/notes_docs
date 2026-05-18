# DDL和DML

SQL 按用途可粗分为几类，日常开发里最常见的是 **DDL**（改结构）和 **DML**（改数据）。二者操作对象不同：DDL 动的是「表、库、索引」等元数据；DML 动的是表里的行。

***

### DDL 定义

**DDL**（Data Definition Language，数据定义语言）用来 **创建、修改、删除** 数据库对象（库、表、视图、索引等），描述的是「装数据的容器」，而不是容器里的数据本身。

常见语句：`CREATE`、`ALTER`、`DROP`、`TRUNCATE`、`RENAME` 等。

**示例：**

```sql
-- 建库、建表
CREATE DATABASE shop;
CREATE TABLE user (
  id   INT PRIMARY KEY,
  name VARCHAR(64) NOT NULL
);

-- 改结构、删对象
ALTER TABLE user ADD COLUMN email VARCHAR(128);
DROP TABLE user;
```

***

### DML 定义

**DML**（Data Manipulation Language，数据操作语言）用来对表中的 **行** 进行增、删、改、查，操作的是「数据本身」。

常见语句：`INSERT`、`UPDATE`、`DELETE`、`SELECT`（有的资料把 `SELECT` 单独归为 DQL，但日常口语里也常算在广义的「数据操作」里）。

**示例：**

```sql
INSERT INTO user (id, name) VALUES (1, 'Alice');
UPDATE user SET name = 'Bob' WHERE id = 1;
DELETE FROM user WHERE id = 1;
SELECT id, name FROM user WHERE id = 1;
```

***

### DDL 与 DML 对比

| 维度     | DDL                          | DML                          |
|----------|------------------------------|------------------------------|
| 操作对象 | 库、表、索引、视图等结构     | 表中的数据（行）             |
| 典型语句 | CREATE、ALTER、DROP、TRUNCATE | INSERT、UPDATE、DELETE、SELECT |
| 是否改结构 | 是                         | 否（只改数据）               |

***

### 举例：同一张表上的两类操作

假设已有表 `orders(id, amount)`：

```sql
-- DDL：改「容器」——加一列
ALTER TABLE orders ADD COLUMN status TINYINT DEFAULT 0;

-- DML：改「内容」——写入、更新、查询
INSERT INTO orders (id, amount) VALUES (1001, 99.00);
UPDATE orders SET amount = 88.00 WHERE id = 1001;
SELECT * FROM orders WHERE id = 1001;
```

**总结：** DDL 管「有没有这张表、列长什么样」；DML 管「表里现在有哪些行、值是多少」。
