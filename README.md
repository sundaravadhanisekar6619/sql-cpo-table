# sql-cpo-table

CREATE TABLE customers(
customer_id INT AUTO_INCREMENT,
customer_name VARCHAR(30),
customer_email VARCHAR(30),
customer_address VARCHAR(50),
PRIMARY KEY (customer_id)

);

//to insert records into customers database

INSERT INTO customers VALUES(1001,"Raju","raju@gmail.com","chennai,TamilNadu");
INSERT INTO customers VALUES(1002,"Manivel","manivel@gmail.com","salem,TamilNadu");
INSERT INTO customers VALUES(1003,"Priya","priya@gmail.com","Kovai,TamilNadu");
INSERT INTO customers VALUES(1004,"Sunitha","sunitha@gmail.com","chennai,TamilNadu");
INSERT INTO customers VALUES(1005,"vijay","vijay@gmail.com","Erode,TamilNadu");

//TO CREATE ORDER TABLE
CREATE TABLE orders(
order_id INT AUTO_INCREMENT PRIMARY KEY,
customer_id INT ,
order_date DATE,
total_amt INT ,
FOREIGN KEY (customer_id) REFERENCES customers(customer_id)

);

//to insert records into order table
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (1, 1002, '2025-06-11', 45000)
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (2, 1005, '2025-06-28', 55000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (3, 1003, '2025-02-05', 15000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (4, 1003, '2025-02-05', 15000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (5, 1004, '2025-11-18', 1000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt)
VALUES (6, 1004, '2025-04-17', 100000);


//To create products table

CREATE TABLE products(
product_id INT AUTO_INCREMENT PRIMARY KEY,
product_name VARCHAR(20) ,
product_price INT,
describtion VARCHAR(50) 

);

//to insert records into database
INSERT INTO products
VALUES (501, "t-shirt", 100, "yellow color free size");
INSERT INTO products
VALUES (502, "watch", 1000, "highest rated watch");
INSERT INTO products
VALUES (503, "toy", 500, "good quality");
INSERT INTO products
VALUES (504, "washing machine", 50000, "LG");
INSERT INTO products
VALUES (505, "tv", 20000, "Mi");



--1) Retrieve all customers who have placed an order in the last 30 days.
SELECT c.customer_name,o.order_date
FROM customers c JOIN orders o
ON c.customer_id = o.customer_id
WHERE order_date >= CURDATE() - INTERVAL 30 DAY;



-- 2) Get the total amount of all orders placed by each customer.

SELECT c.customer_name,SUM(o.total_amt)
FROM customers c join orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id,c.customer_name;


--3) Update the price of Product C to 45.00.
UPDATE products
SET product_price = 45
WHERE product_id=503;

--4)Add a new column discount to the products table.
ALTER TABLE products
ADD discount DECIMAL(5,2) DEFAULT 0.00;


5)--Retrieve the top 3 products with the highest price.
SELECT * 
FROM PRODUCTS
ORDER BY product_price DESC
LIMIT  3;

6)--Get the names of customers who have ordered Product A.
SELECT c.customer_name,p.product_name
FROM customer c JOIN products p
on c.customer_id = p.customer_id
WHERE p.product_name = "t-shirt"

7)--Join the orders and customers tables to retrieve the customer's name and order date for each order.
SELECT c.customer_name AS customer_name, o.order_date
FROM 
customers c join orders o
ON c.customer_id = o.customer_id


8)--Retrieve the orders with a total amount greater than 150.00.
select * 
from orders
where total_amt > 150;

9)--Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.
//to create new table called order_items
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    price DECIMAL(10,2) NOT NULL,  -- price at time of order
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

ALTER TABLE orders
ADD order_item_id INT,
ADD FOREIGN KEY (order_item_id) REFERENCES order_items(id);

10)--Retrieve the average total of all orders.
select AVG(total_amt) AS average_of_all_products
from orders;
