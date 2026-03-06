# SQL Notes — Phase 2

These notes are mine, built from my own journey of practicing SQL for one hour every day. I use AI to clean up and organize the writing so it’s easier to digest, but the learning and structure come from me. Every query included here was written using documentation only, unless I explicitly say I used additional resources. If I note “additional resources,” that means I used AI or another tool to help clarify a concept or confirm my understanding.

## Subqueries, CTEs, UNION, EXISTS, Window Functions

---

# Subqueries

## What is a Subquery?

A query inside another query.

Mental model:

* Start with the inner query
* Understand what it returnsgit pu
* Then see how the outer query uses that result

Always split it into two pieces.

---

## A) Subquery in SELECT

### Goal

Add a calculated value per row.

```sql
SELECT 
    customer_id, 
    amount, 
    (SELECT AVG(amount) FROM payment) AS avg_payment
FROM payment;
```

This was my first exposure to subqueries.

This adds the overall average payment next to every row.

---

## B) Subquery in FROM (Derived Table)

### Goal

Create a temporary “table” and query it.

```sql
SELECT avg_amount.customer_id
FROM (
    SELECT 
        customer_id, 
        AVG(amount) AS avg_amount
    FROM payment
    GROUP BY customer_id
) AS avg_amount;
```

This clicked for me.

Inner query:

* Filters
* Aggregates
* Shapes the data

Outer query:

* Talks to that shaped dataset

You build the table first.
Then you query the table.

---

## C) Subquery in WHERE

### Goal

Filter rows using results from another query.

```sql
SELECT customer_id, amount
FROM payment
WHERE customer_id IN (
    SELECT customer_id
    FROM payment
    WHERE amount < 10
);
```

This is still something I want more reps on.

But the structure makes sense:

* Inner query returns a list
* Outer query filters against it

---

## Subquery Practice Types

* `IN`
* `NOT IN`
* Aggregate comparisons (`> AVG()`)
* GROUP BY inside subquery
* Correlated subqueries (next level)

---

# 2️⃣ EXISTS / NOT EXISTS

## Goal

Check if related rows exist.

EXISTS doesn’t care about what’s returned — only that something exists.

```sql
SELECT *
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM rental r
    WHERE r.customer_id = c.customer_id
);
```

This reads like:

> “Give me customers who have at least one rental.”

---

Another example:

```sql
SELECT title, length, rating
FROM film f
WHERE EXISTS (
    SELECT 1
    FROM language l
    WHERE f.language_id = l.language_id
);
```

My understanding:

* EXISTS stops at first match
* Often more performant than IN
* Very clean for relationship checks

---

# 3️⃣ UNION

## Goal

Combine result sets vertically.

Important:

* Must have same number of columns
* Compatible data types
* UNION removes duplicates
* UNION ALL keeps duplicates

---

### Basic UNION

```sql
SELECT first_name, 'staff' AS role
FROM staff

UNION

SELECT first_name, 'customer' AS role
FROM customer;
```

This combines staff and customers into one result.

It ignores join logic.
It just stacks results.

---

### UNION ALL

```sql
SELECT city_id AS location_id, city AS location_name
FROM city

UNION ALL

SELECT country_id AS location_id, country AS location_name
FROM country;
```

Keeps duplicates.

---

### Filtered UNION

```sql
SELECT staff_id AS ids, first_name
FROM staff
WHERE first_name = 'Mike'

UNION

SELECT customer_id, first_name
FROM customer
WHERE first_name = 'Mike';
```

I wanted to see all the Mikes.

Simple.
Clean.
Effective.

---

# 4️⃣ Common Table Expressions (CTEs)

## What is a CTE?

A named temporary result set.

It’s like creating a variable in SQL.

That analogy helped me a lot.

```python
base = some_dataframe
filtered = base[base["value"] > 10]
```

CTE feels like:

```sql
WITH base AS (...),
filtered AS (...)
SELECT * FROM filtered;
```

---

## A) Single CTE

```sql
WITH movie_time AS (
    SELECT
        film_id,
        AVG(length) AS avg_movie_length
    FROM film
    GROUP BY film_id
)
SELECT *
FROM movie_time
WHERE avg_movie_length > 90;
```

Much easier to read than nesting.

---

## B) CTE With Join + Filtering

**Corrected version:**

```sql
WITH customer_density AS (
    SELECT
        a.address_id,
        a.district,
        COUNT(c.customer_id) AS address_density
    FROM address a
    LEFT JOIN customer c
        ON a.address_id = c.address_id
    GROUP BY a.address_id, a.district
)
SELECT *
FROM customer_density
WHERE district = 'Alberta';
```

CTEs make the logic visible.

---

## C) Multiple CTEs

```sql
WITH customer_density AS (
    SELECT
        a.district,
        COUNT(c.customer_id) AS district_density
    FROM customer c
    JOIN address a
        ON c.address_id = a.address_id
    GROUP BY a.district
),
alberta_only AS (
    SELECT *
    FROM customer_density
    WHERE district = 'Alberta'
)
SELECT *
FROM alberta_only;
```

You build logic step-by-step.

Much cleaner than stacking nested subqueries.

---

## D) Favorite CTE — Best Store

```sql
WITH store_totals AS (
    SELECT
        SUM(p.amount) AS total_amount,
        s.store_id
    FROM payment p
    JOIN staff s
        ON p.staff_id = s.staff_id
    GROUP BY s.store_id
)
SELECT *
FROM store_totals
ORDER BY total_amount DESC
LIMIT 1;
```

This one felt great.

* Aggregate
* Group
* Then rank

Simple logic. Big insight.

---

# 5️⃣ Window Functions

## What Are Window Functions?

Perform calculations across rows
WITHOUT collapsing them like GROUP BY does.

That’s the key difference.

GROUP BY removes row-level detail.
Window functions keep it.

---

## A) Basic Window Example

```sql
SELECT 
    customer_id, 
    amount, 
    SUM(amount) OVER (PARTITION BY customer_id) AS total_sales
FROM payment;
```

This shows:

* Each purchase
* AND total per customer

That was a big “click” moment.

---

## PARTITION BY

PARTITION BY ≈ GROUP BY
But only for the window function.

It defines the subset of rows.

---

## B) Ranking Biggest Spenders

My favorite query so far:

```sql
SELECT 
    p.customer_id,
    c.first_name,
    c.last_name,
    c.email,
    SUM(p.amount) AS total_amount,
    RANK() OVER(ORDER BY SUM(p.amount) DESC) AS best_spenders
FROM payment p
LEFT JOIN customer c
    ON p.customer_id = c.customer_id
GROUP BY 
    p.customer_id,
    c.first_name,
    c.last_name,
    c.email;
```

This was a full-circle moment.

We:

* Aggregated
* Ranked
* Pulled contact info

Now we know who to focus on for customer loyalty.

---

## C) Monthly Purchases

```sql
SELECT 
    COUNT(customer_id) AS customer_purchases,
    EXTRACT(MONTH FROM payment_date) AS payment_month
FROM payment
GROUP BY payment_month
ORDER BY customer_purchases DESC;
```

This was a 2-minute query.

That was the moment I felt natural fluency.

---

## D) Monthly Revenue + LEAD

```sql
SELECT 
    EXTRACT(MONTH FROM payment_date) AS payment_month,
    SUM(amount) AS total_sales,
    LEAD(SUM(amount), 1) OVER(
        ORDER BY EXTRACT(MONTH FROM payment_date)
    ) AS next_months_sales
FROM payment
GROUP BY payment_month;
```

This was instinctual.

The goal:
Compare month-to-month change.

That’s when window functions really started making sense.

---

## E) ROW_NUMBER()

```sql
SELECT 
    customer_id,
    payment_date,
    amount,
    ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY payment_date
    ) AS purchase_number
FROM payment;
```

This acts like a temporary index per customer.

It sequences purchases.

Very powerful for behavioral analysis.

---

## F) FIRST_VALUE()

```sql
SELECT 
    title,
    replacement_cost,
    FIRST_VALUE(replacement_cost) 
        OVER (ORDER BY replacement_cost) AS cheapest_price
FROM film;
```

This compares each row against the minimum replacement cost.

Good introduction to value comparison windows.

---

# Window Function Concepts

You can include:

* PARTITION BY
* ORDER BY
* Window frames
* Multiple arguments (e.g., LEAD(amount, 2, 10))
* CASE logic
* Ranking functions

Window frame example:

```sql
SELECT 
    customer_id,
    SUM(amount) OVER (
        ORDER BY payment_date
        ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING
    )
FROM payment;
```

Defines a subset within the window.

---

# Phase 2 Completion Checklist

I can now comfortably:

* [x] Write subqueries in WHERE, SELECT, and FROM
* [x] Replace subqueries with CTEs
* [x] Use EXISTS and NOT EXISTS
* [x] Combine datasets with UNION
* [x] Use window functions for totals and rankings
* [x] Calculate running totals and row differences

---

You didn’t just learn syntax.

You learned:

* Query layering
* Logical execution order
* Aggregation vs window logic
* Relationship filtering
* Sequence-based analytics

This is real SQL fluency.

Phase 2: Clean.
