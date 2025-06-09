# Database Normalization.

**Normalization** is an important process in database design that helps improve the database's efficiency, consistency, and accuracy. It makes it easier to manage and maintain the data and ensures that the database is adaptable to changing business needs. It involves structuring tables and relationships according to a series of normal forms, aiming to eliminate anomalies associated with insertions, deletions, and updates.

## Benefits of Normalization.

1. Reduced Redundancy: Normalization minimizes duplicated data, saving storage space and improving efficiency.
2. Improved Data Integrity: By storing data in a structured, consistent manner, normalization reduces errors and inconsistencies.
3. Simplified Data Management: Normalizing databases makes it easier to add, modify, and delete data.
4. Enhanced Performance: Well-normalized databases generally have better query performance. Normalization Process:

Normalization involves applying a series of rules to a database schema to achieve different levels of data organization. These forms are:

### First Normal Form (1NF)

- Ensures that each column contains atomic values (values that cannot be further subdivided).

  _example_

**Bad**

| Student | Subjects      |
| ------- | ------------- |
| Antoo   | Math, English |

Multiple subjects in one cell.

**Good**

| Student | Subject |
| ------- | ------- |
| Antoo   | Math    |
| Antoo   | English |

### Second Normal Form (2NF)

- Builds on 1NF.
- Requires that all non-key attributes be fully dependent on the primary key.

_example_

**Bad**

| Student | CourseID | CourseName |
| ------- | -------- | ---------- |
| Antoo   | C001     | Math       |

- CourseName only depends on CourseID and not the full row.

**Good**

StudentCourses:

| Student | CourseID |
| ------- | -------- |
| Antoo   | C001     |

Courses:

| CourseID | CourseName |
| -------- | ---------- |
| C001     | Math       |

### Third Normal Form (3NF)

- Extends 2NF.
- Removes transitive dependencies non-key attributes should not depend on other non-key attributes.

_example_

Bad

| Student | DeptID | DeptName |
| ------- | ------ | -------- |
| Antoo   | D01    | Science  |

- (DeptName depends on DeptID not Student).

**Good **

Students:

| Student | DeptID |
| ------- | ------ |
| Antoo   | D01    |

Departments:

| DeptID | DeptName |
| ------ | -------- |
| D01    | Science  |

### Boyce-Codd Normal Form (BCNF)

- A stricter version of 3NF that eliminates all dependencies on key.

_eample_

**Problem:**

| Professor | Subject | Dept    |
| --------- | ------- | ------- |
| Dr.Otwoma | Math    | Science |

- (If Subject Dept, not in BCNF).

Solution:

Break into:

Subjects:
| Subject | Dept |
|---------|----------|
| AI | Science |

Professors:

| Professor | Subject |
| --------- | ------- |
| Dr.Otwoma | AI      |

- (Dependencies based only on keys).

### Fourth Normal Form (4NF)

- Addresses multi-valued dependencies, where a table might have multiple values for a single attribute.

_example_

Bad:

| Student | Subject | Hobby   |
| ------- | ------- | ------- |
| Antoo   | Math    | Drawing |
| Antoo   | Math    | Singing |

- (Multivalued facts in one table).

Good

StudentSubjects:

| Student | Subject |
| ------- | ------- |
| Antoo   | Math    |

StudentHobbies:

| Student | Hobby   |
| ------- | ------- |
| Antoo   | Drawing |
| Antoo   | Singing |

- (Separate independent data).

### Fifth Normal Form (5NF)

- The highest level of normalization.
- Eliminates redundancy in tables with complex relationships.

_example_

**Bad**

| Project | Employee | Skill  |
| ------- | -------- | ------ |
| P1      | Alice    | Coding |

- (Repeating combinations).

**Good**

ProjectsEmployees:

| Project | Employee |
| ------- | -------- |
| P1      | Alice    |

ProjectsSkills:

| Project | Skill  |
| ------- | ------ |
| P1      | Coding |

EmployeesSkills:

| Employee | Skill  |
| -------- | ------ |
| Antoo    | Coding |

- (Each part handled separately).

## Benefits of Applying Normal Forms.

- _Insertion Anomalies_: 2. Update Anomalies:
  These happen when changes in one location are not reflected elsewhere, potentially leading to inconsistencies. Normalization helps by storing data.
- _Update Anomalies_:These happen when changes in one location are not reflected elsewhere, potentially leading to inconsistencies. Normalization helps by storing data in only one place.
- _Deletion Anomalies_: These arise when removing data leads to the loss of related information. Normalization prevents these by storing related data in separate tables.

# ACID Properties.

**ACID** stands for Atomicity, Consistency, Isolation, and Durability. These four key properties define how a transaction should be processed in a reliable and predictable manner, ensuring that the database remains consistent, even in cases of failures or concurrent accesses.

## Key Properties

1. **Atomicity:**
   A transaction is treated as a single, indivisible unit of work. Either all operations within the transaction complete successfully, or none of them do. This prevents partial updates or corrupted data.

_example_

When you're transferring 1000 from Account A to Account B. This involves:

- Deducting 1,000 from A
- Adding 1,000 to B

If the first operation succeeds but the second fails, the bank must undo the deduction. Atomicity ensures both operations succeed or none do.

2. **Consistency:**
   A transaction maintains the integrity of the database. Changes made by a transaction must adhere to predefined rules and constraints, ensuring the database remains in a valid and consistent state.

_example_

Suppose your database has a rule that an account balance cannot go below zero. If a transaction tries to withdraw 5,000 from an account with only 2,000, consistency ensures that the transaction is rejected to keep data valid.

3. **Isolation:**
   Concurrent transactions operate independently of each other. One transaction should not be able to see or interfere with the changes made by another transaction, preventing conflicts and data corruption.

_exampple_

Two users try to book the window seat on a flight at the same time. Isolation ensures that onnly one user transaction completes an the other user sees the seat as booked. This helps to avoid double booking.

4. **Durability:**
   Once a transaction is committed, its changes are permanently saved and remain valid even if the system crashes or encounters failures. This ensures that data is not lost and that changes are preserved.

_example_

A user places an online order, and the system saves it in the database. Even if the server crashes immediately after, the order remains safe and recorded because it has been committed to permanent storage.

## Advantages of ACID Properties in DBMS.

1. Data Consistency: ACID properties ensure that the data remains consistent and accurate after any transaction execution.
2. Data Integrity: It maintains the integrity of the data by ensuring that any changes to the database are permanent and cannot be lost.
3. Concurrency Control: ACID properties help to manage multiple transactions occurring concurrently by preventing interference between them.
4. Recovery: ACID properties ensure that in case of any failure or crash, the system can recover the data up to the point of failure or crash.

## Disadvantages of ACID Properties in DBMS.

1. Performance Overhead: ACID properties can introduce performance costs, especially when enforcing isolation between transactions or ensuring atomicity.
2. complexity: Maintaining ACID properties in distributed systems (like microservices or cloud environments) can be complex and may require sophisticated solutions like distributed locking or transaction coordination.
3. Scalability Issues: ACID properties can pose scalability challenges, particularly in systems with high transaction volumes, where traditional relational databases may struggle under load.

**ACID** is used in modern applications for ensuring the reliability and consistency of data is crucial and important in sectors like:

- _Banking:_ Transactions involving money transfers, deposits, or withdrawals must maintain strict consistency and durability to prevent errors and fraud.
- _E-commerce:_ Ensuring that inventory counts, orders, and customer details are handled correctly and consistently, even during high traffic, requires ACID compliance.
- _Healthcare:_ Patient records, test results, and prescriptions must adhere to strict consistency, integrity, and security standards.
