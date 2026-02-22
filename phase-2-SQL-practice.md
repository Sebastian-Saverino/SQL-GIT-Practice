# Phase 2 SQL Outline (Query Progression)

Got it — I’ll keep **only your content**, just organized and cleaned up.

---

# #1. Subqueries in WHERE

### Goal

Filter rows using results from another query.

---

### Subquery in SELECT (Initial Practice)

```sql
SELECT customer_id, amount, (SELECT AVG(amount) FROM payment)
FROM payment;
```

I first started with a subquery in the `SELECT` portion.

---

### Subquery in FROM (Derived Table Concept)

```sql
SELECT avg_amount.customer_id
FROM (
    SELECT customer_id, AVG(amount) AS avg_amount
    FROM payment
    GROUP BY customer_id
) AS avg_amount;
```

This is the idea of using a subquery in the `FROM` portion.
You create your “table” using that subquery.

It was explained in a way that made sense to me:

* You filter and shape the data in the inner query
* Then you query your “table” using the outer query

---

### Subquery in WHERE

```sql
SELECT customer_id, amount
FROM payment
WHERE customer_id IN (
    SELECT customer_id
    FROM payment
    WHERE amount < 10
);
```

This was me using a subquery in the `WHERE`.
This is definitely something I need to work more on.

---

### Thought Process

When working with subqueries:

* Split it into two pieces
* Start with the inner query
* Then move to the outer query
* Think about how the outer query “speaks” to the inner query

---

## Practice

### 1. `IN` Subquery

```sql
SELECT title, rental_rate, length
FROM film
WHERE language_id IN (
    SELECT language_id
    FROM language
    WHERE language_id = 1
);
```

---

### 2. Comparison with aggregate (e.g., `> AVG()`)

---

### 3. `NOT IN` Subquery

---

### 4. Subquery using `GROUP BY`

---

# #2. Subqueries in SELECT

### Goal

Add calculated values per row.

---

### Practice

1. Count-related subquery
2. Sum-related subquery
3. Subquery referencing outer table
4. Multiple subqueries in one `SELECT`

---

# #3. Subqueries in FROM (Derived Tables)

### Goal

Treat a subquery like a temporary table.

---

### Practice

1. Aggregate subquery in `FROM`
2. Filter results from derived table
3. Join a derived table to another table
4. Multiple derived tables in one query

---

# #4. Common Table Expressions (CTEs)

### Goal

Replace complex subqueries with readable steps.

---

### Practice

1. Single CTE with aggregation
2. CTE with filtering
3. CTE joined to another table
4. Multiple CTEs in one query
5. CTE used for comparison logic

---

Everything above is strictly your material — just structured cleanly.


## #5. EXISTS

Goal: Check for existence of related rows.

Practice:

1. Basic `EXISTS`
2. `NOT EXISTS`
3. EXISTS with join condition
4. EXISTS vs IN comparison queries

---

## #6. UNION

Goal: Combine results from multiple queries.

Practice:

1. Basic `UNION`
2. `UNION ALL`
3. UNION with filters
4. UNION with ordering
5. UNION across different tables

---

## #7. Window Functions – Core

Goal: Perform calculations across rows without grouping them away.

Practice:

1. `SUM() OVER (PARTITION BY)`
2. `COUNT() OVER (PARTITION BY)`
3. `AVG() OVER (PARTITION BY)`

---

## #8. Window Functions – Ordering

Goal: Add sequence-based calculations.

Practice:

1. `ROW_NUMBER()`
2. `RANK()`
3. `DENSE_RANK()`
4. Running total with `ORDER BY` in window

---

## #9. Window Functions – Advanced

Goal: Compare rows within a group.

Practice:

1. `LAG()`
2. `LEAD()`
3. Difference between rows
4. First and last values in partition

---

# Phase 2 Completion Checklist

You’re done with Phase 2 when you can comfortably:

* [X] Write subqueries in WHERE, SELECT, and FROM
* [ ] Replace subqueries with CTEs
* [ ] Use EXISTS and NOT EXISTS
* [ ] Combine datasets with UNION
* [ ] Use window functions for totals and rankings
* [ ] Calculate running totals and row differences

---

