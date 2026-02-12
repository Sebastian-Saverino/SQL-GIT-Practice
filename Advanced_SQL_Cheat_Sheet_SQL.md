

---

# Advanced SQL Syntax Cheat Sheet

## 1. Subqueries

A **subquery** is a query inside another query.
It’s used to filter, compare, or calculate values dynamically.

### Example: Films longer than the average length

```sql
SELECT title, length
FROM film
WHERE length > (
    SELECT AVG(length)
    FROM film
);
```

**What’s happening**

* Inner query finds the average film length.
* Outer query returns films longer than that value.

---

### Subquery in FROM (derived table)

```sql
SELECT category_id, avg_length
FROM (
    SELECT category_id, AVG(length) AS avg_length
    FROM film
    GROUP BY category_id
) AS category_stats;
```

---

## 2. CASE Statement

A **CASE statement** works like an `if/else` inside SQL.

### Example: Classify films by length

```sql
SELECT 
    title,
    length,
    CASE
        WHEN length < 60 THEN 'Short'
        WHEN length BETWEEN 60 AND 120 THEN 'Medium'
        ELSE 'Long'
    END AS film_length_category
FROM film;
```

**Logic**

* Under 60 minutes → Short
* 60–120 → Medium
* Over 120 → Long

---

## 3. Window Functions

Window functions perform calculations **across rows** without grouping them.

### Example: Rank films by rental rate

```sql
SELECT 
    title,
    rental_rate,
    RANK() OVER (ORDER BY rental_rate DESC) AS rate_rank
FROM film;
```

---

### Example: Running total

```sql
SELECT 
    payment_date,
    amount,
    SUM(amount) OVER (ORDER BY payment_date) AS running_total
FROM payment;
```

---

## 4. Common Table Expressions (CTEs)

A **CTE** is a temporary result set defined with `WITH`.

### Example: Average film length per category

```sql
WITH category_avg AS (
    SELECT category_id, AVG(length) AS avg_length
    FROM film
    GROUP BY category_id
)
SELECT *
FROM category_avg
WHERE avg_length > 100;
```

---

## 5. EXISTS

Checks if a subquery returns any rows.

### Example: Customers who made at least one payment

```sql
SELECT first_name, last_name
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM payment p
    WHERE p.customer_id = c.customer_id
);
```

---

## 6. IN with Subquery

Filters based on values from another query.

### Example: Films in the “Action” category

```sql
SELECT title
FROM film
WHERE film_id IN (
    SELECT film_id
    FROM film_category
    WHERE category_id = (
        SELECT category_id
        FROM category
        WHERE name = 'Action'
    )
);
```

---

## 7. COALESCE (Null Handling)

Replaces `NULL` with a default value.

```sql
SELECT 
    customer_id,
    COALESCE(return_date, 'Not returned') AS return_status
FROM rental;
```

---

## 8. DISTINCT ON (PostgreSQL specific)

Gets one row per group.

```sql
SELECT DISTINCT ON (customer_id)
    customer_id,
    payment_date,
    amount
FROM payment
ORDER BY customer_id, payment_date DESC;
```

Returns the **latest payment per customer**.

---

## 9. FILTER with Aggregates

Conditional aggregation.

```sql
SELECT
    COUNT(*) AS total_payments,
    COUNT(*) FILTER (WHERE amount > 5) AS high_value_payments
FROM payment;
```

---

## Core “Advanced” Concepts to Learn Next

If you’re building your SQL skillset, focus on:

1. Subqueries
2. CASE statements
3. Window functions
4. CTEs
5. EXISTS vs IN
6. Aggregate filters
7. Null handling

These are the **interview-level SQL topics**.

---


