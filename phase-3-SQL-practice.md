# Phase 3.0 — Schema Ownership & Data Modeling Rubric

## Objective: Move from Query Writer → Schema Designer

This phase is complete when you can design, modify, protect, and evolve a relational schema confidently without relying on examples.

---

# Section 1 — Relational Database Foundations (Structural Thinking)

### Goal: Understand how relational systems are structured before analytics.

### Tasks

* [ ] Write in your own words:

  * What is a relational database?
  * What makes something “relational”?
  * What is normalization?
  * What problem does normalization solve?

* [ ] Create three tables:

  1. Completely unnormalized
  2. 1NF
  3. 3NF
     Compare them.

* [ ] Identify redundancy in a poorly designed table.

* [ ] Refactor that table into normalized form.

* [ ] Explain tradeoffs between normalization and denormalization.

* [ ] Explain OLTP vs OLAP.

* [ ] Identify which schema style fits which workload.

You should feel the tension between clean modeling and analytics performance.

---

# Section 2 — DDL Mastery (Owning the Structure)

### Goal: Become comfortable creating and evolving schemas.

### Table Creation Depth

* [ ] Create 5 independent tables from scratch.

* [ ] Create 3 tables with foreign key relationships.

* [ ] Create at least one table with:

  * Composite primary key
  * Composite foreign key

* [ ] Use:

  * PRIMARY KEY
  * UNIQUE
  * NOT NULL
  * CHECK
  * DEFAULT
  * REFERENCES

### Schema Evolution

* [ ] Add columns after creation.

* [ ] Remove columns safely.

* [ ] Rename columns.

* [ ] Rename tables.

* [ ] Add constraints to existing tables.

* [ ] Drop constraints safely.

* [ ] Simulate schema migration:

  * Create v1 schema
  * Modify to v2
  * Ensure data integrity survives

You should stop being afraid of changing schemas.

---

# Section 3 — Data Lifecycle (DML Depth)

### Goal: Understand how data flows and mutates.

* [ ] Insert single rows.
* [ ] Bulk insert many rows.
* [ ] Insert from SELECT.
* [ ] Update using JOIN.
* [ ] Update using subquery.
* [ ] Delete with condition.
* [ ] Delete using JOIN logic.
* [ ] TRUNCATE vs DELETE comparison.
* [ ] Use ON CONFLICT (UPSERT).

### Advanced Lifecycle Practice

* [ ] Simulate late-arriving data.
* [ ] Simulate duplicate data.
* [ ] Build de-duplication query.
* [ ] Build idempotent insert logic.
* [ ] Create data validation checks (row counts, null checks).

You should understand how real pipelines behave.

---

# Section 4 — Keys & Constraints (Integrity Enforcement)

### Goal: Protect the system intentionally.

* [ ] Create surrogate keys.
* [ ] Create natural keys.
* [ ] Compare tradeoffs.
* [ ] Break a foreign key intentionally.
* [ ] Break a CHECK constraint intentionally.
* [ ] Test cascade delete.
* [ ] Test cascade update.
* [ ] Understand ON DELETE behaviors.

You should know how the database prevents corruption.

---

# Section 5 — Table Grain Discipline

### Goal: Think like a warehouse engineer.

For every table you create:

* [ ] Write one sentence defining its grain.
* [ ] Identify what would violate that grain.
* [ ] Write a query that accidentally duplicates grain.
* [ ] Fix it.

Practice:

* Transaction grain
* Snapshot grain
* Event grain

You should start thinking in “rows represent events.”

---

# Section 6 — Star Schema Construction

### Goal: Build your first mini warehouse.

* [ ] Create at least:

  * 1 fact table
  * 3 dimension tables
* [ ] Define fact grain clearly.
* [ ] Use surrogate keys in dimensions.
* [ ] Connect via foreign keys.
* [ ] Insert sample data.
* [ ] Write analytical queries on top.

### Extension

* [ ] Add a new dimension later.
* [ ] Modify fact table accordingly.
* [ ] Maintain referential integrity.

This is the bridge into analytics engineering.

---

# Section 7 — Slowly Changing Dimensions (Type 2)

### Goal: Implement historical tracking manually.

* [ ] Create SCD Type 2 dimension table.
* [ ] Insert initial state.
* [ ] Simulate attribute change.
* [ ] Close old row.
* [ ] Insert new row.
* [ ] Query current version.
* [ ] Query historical version.
* [ ] Join fact table to correct historical dimension version.

You should understand why this is hard and important.

---

# Section 8 — Transactions & ACID

### Goal: Understand reliability guarantees.

* [ ] Use BEGIN / COMMIT.
* [ ] Use ROLLBACK.
* [ ] Use SAVEPOINT.
* [ ] Simulate failure mid-transaction.
* [ ] Test isolation levels.
* [ ] Simulate dirty reads in two sessions.

You should understand how systems stay consistent.

---

# Section 9 — Views & Abstraction

### Goal: Learn abstraction layers.

* [ ] Create view for analytical summary.
* [ ] Modify it.
* [ ] Drop it.
* [ ] Create materialized view.
* [ ] Refresh manually.
* [ ] Compare performance.
* [ ] Explain when to use each.

This introduces reporting layers.

---

# Section 10 — Partitioning Introduction

### Goal: Begin thinking about scale.

* [ ] Create table partitioned by date.
* [ ] Insert into multiple partitions.
* [ ] Query specific partition.
* [ ] Compare performance to unpartitioned table.
* [ ] Explain partition pruning.

This prepares you for real large datasets.

---

# Section 11 — Documentation Discipline

For every schema:

* [ ] Define grain.
* [ ] Define purpose.
* [ ] Define relationships.
* [ ] Define update strategy.
* [ ] Define expected row growth.

This is what separates hobby work from engineering.

---

# Phase 2.5 Completion Criteria

You are done when you can:

* Design relational schema without examples
* Normalize and denormalize intentionally
* Define table grain clearly
* Build star schema from scratch
* Implement SCD Type 2 correctly
* Enforce constraints confidently
* Use transactions safely
* Modify schema without fear
* Think about data lifecycle

At that point:

You are no longer “learning SQL.”

You are learning database engineering.

---

This phase is critical.

If you master this properly, Phase 3 (performance + indexing + query planner) will make much more sense.

Now I want to ask something important:

Do you want this to be a 2-week intensive block or a 4-week structured progression integrated into your 4-hour daily plan?
