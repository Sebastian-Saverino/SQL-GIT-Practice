# SQL Notes — Subqueries + CTEs

---

# 1) Subqueries in WHERE

## Goal
Filter rows using results from another query.

---

## Subquery in SELECT (Initial Practice)

```sql
SELECT customer_id, amount, (SELECT AVG(amount) FROM payment)
FROM payment;
````

I first started with a subquery in the `SELECT` portion.

---

## Subquery in FROM (Derived Table Concept)

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

## Subquery in WHERE

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

## Thought Process

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

### 2. Comparison with aggregate (e.g., `> AVG()`)

* (blank)

### 3. `NOT IN` Subquery

* (blank)

### 4. Subquery using `GROUP BY`

* (blank)

---

# 2) Subqueries in SELECT

## Goal

Add calculated values per row.

---

## Practice

1. Count-related subquery
2. Sum-related subquery
3. Subquery referencing outer table
4. Multiple subqueries in one `SELECT`

---

# 3) Subqueries in FROM (Derived Tables)

## Goal

Treat a subquery like a temporary table.

---

## Practice

1. Aggregate subquery in `FROM`
2. Filter results from derived table
3. Join a derived table to another table
4. Multiple derived tables in one query

---

# 4) Common Table Expressions (CTEs)

## Goal

Replace complex subqueries with readable steps.

---

## Practice

### 1. Single CTE with aggregation

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

---

### 2. CTE with filtering

This is what I had first above, I was struggling, I had the vision but I couldn't dial it down exactly.
CTEs seem much easier to work with as they spell things out a little more.

**First attempt (what you had):**

```sql
WITH customer_density AS (
    SELECT
        a.address_id,
        COUNT(a.district) AS district_density
    FROM customer AS c
    LEFT JOIN address AS a
        ON a.address_id = c.address_id
    GROUP BY a.address_id
)
SELECT *
FROM customer_density
WHERE district = 'Alberta';
```

**Working version (your corrected one):**

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

---

### 3. CTE joined to another table

* (blank)

---

### 4. Multiple CTEs in one query

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

I put this in a pandas format for python; I found creating CTE is like creating a name variable then working off that.

```python
base = some_dataframe
filtered = base[base["value"] > 10]
print(filtered)
```

---

### 5. CTE used for comparison logic

```sql
WITH rent_cost AS (
    SELECT
        title,
        SUM(rental_rate) AS rent_total
    FROM film AS f
    GROUP BY title
)
SELECT *
FROM rent_cost;
```

```sql
WITH rental_start AS (
    SELECT
        rental_id,
        rental_date,
        return_date
    FROM rental
    GROUP BY rental_id, rental_date, return_date
)
SELECT rental_date - return_date AS rental_differnce
FROM rental_start;
```

---

### Film category + title (CTE)

```sql
WITH film_type AS (
    SELECT c.name, f.title
    FROM film_category AS fc
    RIGHT JOIN category AS c
        ON fc.category_id = c.category_id
    LEFT JOIN film AS f
        ON fc.film_id = f.film_id
)
SELECT *
FROM film_type;
```

I do understand that this can be done without a CTE.
I do find CTEs to be great at showing my logic, it overall makes this process easier.

---

### Best store (favorite query)

```sql
WITH store_totals AS (
    SELECT
        SUM(p.amount) AS total_amount,
        s.store_id
    FROM payment AS p
    JOIN staff AS s
        ON p.staff_id = s.staff_id
    GROUP BY s.store_id
)
SELECT *
FROM store_totals
ORDER BY total_amount DESC
LIMIT 1;
```

This was probably my most favorite query to date.
The first query grabbed the total amount and the store id so I can see which one was which, then it grouped the totals by the store_id.

Then we used the select query to figure out the best store using `LIMIT 1`, which is the same idea of using page + offset pagination.





## #5. EXISTS

Goal: Check for existence of related rows.

Practice:

1. Basic `EXISTS`
2. `NOT EXISTS`
3. EXISTS with join condition
4. EXISTS vs IN comparison queries


SELECT *
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM rental r
    WHERE r.customer_id = c.customer_id
);

SELECT title, length, rating FROM film AS f
WHERE EXISTS (
    SELECT 1
    FROM language as l
    WHERE f.language_id = l.language_id
);

Okay so after some reading EXISTS / NOT EXISTS act like a way does this bring back data or not. it is more performant then using IN and NOT IN.








---

## #6. UNION

Goal: Combine results from multiple queries.

SELECT first_name, 'staff' AS customer_or_staff FROM staff UNION SELECT first_name, 'Customer' AS customer FROM customer;

UNIONS seem simple enough, they kinda ignore the rules of a join and just combine all the specified data, i used the following union to combine all the staff and customers and see who is a staff and who is a customer.


This is my query using UNION ALL it just UNIONs countries and cities, but keeps the duplicates with it being a UNION.

SELECT city_id AS location_id, city AS location_name
FROM city

UNION ALL

SELECT country_id AS location_id, country AS location_name
FROM country;


SELECT staff_id AS ids, first_name 
FROM staff 
WHERE first_name = 'Mike'

UNION

SELECT customer_id, first_name 
FROM customer 
WHERE first_name = 'Mike';


I wanted to see all the Mikes in both the customer and staff tables, so I used the UNION function for this.



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


What is window functions?

These are the idea of performing calculations on a subset of data wihtout losing the level of details of rows.


Understanding group by first makes this a little easier to understand, why to use window functions. Group by aggregates data as we already know so for example we have hats and gloves and we see everytime someone buys one of those, when we aggregate those two things we lose the visual each order and we only see the group by sum up total.

So with a window function you can see each row of data while also seeing that final result as in the sum total


Window functions have the same fucntions as group by but have some other advanced functions as well.

This is an example of a group by SELECT customer_id, SUM(amount) AS total_amount, payment_date FROM payment GROUP BY customer_id, payment_date;


This is my first window function;
SELECT customer_id, amount, SUM(amount)
OVER(PARTITION BY customer_id) AS total_sales
FROM payment;

This query sees each amount the customer spent and there total in a column next to it, using the power of window fucntions.

Window functions are awesome. They're more cool version of group by 

PARTITION BY = GROUP BY but for when you're using a window functions

Some window functions will not have a arg for example the rank function does not use a 

Within a window function you can have

Empty

Column Name ()

Number (2) OVER (ORDER BY pyament_date)

Multiple Arguments LEAD(amount, 2, 10)

Conditional Logic (So case statments)

WINDOW FRAME

This is the idea that a subset of rows within each window

Window functions can be used toegehr with GROUP BY in the same query, only if the same columns are used.

Just use group by for simple aggrations. 

SELECT customer_id, SUM(amount) OVER(ORDER BY payment_date ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING) FROM payment;


This is my using ORDER BY to just order by the payment_date as well as using a window frame

---



This is my most favorite query to date. As i did this with no help or anything just logic. 

SELECT p.customer_id, c.first_name, c.last_name, c.email, SUM(p.amount) AS total_amount, 
RANK() OVER(ORDER BY SUM(p.amount) DESC) AS best_spenders 
FROM payment AS p
LEFT JOIN customer AS c
ON p.customer_id = c.customer_id
GROUP BY p.customer_id, c.first_name, c.last_name, c.email;

The idea is to rank our biggest spenders and to get there email and now we can see who to focus on in order to maintain customer loyalty. Big full circle moment with this query.

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
* [X] Replace subqueries with CTEs
* [X] Use EXISTS and NOT EXISTS
* [X] Combine datasets with UNION
* [ ] Use window functions for totals and rankings
* [ ] Calculate running totals and row differences

---

