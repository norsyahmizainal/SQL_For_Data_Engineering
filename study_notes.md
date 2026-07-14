# SQL for Data Engineering - Study Notes

**Course:** Luke Barousse (14 hours)  
**Progress:** Lesson 1.4 (Wildcards) — Working through  
**Platform:** MotherDuck (Browser)  
**Dataset:** Job postings data (data_jobs database)

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

## Next Steps

After finishing lesson 1.4 (Wildcards), add that section here.  
Keep this file updated as you progress through the course.
