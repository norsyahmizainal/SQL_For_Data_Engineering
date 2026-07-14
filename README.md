# SQL for Data Engineering - Study Notes

**Course:** Luke Barousse (14 hours)  
**Progress:** Lesson 1.4 (Wildcards) — Working through  
**Platform:** MotherDuck (Browser)  
**Dataset:** Job postings data (data_jobs database)

---

## 📊 Progress Tracker

### Part 1 - Fundamentals (First Half)

**Environment Setup**
- [x] 1.0 - What is SQL & Databases
- [x] 1.0a - MotherDuck Setup
- [x] 1.0b - Terminal Intro
- [x] 1.0c - DuckDB Installation
- [x] 1.0d - VS Code Setup

**Core SQL Syntax**
- [x] 1.1 - Basic Keywords (SELECT, FROM, LIMIT)
- [x] 1.2 - WHERE & Filtering
- [x] 1.3 - Operators (Comparison & Logical)
- [ ] 1.4 - Wildcards & LIKE (IN PROGRESS)
- [ ] 1.4b - Aliases (AS keyword)
- [ ] 1.5 - Arithmetic Operators
- [ ] 1.6 - Aggregate Functions
- [ ] 1.6b - GROUP BY Clause
- [ ] 1.6c - HAVING Clause
- [ ] 1.7 - Data Modeling Fundamentals
- [ ] 1.7b - Database Hierarchy
- [ ] 1.7c - Keys (Primary, Foreign, Composite)
- [ ] 1.7d - Entity Relationship Diagrams (ERD)
- [ ] 1.7e - Table Relationships
- [ ] 1.7f - Information Schema
- [ ] 1.8 - JOINs (LEFT, RIGHT, INNER, FULL)
- [ ] 1.8a - LEFT JOIN Deep Dive
- [ ] 1.8b - RIGHT JOIN
- [ ] 1.8c - INNER JOIN
- [ ] 1.8d - FULL OUTER JOIN
- [ ] 1.8e - JOIN Real-World Examples
- [ ] 1.9 - Order of Execution
- [ ] 1.9b - EXPLAIN Statement
- [ ] 1.9c - EXPLAIN ANALYZE

**PROJECT 1 - EDA**
- [ ] 1.10 - Project Overview & Setup
- [ ] 1.10a - Query 1: Most In-Demand Skills
- [ ] 1.10b - Query 2: Highest Paying Skills
- [ ] 1.10c - Query 3: Optimal Skills
- [ ] 1.10d - Build README & Portfolio
- [ ] 1.10e - Push to GitHub

### Part 2 - Production Level (Second Half)

**DDL/DML Commands**
- [ ] 2.1 - CREATE TABLE
- [ ] 2.1b - Data Types
- [ ] 2.1c - Table Constraints
- [ ] 2.2 - ALTER TABLE
- [ ] 2.2b - DROP TABLE
- [ ] 2.3 - INSERT INTO
- [ ] 2.3b - UPDATE
- [ ] 2.3c - DELETE FROM

**Advanced Features**
- [ ] 2.4 - Star Schema Design
- [ ] 2.4b - Dimensional Modeling
- [ ] 2.5 - CTEs (WITH Clause)
- [ ] 2.5b - Subqueries
- [ ] 2.6 - Window Functions
- [ ] 2.6b - Advanced Window Functions

**Production Pipelines**
- [ ] 2.7 - Building ELT Pipelines
- [ ] 2.7b - Data Transformation
- [ ] 2.7c - Incremental Loading
- [ ] 2.7d - Data Mart Design

**PROJECT 2 - Production Pipeline**
- [ ] 2.8 - Project Overview
- [ ] 2.8a - Design Warehouse Schema
- [ ] 2.8b - Load Raw Data
- [ ] 2.8c - Transform & Create Data Marts
- [ ] 2.8d - Optimization
- [ ] 2.8e - Publishing & Deployment

---

## 📈 Overall Progress

```
Part 1 Completed: 5/24 lessons (21%)
Part 2 Completed: 0/16 lessons (0%)
Total: 5/40 lessons (12.5%)

████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 12.5%
```

---

---

## Lesson 1.1 - Basic Keywords (SELECT, FROM, LIMIT)

**What it does:**  
SELECT picks which columns to show, FROM picks which table to read from, LIMIT caps how many rows come back

**Syntax:**
```sql
SELECT column1, column2
FROM table_name
LIMIT number_of_rows;
```

**Real examples (from MotherDuck - job_postings_fact table):**

Example 1 - Select all columns:
```sql
SELECT *
FROM job_postings_fact
LIMIT 10;
```

Example 2 - Select specific columns:
```sql
SELECT job_id, job_title_short, salary_year_average
FROM job_postings_fact
LIMIT 10;
```

Example 3 - Select with readability (spacing doesn't matter):
```sql
SELECT 
  job_id,
  job_title_short,
  salary_year_average
FROM job_postings_fact
LIMIT 10;
```

**Key things to remember:**
- Asterisk (*) = all columns
- Column names are case-sensitive (use lowercase for table/column names)
- LIMIT always goes at the END
- Multiple columns need commas between them
- Semicolon (;) is best practice to end queries

---

## Lesson 1.2 - WHERE & Filtering

**What it does:**  
WHERE filters rows based on conditions — only rows that match the condition are returned

**Syntax:**
```sql
SELECT columns
FROM table
WHERE condition;
```

**Real examples:**

Example 1 - Filter by text (needs single quotes):
```sql
SELECT job_title_short, salary_year_average, job_location
FROM job_postings_fact
WHERE job_title_short = 'Data Engineer'
LIMIT 10;
```

Example 2 - Filter by number (no quotes needed):
```sql
SELECT job_id, job_title_short, salary_year_average
FROM job_postings_fact
WHERE salary_year_average > 100000
LIMIT 10;
```

Example 3 - Filter by country:
```sql
SELECT job_title_short, job_location, job_country
FROM job_postings_fact
WHERE job_country = 'United States'
LIMIT 10;
```

Example 4 - Null values (use IS NULL or IS NOT NULL):
```sql
SELECT job_id, salary_year_average
FROM job_postings_fact
WHERE salary_year_average IS NOT NULL
LIMIT 10;
```

**Key things to remember:**
- Text values need single quotes: `'United States'`
- Numbers don't need quotes: `100000`
- NULL is special — use `IS NULL` or `IS NOT NULL`, not `= NULL`
- WHERE comes after FROM in the query order

---

## Lesson 1.3 - Operators (Comparison & Logical)

**What it does:**  
Operators let you create more complex conditions to filter data

**Comparison Operators:**
- `=` equals
- `!=` or `<>` not equals
- `>` greater than
- `<` less than
- `>=` greater than or equal
- `<=` less than or equal

**Logical Operators:**
- `AND` — all conditions must be true (restrictive)
- `OR` — any condition can be true (inclusive)
- `NOT` — reverses the condition

**Real examples:**

Example 1 - Greater than:
```sql
SELECT job_title_short, salary_year_average
FROM job_postings_fact
WHERE salary_year_average > 100000
LIMIT 10;
```

Example 2 - Not equal:
```sql
SELECT job_title_short, job_schedule_type
FROM job_postings_fact
WHERE job_schedule_type != 'Full-time'
LIMIT 10;
```

Example 3 - AND (both conditions must be true — narrows results):
```sql
SELECT job_title_short, job_location, salary_year_average
FROM job_postings_fact
WHERE job_title_short = 'Data Engineer'
  AND salary_year_average > 100000
LIMIT 10;
```

Example 4 - OR (either condition can be true — widens results):
```sql
SELECT job_title_short
FROM job_postings_fact
WHERE job_title_short = 'Data Engineer'
  OR job_title_short = 'Data Analyst'
LIMIT 10;
```

Example 5 - NOT (opposite):
```sql
SELECT job_title_short, job_schedule_type
FROM job_postings_fact
WHERE job_schedule_type != 'Full-time'
-- Same as: WHERE NOT (job_schedule_type = 'Full-time')
LIMIT 10;
```

Example 6 - Complex condition (multiple ANDs + ORs):
```sql
SELECT job_title_short, job_location, salary_year_average
FROM job_postings_fact
WHERE (job_title_short = 'Data Engineer' OR job_title_short = 'Data Analyst')
  AND salary_year_average > 80000
  AND job_country = 'United States'
LIMIT 10;
```

**Key things to remember:**
- AND is restrictive (narrows results — fewer rows)
- OR is inclusive (widens results — more rows)
- Use parentheses () to group conditions for clarity
- Order of operations: NOT, AND, OR (left to right)
- BETWEEN is easier than `>= AND <=`:
  ```sql
  WHERE salary_year_average BETWEEN 80000 AND 120000
  ```

---

## Quick Reference - SQL Clause Order

Remember this order or queries will fail:

```
SELECT columns
FROM table
WHERE conditions
GROUP BY columns (coming in lesson 1.6)
HAVING aggregate_conditions (coming in lesson 1.6)
ORDER BY columns (coming in lesson 1.5)
LIMIT number
```

---

## MotherDuck Quick Tips

- **Run query:** Click the play button (▶️) or Ctrl+Enter
- **See results:** Scroll down to see table output
- **Check row count:** Top right shows "X rows returned"
- **Copy results:** Click copy icon to clipboard

---

## Job Postings Data (Reference)

**Main table:** `job_postings_fact`

Key columns:
- `job_id` — unique job posting ID
- `job_title` — full job title from posting
- `job_title_short` — cleaned up version (Data Engineer, Data Analyst, etc.)
- `salary_year_average` — average yearly salary (numeric)
- `salary_hour_average` — average hourly rate (numeric)
- `job_country` — country of the job
- `job_location` — city, state/country
- `job_work_from_home` — true/false for remote
- `job_schedule_type` — Full-time, Contract, Part-time, Temporary
- `job_via` — where job was posted (LinkedIn, Indeed, etc.)
- `company_id` — links to company_dim table
- `job_posted_date` — when job was posted (date)

Other tables (will use later):
- `company_dim` — company info (name, link, etc.)
- `skills_dim` — skill names (SQL, Python, AWS, etc.)
- `skills_job_dim` — bridge table linking jobs to skills

---

---

## Lesson 1.4 - Wildcards & LIKE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.4b - Aliases (AS Keyword)

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.5 - Arithmetic Operators

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.6 - Aggregate Functions

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.6b - GROUP BY Clause

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.6c - HAVING Clause

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7 - Data Modeling Fundamentals

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7b - Database Hierarchy

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7c - Keys (Primary, Foreign, Composite)

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7d - Entity Relationship Diagrams (ERD)

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7e - Table Relationships

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.7f - Information Schema

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8 - JOINs Introduction

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8a - LEFT JOIN

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8b - RIGHT JOIN

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8c - INNER JOIN

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8d - FULL OUTER JOIN

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.8e - JOIN Real-World Examples

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.9 - Order of Execution

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.9b - EXPLAIN Statement

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.9c - EXPLAIN ANALYZE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 1.10 - PROJECT 1: EDA Overview

**What it does:**

**Key questions to answer:**
- Most in-demand skills?
- Highest paying skills?
- Optimal skills (demand + salary)?

**Key things to remember:**

---

## Lesson 1.10a - Query 1: Most In-Demand Skills

**Problem statement:**

**SQL Query:**
```sql

```

**Results & Insights:**

---

## Lesson 1.10b - Query 2: Highest Paying Skills

**Problem statement:**

**SQL Query:**
```sql

```

**Results & Insights:**

---

## Lesson 1.10c - Query 3: Optimal Skills

**Problem statement:**

**SQL Query:**
```sql

```

**Results & Insights:**

---

## Lesson 1.10d - Build README & Portfolio

**Key components:**
- Executive summary
- Problem statement
- Tech stack
- Analysis findings
- Skills demonstrated

---

## Lesson 1.10e - Push to GitHub

**Steps:**
- [ ] Create GitHub repo
- [ ] Add study_notes.md
- [ ] Add project files
- [ ] Update README
- [ ] Commit to main
- [ ] Share on LinkedIn

---

---

# PART 2 - PRODUCTION LEVEL

---

## Lesson 2.1 - CREATE TABLE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.1b - Data Types

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.1c - Table Constraints

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.2 - ALTER TABLE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.2b - DROP TABLE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.3 - INSERT INTO

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.3b - UPDATE

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.3c - DELETE FROM

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.4 - Star Schema Design

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.4b - Dimensional Modeling

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.5 - CTEs (Common Table Expressions)

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.5b - Subqueries

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.6 - Window Functions

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.6b - Advanced Window Functions

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.7 - Building ELT Pipelines

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.7b - Data Transformation

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.7c - Incremental Loading

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.7d - Data Mart Design

**What it does:**

**Syntax:**
```sql

```

**Real examples:**
```sql

```

**Key things to remember:**

---

## Lesson 2.8 - PROJECT 2: Production Pipeline Overview

**What it does:**

**Key components:**
- Design warehouse schema
- Load raw data
- Transform & create data marts
- Optimize queries

**Key things to remember:**

---

## Lesson 2.8a - Design Warehouse Schema

**Schema design:**

**SQL:**
```sql

```

---

## Lesson 2.8b - Load Raw Data

**Data loading approach:**

**SQL:**
```sql

```

---

## Lesson 2.8c - Transform & Create Data Marts

**Transformations:**

**SQL:**
```sql

```

---

## Lesson 2.8d - Optimization

**Optimization techniques:**

**SQL:**
```sql

```

---

## Lesson 2.8e - Publishing & Deployment

**Deployment notes:**

---

---

## 🎯 How to Use This Document

1. **After each lesson**, check the box: `- [x] Lesson X.X - Title`
2. **Fill in the section** with:
   - What the concept does (1 sentence)
   - Syntax template
   - Real example from MotherDuck
   - Key things to remember
3. **Copy-paste your query results** if helpful
4. **Update progress tracker** at the top

---

## 💡 Quick Tips

- **Keep it simple** — 5 mins per lesson max
- **Test on MotherDuck** — Always run the examples
- **Update GitHub** — Push changes weekly
- **Reference later** — Use this for Lance Data work

---

## 📚 Reference Sections (Don't Change These)

[Kept from original content below - Quick Reference, MotherDuck Tips, Job Postings Data]
