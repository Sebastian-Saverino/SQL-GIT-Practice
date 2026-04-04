# Data Warehousing Toolkit Notes

For this section of notes, I will be reading *The Data Warehousing Toolkit*.

This will be slightly different from my previous notes, as some parts of this book are outdated. Because of that, the depth of notes per chapter may vary depending on relevance.

The goal here is not to capture everything, but to focus on what is still useful and applicable to modern data work.

---

## How I’m Approaching This Book

I am primarily looking for:
- core concepts that still apply today  
- patterns that show up in real-world systems  
- terminology that I should understand  
- ideas that expand how I think about data modeling  

One thing to keep in mind:

Modern data analytics does not always follow a strict, upfront modeling process with stakeholders like Kimball describes. With modern tooling, it’s much easier to iterate and remodel later.

So instead of:
> design everything upfront

It is often:
> start simple → refine as usage grows

---

## Key Areas of Focus

### 1. Kimball’s Four-Step Dimensional Modeling Process

This is the foundation.

I want to understand:
- how Kimball approaches modeling step-by-step  
- how this compares to modern workflows  
- where this is still useful vs where it is less practical today  

---

### 2. Fact Tables (Core Concept)

Focus areas:
- the three main types of fact tables  
- the fourth (less common) type  

Goal:
Understand when and why each type is used.

---

### 3. Dimension Tables

Key concepts to focus on:
- degenerate dimensions  
- multiple hierarchies  

Keys (important):
- surrogate keys  
- natural keys  
- durable keys  
- “supernatural” keys  

Goal:
Understand how dimensions are structured and identified.

---

### 4. Integration via Conformed Dimensions

Focus areas:
- Enterprise Data Warehouse Bus Architecture  
- Bus Matrix  
- Opportunity / Stakeholder Matrix  

Note:
This is more historical and not commonly used today due to modern cloud data warehouses.

Still useful because:
- it shows how systems were designed at scale  
- it builds appreciation for how far tooling has come  

---

### 5. Slowly Changing Dimensions (Very Important)

Read all of this.

This is still highly relevant.

Also worth understanding:
- how SCDs are handled in modern systems  
- how cloud warehouses may simplify or change implementation  

---

### 6. Dimension Hierarchies

Go through all of this.

Main takeaway:
Some business scenarios create complex or non-standard hierarchies.

Example:
- employees managing each other in different contexts  

Goal:
Expand understanding of edge cases and modeling flexibility.

---

### 7. Advanced Fact Table Techniques

Skim:
- multiple currency facts  
- timespan tracking  

Focus:
- late arriving facts (important and common pattern)

---

### 8. Advanced Dimension Techniques

Focus areas:
- audit dimensions (data quality related, still very relevant)  
- late arriving dimensions  

Goal:
Understand how to handle imperfect or delayed data.

---

### 9. Special Purpose Schemas

Key concept:
- error event schemas  

This ties into:
- data quality  
- tracking failures or anomalies  

Still very applicable in modern systems.

---

## Overall Goal

The goal is not to follow Kimball exactly, but to:

- understand how dimensional modeling is *supposed* to work  
- recognize patterns when they appear in real systems  
- build a stronger mental model for designing analytics data  

This is more about:
> understanding the design space  

than:
> copying exact implementations

source: https://www.holistics.io/blog/how-to-read-data-warehouse-toolkit/


# Chapter One

## Different Worlds of Data Capture and Data Analysis

Understanding the idea of the operational database where its primary goal is to hold the individuals data opposed to the idea of DW/BI, this is the idea of creating a data warehouse and the focus on business intelligence to figure out why did those transactions last week fail to the meet the quota unlike the week prior. Bad example but you get the idea.

## Goals of Data Warehousing and Business Intelligence

These can be the questions business management may be asking.

“We collect tons of data, but we can’t access it.”
“We need to slice and dice the data every which way.”
“Business people need to get at the data easily.”
“Just show me what is important.”
“We spend entire meetings arguing about who has the right numbers rather 
than making decisions.”
“We want people to use information to support more fact-based decision 
making.

The DW/Bi system must make information easily accessible. 
    The data should be intuitive and obvious to anyone using the data.
    They want the data simple and fast.

The DW/BI system must present information consistently.
    The data must be credible.
    Simple, if one table is named one thing it should be the same name in the DW.

The DW/BI system must adapt to change.
     User needs, business conditions, data,  and technology are all subject to change.

The DW/BI system must present information in a timely way.
    As the DW/BI system is  used more intensively for operational decisions, raw data may need to be converted into actionable information within hours, minutes, or even seconds.

The DW/BI system must be a secure bastion that protects the information assets.
    You must effectively control access of the organization's confidential information

The DW/BI system must serve as the authoritative and trustworthy foundation for improved decision making. 
     The original label that predates DW/BI is still the best description of what you are designing: a decision support system.
     Keep in mind this data is used to make decisions.

The business community must accept the DW/BI system to deem it successful.
    Business users will embrace the DW/BI system if it is the “simple and fast” source for actionable information.


Clearly, you need to bring a spectrum of skills to the party to behave like you’re a hybrid DBA/MBA.

You need to understand the data as well how to know to create the needed DW/BI for the users and so on.

Quick analogy, data is a like a magazine, as you need to bring value to that the data for the people that want to use it same way someone wants an attractive magazine.

## Dimensional Modeling Introduction

This is the preferred technique for presenting analytics data because it addresses two simultaneous requirements:
    
    Deliver data that's understandable to the business users.
    Deliver fast query performance.

Simplicity is the language we strive for as this helps the users better understand the data as well allows the software to navigate and deliver results quickly and efficiently.

Our wonderful dimensional design might make you think of a normalized data base in a RDBMS but they are quite different from each other as a normalized structure strives to remove data redundancies.

As we already know of our wonderful relational database is that its typically consists of many different tables with relations between them, but we can also do this with dimensional modelings as well.

For note: A dimensional model contains the same information as a normalized model, but packages the data in a format that delivers user understandability, query performance, and resilience to change.

We're introduced with the idea of OLAP Cubes, this like dimensional modelings is an idea of how an OLAP system may hold data.

THis is best said from the book pretty much OLAP Cubes support better query support but sacrifice but you pay a load performance price for these capabilities.

When data is loaded into an OLAP cube, it is stored and indexed using formats and techniques that are designed for dimensional data. Performance aggregations or precalculated summary tables are often created and managed by the OLAP cube engine. Consequently, cubes deliver superior query performance because of the precalculations, indexing strategies, and other optimizations. Business users can drill down or up by adding or removing attributes from their analyses with excellent performance without issuing new queries. OLAP cubes also provide more analytically robust functions that exceed those available with SQL. The downside is that you pay a load performance price for these capabilities, especially with large data sets.



![Star vs Cube](image.png)

We generally recommend that detailed, atomic information be loaded into a star schema; optional OLAP cubes are then populated from the star schema. For this reason, most dimensional modeling techniques in this book are couched in terms of a relational star schema.

The big takeaway of an OLAP cube is the logical implementation of it is still prevalent but with the increase of computers hardware they are not as needed.




# Fact Tables for Measurements

The fact table in a dimensional model stores the performance measurements resulting from an organization's business process events. You're fact represents a business measure. 
Each row in a fact table corresponds to a measurement event such as the dollar sales, unit quantity of each product for example.

The data on each row is at a specific level detail, referred to as the grain, such as one row per product sold on a sales transaction. 
 
A core tenet is that all measurements rows in a fact table must be a the same grain so they all have to be as specific as the next if one row adds too much to the grain then it could effect the integrity of the data.

Measurement event in the physical world has a one-to-one relationship to a single row in the corresponding fact table is a bedrock principle for dimensional modeling.

Most useful facts are numeric such as dollar sales amount.


Fact tables make up 90% of dimensional models


Fact table grains fall into one of the three categories: transaction, periodic snapshot, and accumulating snapshot. 

Transaction grain fact tables are the most common.

Fact tables have two or more foreign keys to the connect the dimension tables, You access the fact table via the dimension tables joint to it

Fact table generally has its own primary key composed of a subset of foreign keys called our lovely composite key. 

Fact tables express many to many relationships

Stopped at page 13 




