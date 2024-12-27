Creating Database;

mysql> create database ecommerce;


mysql> show databases;

| Database           |
| ecommerce          |
| fsdwtb11           |
| information_schema |
| mysql              |
| performance_schema |
| products1          |
| sys                |


---------------------<>-------------------------------------<>---------------------------------------------------------------------------





selecting ecommerce;

mysql> select database();

| database() |

| NULL       |




_____________________________________________________________<>_________________________________________________________________________







mysql> use ecommerce;
Database changed
mysql> select database();
+------------+
| database() |
+------------+
| ecommerce  |
+------------+
\



_____________________________________________________________________________<>_____________________________________________________________


creating tables customers, orders and products;

mysql> use ecommerce;
Database changed
mysql> CREATE TABLE customers (
    ->     id INT AUTO_INCREMENT PRIMARY KEY, 
    ->     name VARCHAR(255) NOT NULL,
    ->     email VARCHAR(255) UNIQUE NOT NULL, 
    ->     address TEXT
    -> );


mysql> select * from customers;


mysql> desc customers;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra         
 |
+---------+--------------+------+-----+---------+----------------+
| id      | int          | NO   | PRI | NULL    | auto_increment |
| name    | varchar(255) | NO   |     | NULL    |               
 |
| email   | varchar(255) | NO   | UNI | NULL    |               
 |
| address | text         | YES  |     | NULL    |               
 |
+---------+--------------+------+-----+---------+----------------+


___________________________________________________________________________<>__________________________________________________________


mysql> CREATE TABLE orders (
    ->     id INT AUTO_INCREMENT PRIMARY KEY,
    ->     customer_id INT NOT NULL,
    ->     order_date DATE NOT NULL, 
    ->     total_amount DECIMAL(10, 2) NOT NULL, 
    ->     CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(id)
    -> );

___________________________________________________________________<>________________________________________________________________________


mysql> desc customers;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra         
 |
+---------+--------------+------+-----+---------+----------------+
| id      | int          | NO   | PRI | NULL    | auto_increment |
| name    | varchar(255) | NO   |     | NULL    |               
 |
| email   | varchar(255) | NO   | UNI | NULL    |               
 |
| address | text         | YES  |     | NULL    |               
 |
+---------+--------------+------+-----+---------+----------------+


mysql> CREATE TABLE products (
    ->     id INT AUTO_INCREMENT PRIMARY KEY, 
    ->     name VARCHAR(255) NOT NULL,
    ->     price DECIMAL(10, 2) NOT NULL, 
    ->     description TEXT
    -> );


mysql> desc products;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int           | NO   | PRI | NULL    | auto_increment |
| name        | varchar(255)  | NO   |     | NULL    |          
      |
| price       | decimal(10,2) | NO   |     | NULL    |          
      |
| description | text          | YES  |     | NULL    |          
      |
+-------------+---------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)
mysql> CREATE TABLE order_items (
    ->     id INT AUTO_INCREMENT PRIMARY KEY,
    ->     order_id INT NOT NULL,
    ->     product_id INT NOT NULL,
    ->     quantity INT NOT NULL,
    ->     FOREIGN KEY (order_id) REFERENCES orders(id),
    ->     FOREIGN KEY (product_id) REFERENCES products(id)
    -> );


________________________________________________________________________<>___________________________________________________________________

mysql> 
mysql> desc order_items;
+------------+------+------+-----+---------+----------------+
| Field      | Type | Null | Key | Default | Extra          |
+------------+------+------+-----+---------+----------------+
| id         | int  | NO   | PRI | NULL    | auto_increment |
| order_id   | int  | NO   | MUL | NULL    |                |
| product_id | int  | NO   | MUL | NULL    |                |
| quantity   | int  | NO   |     | NULL    |                |
+------------+------+------+-----+---------+----------------+

_________________________________________________________________________<>___________________________________________________________________

inserting values;
mysql> INSERT INTO products (name, price, description) VALUES
    -> ('Product A', 20.00, 'Description of Product A'),
    -> ('Product B', 30.00, 'Description of Product B'),
    -> ('Product C', 40.00, 'Description of Product C'),
    -> ('Product D', 50.00, 'Description of Product D'),
    -> ('Product E', 60.00, 'Description of Product E');
Query OK, 5 rows affected (0.01 sec)

_______________________________________________________________________<>______________________________________________________________________

______________________________________________________________________<>_______________________________________________________________________

mysql> INSERT INTO orders (customer_id, order_date, total_amount) VALUES
    -> (1, CURDATE() - INTERVAL 15 DAY, 100.00),
    -> (2, CURDATE() - INTERVAL 10 DAY, 200.00),
    -> (3, CURDATE() - INTERVAL 35 DAY, 50.00),
    -> (4, CURDATE() - INTERVAL 5 DAY, 150.00),
    -> (5, CURDATE() - INTERVAL 25 DAY, 300.00);
Query OK, 5 rows affected (0.01 sec)

_____________________________________________________________________<>________________________________________________________________________



mysql> INSERT INTO order_items (order_id, product_id, quantity) VALUES
    -> (1, 1, 2), -- Order 1 includes 2 units of Product A
    -> (1, 3, 1), -- Order 1 includes 1 unit of Product C
    -> (2, 2, 3), -- Order 2 includes 3 units of Product B
    -> (3, 4, 1), -- Order 3 includes 1 unit of Product D
    -> (4, 5, 2), -- Order 4 includes 2 units of Product E
    -> (5, 1, 1); -- Order 5 includes 1 unit of Product A
Query OK, 6 rows affected (0.01 sec)

__________________________________________________________________________<>___________________________________________________________________

First Query;
 select c.name from customers as c join orders as 0 on c.i

    -> d = 0.customer_id where o.order_date >= CURDATE() - INTERVAL 30
    -> (5, 1, 1); -- Order 5 includes 1 unit of Product A^C     
mysql> SELECT DISTINCT c.name                                   
    -> FROM customers c
    -> JOIN orders o ON c.id = o.customer_id
    -> WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;
+---------------+
| name          |
+---------------+
| Alice Johnson |
| Bob Smith     |
| Diana Prince  |
| Edward Stone  |
+---------------+

______________________________________________________________________<>_______________________________________________________________________

2nd QUERY:
Get the total amount of all orders placed by each customer.

SELECT c.name, SUM(o.total_amount) AS total_spent
    -> FROM customers c
    -> JOIN orders o ON c.id = o.customer_id
    -> GROUP BY c.name;
+---------------+-------------+
| name          | total_spent |
+---------------+-------------+
| Alice Johnson |      100.00 |
| Bob Smith     |      200.00 |
| Charlie Brown |       50.00 |
| Diana Prince  |      150.00 |
| Edward Stone  |      300.00 |
+---------------+-------------+


_____________________________________________________________________________<>________________________________________________________________



3rd Query:
Update the price of Product C to 45.00.

mysql> update products set price = 45.00 where name = "product c
";
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from products;
+----+-----------+-------+--------------------------+
| id | name      | price | description              |
+----+-----------+-------+--------------------------+
|  1 | Product A | 20.00 | Description of Product A |
|  2 | Product B | 30.00 | Description of Product B |
|  3 | Product C | 45.00 | Description of Product C |
|  4 | Product D | 50.00 | Description of Product D |
|  5 | Product E | 60.00 | Description of Product E |
+----+-----------+-------+--------------------------+


_______________________________________________________________________________<>______________________________________________________________



4th Query:
Add a new column discount to the products table.

mysql> alter table products add column discount decimal(5, 2);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc products;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int           | NO   | PRI | NULL    | auto_increment |
| name        | varchar(255)  | NO   |     | NULL    |          
      |
| price       | decimal(10,2) | NO   |     | NULL    |          
      |
| description | text          | YES  |     | NULL    |          
      |
| discount    | decimal(5,2)  | YES  |     | NULL    |          
      |
+-------------+---------------+------+-----+---------+----------------+


_____________________________________________________________________________<>________________________________________________________________





5th Query:
Retrieve the top 3 products with the highest price.


mysql> select price from products order by price desc limit 3;
+-------+
| price |
+-------+
| 60.00 |
| 50.00 |
| 45.00 |
+-------+


____________________________________________________________________________<>________________________________________________________________




6th QUERY:
Get the names of customers who have ordered Product A.

mysql> SELECT DISTINCT c.name
    -> FROM customers c
    -> JOIN orders o ON c.id = o.customer_id
    -> JOIN order_items oi ON o.id = oi.order_id
    -> JOIN products p ON oi.product_id = p.id
    -> WHERE p.name = 'Product A';
+---------------+
| name          |
+---------------+
| Alice Johnson |
| Edward Stone  |
+---------------+


___________________________________________________________________________<>_________________________________________________________________



7th Query:
Join the orders and customers tables to retrieve the customer's name and order date for each order. 

mysql> select customers.name, orders.order_date from customers
    -> join 
    -> orders
    -> on customers.id = orders.id;
+---------------+------------+
| name          | order_date |
+---------------+------------+
| Alice Johnson | 2024-12-12 |
| Bob Smith     | 2024-12-17 |
| Edward Stone  | 2024-12-02 |
| Charlie Brown | 2024-11-22 |
| Diana Prince  | 2024-12-22 |
| Edward Stone  | 2024-12-02 |
+---------------+------------+


____________________________________________________________________________________<>_________________________________________________________



8th QUERY:
Retrieve the orders with a total amount greater than 150.00.

mysql> select * from orders where total_amount > 150.00;
+----+-------------+------------+--------------+
| id | customer_id | order_date | total_amount |
+----+-------------+------------+--------------+
|  2 |           2 | 2024-12-17 |       200.00 |
|  5 |           5 | 2024-12-02 |       300.00 |
+----+-------------+------------+--------------+



________________________________________________________________________________<>_____________________________________________________________


9th QUERY:
Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

mysql> CREATE TABLE order_items (
    ->     id INT AUTO_INCREMENT PRIMARY KEY,
    ->     order_id INT NOT NULL,
    ->     product_id INT NOT NULL,
    ->     quantity INT NOT NULL,
    ->     FOREIGN KEY (order_id) REFERENCES orders(id),        
    ->     FOREIGN KEY (product_id) REFERENCES products(id)     
    -> );



_____________________________________________________________________________<>________________________________________________________________




10th Query:
Retrieve the average total of all orders.

mysql> select AVG(total_amount) as Average_Order_Total  from orders;
+---------------------+
| Average_Order_Total |
+---------------------+
|          160.000000 |
+---------------------+


_______________________________________________________________________<>_____________________________________________________________________