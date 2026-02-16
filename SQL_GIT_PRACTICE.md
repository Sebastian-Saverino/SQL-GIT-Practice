
## SELECT Fundamentals Progression

### Starting with SELECT queries

I started with basic SELECT queries first.

Used `\d` to view the table contents.
This was important to understand the structure of each table before running queries.

Practiced:

SELECT * FROM table;
SELECT column_name1, column_name2 FROM table;

on different tables to get comfortable with pulling specific data.

---

### Working with DISTINCT

Used:

SELECT DISTINCT name FROM customer;

to see the unique customer names.

Also tried:

SELECT DISTINCT release_year FROM film;

to see the range of DVD release dates.
This dataset is only for 2006.

SELECT DISTINCT amount FROM payment;

to see the different payment values.

---

### Moving into WHERE

Working on WHERE next.

SELECT name FROM category WHERE name = 'Action';

SELECT * FROM actor WHERE last_name = 'Wahlberg';

This made me wonder if the data was real or not.
Turns out it’s made-up sample data.

SELECT * FROM payment WHERE amount > 10;

I was curious how often people spent more than $10.
This follows the idea of low-ticket, high-frequency sales.

SELECT title, rental_duration, rental_rate, replacement_cost
FROM film;

I wanted to better understand the film data using a simple SELECT.

SELECT * FROM film
WHERE rating = 'G' AND language_id = '5';

This was eye-opening because it showed a data issue.
There aren’t any foreign films with a maturity rating in this dataset.

---

### Working on ORDER BY

SELECT amount FROM payment ORDER BY amount;

This showed that some movies were free, or at least some people paid nothing.

SELECT amount FROM payment ORDER BY amount DESC;

I learned that the highest payment was $11.99.

---

### Working on LIMIT and OFFSET

SELECT title FROM film LIMIT 10 OFFSET 2;
SELECT title FROM film LIMIT 5;

Used these to better understand how OFFSET shifts the starting point of results.

---

### Working with aggregates

SELECT MAX(amount) FROM payment;

Similar to using ORDER BY, but faster and cleaner to get the highest value.

SELECT SUM(amount) FROM payment;

This gives the total money made from rentals.
Very powerful, since this is the start of real business metrics.

Practiced:

SELECT MIN(length), MAX(length) FROM film;

SELECT title, MAX(length) AS length
FROM film
GROUP BY title
ORDER BY length DESC;

This expands on earlier queries.
It shows the longest films by grouping and ordering.
Also highlights the use of aliases, which are very powerful.

---

### First HAVING attempts

SELECT COUNT(language_id), title
FROM film
GROUP BY title
HAVING COUNT(language_id) < 1;

This was what I was trying to write earlier.
I finally got the structure right.

SELECT payment_date, AVG(amount)
FROM payment
GROUP BY payment_date
HAVING AVG(amount) > 5;

Worked with HAVING today.
It was a bit of a pain, but it was nice to use aggregates more.
There’s a lot of power in averages, counts, min, max, and sum.

---

### WHERE vs HAVING example

SELECT COUNT(title)
FROM film
WHERE length > 100;

Used this to see how many films were longer than 100 minutes.
The answer was 610 films.

---

### Counting grouped data

SELECT rating, COUNT(title)
FROM film
GROUP BY rating;

This counts how many movies exist in each rating category from G to NC-17.
HAVING wasn’t even necessary here.
This is a case where query optimization matters.

---

### Counting customers

SELECT COUNT(first_name) FROM customer;

I wanted to see how many customers there were, then realized people can share the same name.

Tried:

SELECT DISTINCT first_name FROM customer;

to see the unique first names.
Realized customer_id would be a better counting field, but this still answered the question.

---

### Proper HAVING example

SELECT activebool, COUNT(*) AS customer_activity
FROM customer
GROUP BY activebool
HAVING COUNT(*) > 1;

Big takeaway:
HAVING is best used with aggregates.

It felt most useful when working with the payment table, since aggregates are extremely valuable for averages, counts, min, max, and sum.

---

## Next: WHERE Logic Before Joins

OR, NOT, BETWEEN, IN, LIKE, and IS NULL are next.

These are more logic tools we can add to our WHERE statements.

Before moving to joins, only a few core WHERE-clause tools remain.

---

### OR and NOT

SELECT * FROM city WHERE city = 'Bern' OR country_id = 44;

I wanted to see Bern and every country with an ID of 44.

SELECT * FROM film
WHERE rating = 'G' OR rating = 'PG';

SELECT * FROM customer
WHERE NOT activebool = true;

SELECT * FROM city WHERE NOT country_id = 44;

I wanted to see every country without the ID of 44.

---

### BETWEEN (ranges)

SELECT * FROM payment
WHERE amount BETWEEN 5 AND 10;

Simple but useful.
Shows payments between $5 and $10.

---

### IN (multiple values)

SELECT * FROM film
WHERE rating IN ('G', 'PG', 'PG-13');

This feels similar to Python lists.
You define a set of allowed values.

---

### LIKE (text searching)

SELECT * FROM customer
WHERE first_name LIKE 'A%';

Using wildcards is great, but it reminds me of regex.

Common patterns:

* 'A%' → starts with A
* '%son' → ends with son
* '_a%' → second letter is a

Example:
SELECT address, address2
FROM address
WHERE address LIKE '%Hanoi%';

---

### NULL handling

SELECT * FROM customer
WHERE email IS NULL;

SELECT title, rental_rate FROM film WHERE rating IS NULL;

SELECT * FROM customer
WHERE email IS NOT NULL;

SELECT * FROM inventory WHERE last_update IS NOT NULL;

---

## Final takeaway before joins

So far, the core topics covered:

* SELECT
* WHERE
* ORDER BY
* LIMIT
* Aggregates
* GROUP BY
* HAVING

Before joins, just need to be comfortable with:

OR, NOT, BETWEEN, IN, LIKE, and IS NULL.

Once those feel natural, moving into joins will make a lot more sense.

---

 SELECT title, rating FROM film JOIN language ON film.language_id = language.language_id;

SELECT * FROM city JOIN country on city.city_id = country.country_id;

I wanted to see the city and countries next to each other as they are both in seperate tables

SELECT customer.first_name, customer.last_name, rental.rental_date, rental.return_date FROM customer LEFT JOIN rental on rental.customer_id = customer.customer_id;

SELECT 
    c.first_name, 
    c.last_name, 
    r.rental_date, 
    r.return_date
FROM customer c
LEFT JOIN rental r
    ON r.customer_id = c.customer_id;


I created this left join in the intent to see when someone rented out a dvd and as well when they would need to return it


SELECT * FROM actor CROSS JOIN film_actor;
This is me practicing me a CROSS JOIN to see all possible joins, this is not the greatest as this can give us an unessary amount of data.

SELECT i.last_update, r.rental_date, r.customer_id, i.store_id FROM rental AS r RIGHT JOIN inventory AS i ON i.inventory_id = r.inventory_id;

I wanted to practice a RIGHT JOIN and I felt this went well so that was nice, not too bad of a query chatgpt suggested I do a left join which makes total sense. As keeping the rentals so what physicallly out compared to the inventory makes plenty of sense.






SELECT f.title, a.first_name, a.last_name FROM film AS f JOIN film_actor AS fm ON f.film_id = fm.film_id LEFT JOIN actor AS a ON fm.actor_id = a.actor_id;

This is a muti join query and I'm proud of it, as this shows the actors in a movie, I can work on updating it a little more tommorow :)

SELECT f.title, f.description, c.name FROM film AS f LEFT JOIN film_category AS fc ON f.film_id = fc.film_id JOIN category AS c ON fc.category_id = fc.category_id;

Look into this tommorow

 SELECT s.first_name, s.last_name, s.store_id, r.rental_id FROM staff AS s LEFT JOIN rental AS r ON s.staff_id = r.staff_id;

I in fact feel like i'm cooking today as I wanted to compare the amount of movies sold by both Mike Hillyer and Jon Stephens

So I created the first query to figure out the join portion such as figuring how to see the rentals that each person sold; then I followed this up with a count query to see how many rentals in told were sold

 SELECT 
    s.first_name,
    s.last_name,
    COUNT(r.rental_id) AS rental_count
FROM staff AS s
LEFT JOIN rental AS r
ON s.staff_id = r.staff_id
WHERE s.staff_id = 1 OR s.staff_id = 2
GROUP BY s.first_name, s.last_name, s.staff_id;

I added on to the orginal to see both staffs counts makinng it easier to see now I'm going to attempt to get the different between the rental counts


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



WITH counts AS (
    SELECT 
        s.staff_id,
        COUNT(r.rental_id) AS rental_count
    FROM staff AS s
    LEFT JOIN rental AS r
        ON s.staff_id = r.staff_id
    WHERE s.staff_id IN (1, 2)
    GROUP BY s.staff_id
)
SELECT 
    c1.rental_count AS staff_1_rentals,
    c2.rental_count AS staff_2_rentals,
    c1.rental_count - c2.rental_count AS difference
FROM counts c1
JOIN counts c2
ON c1.staff_id = 1 AND c2.staff_id = 2;

Shoutout chatgpt for this; I'm still working on my subquery skills but finding the difference between the staff is helpful as this can help decide who I want to give a bonus to if that makes since

SELECT s.first_name, s.last_name, st.store_id FROM staff AS s LEFT JOIN store AS st ON s.store_id = st.store_id;
 first_name | last_name | store_id
------------+-----------+----------
 Mike       | Hillyer   |        1
 Jon        | Stephens  |        2

 this was a practice query just keep up my consistentiency, I enjoyed this as I'm to the point where joins are feeling easy, and it is rather exciting as I did this query even if it is very simple but no errors first try very quickly.


 I wanted to use concat as it fell through the cracks when working through 

 SELECT CONCAT(first_name,' ',last_name) AS actor, last_update FROM actor;


 SELECT rating, LOWER(title) AS lower_name FROM film;
 This is me starting my string functions
 This is a simple use of lower and film these will go by quickly as they are simply but still need to be used.