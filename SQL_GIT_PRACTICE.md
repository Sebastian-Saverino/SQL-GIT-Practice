**Starting with SELECT queries first**
Used \d to view the table contents; Important to understand table structure
Practiced SELECT * / SELECT column_name1, column2 FROM table on different tables.
Practiced SELECT DISTINCT name FROM customer to see the unique names of customers.
Used SELECT DISTICT release_year FROM film; to see the range of dvd release dates, and this data set is for 2006 only.
SELECT DISTINCT amount FROM payment;
*Working on WHERES next*
SELECT name FROM category WHERE name = 'Action'; 
SELECT * FROM actor WHERE last_name = 'Wahlberg'; Wondered if this was made up data or not, this data is infact made up data. 
SELECT * FROM payment WHERE amount > 10; Wondered how often people spent more then 10 dollars; This follows the idea of Low-Ticket, High-Frequency Sales
*Working on ORDER BY next*
SELECT amount FROM payment ORDER BY amount; This showed me that movies are free or atleast individuals were getting movies for free some how
SELECT amount FROM payment ORDER BY amount DESC; I learned that the most people are spending is 11.99;
SELECT payment_date, 
*Working on LIMIT and OFFSET*
SELECT title FROM film LIMIT 10 OFFSET 2;
SELECT title FROM film LIMIT 5;
Working with this two syntax to better understand how offset works.
