# SQL Assignment: RetailDB_2 Analysis

This repository contains a comprehensive set of SQL queries designed for the **RetailDB_2** database. The assignment covers various SQL concepts including Joins, Subqueries, Aggregations, and Data Filtering to extract meaningful retail insights.

---

## 🛠️ Database Setup
**Database Name:** `RetailDB_2`  
The database consists of tables for `Customers`, `Orders`, `Products`, `Suppliers`, and `Order_Items`.

---

## 📊 Query Solutions

### 1. Basic & Advanced Joins
*   **Inner Join:** Fetch all products along with their supplier names.
    ```sql
    SELECT products.product_name, products.price, suppliers.supplier_name
    FROM products 
    INNER JOIN suppliers ON products.supplier_id = suppliers.supplier_id;
    ```
*   **Left Join:** Find all customers and their orders, including those who haven't placed any.
    ```sql
    SELECT customers.name, orders.order_id, orders.total_amount
    FROM customers
    LEFT JOIN orders ON customers.customer_id = orders.customer_id;
    ```
*   **Right Join:** List all suppliers and their products, including suppliers with no current products.
    ```sql
    SELECT suppliers.supplier_name, suppliers.city, products.product_name 
    FROM products 
    RIGHT JOIN suppliers ON products.supplier_id = suppliers.supplier_id;
    ```
*   **Full Outer Join (Simulation):** Combine Left and Right joins using `UNION` to show all customers and all orders.
    ```sql
    SELECT c.customer_id, c.name, o.order_id, o.order_date FROM customers c LEFT JOIN orders o ON c.customer_id = o.customer_id
    UNION
    SELECT c.customer_id, c.name, o.order_id, o.order_date FROM customers c RIGHT JOIN orders o ON c.customer_id = o.customer_id;
    ```

### 2. Filtering & Conditions
*   **Mumbai Electronics:** Products priced between ₹5,000 and ₹50,000 supplied from Mumbai.
    ```sql
    SELECT p.product_name, p.price, s.supplier_name 
    FROM products p JOIN suppliers s ON p.supplier_id = s.supplier_id 
    WHERE p.price BETWEEN 5000 AND 50000 AND s.city = 'Mumbai';
    ```
*   **Geographic Filtering:** Orders from customers in Mumbai, Delhi, or Bengaluru.
    ```sql
    SELECT o.order_id, o.order_date, c.name, c.city 
    FROM orders o JOIN customers c ON o.customer_id = c.customer_id 
    WHERE c.city IN ('Mumbai', 'Delhi', 'Bengaluru');
    
```

### 3. Aggregations (GROUP BY & HAVING)
*   **Frequent Buyers:** Customers who placed more than 2 orders.
    ```sql
    SELECT customers.customer_id, customers.name, COUNT(orders.order_id) AS total_orders 
    FROM customers JOIN orders ON customers.customer_id = orders.customer_id 
    GROUP BY customers.customer_id, customers.name 
    HAVING COUNT(orders.order_id) > 2;
    ```
*   **Supplier Sales:** Total sales value per supplier.
    ```sql
    SELECT s.supplier_id, s.supplier_name, SUM(oi.quantity * oi.price_each) AS total_sales_value 
    FROM order_items oi 
    JOIN products p ON oi.product_id = p.product_id 
    JOIN suppliers s ON p.supplier_id = s.supplier_id 
    GROUP BY s.supplier_id, s.supplier_name;
    ```
*   **Category Metrics:** Average, highest, and lowest price per product category.
    ```sql
    SELECT category, AVG(price) AS average_price, MAX(price) AS highest_price, MIN(price) AS lowest_price 
    FROM products GROUP BY category;
    ```

### 4. Subqueries & Advanced Analytics
*   **High Value Orders:** Customers whose order amount is greater than the average.
    ```sql
    SELECT DISTINCT name FROM customers JOIN orders ON customers.customer_id = orders.customer_id 
    WHERE total_amount > (SELECT AVG(total_amount) FROM orders);
    ```
*   **Top Spenders:** Top 5 customers by total spending.
    ```sql
    SELECT customer_id, name, SUM(total_amount) AS total_spent 
    FROM customers JOIN orders USING(customer_id) 
    GROUP BY customer_id ORDER BY total_spent DESC LIMIT 5;
    ```
*   **Inventory Gaps:** Products that have never been ordered.
    ```sql
    SELECT product_name FROM products 
    WHERE product_id NOT IN (SELECT product_id FROM order_items);
    ```

---

## 📈 Key Insights Summary
| Metric | Description |
| :--- | :--- |
| **Top Product** | iPhone 15 (₹79,999) |
| **Top Supplier** | Croma Electronics (Highest Sales Value) |
| **Key Regions** | Mumbai, Delhi, Bengaluru |
| **Top Customer** | Ritika Gupta |

---

## 🚀 How to use
1.  Ensure you have **MySQL Workbench** or a similar SQL client installed.
2.  Run the schema creation scripts for `RetailDB_2`.
3.  Copy and paste the queries from this README to perform data analysis.
```
