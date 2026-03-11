> Phase 3.0 — Logical Data Modeling & Schema Design

---

# Phase 3.0 — Logical Data Modeling & Schema Design

1. Name the two main types of databases in use today. 
- SQL
- NoSQL
or 
-OLAP
-OLTP
2. What type of data does an analytical database store? 
-Static Data
3. True or False: An operational database is used primarily in online transaction processing (OLTP) scenarios.
True
 4. Name one of the branches of mathematics on which the relational model is based. 
 Set theory and first-order predicate logic
 5. How does a relational database store data? 
 Through relations (tables with stuff within them)
 6. Name the three types of relationships in a relational database. 
 one to many
 one to one
 many to many
 7. How do you retrieve data in a relational database? 
 SQL
 8. State two advantages of a relational database. 
 Multi level data integrity through creating a tables and within the table have constraints set
 9. What is a relational database management system? 
 This is a system that runs a relational database model such as mySQL for example
 10. True or False: Mobile devices are limited to gigabytes of storage.
 False
  11. State why database software companies have had a hard time implementing the relational database.
  They don't properly plan out the database and we now live in an age where data is more then just text we have things like pdfs, videos, all kind of different data types.




When is the best time to use an RDBMS program’s design tools? 
After the logical design step
2. True or False: Design is crucial to the consistency, integrity, and accuracy of data. 
True
3. What is the most detrimental result of improper database design?
Inaccurate company data
4. What fact makes the relational database structurally sound and able to guarantee accurate information?
It gives you set of rules to follow, it provides a structure approach to organizing your data
5. State two advantages of learning a design methodology. 
You gain scalabilyt of your data because you have those set of rules to follow making data easier to modify
You can spend more time for the front end because your data is already properly organized
6. True or False: You will use your RDBMS program more effectively if you understand database design.
True
7. State two objectives of good design. 
Have a structured base to work from
Can better query and assist customers as your data is properly organized
8. What helps to guarantee that data structures and their values are valid and accurate at all times? 
Asking your company and the users what they need
Figuring out what data goes to what tables 
Normalization 
10. True or False: You can take shortcuts through some of the design processes and still arrive at a good, sound design.
True but with exceptions as there is still different ways about going about design processes that could make ti easier.


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


Why is terminology important? Terminolgoy is important as it is the universiel language of the topic you may want to learn such as understanding the terms for relational databases
2. Name the four categories of terms. Value related terms, structure related terms, relationship-related terms, and Integrity related terms 
3. What is the difference between data and information? Data is my name Sebastian information is the value I can get from my name and its assocaited information 
4. What does Null represent? Null, so it represents Null so nothing but not to be confused with 0 
5. What is the major disadvantage of Null? Null can greatly effect aggregate functions 
6. What are the chief structures in the database? relations so the table, tuples so the records, and then attributes or a field 
7. Name the three types of tables. data tables, validation table and virtual table 
8. What is a view? This is a virtual table so like a set of data you picked from one or more databases then put together
 9. State the difference between a key and an index. A key is the unique identifier for a tuple or a record or row and an index is a way of database organization
10. What are the three types of relationships that can exist between a pair of tables? one-to-one, one-to-many, many-to-many
11. What are the three ways in which you can characterize a relationship?  You can characterize every relationship in three ways: by the type of relationship that exists between the tables, the manner in which each table participates, and the degree to which each table participates.
12. What is a field specification? This is what we decide what fields we want in our table 13. What three types of elements does a field specification incorporate? General data so your name, category. Physical so should it be an int or not. Logical so is this field needed can it be Null and etc
 14. What is data integrity? this is the idea of keeping your data from redudancy and as well avoiding not bring value to your data to create that information 
 15. Name the four types of data integrity. We have table level so this is like making sure we don't have duplicate records and making sure we don't have random nulls where we should not
 Field level our data is the correct type so our age should be an int for example
 Relationship level this makes sure our records are synchronized whenever data is enterd into and properly updated throughout your tables within your db
 Business rules just mean you data falls the rules that your business originally needs or wants from the data while maintaining integrity.

# Chapter 4

1. Why is it important to complete the design process thoroughly?
As the idea of data integrity is about being thorough, its like you building credit you have to build up to it.

2. True or False: The level of structural integrity is in direct proportion to how thoroughly you follow the design process. True
3. What is the purpose of a mission statement? To understand the why of your database
4. What are mission objectives? These are the does so does the database function correctly like does it store client data properly can I see each persons orders and so on
5. What constitutes your organization’s fundamental data requirements? The mission statement and mission objectives
 6. How do you determine the various subjects that the tables will represent? Through interviews with people such as your management, users, and so on
7. True or False: You establish field specifications for each field in the database during the second phase of the database design process. False
 8. How do you establish a logical connection between the tables in a relationship? By speaking with users and etc while using logic
  9. What determines a set of limitations and requirements that you must build into the database? Understanding the use cases of different management and users
  10. What can you design and implement to support certain business rules? You can design views to support business rules  11. How do you determine the types of views you need to build in the database? By speaking with the users
  12. When can you implement your logical structure in an RDBMS program? After you have checked your data integrity as well as you have made sure to abide by the business rules.



# Chapter 5 
1. Why are interviews important? They help understand the mission statement and the mission objectives that your database is built off of, as well as understanding the why to the what your developing
2. What problem can arise when you conduct an interview with a large number of people? A vareity of answers and possibly disputes between individuals and different prespectives
3. What is the primary reason for conducting separate interviews with users and management? To best get undivided attentions by different parts of the chain to best understand each portion with any one answer overshadowing another by interviewing them all together
4. True or False: You’ll commonly use closed questions in your interviews. False
5. What kind of responses should you try to evoke from the interview participants? GOod ones, so one that provides substance but most importantly there authentic
. What is the single most important guideline for every interview you conduct? Always maintain control of the interview
7. What is a mission statement? This is the overarching idea behind your database so why are we developing this in the first place
8. State two characteristics of a well-written mission statement. Succinct and direct 
9. True or False: You need not learn about the organization to compose a mission statement. False
 10. When is your mission statement complete? Once you can write to the focus and feel it is both succinct and focused 
  11. What is a mission objective? This should be what the database can do such as hold all customer records
   12. State two characteristics of a well-written mission objective. Succinct and to the point
    13. True or False: You should interview users and management to help you define mission objectives. True
     14. How does the staff’s daily work relate to the mission objectives? The point of gathering the mission objectives is to be able to cater the actual needs of the staff daily work such as hold those records
      15. True or False: A mission objective can describe more than one task. False
       16. State two ways that a mission objective can be derived from a response. The inital what and how


# Chapter 6
1. State two goals of analyzing the current database. To see how the data is collected and presented 
2. True or False: You can adopt the current database structure as the basis for the new structure. False 
3. What is a legacy database? This is a database that was used to store the data previously 
4. State two steps of the analysis process. Review how data is collected and how it is presented
5. Which types of computer software programs should you review during the analysis? What the users use to input data for example
6. Why should you conduct interviews after you gather data collection and information presentation samples? Interviews are always important but they can refine the data we have collected during our analysis of paper / legacy based databases 
7. How do you use “open-ended” and “closed” questions? Asking about there workflow 
8. What is the Subject-Identification Technique? This is where we look for nouns in what they say so like events or for example customers
9. How do you identify specific attributes for a particular subject? The say I input data on customers such as there age, address, and so on these are characteristics that describe the customers
10. True or False: You should interview users and management at the same time. False
11. What three basic types of information requirements must you identify? I don't know
12. What is the Preliminary Field List? This is our list of characteristics that can be used as our fields
13. State why each item on the Preliminary Field List should have a unique name. We want to remove duplicates and this is the place to do, we want each characteristics to have a purpose.
14. What is a value list? This is a list of values a user might have to choose from for one characteristic
15. What are calculated fields? These are aggregated fields,  What (if anything) should you do about them? make sure based off the characteristics you have named should be able to create this aggregated data.

# Chapter 7 

1. How do you identify and establish tables for a new database? We define the preliminary table list
2. Why do you use the Preliminary Field List to help you define tables for the database? You use the characteristics and you play connect the dots
3. What action do you take when an item on the list of subjects and a differently named item on the Preliminary Table List both represent the same subject? You keep the preliminary table list name and get rid of the duplicate from the subjects list
 4. What information does the Final Table List provide? The table type
 5. State three guidelines for creating table names. No acronyms, be accurate, be unique and meaningful.
6. State two guidelines for composing table descriptions. Be descriptive
 7. How do you assign fields to a table on the Final Table List? By the subject that it describes
 8. State three guidelines for creating field names. Be accurate, use singular, using an ideal field to resolve anomalies
   9. What two problems can poorly designed fields cause? Duplicate data or confusing data.
   
   10. What can you use to resolve field anomalies? Through creating more tables tbh
   11. State three of the Elements of the Ideal Field. Primary key, no multipart or multivalued fields, no calculated fields
    12. Under what condition is redundant data acceptable? Only in a when you have mutiple subjects
13. In general terms, what three steps do you follow to resolve a multi-valued field? You seperate the values then decide whether or not to create a new table
 14. When is it necessary to use a duplicate field in a table? When you have many to many relationships
  15. How can you refine table structures? By creating more tables to reduce redundancy 
   17. What is a subset table? This is an inital table broken up into different tables in order to reduce blank data.


# Chapter 8 




# Section 1 — Relational Foundations (Conceptual & Logical Thinking)

### Goal: Understand structure before implementation.

### Tasks

* [ ] Define in my own words:

  * What is a relational database?
A relational database is a database where you have different tables that are connected to each other via relations or relationships. 


  * What makes a model relational?
  A relational model is a model where you have a table called clients and one called agent, this one agent can have many agents creating a relation between the two, this is the core idea of a relational model.
  * What is an entity?
  * What is an attribute?
  * What is a relationship?
  * What is cardinality?

* [ ] Explain:

  * 1:1 relationships
  * 1:N relationships
  * N:M relationships
  * How N:M is resolved in relational systems

* [ ] Define:

  * Primary key
  * Foreign key
  * Candidate key
  * Surrogate key
  * Natural key

* [ ] Explain normalization:

  * 1NF
  * 2NF
  * 3NF
  * Why over-normalization can hurt analytics

---

### Structural Modeling Practice

* [ ] Design an intentionally bad table.
* [ ] Identify redundancy.
* [ ] Normalize it to 3NF.
* [ ] Explain what improved.
* [ ] Explain what became more complex.

You should feel the modeling tradeoffs.

---

# Section 2 — Entity Relationship Design

### Goal: Think visually and structurally.

* [ ] Design at least 3 ER diagrams (can be hand-drawn or digital):

  * E-commerce system
  * Rental system
  * Event tracking system

For each:

* [ ] Identify entities
* [ ] Identify relationships
* [ ] Define cardinality
* [ ] Define mandatory vs optional relationships
* [ ] Translate diagram into logical tables

No SQL yet.
Just structure.

---

# Section 3 — Logical DDL Design (Structure Only)

### Goal: Implement logical design using SQL DDL.

Focus on structure, not optimization.

---

### Table Design Practice

* [ ] Create 5 independent tables.
* [ ] Create 3 related tables with foreign keys.
* [ ] Create 1 many-to-many bridge table.
* [ ] Use:

  * PRIMARY KEY
  * FOREIGN KEY
  * UNIQUE
  * NOT NULL
  * CHECK
  * DEFAULT

---

### Composite Key Practice

* [ ] Create at least one composite primary key.
* [ ] Create at least one composite foreign key.
* [ ] Explain when composite keys are appropriate.
* [ ] Explain when surrogate keys are better.

---

# Section 4 — Grain Discipline (Warehouse Thinking Begins)

### Goal: Learn to define what one row represents.

For every table:

* [ ] Write one sentence defining the grain.
* [ ] Identify what would violate the grain.
* [ ] Write a query that accidentally duplicates the grain.
* [ ] Refactor the schema or query to fix it.

Practice defining:

* Transaction grain
* Event grain
* Snapshot grain

This is a major mindset shift.

---

# Section 5 — Business Rule Translation

### Goal: Convert business logic into schema rules.

For a fictional business:

* [ ] Define rules in plain English.
* [ ] Convert them into:

  * Constraints
  * Relationships
  * Required fields
  * Unique restrictions

Example rule types:

* “A customer must have a unique email.”
* “An order must belong to exactly one customer.”
* “A rental cannot exist without inventory.”

Translate logic into structure.

---

# Section 6 — Logical Star Schema Design

### Goal: Transition into dimensional modeling (logically).

No performance tuning.
No partitioning.

---

* [ ] Design:

  * 1 fact table
  * 3 dimension tables

* [ ] Clearly define fact grain.

* [ ] Clearly define dimension attributes.

* [ ] Define surrogate keys in dimensions.

* [ ] Define foreign key relationships.

---

### Extension

* [ ] Add a new dimension after initial design.
* [ ] Modify schema logically.
* [ ] Ensure grain remains correct.

This builds warehouse discipline.

---

# Section 7 — Slowly Changing Dimensions (Logical Version)

### Goal: Understand historical tracking conceptually.

* [ ] Define what SCD Type 2 is in your own words.
* [ ] Design a Type 2 dimension table.
* [ ] Define:

  * Start date
  * End date
  * Current flag
* [ ] Explain how fact tables join correctly to historical dimension rows.

No performance concern.
Just correctness.

---

# Section 8 — Data Lifecycle (Logical Behavior)

### Goal: Understand how data evolves over time.

* [ ] Define insert strategy.
* [ ] Define update strategy.
* [ ] Define delete strategy.
* [ ] Define soft delete vs hard delete.
* [ ] Explain late-arriving data conceptually.
* [ ] Explain idempotency conceptually.

You should be able to describe how data moves through your system.

---

# Section 9 — Transactions & ACID (Conceptual Level)

### Goal: Understand integrity guarantees.

* [ ] Define ACID in your own words.
* [ ] Explain why transactions matter.
* [ ] Simulate a logical failure case.
* [ ] Explain what should happen.

Focus on reasoning, not physical locking mechanics.

---

# Section 10 — Schema Evolution (Logical Perspective)

### Goal: Think like a long-term engineer.

* [ ] Design schema v1.
* [ ] Modify to v2.
* [ ] Explain:

  * What changed?
  * What breaks?
  * How to migrate safely?
  * What business rule changed?

This builds engineering maturity.

---

# Section 11 — Documentation Discipline

For every schema:

* [ ] Define purpose.
* [ ] Define grain.
* [ ] Define relationships.
* [ ] Define update lifecycle.
* [ ] Define business assumptions.

This is part of modeling, not optional.

---

# Phase 3 Completion Criteria (Logical Design)

You are done when you can:

* Design normalized relational schemas without examples
* Define table grain instinctively
* Build ER diagrams confidently
* Translate business rules into constraints
* Design star schemas logically
* Implement SCD Type 2 conceptually
* Explain lifecycle behavior
* Modify schema safely
* Think in entities instead of queries



---

Phase 4 will then move into:

Physical implementation
Indexing
Query planner
Partitioning
Storage considerations

