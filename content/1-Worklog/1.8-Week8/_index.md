---
title: "Week 8 Worklog"
date: 2025-10-23
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives:

* Understand database fundamentals: Primary Key, Foreign Key, and Normalization.
* Learn Indexing, Partitioning, and Execution Plans for query optimization.
* Compare RDBMS vs NoSQL, and OLTP vs OLAP/Data Warehouse concepts.

### Tasks to be carried out this week:

| Day | Task                                              | Start Date | Completion Date | Reference Material                                                                                          |
| --- | ------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------------------------------------- |
| 2   | Basic database concepts on AWS & managed services | 10/23/2025 | 10/23/2025      | [YouTube Video](https://www.youtube.com/watch?v=OOD2RwWuLRw&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=217) |
| 3   | Primary Key, Foreign Key & Normalization          | 10/24/2025 | 10/24/2025      | cont                                                                                                        |
| 4   | Indexing, Partitioning & Execution Plan           | 10/25/2025 | 10/25/2025      | cont                                                                                                        |
| 5   | Database Logs, Buffer Cache & Transactions        | 10/26/2025 | 10/26/2025      | cont                                                                                                        |
| 6   | RDBMS vs NoSQL, OLTP vs OLAP / Data Warehouse     | 10/27/2025 | 10/27/2025      | cont                                                                                                        |

### Week 8 Achievements:

#### Basic database concepts on AWS & managed services
* AWS manages the database infrastructure.
* You focus on data design & query optimization.
* Databases handle multi-user access via sessions.
* Must understand schema, keys, indexes, partitions, logs before using AWS DB services.

#### Primary Key, Foreign Key & Normalization

**Primary Key (PK)**

* Uniquely identifies each record in a table.
* Cannot be duplicated or null.
* Ensures data integrity.

**Foreign Key (FK)**

* A column that references the Primary Key of another table.
* Creates relationships between tables.
* Ensures referential integrity.

**Normalization**

* Organizes data to **avoid duplication** and  **reduce storage usage** .
* Ensures consistency and improves data integrity.
* Common forms: 1NF, 2NF, 3NF (increasing levels of structure).

#### Indexing, Partitioning & Execution Plan

**Indexing**

* Speeds up data retrieval by avoiding full table scans.
* Requires extra storage and increases write cost.
* Useful for frequently queried columns.

**Partitioning**

* Splits large tables into smaller parts for faster queries.
* Queries only scan relevant partitions instead of the whole table.
* Improves performance and scalability.

**Execution Plan**

* The database's step-by-step strategy for running a query.
* Shows whether indexes, scans, or joins are used.
* Essential for query optimization.

#### Database Logs, Buffer Cache & Transactions

**Database Logs**

* Record all changes made to the database.
* Enable recovery after failures and support replication.
* Ensure data can be restored to a consistent state.

**Buffer Cache**

* In-memory area that stores frequently accessed data blocks.
* Reduces disk reads and improves query speed.
* Holds data temporarily before writing to disk.

**Transactions**

* A set of operations that must be completed successfully as a unit.
* Follow **ACID** principles to ensure reliability.
* Only committed transactions are written permanently to the database.

#### RDBMS vs NoSQL, OLTP vs OLAP / Data Warehouse

**RDBMS vs NoSQL**

**RDBMS (Relational Databases)**

* Structured tables with fixed schema.
* Strong consistency, supports complex queries (SQL).
* Scales vertically (add more power to one server).
* Best for transactional, structured data.

**NoSQL**

* Flexible or schema-less data model (document, key-value, wide-column, graph).
* High scalability, often horizontal scaling.
* Optimized for performance and large datasets.
* Suitable for unstructured/semi-structured data.

#### **OLTP vs OLAP / Data Warehouse**

**OLTP (Online Transaction Processing)**

* Handles real-time, frequent read/write operations.
* Used for transactions like orders, payments, banking.
* Requires high consistency and fast response.

**OLAP (Online Analytical Processing) / Data Warehouse**

* Stores large amounts of historical data for analysis.
* Supports complex queries for reporting and business insights.
* Optimized for read-heavy, analytical workloads, not frequent updates.
