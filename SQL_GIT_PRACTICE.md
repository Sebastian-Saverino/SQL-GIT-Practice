Starting with SELECT queries first

Used \d to view the table contents. This was important to understand the structure of each table before running queries.

Practiced:

SELECT * FROM table;
SELECT column_name1, column_name2 FROM table;


on different tables to get comfortable with pulling specific data.

Used:

SELECT DISTINCT name FROM customer;


to see the unique customer names.

Also tried:

SELECT DISTINCT release_year FROM film;


to see the range of DVD release dates. This dataset is only for 2006.

SELECT DISTINCT amount FROM payment;


to see the different payment values.

Working on WHERE next
SELECT name FROM category WHERE name = 'Action';

SELECT * FROM actor WHERE last_name = 'Wahlberg';


This made me wonder if the data was real or not. Turns out it’s made-up sample data.

SELECT * FROM payment WHERE amount > 10;


I was curious how often people spent more than $10. This follows the idea of low-ticket, high-frequency sales.

SELECT title, rental_duration, rental_rate, replacement_cost
FROM film;


I wanted to better understand the film data using a simple SELECT.

SELECT * FROM film
WHERE rating = 'G' AND language_id = '5';


This was eye-opening because it showed a data issue. There aren’t any foreign films with a maturity rating in this dataset.

Working on ORDER BY next
SELECT amount FROM payment ORDER BY amount;


This showed that some movies were free, or at least some people paid nothing.

SELECT amount FROM payment ORDER BY amount DESC;


I learned that the highest payment was $11.99.

Working on LIMIT and OFFSET
SELECT title FROM film LIMIT 10 OFFSET 2;
SELECT title FROM film LIMIT 5;


Used these to better understand how OFFSET shifts the starting point of results.

Working with aggregates
SELECT MAX(amount) FROM payment;


Similar to using ORDER BY, but faster and cleaner to get the highest value.

SELECT SUM(amount) FROM payment;


This gives the total money made from rentals. Very powerful, since this is the start of real business metrics.

Practiced:

SELECT MIN(length), MAX(length) FROM film;

SELECT title, MAX(length) AS length
FROM film
GROUP BY title
ORDER BY length DESC;


This expands on earlier queries. It shows the longest films by grouping and ordering.
Also highlights the use of aliases, which are very powerful.

First HAVING attempts
SELECT COUNT(language_id), title
FROM film
GROUP BY title
HAVING COUNT(language_id) < 1;


This was what I was trying to write earlier. I finally got the structure right.

SELECT payment_date, AVG(amount)
FROM payment
GROUP BY payment_date
HAVING AVG(amount) > 5;


Worked with HAVING today. It was a bit of a pain, but it was nice to use aggregates more. There’s a lot of power in averages, counts, min, max, and sum.

WHERE vs HAVING example
SELECT COUNT(title)
FROM film
WHERE length > 100;


Used this to see how many films were longer than 100 minutes.
The answer was 610 films.

Counting grouped data
SELECT rating, COUNT(title)
FROM film
GROUP BY rating;


This counts how many movies exist in each rating category from G to NC-17.
HAVING wasn’t even necessary here. This is a case where query optimization matters.

Counting customers
SELECT COUNT(first_name) FROM customer;


I wanted to see how many customers there were, then realized people can share the same name.

Tried:

SELECT DISTINCT first_name FROM customer;


to see the unique first names.
Realized customer_id would be a better counting field, but this still answered the question.

Proper HAVING example
SELECT activebool, COUNT(*) AS customer_activity
FROM customer
GROUP BY activebool
HAVING COUNT(*) > 1;


Big takeaway:
HAVING is best used with aggregates.
It felt most useful when working with the payment table, since aggregates are extremely valuable for averages, counts, min, max, and sum.


OR, NOT, BETWEEN, IN, LIKE, and IS NULL

This is for tommorow, as you see here
These are more logic we can add to our where statments 

Here is an example for tommorow; Before moving to joins, you’re only missing a few **core WHERE-clause tools**. Everything else in your notes is solid.

## Must-know topics before joins

Add these so your fundamentals are complete:

### 1. OR and NOT

You used `AND`, but you should also know:

```sql
SELECT * FROM film
WHERE rating = 'G' OR rating = 'PG';
```

```sql
SELECT * FROM customer
WHERE NOT activebool = true;
```

---

### 2. BETWEEN (ranges)

Used for numeric or date ranges.

```sql
SELECT * FROM payment
WHERE amount BETWEEN 5 AND 10;
```

---

### 3. IN (multiple values)

Cleaner than multiple OR conditions.

```sql
SELECT * FROM film
WHERE rating IN ('G', 'PG', 'PG-13');
```

---

### 4. LIKE (text searching)

For pattern matching.

```sql
SELECT * FROM customer
WHERE first_name LIKE 'A%';
```

Common patterns:

* `'A%'` → starts with A
* `'%son'` → ends with son
* `'_a%'` → second letter is a

---

### 5. NULL handling

Very important in real data.

```sql
SELECT * FROM customer
WHERE email IS NULL;
```

```sql
SELECT * FROM customer
WHERE email IS NOT NULL;
```

---

## That’s it

You already know:

* SELECT
* WHERE
* ORDER BY
* LIMIT
* Aggregates
* GROUP BY
* HAVING

So before joins, just make sure you understand:

**OR, NOT, BETWEEN, IN, LIKE, and IS NULL**

Once those are comfortable, you’re fully ready to move into joins.


