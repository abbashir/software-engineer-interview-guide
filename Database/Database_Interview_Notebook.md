# Database Interview Notebook (SQL, MySQL, PostgreSQL)

> **Mid to Senior Level Preparation - 100 Essential Questions & Answers**

## 1. General SQL & Relational Database Concepts

### Q1. What is an RDBMS?
**Answer:** A Relational Database Management System stores data in structured tables with rows and columns. It enforces relationships between tables and uses SQL for data manipulation.

### Q2. What are the ACID properties?
**Answer:** Atomicity (all or nothing), Consistency (valid state transitions), Isolation (concurrent transactions don't interfere), and Durability (committed data is saved permanently).

### Q3. What is Database Normalization?
**Answer:** The process of organizing data to reduce redundancy and improve data integrity by dividing large tables into smaller, related ones.

### Q4. Explain 1NF, 2NF, and 3NF.
**Answer:** 1NF: No repeating groups, atomic values. 2NF: 1NF + no partial dependency (all non-key attributes depend on the whole primary key). 3NF: 2NF + no transitive dependency (non-key attributes do not depend on other non-key attributes).

### Q5. What is BCNF (Boyce-Codd Normal Form)?
**Answer:** A stronger version of 3NF where every determinant must be a candidate key, handling anomalies in tables with multiple overlapping candidate keys.

### Q6. What is Denormalization, and when is it used?
**Answer:** Intentionally adding redundancy to a database to improve read performance and reduce the complexity of JOIN operations in read-heavy analytical systems.

### Q7. What is a Primary Key?
**Answer:** A column (or set of columns) that uniquely identifies each row in a table. It cannot be NULL and must be unique.

### Q8. What is a Foreign Key?
**Answer:** A column that creates a relationship between two tables by referencing the Primary Key of another table, maintaining referential integrity.

### Q9. What is a Unique Key?
**Answer:** Ensures all values in a column are distinct. Unlike a Primary Key, a table can have multiple Unique Keys, and it typically allows a single NULL value (depending on the RDBMS).

### Q10. What is a Composite Key?
**Answer:** A Primary Key consisting of two or more columns that together uniquely identify a record.

### Q11. Surrogate Key vs. Natural Key?
**Answer:** A Surrogate Key is an artificially generated key (like an auto-incrementing integer or UUID). A Natural Key is formed from existing business data (like an email address or SSN).

### Q12. Explain the different types of JOINs.
**Answer:** INNER (matching rows only), LEFT (all from left + matching right), RIGHT (all from right + matching left), FULL OUTER (all rows from both when there is a match in either), CROSS (Cartesian product).

### Q13. How do you perform a FULL OUTER JOIN in MySQL?
**Answer:** MySQL does not support FULL OUTER JOIN natively. You must use a LEFT JOIN, a UNION, and a RIGHT JOIN together to achieve the same result.

### Q14. What is a Self Join?
**Answer:** A query in which a table is joined to itself. Useful for hierarchical data, like finding employees and their managers within the same table.

### Q15. What is a CROSS JOIN?
**Answer:** It returns the Cartesian product of the two tables. If Table A has 10 rows and Table B has 10 rows, the result is 100 rows.

### Q16. UNION vs. UNION ALL?
**Answer:** Both combine result sets of two or more SELECT statements. `UNION` removes duplicates, while `UNION ALL` keeps duplicates and is significantly faster.

### Q17. What are INTERSECT and EXCEPT?
**Answer:** `INTERSECT` returns rows common to both queries. `EXCEPT` (or `MINUS` in Oracle) returns rows from the first query that are not present in the second.

### Q18. What are DDL and DML?
**Answer:** DDL (Data Definition Language) defines schema (CREATE, ALTER, DROP, TRUNCATE). DML (Data Manipulation Language) manipulates data (INSERT, UPDATE, DELETE).

### Q19. What are DCL and TCL?
**Answer:** DCL (Data Control Language) manages permissions (GRANT, REVOKE). TCL (Transaction Control Language) manages transactions (COMMIT, ROLLBACK, SAVEPOINT).

### Q20. What is the difference between DELETE and TRUNCATE?
**Answer:** `DELETE` is a DML command that deletes rows one by one and writes to the transaction log. `TRUNCATE` is a DDL command that deallocates data pages entirely, making it much faster but unrecoverable without backups.

---

## 2. Advanced SQL Querying

### Q21. WHERE vs. HAVING?
**Answer:** `WHERE` filters rows before grouping occurs. `HAVING` filters grouped records after the `GROUP BY` clause is applied.

### Q22. What is a Subquery?
**Answer:** A query nested inside another query. It can be used in SELECT, FROM, or WHERE clauses. Correlated subqueries reference columns from the outer query.

### Q23. What is a CTE (Common Table Expression)?
**Answer:** A temporary named result set defined using the `WITH` clause. It improves readability and can be referenced multiple times within the main query.

### Q24. What is a Recursive CTE?
**Answer:** A CTE that references itself. It is extremely useful for querying hierarchical data, such as organizational charts or category trees.

### Q25. What are Window Functions?
**Answer:** Functions that perform calculations across a set of table rows related to the current row without collapsing the rows (unlike GROUP BY). Examples: `OVER()`, `PARTITION BY`.

### Q26. ROW_NUMBER() vs. RANK() vs. DENSE_RANK()?
**Answer:** `ROW_NUMBER()` assigns a unique sequential integer. `RANK()` gives the same rank to ties but skips subsequent numbers (1, 2, 2, 4). `DENSE_RANK()` gives the same rank to ties but does not skip (1, 2, 2, 3).

### Q27. What do LEAD() and LAG() do?
**Answer:** They are window functions that allow you to access data from subsequent (`LEAD`) or previous (`LAG`) rows in the same result set without using a self-join.

### Q28. PARTITION BY vs. GROUP BY?
**Answer:** `GROUP BY` aggregates data and returns one row per group. `PARTITION BY` divides the result set into partitions for window functions to operate on, keeping all original rows intact.

### Q29. IN vs. EXISTS?
**Answer:** `IN` compares a value against a literal list or a subquery result. `EXISTS` checks if a subquery returns any rows. `EXISTS` is often faster for large datasets because it stops scanning as soon as a match is found.

### Q30. What is the danger of NOT IN with NULL values?
**Answer:** If the subquery for a `NOT IN` clause returns even a single NULL value, the entire `NOT IN` condition will evaluate to UNKNOWN, returning zero results. Use `NOT EXISTS` instead.

### Q31. COALESCE vs. ISNULL?
**Answer:** `COALESCE` is ANSI standard and returns the first non-null value in a list of arguments. `ISNULL` (SQL Server) or `IFNULL` (MySQL) is specific to the RDBMS and takes only two arguments.

### Q32. What does NULLIF() do?
**Answer:** It takes two arguments and returns NULL if they are equal; otherwise, it returns the first argument. Useful for avoiding divide-by-zero errors (`value / NULLIF(divisor, 0)`).

### Q33. LIKE vs. ILIKE?
**Answer:** `LIKE` is used for pattern matching (case-sensitive in Postgres, often case-insensitive in MySQL depending on collation). `ILIKE` is a PostgreSQL-specific operator for case-insensitive matching.

### Q34. How do you aggregate strings from multiple rows?
**Answer:** In MySQL, use `GROUP_CONCAT()`. In PostgreSQL, use `STRING_AGG()`. Both allow you to combine values into a single comma-separated string based on a grouping.

### Q35. Temporary Tables vs. CTEs?
**Answer:** Temporary Tables are physically stored in memory/tempdb and can have indexes; good for heavy, multi-step processing. CTEs exist only in memory for the duration of a single query.

### Q36. What is an UPSERT?
**Answer:** A command that inserts a record if it doesn't exist, or updates it if it does. MySQL uses `INSERT ... ON DUPLICATE KEY UPDATE`. PostgreSQL uses `INSERT ... ON CONFLICT DO UPDATE`.

### Q37. Offset/Fetch vs. Cursor Pagination?
**Answer:** Offset pagination (`LIMIT 10 OFFSET 10000`) is slow for deep pages because the DB must scan and discard 10,000 rows. Cursor (Keyset) pagination (`WHERE id > 10000 LIMIT 10`) utilizes indexes and is much faster.

### Q38. What is a Pivot table?
**Answer:** A data summarization technique that transforms rows into columns, often used for reporting (e.g., turning a 'month' column into 12 separate columns).

### Q39. How do you find duplicate records in a table?
**Answer:** Use `GROUP BY` on the columns that should be unique and add `HAVING COUNT(*) > 1`.

### Q40. How do you delete duplicate records?
**Answer:** Use a CTE with `ROW_NUMBER() OVER(PARTITION BY unique_columns ORDER BY id)` and delete where the row number is greater than 1.

---

## 3. Database Performance & Indexing

### Q41. What is a Database Index?
**Answer:** A data structure (usually a B-Tree) that improves the speed of data retrieval operations on a table at the cost of additional storage space and slower writes (INSERT/UPDATE/DELETE).

### Q42. B-Tree vs. Hash Index?
**Answer:** B-Tree is the default, sorting data in a tree structure, great for range queries (`>`, `<`). Hash indexes use a hash table, excellent for exact matches (`=`) but useless for range queries or sorting.

### Q43. Clustered vs. Non-Clustered Index?
**Answer:** A Clustered Index determines the physical order of data in the table (usually the Primary Key). A Non-Clustered Index is a separate structure containing a pointer back to the actual data row.

### Q44. What is a Composite Index?
**Answer:** An index containing multiple columns. It is useful for queries that filter on multiple columns simultaneously.

### Q45. What is the Left-Most Prefix Rule?
**Answer:** In a composite index (A, B, C), the index can only be used if the query filters by A, (A,B), or (A,B,C). If you filter only by B or C, the index cannot be utilized.

### Q46. What is a Covering Index?
**Answer:** An index that contains all the columns requested in the `SELECT`, `WHERE`, and `JOIN` clauses. The DB can return results purely from the index without doing a secondary lookup to the physical table data.

### Q47. What is a Partial / Filtered Index?
**Answer:** An index built on a subset of rows defined by a conditional expression (e.g., `WHERE status = 'active'`). Common in PostgreSQL to save space and improve performance.

### Q48. What is a Bitmap Index?
**Answer:** An index that uses bitmaps (bit arrays) for highly repetitive data (low cardinality, like Gender or Status). Not available in MySQL/Postgres natively, mostly used in Oracle/Data Warehouses.

### Q49. What does EXPLAIN do?
**Answer:** It reveals the query execution plan the database engine has chosen, showing index usage, join types, and estimated row scans, helping you identify performance bottlenecks.

### Q50. EXPLAIN vs. EXPLAIN ANALYZE?
**Answer:** `EXPLAIN` only estimates the query plan without running it. `EXPLAIN ANALYZE` (available in Postgres and MySQL 8.0+) actually executes the query and shows real execution times alongside the estimates.

### Q51. What is a Table Scan (Sequential Scan)?
**Answer:** When the database must read every single row in a table to evaluate a query because no suitable index exists. Very slow for large tables.

### Q52. Index Scan vs. Index Seek?
**Answer:** An Index Seek jumps directly to the matching nodes in the B-Tree (fast). An Index Scan reads through the leaf nodes of the index sequentially (slower, but better than a full table scan).

### Q53. What is Cardinality?
**Answer:** The uniqueness of data values in a column. A column like 'ID' has high cardinality. A column like 'Gender' has low cardinality. High cardinality columns make the best indexes.

### Q54. What is SARGable?
**Answer:** Search ARGument ABLE. It means a query's WHERE clause is written in a way that can utilize an index. `WHERE age = 20` is SARGable. `WHERE YEAR(dob) = 2020` is NOT SARGable because it uses a function.

### Q55. How do you index a column used inside a function?
**Answer:** You must create a Function-Based Index (or Expression Index in PostgreSQL) specifically on the function, e.g., `CREATE INDEX idx on users (LOWER(email))`.

### Q56. What causes Index Fragmentation?
**Answer:** Frequent INSERT, UPDATE, and DELETE operations cause index pages to split and data to become scattered, leading to inefficient reads. Fixed by rebuilding or reorganizing the index.

### Q57. What is Database Partitioning?
**Answer:** Dividing a very large table into smaller, more manageable pieces (partitions) horizontally based on a key (like Date/Month), improving query performance and maintenance.

### Q58. What is Sharding?
**Answer:** A type of horizontal partitioning where data is split across multiple independent database servers (nodes) to scale out and handle massive loads.

### Q59. What is a Materialized View?
**Answer:** Unlike a standard View (which is just a saved query), a Materialized View caches the result set on disk. It is extremely fast for reads but requires manual or scheduled refreshes when underlying data changes.

### Q60. How does the N+1 problem manifest in the Database?
**Answer:** The application sends 1 query to get a list of IDs, and then N separate, nearly identical queries to fetch details for each ID, overwhelming the DB connection pool. Solved via `JOIN` or `IN` clauses.

---

## 4. Transactions & Concurrency

### Q61. What is a Database Transaction?
**Answer:** A logical unit of work containing one or more SQL statements. It must be committed as a whole, or rolled back completely if any statement fails, maintaining data integrity.

### Q62. What is MVCC (Multi-Version Concurrency Control)?
**Answer:** A concurrency control method used by Postgres and InnoDB. Each transaction sees a 'snapshot' of data. Readers don't block writers, and writers don't block readers.

### Q63. What is a Dirty Read?
**Answer:** When a transaction reads data that has been modified by another concurrent, uncommitted transaction. If the second transaction rolls back, the first read invalid data.

### Q64. What is a Non-Repeatable Read?
**Answer:** When a transaction reads the same row twice and gets different results because another committed transaction updated the row in between the reads.

### Q65. What is a Phantom Read?
**Answer:** When a transaction executes a range query twice, but gets a different set of rows the second time because another transaction inserted or deleted rows in that range.

### Q66. Explain the 'Read Uncommitted' isolation level.
**Answer:** The lowest isolation level. Transactions can see uncommitted changes from other transactions. Allows Dirty Reads, Non-repeatable Reads, and Phantom Reads.

### Q67. Explain the 'Read Committed' isolation level.
**Answer:** Transactions can only see changes committed before the query began. Prevents Dirty Reads, but allows Non-Repeatable and Phantom Reads. Default in PostgreSQL.

### Q68. Explain the 'Repeatable Read' isolation level.
**Answer:** Ensures that if you read a row, you will see the exact same data if you read it again within the same transaction. Default in MySQL (InnoDB).

### Q69. Explain the 'Serializable' isolation level.
**Answer:** The highest isolation level. It forces transactions to execute sequentially, preventing all concurrency anomalies, but severely impacting performance and causing lock timeouts.

### Q70. What is a Deadlock?
**Answer:** A situation where Transaction A holds a lock on Resource 1 and waits for Resource 2, while Transaction B holds a lock on Resource 2 and waits for Resource 1. Both are stuck infinitely.

### Q71. How do you prevent Deadlocks?
**Answer:** Access tables/rows in the same consistent order across all application transactions, keep transactions short, and use lower isolation levels if possible.

### Q72. What is Optimistic Locking?
**Answer:** Assumes conflicts are rare. It uses a `version` or `updated_at` column. Before updating, it checks if the version matches the one initially read. If not, the application throws an error and retries.

### Q73. What is Pessimistic Locking?
**Answer:** Assumes conflicts are highly likely. It locks the row immediately upon reading it using `SELECT ... FOR UPDATE`, preventing any other transaction from reading/writing until the lock is released.

### Q74. What is `SELECT ... FOR UPDATE`?
**Answer:** A pessimistic locking command that places an exclusive lock on the selected rows, preventing them from being modified or read with another `FOR UPDATE` until the current transaction commits.

### Q75. Shared Lock vs. Exclusive Lock?
**Answer:** A Shared Lock (`LOCK IN SHARE MODE`) allows other transactions to read the row but not update it. An Exclusive Lock (`FOR UPDATE`) prevents others from reading or updating the row.

### Q76. Row-level vs. Table-level locking?
**Answer:** Row-level locks only lock the specific rows being modified, allowing high concurrency. Table-level locks lock the entire table, blocking all other writes to that table (common in MyISAM, rare in InnoDB/Postgres).

### Q77. What is an Intent Lock?
**Answer:** A table-level lock that indicates a transaction's intention to acquire a row-level lock further down in the hierarchy. It prevents another transaction from acquiring a table-level lock.

### Q78. What is Two-Phase Commit (2PC)?
**Answer:** A distributed algorithm that coordinates all processes participating in a distributed transaction to either commit or abort, ensuring consistency across multiple databases.

### Q79. What is WAL (Write-Ahead Logging)?
**Answer:** A standard approach to ensuring data integrity. Changes are written to a sequential log file on disk *before* the actual data pages are updated. This allows recovery after a crash.

### Q80. How does the database handle power failures?
**Answer:** During a reboot, the DB checks the Write-Ahead Log (WAL or Redo Log). It replays committed transactions that weren't written to data files and undoes uncommitted ones.

---

## 5. MySQL vs. PostgreSQL Specifics & Architecture

### Q81. InnoDB vs. MyISAM (MySQL)?
**Answer:** InnoDB supports ACID transactions, foreign keys, and row-level locking. MyISAM is an older engine with table-level locking, no transactions, and no foreign keys. Always use InnoDB for modern apps.

### Q82. MySQL JSON vs. PostgreSQL JSONB?
**Answer:** MySQL supports JSON, but PostgreSQL's `JSONB` stores data in a decomposed binary format, making it significantly faster to query and index (using GIN indexes).

### Q83. What are PostgreSQL Arrays?
**Answer:** PostgreSQL allows you to define a column as an array of any valid data type (e.g., `integer[]` or `text[]`), allowing multiple values per cell, strictly typed.

### Q84. Auto_Increment vs. Serial/Sequence?
**Answer:** MySQL uses the `AUTO_INCREMENT` attribute on a column. PostgreSQL historically used the `SERIAL` pseudo-type, which creates a background `SEQUENCE` object. Postgres 10+ introduced standard `GENERATED ALWAYS AS IDENTITY`.

### Q85. What is VACUUM in PostgreSQL?
**Answer:** Because of MVCC, updating/deleting rows in Postgres creates 'dead tuples'. `VACUUM` reclaims this space so it can be reused, preventing database bloat.

### Q86. What is Autovacuum?
**Answer:** A background daemon in PostgreSQL that automatically runs `VACUUM` and `ANALYZE` commands based on table activity thresholds, preventing bloat without manual intervention.

### Q87. What is Connection Pooling, and what is PgBouncer?
**Answer:** Databases have limits on concurrent connections. A pooler maintains open connections and shares them among application threads. PgBouncer is a popular, lightweight connection pooler for PostgreSQL.

### Q88. Master-Slave vs. Master-Master Replication?
**Answer:** Master-Slave: All writes go to Master, reads go to Slaves. Master-Master: Writes can go to multiple nodes, requiring complex conflict resolution logic.

### Q89. Logical vs. Physical Replication (PostgreSQL)?
**Answer:** Physical replication copies byte-for-byte block changes (WAL). Logical replication decodes the WAL into SQL statements, allowing replication of specific tables or between different Postgres versions.

### Q90. What are MySQL's Redo and Undo logs?
**Answer:** Redo logs ensure Durability (re-applying committed changes after a crash). Undo logs ensure Atomicity and support MVCC (rolling back uncommitted changes).

### Q91. What is PostGIS?
**Answer:** A powerful spatial database extension for PostgreSQL that adds support for geographic objects, allowing location queries to be run in SQL.

### Q92. What is `utf8mb4` in MySQL?
**Answer:** Standard `utf8` in MySQL only supports 3 bytes per character, failing on emojis. `utf8mb4` fully supports 4-byte Unicode characters and should be the default collation.

### Q93. What are Table Spaces?
**Answer:** A feature allowing database administrators to define locations on the physical file system where the database files should reside (e.g., placing slow historical data on an HDD and fast active data on an SSD).

### Q94. How did CTE materialization change in PostgreSQL 12?
**Answer:** Prior to v12, Postgres unconditionally evaluated CTEs (`WITH` clauses) separately and materialized them. In v12+, CTEs are inlined into the outer query by default, improving optimizer performance.

### Q95. What are Gap Locks in MySQL?
**Answer:** In Repeatable Read isolation, InnoDB places locks on the 'gaps' between index records to prevent Phantom Reads (other transactions inserting rows into a range you are currently querying).

### Q96. What are GIN and GiST indexes (PostgreSQL)?
**Answer:** GIN (Generalized Inverted Index) is ideal for indexing JSONB, Arrays, and Full-Text Search. GiST (Generalized Search Tree) is used for complex geometric data like PostGIS.

### Q97. How do MySQL and PostgreSQL handle Boolean values differently?
**Answer:** PostgreSQL has a true `BOOLEAN` data type (`true`/`false`). MySQL does not; it uses `TINYINT(1)` where 1 is true and 0 is false.

### Q98. What happens when you hit the `max_connections` limit?
**Answer:** The database rejects new connection attempts (e.g., 'Too many connections' error). Resolved by increasing the limit, adding a connection pooler, or optimizing application connection handling.

### Q99. Stored Procedures vs. Functions?
**Answer:** Functions must return a value and can be called inside a `SELECT` statement. Stored Procedures do not have to return a value, can manage transaction states (commit/rollback), and are executed using `CALL`.

### Q100. What are Triggers, and what is their downside?
**Answer:** A Trigger is a procedural code automatically executed in response to certain events (INSERT/UPDATE/DELETE). Downside: They hide business logic inside the DB, are hard to debug, and drastically slow down bulk data operations.