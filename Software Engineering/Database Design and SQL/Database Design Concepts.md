## Quick Reference

| Term | Definition | Example |
|--------|------------|----------|
| Primary Key (PK) | Column(s) that uniquely identify each row | employee_id |
| Foreign Key (FK) | Column referencing a PK in another table | department_id |
| Candidate Key | Any column set that could uniquely identify a row | SSN, employee_id |
| Alternate Key | Candidate key not chosen as PK | SSN if employee_id is PK |
| Composite Key | Key consisting of multiple columns | (order_id, product_id) |
| Surrogate Key | Artificial/generated identifier | Auto-increment id |
| Natural Key | Real-world identifier | Email, SSN |
| Superkey | Any column set that uniquely identifies rows | {employee_id, name} |
| Entity | Object being modeled | Customer |
| Attribute | Property of an entity | Customer Name |
| Relationship | Connection between entities | Customer places Order |
| Cardinality | Number of related records | 1:1, 1:N, M:N |
| Schema | Database structure definition | Tables + Constraints |
| Constraint | Rule enforced by database | UNIQUE, NOT NULL |

| Normal Form | Main Rule                                                             | Problem It Prevents                                      | Violation Example                                       | Fix                                               |
| ----------- | --------------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------- |
| **1NF**     | Every column contains atomic (indivisible) values                     | Repeating groups, arrays, lists in cells                 | Customer(id, name, phones="111,222")                    | Split into separate rows or table                 |
| **2NF**     | In 1NF, and every non-key attribute depends on the entire primary key | Partial dependency on part of a composite key            | Enrollment(student_id, course_id, student_name)         | Move student_name to Students table               |
| **3NF**     | In 2NF, and no transitive dependencies                                | Non-key attributes depending on other non-key attributes | Employee(emp_id, dept_id, dept_name)                    | Move dept_name to Departments table               |
| **BCNF**    | Every determinant must be a candidate key                             | Certain redundancy still allowed by 3NF                  | CourseRoom(professor, course, room) where course → room | Decompose tables                                  |
| **4NF**     | No non-trivial multivalued dependencies                               | Independent many-to-many facts stored together           | Professor(professor, language, skill)                   | Split into ProfessorLanguages and ProfessorSkills |
| **5NF**     | No join dependencies                                                  | Info only reconstructable through problematic joins      | Complex supplier-part-project relationships             | Decompose into smaller tables                     |

---

# Why Database Design Matters

Good database design aims to:

- Eliminate redundancy
- Prevent inconsistencies
- Maintain data integrity
- Improve query performance
- Simplify maintenance
- Support future growth

Bad design leads to:

- Duplicate data
- Update anomalies
- Insert anomalies
- Delete anomalies
- Difficult queries

---

# Core Concepts

## Entity

A real-world object being modeled.

Examples:

- Customer
- Product
- Employee
- Trade
- Account

Example:

Customer

| customer_id | name |
|------------|--------|
| 1 | Alice |
| 2 | Bob |

---

## Attribute

A property describing an entity.

Customer:

- customer_id
- name
- email
- address

---

## Relationship

Connection between entities.

Example:

Customer → Order

A customer can place many orders.

---

# Keys

## Primary Key (PK)

Uniquely identifies each row.

Properties:

- Unique
- Not NULL
- Stable
- Minimal

Example:

Customers

| customer_id | name |
|------------|------|
| 1 | Alice |
| 2 | Bob |

customer_id is PK.

---

## Foreign Key (FK)

References another table's PK.

Customers

| customer_id | name |
|------------|------|
| 1 | Alice |

Orders

| order_id | customer_id |
|----------|-------------|
| 100 | 1 |

customer_id in Orders is FK.

Relationship:

Order → Customer

---

## Candidate Key

Any column(s) capable of uniquely identifying rows.

Employees

| employee_id | ssn |
|------------|-----|
| 100 | 123-45-6789 |

Possible candidate keys:

- employee_id
- ssn

Both uniquely identify employees.

---

## Alternate Key

Candidate key not chosen as primary.

If employee_id is PK:

Alternate key = ssn

---

## Composite Key

Multiple columns form uniqueness.

OrderItems

| order_id | product_id | qty |
|----------|-----------|------|

Neither column alone is unique.

Together:

(order_id, product_id)

forms PK.

---

## Surrogate Key

Artificial identifier.

Example:

customer_id = 12345

Advantages:

- Never changes
- Fast indexing
- Simple joins

Most modern systems prefer surrogate keys.

---

## Natural Key

Real-world identifier.

Examples:

- Email
- SSN
- VIN
- Passport number

Pros:

- Meaningful

Cons:

- Can change
- Business rules may evolve

---

# Constraints

## NOT NULL

Value required.

Example:

email VARCHAR NOT NULL

---

## UNIQUE

No duplicates allowed.

Example:

UNIQUE(email)

---

## CHECK

Enforces conditions.

Example:

age >= 18

---

## DEFAULT

Provides default value.

Example:

status = 'ACTIVE'

---

# Cardinality

## One-to-One (1:1)

Person ↔ Passport

One passport per person.

One person per passport.

---

## One-to-Many (1:N)

Customer → Orders

One customer:

- many orders

One order:

- one customer

Most common relationship.

---

## Many-to-Many (M:N)

Students ↔ Courses

Many students can take:

- many courses

Implemented using a junction table.

Example:

Enrollment

| student_id | course_id |
|------------|-----------|

---

# Database Anomalies

Normalization exists to prevent these.

---

## Update Anomaly

Same information stored repeatedly.

Example:

| employee | dept | dept_phone |
|-----------|------|-----------|
| Alice | IT | 555-1234 |
| Bob | IT | 555-1234 |

IT changes phone number.

Must update multiple rows.

Risk of inconsistency.

---

## Insert Anomaly

Cannot insert information without unrelated data.

Example:

Cannot add new department until an employee exists.

---

## Delete Anomaly

Deleting one row unintentionally removes information.

Delete last employee in department:

Department information disappears.

---

# Functional Dependencies

Foundation of normalization.

Notation:

A → B

Meaning:

If A is known,
B is determined.

Example:

employee_id → employee_name

Knowing employee_id determines employee_name.

---

# Normalization Overview

Goal:

Reduce redundancy and anomalies.

Progression:

Unnormalized
→ 1NF
→ 2NF
→ 3NF
→ BCNF
→ 4NF
→ 5NF

Most production systems stop at:

- 3NF
- BCNF

---

# First Normal Form (1NF)

## Rule

All columns must contain atomic values.

No arrays.
No lists.

Bad:

| customer | phones |
|----------|---------|
| Alice | 111,222 |

Good:

| customer | phone |
|----------|-------|
| Alice | 111 |
| Alice | 222 |

Requirements:

- Atomic values
- No repeating groups
- Unique rows

---

# Second Normal Form (2NF)

## Rule

Must be in 1NF.

Every non-key attribute must depend on the entire key.

Relevant mainly for composite keys.

---

Example:

Enrollment

| student_id | course_id | student_name |
|-----------|-----------|--------------|

PK:

(student_id, course_id)

Problem:

student_name depends only on student_id.

Not entire composite key.

Violation.

Fix:

Students

| student_id | student_name |

Enrollment

| student_id | course_id |

Now 2NF achieved.

---

# Third Normal Form (3NF)

## Rule

Must be in 2NF.

No transitive dependencies.

Non-key attributes cannot depend on other non-key attributes.

---

Bad:

Employees

| employee_id | dept_id | dept_name |
|------------|----------|----------|

Dependencies:

employee_id → dept_id

dept_id → dept_name

Thus:

employee_id → dept_name

Transitively dependent.

Violation.

---

Fix

Employees

| employee_id | dept_id |

Departments

| dept_id | dept_name |

Now:

dept_name stored once.

---

# Boyce-Codd Normal Form (BCNF)

Stronger version of 3NF.

Rule:

Every determinant must be a candidate key.

---

Example:

| professor | course | room |
|------------|--------|------|

Suppose:

course → room

professor → course

Neither determinant may be a candidate key.

BCNF forces decomposition.

---

Practical Rule:

BCNF handles edge cases where 3NF still allows redundancy.

---

# Fourth Normal Form (4NF)

Addresses multivalued dependencies.

Example:

Professor

| professor | language | skill |
|-----------|----------|--------|
| Smith | English | SQL |
| Smith | English | Python |
| Smith | French | SQL |
| Smith | French | Python |

Languages and skills are independent.

Redundant combinations appear.

Decompose into:

ProfessorLanguages

ProfessorSkills

Now 4NF.

---

# Fifth Normal Form (5NF)

Addresses join dependencies.

Very rare.

Usually encountered only in highly complex enterprise schemas.

Most engineers never explicitly normalize to 5NF.

---

# Denormalization

Sometimes we intentionally break normalization.

Reason:

Performance.

Example:

Store customer_name directly in Orders.

Benefits:

- Faster reads
- Fewer joins

Costs:

- More redundancy
- Possible inconsistencies

Common in:

- Data warehouses
- Analytics systems
- Reporting systems

---

# OLTP vs OLAP

## OLTP (Operational Databases)

Examples:

- Banking
- Trading
- E-commerce

Characteristics:

- Heavy writes
- Small transactions
- Highly normalized

Typical design:

3NF / BCNF

---

## OLAP (Analytics)

Examples:

- Snowflake
- BigQuery
- Redshift

Characteristics:

- Heavy reads
- Large aggregations
- Often denormalized

Typical design:

Star Schema

---

# Star Schema

Fact table surrounded by dimensions.

FactTrades

| trade_id | trader_id | date_id | pnl |

Dimensions:

TraderDimension

DateDimension

InstrumentDimension

Benefits:

- Fast analytics
- Simple joins

Common in quant finance.

---

# Indexes

## Purpose

Speed up lookups.

Without index:

Full table scan

O(N)

With index:

Usually O(log N)

---

Example

Index on:

email

Query:

SELECT *
FROM Users
WHERE email='alice@example.com';

Becomes much faster.

---

## Clustered Index

Physical row order follows index.

Only one per table.

---

## Non-Clustered Index

Separate lookup structure.

Many allowed.

---

# Common Interview Questions

## Why not use email as primary key?

Problems:

- Can change
- Long string comparisons
- Business rules evolve

Prefer:

customer_id

with UNIQUE(email)

---

## When use composite key?

Junction tables:

Enrollment

OrderItems

PositionSnapshots

---

## Why normalize?

Prevents:

- Update anomalies
- Insert anomalies
- Delete anomalies
- Redundancy

---

## Why denormalize?

Improves:

- Read performance
- Reporting queries
- Analytics workloads

---

# Quant Finance Examples

## Trades

Trades

| trade_id | trader_id | instrument_id | qty | price |

Foreign keys:

- trader_id → Traders
- instrument_id → Instruments

---

## Market Data

Instruments

| instrument_id | symbol |

Quotes

| quote_id | instrument_id | bid | ask | ts |

One instrument:

many quotes

Relationship:

Instrument 1:N Quotes

---

## Order Book

Orders

| order_id | symbol | side | qty | price |

Executions

| execution_id | order_id | fill_qty |

One order:

many executions

Relationship:

Order 1:N Execution

---

# Practical Design Checklist

1. Identify entities.
2. Identify attributes.
3. Choose primary keys.
4. Define relationships.
5. Add foreign keys.
6. Normalize to at least 3NF.
7. Add indexes for common queries.
8. Consider denormalization only after measuring performance.
9. Enforce constraints.
10. Document assumptions and dependencies.