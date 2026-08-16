<img width="1194" height="758" alt="image" src="https://github.com/user-attachments/assets/aaff5668-00db-4524-ba4f-52a9ffad7f52" /># 🍕 SQL Pizza Sales Analysis

## 📌 Project Overview

This project analyzes pizza sales data using **MySQL** to understand order trends, revenue performance, pizza popularity, product pricing, and category-wise sales.

The project uses four relational tables:

* `orders`
* `order_details`
* `pizzas`
* `pizza_types`

A set of SQL queries was created to answer real-world business questions and extract meaningful insights from the pizza sales data.

- <a href="https://github.com/Shamsher2/SQL-Pizza-Sales-Project/blob/main/Brown%20Yellow%20Modern%20Bold%20Pizza%20Restaurant%20Presentation-compressed.pdf">Sales Pdf<a/>


---

## 🗂️ Dataset Tables

| Table           | Description                                       |
| --------------- | ------------------------------------------------- |
| `orders`        | Contains order ID, order date, and order time     |
| `order_details` | Contains order details ID, order ID, pizza id, and pizza quantities       |
| `pizzas`        | Contains pizza ID, size, price, and pizza type ID |
| `pizza_types`   | Contains pizza name, category, and pizza type ID, ingredients |

---

# 🔍 SQL Questions & Solutions

## 1. Retrieve the total number of orders placed.

```sql
SELECT 
    COUNT(order_id)
FROM
    orders;
```

**Purpose:** Counts the total number of orders placed.

---

## 2. Calculate the total revenue generated from pizza sales.

```sql
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price), 2) AS pizza_sales
FROM
    order_details
JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id;
```

**Purpose:** Calculates total revenue using pizza quantity × pizza price.

---

## 3. Identify the highest-priced pizza.

```sql
SELECT 
    pizza_types.name, 
    pizzas.price
FROM
    pizza_types
JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY 
    pizzas.price DESC
LIMIT 1;
```

**Purpose:** Finds the pizza with the highest selling price.

---

## 4. Identify the most common pizza size ordered.

```sql
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY 
    pizzas.size
ORDER BY 
    order_count DESC
LIMIT 1;
```

**Purpose:** Determines which pizza size is ordered most frequently.

---

## 5. List the Top 5 most ordered pizza types along with their quantities.

```sql
SELECT 
    pizza_types.name, 
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY 
    pizza_types.name
ORDER BY 
    quantity DESC
LIMIT 5;
```

**Purpose:** Identifies the five most popular pizza types based on total quantity ordered.

> **Note:** `SUM(quantity)` is used here because the goal is to calculate the actual number of pizzas ordered.

---

## 6. Find the total quantity of each pizza category ordered.

```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY 
    pizza_types.category
ORDER BY 
    quantity;
```

**Purpose:** Compares the total number of pizzas ordered across different categories.

---

## 7. Determine the distribution of orders by hour of the day.

```sql
SELECT 
    HOUR(order_time) AS hour, 
    COUNT(order_id) AS order_count
FROM
    orders
GROUP BY 
    HOUR(order_time);
```

**Purpose:** Identifies the number of orders placed during each hour of the day.

---

## 8. Find the category-wise distribution of pizzas.

```sql
SELECT 
    category, 
    COUNT(name) AS pizza_count
FROM
    pizza_types
GROUP BY 
    category;
```

**Purpose:** Shows how many pizza types belong to each category.

---

## 9. Calculate the average number of pizzas ordered per day.

```sql
SELECT 
    ROUND(AVG(quantity), 0) AS avg_pizzas_per_day
FROM
    (
        SELECT 
            orders.order_date, 
            SUM(order_details.quantity) AS quantity
        FROM
            orders
        JOIN 
            order_details 
            ON orders.order_id = order_details.order_id
        GROUP BY 
            orders.order_date
    ) AS order_quantity;
```

**Purpose:** First calculates the total pizzas ordered each day, then calculates the average daily quantity.

---

## 10. Determine the Top 3 most ordered pizza types based on revenue.

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY 
    pizza_types.name
ORDER BY 
    revenue DESC
LIMIT 3;
```

**Purpose:** Identifies the three pizza types generating the highest revenue.

---

## 11. Calculate the percentage contribution of each pizza category to total revenue.

```sql
SELECT 
    pizza_types.category,
    ROUND(
        SUM(order_details.quantity * pizzas.price) /
        (
            SELECT 
                SUM(order_details.quantity * pizzas.price)
            FROM
                order_details
            JOIN
                pizzas 
                ON pizzas.pizza_id = order_details.pizza_id
        ) * 100,
        2
    ) AS revenue_percentage
FROM
    pizza_types
JOIN
    pizzas 
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN
    order_details 
    ON order_details.pizza_id = pizzas.pizza_id
GROUP BY 
    pizza_types.category
ORDER BY 
    revenue_percentage DESC;
```

**Purpose:** Calculates how much each pizza category contributes to total revenue.

---

# 📊 Key Insights

The analysis provides insights into:

* 📦 Total number of orders
* 💰 Overall pizza sales revenue
* 🍕 Highest-priced pizza
* 📏 Most popular pizza size
* 🏆 Top 5 most ordered pizza types
* 📂 Category-wise pizza demand
* 🕐 Peak ordering hours
* 📅 Average pizzas ordered per day
* 💵 Top 3 revenue-generating pizza types
* 📈 Revenue contribution by pizza category

---

# 🛠️ Tools & SQL Skills

### Tools

* **MySQL**
* **SQL**

### SQL Concepts Used

* `SELECT`
* `COUNT()`
* `SUM()`
* `AVG()`
* `ROUND()`
* `HOUR()`
* `JOIN`
* `GROUP BY`
* `ORDER BY`
* `LIMIT`
* Subqueries
* Aggregate Functions
* Revenue Calculations
* Percentage Analysis

---

# 🎯 Project Objective

The objective of this project is to demonstrate practical SQL skills by analyzing a relational pizza sales database and answering real-world business questions.

The analysis helps identify **sales performance, popular products, customer ordering patterns, revenue-generating pizzas, and category-level performance**.

---

# 📁 Project Structure

```text
Pizza-Sales-SQL-Analysis/
│
├── orders.csv
├── order_details.csv
├── pizzas.csv
├── pizza_types.csv
│
└── Pizza_Sales_Analysis.sql
```

---

# 🚀 Conclusion

This project demonstrates how SQL can be used to transform raw sales data into meaningful business insights.

By using **joins, aggregate functions, grouping, sorting, subqueries, and calculations**, the project analyzes different aspects of pizza sales and provides data-driven insights for business decision-making.
