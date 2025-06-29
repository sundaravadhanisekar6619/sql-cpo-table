
create database ecommerce;
### 1. Customers Table


CREATE TABLE customers (
  customer_id INT AUTO_INCREMENT,
  customer_name VARCHAR(30),
  customer_email VARCHAR(30),
  customer_address VARCHAR(50),
  PRIMARY KEY (customer_id)
);


### 2. Orders Table


CREATE TABLE orders (
  order_id INT AUTO_INCREMENT PRIMARY KEY,
  customer_id INT,
  order_date DATE,
  total_amt INT,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);


### 3. Products Table


CREATE TABLE products (
  product_id INT AUTO_INCREMENT PRIMARY KEY,
  product_name VARCHAR(20),
  product_price INT,
  describtion VARCHAR(50)
);


### 4. Order Items Table (for Normalization)


CREATE TABLE order_items (
  id INT AUTO_INCREMENT PRIMARY KEY,
  order_id INT NOT NULL,
  product_id INT NOT NULL,
  quantity INT NOT NULL DEFAULT 1,
  price DECIMAL(10,2) NOT NULL, -- price at time of order
  FOREIGN KEY (order_id) REFERENCES orders(order_id),
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);


---

## 📥 Sample Data Inserts

### Customers

INSERT INTO customers VALUES (1001, 'Raju', 'raju@gmail.com', 'Chennai, TamilNadu');
INSERT INTO customers VALUES (1002, 'Manivel', 'manivel@gmail.com', 'Salem, TamilNadu');
INSERT INTO customers VALUES (1003, 'Priya', 'priya@gmail.com', 'Kovai, TamilNadu');
INSERT INTO customers VALUES (1004, 'Sunitha', 'sunitha@gmail.com', 'Chennai, TamilNadu');
INSERT INTO customers VALUES (1005, 'Vijay', 'vijay@gmail.com', 'Erode, TamilNadu');
```

### Orders


INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (1, 1002, '2025-06-11', 45000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (2, 1005, '2025-06-28', 55000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (3, 1003, '2025-02-05', 15000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (4, 1003, '2025-02-05', 15000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (5, 1004, '2025-11-18', 1000);
INSERT INTO orders (order_id, customer_id, order_date, total_amt) VALUES (6, 1004, '2025-04-17', 100000);
```

### Products


INSERT INTO products VALUES (501, 't-shirt', 100, 'yellow color free size');
INSERT INTO products VALUES (502, 'watch', 1000, 'highest rated watch');
INSERT INTO products VALUES (503, 'toy', 500, 'good quality');
INSERT INTO products VALUES (504, 'washing machine', 50000, 'LG');
INSERT INTO products VALUES (505, 'tv', 20000, 'Mi');




### 1. Customers with Orders in Last 30 Days


SELECT c.customer_name, o.order_date
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;


### 2. Total Order Amount by Customer


SELECT c.customer_name, SUM(o.total_amt) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

### 3. Update Product Price


UPDATE products
SET product_price = 45
WHERE product_id = 503;


### 4. Add Discount Column to Products


ALTER TABLE products
ADD discount DECIMAL(5,2) DEFAULT 0.00;


### 5. Top 3 Most Expensive Products


SELECT *
FROM products
ORDER BY product_price DESC
LIMIT 3;


### 6. Customers Who Ordered Product A (T-shirt)


SELECT DISTINCT c.customer_name, p.product_name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
WHERE p.product_name = 't-shirt';


### 7. Join Orders and Customers


SELECT c.customer_name, o.order_date
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;


### 8. Orders with Total Amount Greater Than 150


SELECT *
FROM orders
WHERE total_amt > 150;


### 9. Normalize Orders Using Order Items

- Already covered in the **Order Items Table** section above.

### 10. Average of All Order Totals


SELECT AVG(total_amt) AS average_order_amount
FROM orders;

