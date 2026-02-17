# SELECT Fundamentals Progression

## Starting with SELECT queries

I started with basic SELECT queries first.

Used `\d` to view the table contents.
This was important to understand the structure of each table before running queries.

Practiced:

```sql
SELECT * FROM table;

SELECT column_name1, column_name2
FROM table;
```

On different tables to get comfortable with pulling specific data.

---

## Working with DISTINCT

Used:

```sql
SELECT DISTINCT name FROM customer;
```

to see the unique customer names.

Also tried:

```sql
SELECT DISTINCT release_year FROM film;
```

to see the range of DVD release dates.
This dataset is only for 2006.

```sql
SELECT DISTINCT amount FROM payment;
```

to see the different payment values.

---

## Moving into WHERE

Working on WHERE next.

```sql
SELECT name
FROM category
WHERE name = 'Action';
```

```sql
SELECT *
FROM actor
WHERE last_name = 'Wahlberg';
```

This made me wonder if the data was real or not.
Turns out it’s made-up sample data.

```sql
SELECT *
FROM payment
WHERE amount > 10;
```

I was curious how often people spent more than $10.
This follows the idea of low-ticket, high-frequency sales.

```sql
SELECT title, rental_duration, rental_rate, replacement_cost
FROM film;
```

I wanted to better understand the film data using a simple SELECT.

```sql
SELECT *
FROM film
WHERE rating = 'G' AND language_id = '5';
```

This was eye-opening because it showed a data issue.
There aren’t any foreign films with a maturity rating in this dataset.

---

## Working on ORDER BY

```sql
SELECT amount
FROM payment
ORDER BY amount;
```

This showed that some movies were free, or at least some people paid nothing.

```sql
SELECT amount
FROM payment
ORDER BY amount DESC;
```

I learned that the highest payment was $11.99.

---

## Working on LIMIT and OFFSET

```sql
SELECT title FROM film LIMIT 10 OFFSET 2;
SELECT title FROM film LIMIT 5;
```

Used these to better understand how OFFSET shifts the starting point of results.

---

## Working with aggregates

```sql
SELECT MAX(amount) FROM payment;
```

Similar to using ORDER BY, but faster and cleaner to get the highest value.

```sql
SELECT SUM(amount) FROM payment;
```

This gives the total money made from rentals.
Very powerful, since this is the start of real business metrics.

Practiced:

```sql
SELECT MIN(length), MAX(length)
FROM film;
```

```sql
SELECT title, MAX(length) AS length
FROM film
GROUP BY title
ORDER BY length DESC;
```

This expands on earlier queries.
It shows the longest films by grouping and ordering.
Also highlights the use of aliases, which are very powerful.

---

## First HAVING attempts

```sql
SELECT COUNT(language_id), title
FROM film
GROUP BY title
HAVING COUNT(language_id) < 1;
```

This was what I was trying to write earlier.
I finally got the structure right.

```sql
SELECT payment_date, AVG(amount)
FROM payment
GROUP BY payment_date
HAVING AVG(amount) > 5;
```

Worked with HAVING today.
It was a bit of a pain, but it was nice to use aggregates more.
There’s a lot of power in averages, counts, min, max, and sum.

---

## WHERE vs HAVING example

```sql
SELECT COUNT(title)
FROM film
WHERE length > 100;
```

Used this to see how many films were longer than 100 minutes.
The answer was 610 films.

---

## Counting grouped data

```sql
SELECT rating, COUNT(title)
FROM film
GROUP BY rating;
```

This counts how many movies exist in each rating category from G to NC-17.
HAVING wasn’t even necessary here.
This is a case where query optimization matters.

---

## Counting customers

```sql
SELECT COUNT(first_name) FROM customer;
```

I wanted to see how many customers there were, then realized people can share the same name.

Tried:

```sql
SELECT DISTINCT first_name FROM customer;
```

to see the unique first names.
Realized `customer_id` would be a better counting field, but this still answered the question.

---

## Proper HAVING example

```sql
SELECT activebool, COUNT(*) AS customer_activity
FROM customer
GROUP BY activebool
HAVING COUNT(*) > 1;
```

Big takeaway:
HAVING is best used with aggregates.

It felt most useful when working with the payment table, since aggregates are extremely valuable.

---

## WHERE logic before joins

Next up:

* OR
* NOT
* BETWEEN
* IN
* LIKE
* IS NULL

These are more logic tools we can add to our WHERE statements.

---

### OR and NOT

```sql
SELECT * FROM city
WHERE city = 'Bern' OR country_id = 44;
```

I wanted to see Bern and every country with an ID of 44.

```sql
SELECT *
FROM film
WHERE rating = 'G' OR rating = 'PG';
```

```sql
SELECT *
FROM customer
WHERE NOT activebool = true;
```

```sql
SELECT *
FROM city
WHERE NOT country_id = 44;
```

I wanted to see every country without the ID of 44.

---

### BETWEEN (ranges)

```sql
SELECT *
FROM payment
WHERE amount BETWEEN 5 AND 10;
```

Simple but useful.
Shows payments between $5 and $10.

---

### IN (multiple values)

```sql
SELECT *
FROM film
WHERE rating IN ('G', 'PG', 'PG-13');
```

This feels similar to Python lists.
You define a set of allowed values.

---

### LIKE (text searching)

```sql
SELECT *
FROM customer
WHERE first_name LIKE 'A%';
```

Using wildcards is great, but it reminds me of regex.

Common patterns:

* `'A%'` → starts with A
* `'%son'` → ends with son
* `'_a%'` → second letter is a

Example:

```sql
SELECT address, address2
FROM address
WHERE address LIKE '%Hanoi%';
```

---

### NULL handling

```sql
SELECT *
FROM customer
WHERE email IS NULL;
```

```sql
SELECT title, rental_rate
FROM film
WHERE rating IS NULL;
```

```sql
SELECT *
FROM customer
WHERE email IS NOT NULL;
```

```sql
SELECT *
FROM inventory
WHERE last_update IS NOT NULL;
```

---

## Final takeaway before joins

Core topics covered:

* SELECT
* WHERE
* ORDER BY
* LIMIT
* Aggregates
* GROUP BY
* HAVING

Before joins, just needed to be comfortable with:

* OR
* NOT
* BETWEEN
* IN
* LIKE
* IS NULL

Once those feel natural, moving into joins makes more sense.

---

# Join practice

```sql
SELECT title, rating
FROM film
JOIN language
ON film.language_id = language.language_id;
```

```sql
SELECT *
FROM city
JOIN country
ON city.city_id = country.country_id;
```

I wanted to see the city and countries next to each other since they are in separate tables.

---

### First LEFT JOIN

```sql
SELECT
    c.first_name,
    c.last_name,
    r.rental_date,
    r.return_date
FROM customer c
LEFT JOIN rental r
ON r.customer_id = c.customer_id;
```

I created this left join to see when someone rented a DVD and when they returned it.

---

### CROSS JOIN practice

```sql
SELECT *
FROM actor
CROSS JOIN film_actor;
```

This was me practicing a CROSS JOIN to see all possible combinations.
Not great in practice because it can create unnecessary data.

---

### RIGHT JOIN practice

```sql
SELECT
    i.last_update,
    r.rental_date,
    r.customer_id,
    i.store_id
FROM rental AS r
RIGHT JOIN inventory AS i
ON i.inventory_id = r.inventory_id;
```

I wanted to practice a RIGHT JOIN.
ChatGPT suggested using a LEFT JOIN instead, which makes sense because we usually care about what’s rented out compared to inventory.

---

### Multi-join actor query

```sql
SELECT
    f.title,
    a.first_name,
    a.last_name
FROM film AS f
JOIN film_actor AS fm
    ON f.film_id = fm.film_id
LEFT JOIN actor AS a
    ON fm.actor_id = a.actor_id;
```

This is a multi-join query and I’m proud of it.
It shows the actors in a movie.

---

### Staff rental comparison

```sql
SELECT
    s.first_name,
    s.last_name,
    COUNT(r.rental_id) AS rental_count
FROM staff AS s
LEFT JOIN rental AS r
ON s.staff_id = r.staff_id
WHERE s.staff_id IN (1, 2)
GROUP BY s.first_name, s.last_name, s.staff_id;
```

I wanted to compare how many movies were sold by both staff members.

---

### Difference between staff rentals

```sql
SELECT
    MAX(rental_count) - MIN(rental_count) AS rental_difference
FROM (
    SELECT
        s.staff_id,
        COUNT(r.rental_id) AS rental_count
    FROM staff AS s
    LEFT JOIN rental AS r
        ON s.staff_id = r.staff_id
    WHERE s.staff_id IN (1, 2)
    GROUP BY s.staff_id
) AS counts;
```

Still working on subqueries, but this helps decide who deserves a bonus.

---

# String and math functions

### CONCAT

```sql
SELECT
    CONCAT(first_name, ' ', last_name) AS actor,
    last_update
FROM actor;
```

---

### LOWER

```sql
SELECT rating, LOWER(title) AS lower_name
FROM film;
```

Starting string functions.
Simple but still important.

---

### UPPER

```sql
SELECT UPPER(name) AS upper_name, last_update
FROM category;
```

Another simple uppercase query.

---

### Arithmetic

```sql
SELECT
    title,
    replacement_cost,
    replacement_cost - rental_rate AS standard_replacement
FROM film;
```

This could be used to check profit margin.

---

```sql
SELECT
    title,
    rental_duration * rental_rate AS customer_cost
FROM film;
```

I wanted to see how much money is owed on each film.

---

### Full join with cost estimate

```sql
SELECT
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    f.title,
    f.rental_rate * f.rental_duration AS customer_cost
FROM film AS f
LEFT JOIN inventory AS i
    ON f.film_id = i.film_id
LEFT JOIN rental AS r
    ON i.inventory_id = r.inventory_id
LEFT JOIN customer AS c
    ON r.customer_id = c.customer_id;
```

Estimated cost if they kept it for the full rental duration.

---

### TRIM

```sql
SELECT TRIM(title)
FROM film;
```

Removes spaces.
Simple but effective when data is messy.

---

### LENGTH

```sql
SELECT
    title,
    LENGTH(title) AS title_length
FROM film
WHERE LENGTH(title) > 15;
```

Used this to find long titles.

---

### SUBSTRING

```sql
SELECT SUBSTRING(description FROM 1 FOR 15)
FROM film;
```

I wanted to see what each description began with.
Surprisingly, they all started with “A” followed by an adjective.

---
