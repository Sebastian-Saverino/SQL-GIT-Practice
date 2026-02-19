# Phase 2 – Intermediate SQL Logic

Phase 1 was about:

* Pulling data
* Filtering it
* Aggregating it

Phase 2 is about:

> Using SQL to solve layered problems and build logic inside queries.

Core topics:

* Subqueries
* CTEs
* EXISTS
* UNION
* Window functions

---

# 1. Subqueries

## Core idea

A **subquery** is a query inside another query.

It lets you:

* Ask a question
* Then use that answer in another question

---

## Subquery in WHERE

Used to filter based on results from another query.

### Example

Customers who made payments over $10:

```sql
SELECT *
FROM customer
WHERE customer_id IN (
    SELECT customer_id
    FROM payment
    WHERE amount > 10
);
```

### Feynman explanation

1. First query:

   * “Who paid more than $10?”
2. Second query:

   * “Give me those customers.”

---

## Subquery with a comparison

Find films longer than the average length:

```sql
SELECT title, length
FROM film
WHERE length > (
    SELECT AVG(length)
    FROM film
);
```

### Thought process

1. Get average length
2. Compare every film to that number

---

## Subquery in SELECT

Used to add a calculated value per row.

Example:

```sql
SELECT
    first_name,
    last_name,
    (
        SELECT COUNT(*)
        FROM rental r
        WHERE r.customer_id = c.customer_id
    ) AS rental_count
FROM customer c;
```

This adds a rental count to each customer row.

---

## Subquery in FROM (derived table)

This treats a subquery like a temporary table.

Example:

```sql
SELECT
    customer_id,
    total_spent
FROM (
    SELECT
        customer_id,
        SUM(amount) AS total_spent
    FROM payment
    GROUP BY customer_id
) AS spending
WHERE total_spent > 100;
```

---

## Mental model

Subqueries are like:

> “Answer this question first, then use the result.”

---

# 2. Common Table Expressions (CTEs)

## Core idea

A CTE is a **named subquery**.

It makes complex queries:

* Easier to read
* Easier to debug
* More structured

---

## Syntax

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT ...
FROM cte_name;
```

---

## Example

Find customers who spent more than $100.

```sql
WITH customer_spending AS (
    SELECT
        customer_id,
        SUM(amount) AS total_spent
    FROM payment
    GROUP BY customer_id
)
SELECT *
FROM customer_spending
WHERE total_spent > 100;
```

---

## Why CTEs are better than subqueries

Without CTE:

```sql
SELECT *
FROM (
    SELECT ...
) AS temp;
```

With CTE:

```sql
WITH temp AS (...)
SELECT * FROM temp;
```

Cleaner and easier to follow.

---

## Feynman explanation

A CTE is like:

> Giving a name to a step in your thought process.

Instead of:

```
Do this giant nested query
```

You say:

```
Step 1: calculate this
Step 2: use it
```

---

# 3. EXISTS

## Core idea

`EXISTS` checks:

> “Does at least one matching row exist?”

It returns:

* TRUE if a match exists
* FALSE if not

---

## Example

Customers who have rented at least one film:

```sql
SELECT *
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM rental r
    WHERE r.customer_id = c.customer_id
);
```

---

## Why use EXISTS instead of IN

* Often faster
* Stops searching after first match
* Used heavily in real databases

---

## Feynman explanation

`EXISTS` asks:

> “Is there at least one record that matches this condition?”

If yes:

* Keep the row

If no:

* Remove it

---

# 4. UNION and UNION ALL

## Core idea

Used to **combine results from multiple queries**.

---

## UNION

* Combines results
* Removes duplicates

```sql
SELECT first_name FROM customer
UNION
SELECT first_name FROM staff;
```

---

## UNION ALL

* Combines results
* Keeps duplicates
* Faster

```sql
SELECT first_name FROM customer
UNION ALL
SELECT first_name FROM staff;
```

---

## Rules for UNION

Both queries must have:

* Same number of columns
* Same data types

---

## Feynman explanation

UNION is like stacking two lists on top of each other.

UNION:

> Stack lists and remove duplicates.

UNION ALL:

> Stack lists exactly as they are.

---

# 5. Window Functions (Core of Phase 2)

## Core idea

Window functions:

* Perform calculations across rows
* Without grouping away data

They use:

```sql
OVER ()
```

---

## Example: total spending per customer

```sql
SELECT
    customer_id,
    amount,
    SUM(amount) OVER (PARTITION BY customer_id) AS total_spent
FROM payment;
```

---

### What this does

* Groups by customer
* But still shows each payment row

Example output:

| customer_id | amount | total_spent |
| ----------- | ------ | ----------- |
| 1           | 5.99   | 120.50      |
| 1           | 3.99   | 120.50      |
| 1           | 2.99   | 120.50      |

---

## Running total example

```sql
SELECT
    payment_date,
    amount,
    SUM(amount) OVER (
        ORDER BY payment_date
    ) AS running_total
FROM payment;
```

---

## Row numbering

```sql
SELECT
    customer_id,
    amount,
    ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY amount DESC
    ) AS rank
FROM payment;
```

This ranks payments per customer.

---

## Feynman explanation

GROUP BY:

* Combines rows into one result

Window functions:

* Look at the group
* But keep every row

So instead of:

```
1 row per customer
```

You get:

```
Every payment row,
plus customer totals
```

---

# Phase 2 Summary Cheat Sheet

## Subquery

```sql
SELECT *
FROM table
WHERE column IN (
    SELECT column FROM other_table
);
```

---

## CTE

```sql
WITH temp AS (
    SELECT ...
)
SELECT *
FROM temp;
```

---

## EXISTS

```sql
SELECT *
FROM table t
WHERE EXISTS (
    SELECT 1
    FROM other_table o
    WHERE o.id = t.id
);
```

---

## UNION

```sql
SELECT column FROM table1
UNION
SELECT column FROM table2;
```

---

## Window function

```sql
SELECT
    column,
    SUM(column) OVER (PARTITION BY group_column)
FROM table;
```

---

# Mental Shift from Phase 1 to Phase 2

### Phase 1

* Pull data
* Filter
* Aggregate

### Phase 2

* Ask layered questions
* Build logic inside SQL
* Create intermediate result sets
* Perform analytics calculations

