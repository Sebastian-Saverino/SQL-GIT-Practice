# Phase 1 – Core SQL Query Logic

These tools make queries more readable, flexible, and closer to real business logic.

---

# 1. Aliases (Renaming Columns and Tables)

## Core Idea

An **alias** is a temporary name given to a column or table in a query.

It helps:

* Make output more readable
* Shorten long table names
* Avoid confusion in joins

---

## Column Alias Syntax

```sql
SELECT column_name AS alias_name
FROM table;
```

### Example

```sql
SELECT first_name AS fname
FROM customer;
```

Output column:

```
fname
```

---

## Table Alias Syntax

```sql
SELECT c.first_name
FROM customer AS c;
```

### With a join

```sql
SELECT
    c.first_name,
    r.rental_date
FROM customer AS c
JOIN rental AS r
ON c.customer_id = r.customer_id;
```

---

## Feynman Explanation

Aliases are like **nicknames**.

Instead of saying:

```
customer.first_name
```

you say:

```
c.first_name
```

It makes queries shorter and easier to read.

---

# 2. Arithmetic in SELECT

## Core Idea

You can perform **math operations directly in queries**.

---

## Basic operators

```
+   addition
-   subtraction
*   multiplication
/   division
```

---

## Example

```sql
SELECT
    amount,
    amount * 1.10 AS increased_price
FROM payment;
```

This simulates a **10% price increase**.

---

## Real-world example

```sql
SELECT
    replacement_cost,
    replacement_cost - rental_rate AS profit_margin
FROM film;
```

This estimates profit per film.

---

## Feynman Explanation

SQL isn’t just for pulling data.
It can also **calculate new values on the fly**.

So instead of:

```
store price
then calculate later
```

You can calculate directly in the query.

---

# 3. String Functions

## Core Idea

String functions help **modify or clean text data**.

---

## UPPER()

Converts text to uppercase.

```sql
SELECT UPPER(first_name)
FROM customer;
```

Result:

```
JOHN
JANE
```

---

## LOWER()

Converts text to lowercase.

```sql
SELECT LOWER(first_name)
FROM customer;
```

---

## LENGTH()

Counts characters in text.

```sql
SELECT first_name, LENGTH(first_name)
FROM customer;
```

---

## TRIM()

Removes extra spaces.

```sql
SELECT TRIM('   hello   ');
```

Result:

```
hello
```

---

## SUBSTRING()

Extracts part of a string.

```sql
SELECT SUBSTRING(first_name FROM 1 FOR 3)
FROM customer;
```

If name = `Michael`
Result:

```
Mic
```

---

## Feynman Explanation

String functions are like **text editing tools**.

They:

* Change case
* Cut text
* Clean spaces
* Measure length

They’re used when working with messy real-world data.

---

# 4. Date Functions

## Core Idea

Dates are very important for:

* Sales reports
* Trends
* Time-based analysis

SQL lets you extract parts of a date.

---

## Extract date only (remove time)

```sql
SELECT DATE(payment_date)
FROM payment;
```

---

## Group by day

```sql
SELECT
    DATE(payment_date) AS day,
    COUNT(*) AS payments
FROM payment
GROUP BY day;
```

---

## EXTRACT() function

Pull specific parts of a date.

### Extract year

```sql
SELECT
    EXTRACT(YEAR FROM payment_date)
FROM payment;
```

### Extract month

```sql
SELECT
    EXTRACT(MONTH FROM payment_date)
FROM payment;
```

---

## Real business example

Monthly revenue:

```sql
SELECT
    EXTRACT(MONTH FROM payment_date) AS month,
    SUM(amount) AS revenue
FROM payment
GROUP BY month
ORDER BY month;
```

---

## Feynman Explanation

Dates in SQL are like **timestamps on receipts**.

But businesses don’t care about:

```
2007-03-15 14:23:11
```

They care about:

* The day
* The month
* The year

Date functions let you **pull out those parts**.

---

# 5. COALESCE (Handling NULL Values)

## Core Idea

`COALESCE` replaces NULL values with something else.

---

## Syntax

```sql
COALESCE(column, replacement_value)
```

---

## Example

```sql
SELECT
    first_name,
    COALESCE(email, 'No email provided')
FROM customer;
```

If email is NULL:

```
No email provided
```

---

## Multiple fallbacks

```sql
SELECT
    COALESCE(phone, email, 'No contact info')
FROM customer;
```

SQL checks:

1. phone
2. email
3. fallback text

---

## Feynman Explanation

NULL means:

> “We don’t know this value.”

COALESCE says:

> “If we don’t know it, show this instead.”

It’s like:

```
If no phone number,
use email.
If no email,
say 'No contact info'.
```

---

# 6. CASE (Conditional Logic)

## Core Idea

`CASE` lets you add **if-then logic** inside SQL.

This is one of the most important tools in SQL.

---

## Syntax

```sql
CASE
    WHEN condition THEN result
    WHEN condition THEN result
    ELSE result
END
```

---

## Basic Example

```sql
SELECT
    amount,
    CASE
        WHEN amount < 3 THEN 'Low'
        WHEN amount < 7 THEN 'Medium'
        ELSE 'High'
    END AS price_category
FROM payment;
```

---

## Example: Customer status

```sql
SELECT
    first_name,
    last_name,
    CASE
        WHEN activebool = true THEN 'Active'
        ELSE 'Inactive'
    END AS status
FROM customer;
```

---

## CASE with aggregation

```sql
SELECT
    COUNT(*) AS total_rentals,
    CASE
        WHEN COUNT(*) > 5000 THEN 'High volume'
        ELSE 'Normal volume'
    END AS activity_level
FROM rental;
```

---

## Feynman Explanation

CASE is like an **if-statement in SQL**.

In Python:

```python
if amount < 3:
    label = "Low"
```

In SQL:

```sql
CASE
    WHEN amount < 3 THEN 'Low'
END
```

It lets SQL make **decisions about the data**.

---

# Phase 1 Summary Cheat Sheet

## Aliases

```sql
SELECT column AS alias
FROM table;
```

---

## Arithmetic

```sql
SELECT amount * 1.1
FROM payment;
```

---

## String functions

```sql
UPPER()
LOWER()
LENGTH()
TRIM()
SUBSTRING()
```

---

## Date functions

```sql
DATE(column)
EXTRACT(YEAR FROM column)
EXTRACT(MONTH FROM column)
```

---

## COALESCE

```sql
COALESCE(column, 'replacement')
```

---

## CASE

```sql
CASE
    WHEN condition THEN result
    ELSE result
END
```

---

# Mental Model of Phase 1

Before Phase 1:

* SQL pulls data

After Phase 1:

* SQL can **transform, clean, categorize, and calculate** data

This is where SQL starts to feel like a **real analytics tool**, not just a data viewer.

