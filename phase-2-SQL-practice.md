# Phase 2 SQL Outline (Query Progression)

## #1. Subqueries in WHERE

SELECT customer_id, amount, (SELECT AVG(amount) FROM payment)
FROM payment;

I first started with a subquery in the SELECT portion.

SELECT avg_amount.customer_id
FROM (
    SELECT customer_id, AVG(amount) AS avg_amount
    FROM payment
    GROUP BY customer_id
) AS avg_amount;

This is the idea of using a subquery in the FROM portion, a great way I saw this concept explained was like you create you're table using that subquery like how you filter data with sql then with it being in the FROM portion you can query your "table" you made using your outer query.

SELECT customer_id, amount FROM payment WHERE customer_id IN(SELECT customer_id FROM payment WHERE amount < 10);

This was me using the subquery in the WHERE this is defintely something I'm going to have to work a little bit more on for sure.

When working with subqueries, it helps to split it up into two pieces, so I start with the inner query first, I then move to the outer query. When writing my queries I was thinking how does the outer query "speak" to the inner query

Goal: Filter rows using results from another query.

Practice:

1. `IN` subquery
2. Comparison with aggregate (e.g., `> AVG()`)
3. `NOT IN` subquery
4. Subquery using `GROUP BY`

---

## #2. Subqueries in SELECT

Goal: Add calculated values per row.

Practice:

1. Count-related subquery
2. Sum-related subquery
3. Subquery referencing outer table
4. Multiple subqueries in one SELECT

---

## #3. Subqueries in FROM (Derived Tables)

Goal: Treat a subquery like a temporary table.

Practice:

1. Aggregate subquery in FROM
2. Filter results from derived table
3. Join a derived table to another table
4. Multiple derived tables in one query

---

## #4. Common Table Expressions (CTEs)

Goal: Replace complex subqueries with readable steps.

Practice:

1. Single CTE with aggregation
2. CTE with filtering
3. CTE joined to another table
4. Multiple CTEs in one query
5. CTE used for comparison logic

---

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

