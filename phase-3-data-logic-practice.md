# Phase 3.0 — Logical Data Modeling & Schema Design

1. Name the two main types of databases in use today.

* SQL
* NoSQL

or another common way people talk about database workloads is:

* OLAP
* OLTP

(clarification: SQL vs NoSQL is a database type classification, while OLAP vs OLTP is more about workload/use case)

2. What type of data does an analytical database store?

* Static or historical data used for analysis

3. True or False: An operational database is used primarily in online transaction processing (OLTP) scenarios.
   True

4. Name one of the branches of mathematics on which the relational model is based.

* Set theory and first-order predicate logic

5. How does a relational database store data?

* Through relations, which are tables with rows and columns in them

6. Name the three types of relationships in a relational database.

* one to many
* one to one
* many to many

7. How do you retrieve data in a relational database?

* SQL

8. State two advantages of a relational database.

* Strong data integrity through constraints
* Structured organization which makes data easier to query and manage

9. What is a relational database management system?

* This is software that lets you create, manage, and work with a relational database, such as MySQL or PostgreSQL

10. True or False: Mobile devices are limited to gigabytes of storage.
    False

11. State why database software companies have had a hard time implementing the relational database.

* Because not all modern data fits neatly into rows and columns anymore. We now work with PDFs, videos, images, logs, and all kinds of other data types. (correction: this is less about companies not planning properly and more about the fact that real-world data became more varied and less structured)

---

When is the best time to use an RDBMS program’s design tools?

* After the logical design step

2. True or False: Design is crucial to the consistency, integrity, and accuracy of data.
   True

3. What is the most detrimental result of improper database design?

* Inaccurate company data

4. What fact makes the relational database structurally sound and able to guarantee accurate information?

* It gives you a set of rules to follow, so there is a structured way to organize the data

5. State two advantages of learning a design methodology.

* You gain scalability because it is easier to modify organized data
* You reduce problems later because the data is already structured correctly

6. True or False: You will use your RDBMS program more effectively if you understand database design.
   True

7. State two objectives of good design.

* Have a structured base to work from
* Make the data easier to query, manage, and use

8. What helps to guarantee that data structures and their values are valid and accurate at all times?

* Understanding what the business and users actually need
* Figuring out what data belongs in what tables
* Normalization
* Constraints and rules

10. True or False: You can take shortcuts through some of the design processes and still arrive at a good, sound design.
    False (correction: in general, shortcuts in database design usually come back to hurt you later)

---

### Note

For this practice, I used a mix of “what I need to know” combined with the SQL roadmap to structure this phase.

For this section, I will be writing all notes myself.

---

## Objective: Move from Query Writer → Logical Schema Designer

This phase is complete when I can:

* Translate business requirements into relational structures
* Define table grain clearly
* Normalize and denormalize intentionally
* Enforce integrity through constraints
* Design schemas without relying on examples
* Think in entities, relationships, and lifecycle

This is not about performance.

This is about structure.

---

Why is terminology important?
Terminology is important because it is basically the universal language of the topic. If I do not understand the terms for relational databases, then it becomes harder to learn everything else built on top of them.

2. Name the four categories of terms.

* Value-related terms
* Structure-related terms
* Relationship-related terms
* Integrity-related terms

3. What is the difference between data and information?

* Data is the raw fact, like my name Sebastian. Information is when that data becomes useful in context, like Sebastian as a customer, student, or employee with associated details.

4. What does Null represent?

* Null represents the absence of a value, or an unknown/missing value. It is not the same thing as 0.

5. What is the major disadvantage of Null?

* Null can complicate comparisons, logic, and aggregate functions because it means unknown, not an actual value

6. What are the chief structures in the database?

* Relations, which are tables
* Tuples, which are records/rows
* Attributes, which are fields/columns

7. Name the three types of tables.

* data tables
* validation tables
* virtual tables

8. What is a view?

* A view is a virtual table built from a query. It shows data from one or more tables without storing the data itself. (correction: usually from one or more tables, not really “one or more databases” in the general definition)

9. State the difference between a key and an index.

* A key is used to identify records or connect tables and enforce integrity
* An index is mainly used to speed up searching and querying

10. What are the three types of relationships that can exist between a pair of tables?

* one-to-one
* one-to-many
* many-to-many

11. What are the three ways in which you can characterize a relationship?

* By the type of relationship
* The manner in which each table participates
* The degree to which each table participates

12. What is a field specification?

* This is the definition of what a field is supposed to be, like its name, type, whether it can be null, and other rules around it

13. What three types of elements does a field specification incorporate?

* General elements, like name and description
* Physical elements, like data type and size
* Logical elements, like whether it is required, unique, or can be Null

14. What is data integrity?

* Data integrity is the idea that the data stays accurate, valid, and consistent over time (correction: it is not just about reducing redundancy, even though normalization helps with that)

15. Name the four types of data integrity.

* Table-level integrity, so the table has valid rows and proper keys
* Field-level integrity, so the values inside fields are valid
* Relationship-level integrity, so related records stay synchronized correctly
* Business-rule integrity, so the data follows the rules the business needs

---

# Chapter 4

1. Why is it important to complete the design process thoroughly?

* Because the better and more thorough the design is, the stronger the integrity of the database will be. If you rush the structure, you create problems later.

2. True or False: The level of structural integrity is in direct proportion to how thoroughly you follow the design process.
   True

3. What is the purpose of a mission statement?

* To understand the why of the database

4. What are mission objectives?

* These are the actual things the database needs to do, like store client data properly, track orders, or support reports

5. What constitutes your organization’s fundamental data requirements?

* The mission statement and mission objectives

6. How do you determine the various subjects that the tables will represent?

* Through interviews with people such as management, users, and other stakeholders

7. True or False: You establish field specifications for each field in the database during the second phase of the database design process.
   False

8. How do you establish a logical connection between the tables in a relationship?

* By understanding how the real-world subjects connect through the business process

9. What determines a set of limitations and requirements that you must build into the database?

* The business rules and actual use cases of management and users

10. What can you design and implement to support certain business rules?

* Constraints, validation rules, triggers, and views (correction: views can support access/use, but they do not enforce business rules by themselves)

11. How do you determine the types of views you need to build in the database?

* By speaking with users and understanding how they need to see the data

12. When can you implement your logical structure in an RDBMS program?

* After you have checked the design for integrity and made sure it follows the business rules

---

# Chapter 5

1. Why are interviews important?

* They help you understand the mission statement and mission objectives the database is built from, and they help you understand the why behind what you are developing

2. What problem can arise when you conduct an interview with a large number of people?

* You may get a wide variety of answers, conflicting perspectives, and possible disputes

3. What is the primary reason for conducting separate interviews with users and management?

* So you can give each group focused attention and understand their needs without one group overshadowing the other

4. True or False: You’ll commonly use closed questions in your interviews.
   False

5. What kind of responses should you try to evoke from the interview participants?

* Useful and detailed ones, especially responses that are authentic and give real substance

6. What is the single most important guideline for every interview you conduct?

* Always maintain control of the interview

7. What is a mission statement?

* This is the overarching idea behind the database, so why are we developing it in the first place

8. State two characteristics of a well-written mission statement.

* Succinct
* Direct

9. True or False: You need not learn about the organization to compose a mission statement.
   False

10. When is your mission statement complete?

* Once it is focused, concise, and clearly explains the purpose of the database

11. What is a mission objective?

* This is what the database should be able to do, such as hold all customer records

12. State two characteristics of a well-written mission objective.

* Succinct
* To the point

13. True or False: You should interview users and management to help you define mission objectives.
    True

14. How does the staff’s daily work relate to the mission objectives?

* The mission objectives should reflect and support the actual daily work the staff needs to perform

15. True or False: A mission objective can describe more than one task.
    False

16. State two ways that a mission objective can be derived from a response.

* The initial what
* The initial how

---

# Chapter 6

1. State two goals of analyzing the current database.

* To see how data is collected
* To see how data is presented

2. True or False: You can adopt the current database structure as the basis for the new structure.
   False

3. What is a legacy database?

* This is an older existing database that was previously used to store the data

4. State two steps of the analysis process.

* Review how data is collected
* Review how data is presented

5. Which types of computer software programs should you review during the analysis?

* The programs users rely on to input, store, or present data

6. Why should you conduct interviews after you gather data collection and information presentation samples?

* Because those samples give you context first, and then the interviews help refine and explain what you found

7. How do you use “open-ended” and “closed” questions?

* Open-ended questions help you understand workflow and reasoning
* Closed questions help confirm specific facts

8. What is the Subject-Identification Technique?

* This is where you listen for nouns in what people say, such as customers, orders, events, or products, because those nouns often point to possible subjects/entities

9. How do you identify specific attributes for a particular subject?

* You look at the characteristics that describe that subject, like a customer having a name, age, address, or phone number

10. True or False: You should interview users and management at the same time.
    False

11. What three basic types of information requirements must you identify?

* Data-entry requirements
* Information retrieval requirements
* Information presentation requirements
  (correction)

12. What is the Preliminary Field List?

* This is the early list of possible characteristics that may become fields

13. State why each item on the Preliminary Field List should have a unique name.

* Because we want to avoid duplicates and make sure each field has one clear purpose

14. What is a value list?

* This is a list of acceptable values a user can choose from for a given field

15. What are calculated fields? What (if anything) should you do about them?

* These are fields whose values can be calculated from other existing fields. Usually you should not store them unless there is a strong reason to do so. (correction)

---

# Chapter 7

1. How do you identify and establish tables for a new database?

* We define the preliminary table list

2. Why do you use the Preliminary Field List to help you define tables for the database?

* Because the fields help show what subjects exist and what belongs together, so you are basically connecting the dots

3. What action do you take when an item on the list of subjects and a differently named item on the Preliminary Table List both represent the same subject?

* You keep one final consistent name and remove the duplicate

4. What information does the Final Table List provide?

* The final set of tables and their table types

5. State three guidelines for creating table names.

* No acronyms if they are unclear
* Be accurate
* Be unique and meaningful

6. State two guidelines for composing table descriptions.

* Be clear
* Be descriptive

7. How do you assign fields to a table on the Final Table List?

* By matching the field to the subject it describes

8. State three guidelines for creating field names.

* Be accurate
* Use singular names where appropriate
* Make the name meaningful and clear
  (correction: “using an ideal field to resolve anomalies” is not really a field naming guideline)

9. What two problems can poorly designed fields cause?

* Duplicate data
* Confusing or misleading data

10. What can you use to resolve field anomalies?

* Redesigning the field, splitting the field, or creating new tables when needed

11. State three of the Elements of the Ideal Field.

* It contains a single value
* It is not multivalued
* It is not calculated if it can be derived from other data
  (correction: primary key is not one of the general elements of an ideal field)

12. Under what condition is redundant data acceptable?

* Only when there is a very strong and intentional reason for it, usually for business or performance reasons (correction)

13. In general terms, what three steps do you follow to resolve a multi-valued field?

* Separate the multiple values
* Decide if they represent a new subject
* Create a new table if needed

14. When is it necessary to use a duplicate field in a table?

* When the table needs to store the same type of data for different roles, such as billing address and shipping address, or when a linking table needs repeated key types for many-to-many relationships (clarification)

15. How can you refine table structures?

* By restructuring fields and creating additional tables when needed to reduce redundancy and improve integrity

17. What is a subset table?

* This is a table split off from a larger table to store only a subset of records or attributes, often to reduce blank data or isolate optional information

---

# Chapter 8

1. State the three reasons why keys are important.

* They identify records
* They enforce integrity
* They create relationships between tables

2. What are the four main types of keys?

* Primary keys
* Candidate keys
* Foreign keys
* Alternate keys
  (correction: non-keys are not a key type)

3. What is the purpose of a candidate key?

* It is a possible choice for the primary key

4. State four items of the Elements of a Candidate Key.

* Must uniquely identify each record
* Must not contain null values
* Must contain minimal data needed for uniqueness
* Can be made of one field or multiple fields
  (correction: a candidate key absolutely can be composite)

5. True or False: A candidate key can be composed of more than one field.
   True

6. Can a table have more than one candidate key?
   Yes

7. What is an artificial candidate key?

* This is a created key, such as an auto-incrementing ID, used when no natural candidate key works well

8. What is the most important key you assign to a table?

* Primary key

9. Why is this key important?

* It is important for table integrity, relationships, and uniquely identifying each record

10. How do you establish a primary key?

* By evaluating the candidate keys and choosing the best one

11. State four items of the Elements of a Primary Key.

* It uniquely identifies each record
* It cannot be null
* It should be stable
* It should be as simple as possible
  (correction)

12. What must you do before you finalize your selection of a primary key?

* Make sure it really uniquely identifies each record and is the best practical candidate

13. What is an alternate key?

* This is any candidate key that was not chosen as the primary key

14. What do you ensure by establishing table-level integrity?

* You ensure that each table has a proper structure and valid records

15. Why should you review the initial table structures?

* To make sure they still support integrity and do not contain structural problems

---

# Chapter 9

1. State two major reasons why field specifications are important.

* They help maintain data integrity
* They give clear rules for how each field should behave

2. What do you gain by establishing field-level integrity?

* More accurate and valid values at the field level

3. What are the three categories of elements in a field specification?

* General elements
* Physical elements
* Logical elements

4. Name the three types of specifications.

* Unique
* Generic
* Replica

5. Why is it beneficial for you to compose a proper field description?

* Because it helps make the field’s purpose clear and supports a more organized design

6. What does the Data Type element indicate?

* Whether the field accepts text, numbers, dates, and so on

7. What does the Character Support element indicate?

* What characters or formats the field supports, such as letters, numbers, symbols, or combinations of them

8. What types of keys are indicated on a field specification?

* Primary
* Foreign
* Alternate
* Nonkey

9. True or False: Null represents a blank value.
   False

10. What is the significance of the Range of Values element?

* It defines the acceptable set or limits of values allowed in that field

11. What is the purpose of an Edit Rule?

* To control and validate how data can be entered or changed

12. When do you use a generic specification?

* When multiple fields share the same general characteristics and rules

---

# Chapter 10

1. State two major reasons why a relationship is important.

* It helps refine table structures
* It helps minimize redundant data

2. Name the three types of relationships.

* one-to-one
* one-to-many
* many-to-many

3. Which relationship will pose the most problems?

* many-to-many

4. State two problems you could possibly encounter with a many-to-many relationship.

* Redundant data
* Difficulty implementing it directly in a relational model without an intermediate table
  (correction)

5. What is a self-referencing relationship?

* A relationship where a table relates to itself, such as an employee having another employee as a manager (correction: not just “between two fields”)

6. How do you begin the process of identifying the relationships among the tables in the database?

* Use the table matrix and ask whether and how the tables connect

7. What are the two types of questions you can ask to help you identify existing relationships?

* Associative questions
* Contextual questions

8. What shorthand symbol do you use to designate a one-to-many relationship in the table matrix?

* 1:N

9. How do you determine what type of relationship officially exists between each pair of tables in the matrix?

* By examining how records in one table relate to records in the other table in the real business process

10. How do you establish a one-to-many relationship?

* By placing the primary key from the one side into the many side as a foreign key

11. True or False: Retrieving information from tables with a self-referencing relationship can be tedious and somewhat difficult.
    True

12. How do you establish a self-referencing many-to-many relationship?

* You create a linking/intersection table that references the same parent table twice (correction: not a linked list)

13. How do you refine the foreign keys in the database?

* By making sure they correctly reference the parent key and follow the right naming and specification rules

14. What two element categories must you modify for a foreign key’s field specification?

* The key type
* The specification so it matches the parent field as a replica where needed

15. What is the function of a deletion rule?

* To define what happens to related records if a parent record is deleted

16. What two types of participation can you designate for a table?

* Mandatory participation
* Optional participation
  (correction)

17. What does the degree of participation indicate?

* It indicates the minimum and maximum number of times a record can participate in a relationship

18. When does a relationship attain relationship-level integrity?

* the connection between the two tables or key fields in a relationship is sound
* you can insert new records into each table in a meaningful manner
* you can delete an existing record without producing adverse effects
* a meaningful limit exists for the number of records that can be interrelated within the relationship

---


# Chapter 11

1. What is a business rule? A statement saying this is how we want to have the data function and therefore how we put constraints on our data.
2. Name the two major types of business rules. database-oriented and application based
3. Can you establish application-oriented business rules within the logical design of the database? Negative this has to be done in the physical design portion
4. What are the two categories of database-oriented business rules? Field and relationship rules
5. What is a field-specific business rule? This could ask what can go into a certain field 
6. When is a business rule tested? Through mock testing and conversations 
7. How do you document a business rule? Business rule specification sheet and your erd 
8. State two advantages a Business Rule Specifications sheet provides. Organization and documentation
9. What is the purpose of the Action Taken section of a Business Rule Specifications sheet? To specify how it was came about 
10. What is the purpose of a validation table? As an answer to relationship questions 
11. What is the typical structure of a validation table? Primary key and the validation data 
12. What is the association between a business rule and a validation table? It commonly can be used to satisfy a business rule 
13. Why should you review all of your completed Business Rule Specifications sheets? To make sure you maintain data integrity.