## 1. What is Data?

**Data** is raw information that represents facts, observations, or measurements.
**Examples:**

- A name: `Ram`
- A number: `25`
- A date: `2026-01-10`
- A status: `DONE`

By itself, data has limited value. Its real value comes when it is **stored, structured, and queried** efficiently.

---
## 2. What is a Database?

A **database** is an organized collection of data that allows:

- Storing data permanently
- Reading data efficiently
- Updating data safely
- Deleting data when required
Instead of storing data in files (like text files or Excel), databases provide:
- Faster access
- Data safety
- Concurrency (multiple users at the same time)
- Reliability

---

## 3. How is Data Stored in Relational Databases?

In a **Relational Database**, data is stored in **tables**.

**Table Structure**

- **Rows** → Individual records
- **Columns** → Attributes (fields)

**Example: `Users` Table**

| id  | name | email                                   | age |
| --- | ---- | --------------------------------------- | --- |
| 1   | Ram  | [ram@gmail.com](mailto:ram@gmail.com)   | 25  |
| 2   | Amit | [amit@gmail.com](mailto:amit@gmail.com) | 28  |

- Each **row** represents one user
- Each **column** represents a property of the user
---

## 4. What is a Relational Database (RDBMS)?

A **Relational Database** is a database that:

- Stores data in tables
- Establishes **relationships** between tables
- Uses **keys** (Primary Key, Foreign Key) to maintain these relationships

**Example of Relationship**

**Orders Table**

| order_id | user_id | amount |
| -------- | ------- | ------ |
| 101      | 1       | 500    |
| 102      | 2       | 300    |

Here:
- `user_id` refers to `Users.id`
- This creates a **relationship** between Users and Orders

---

## 5. Key Properties of Relational Databases

### 5.1 Data Consistency

Data remains **correct and reliable** across the database.

Example:

- If a user balance is `₹1000`, it should not show `₹800` in another query unless updated properly.


---

### 5.2 Data Durability

Once data is committed, it **will not be lost**, even if:

- The system crashes
- Power goes off

Databases achieve this using:

- Write‑Ahead Logs (WAL)
- Disk persistence

---

### 5.3 Data Integrity

Ensures data is **accurate and valid**.

Types of integrity:

- **Entity Integrity** – Primary key cannot be NULL
- **Referential Integrity** – Foreign key must refer to existing data
- **Domain Integrity** – Data must follow rules (age > 0)

---

### 5.4 Constraints

Constraints are **rules enforced by the database**.

Common constraints:

- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `CHECK`

Example:

```sql
age INT CHECK (age >= 18)
```

---

### 5.5 Everything in One Place (Centralized Source of Truth)

- All data lives in a single system
- Prevents duplication
- Avoids conflicts between multiple data copies

This makes RDBMS ideal for **enterprise systems**.

---

## 6. Why Do Relational Databases Support Transactions?

Because of the above properties, RDBMS supports **transactions**.

A **transaction** is a group of operations that must execute **completely or not at all**.

Example:

- Deduct money from Account A
- Add money to Account B

Both must succeed, or both must fail.

---

## 7. ACID Properties

ACID guarantees **safe and reliable transactions**.
ACID = **Atomicity, Consistency, Isolation, Durability**

A **transaction** is a logical unit of work that contains one or more database operations.
Example:
`Transfer ₹100 from Account A to Account B`
This involves multiple steps but must behave as **one unit**.
### 7.1 Atomicity

- All operations succeed or none do - *All or Nothing*

**Example:**
Steps in a transaction:
1. Deduct ₹100 from Account A
2. Add ₹100 to Account B
#### Possible Outcomes:

- Both steps succeed → ✅ Transaction committed
- Any step fails → ❌ Entire transaction rolled back

❌ **Not allowed**:

- Money deducted from A but not added to B

**How Databases Achieve Atomicity**
- Use **undo logs / rollback logs**
- If a failure occurs, changes are reverted to the previous state

**Real-World Analogy**
Think of an **ATM transaction**:
- Either you get cash **and** your balance is reduced
- Or you get nothing and your balance remains unchanged
---

### 7.2 Consistency
Always Valid State
**Consistency ensures that a transaction takes the database from one valid state to another valid state**, following all rules and constraints.
**What “Rules” Mean**
Rules include:
- Primary keys
- Foreign keys
- Unique constraints
- Check constraints
- Business rules
**Example:**
Constraint:
`balance >= 0`
Transaction:
`Withdraw ₹500 from account with ₹300`
❌ Result:
- Balance becomes `-200` → violates constraint

➡️ **Transaction is rejected**  
➡️ Database remains consistent
**Important Clarification**
- Consistency is **not about concurrency**
- It is about **data correctness**

**Real-World Analogy**
A form submission:
- Age field must be a number
- Email must be valid
If rules fail → submission rejected
- Database moves from one valid state to another
- Constraints are always respected

---

### 7.3 Isolation
Transactions Don’t Interfere
**Isolation ensures that multiple transactions running at the same time do not affect each other’s intermediate states.**

Each transaction behaves **as if it is the only one running**.
#### Why Isolation Is Needed
In real systems:
- Thousands of users
- Many transactions running concurrently
Without isolation → **data corruption**

**Example:**
Initial balance: ₹1000
Two transactions run at the same time:
- **T1**: Withdraw ₹300
- **T2**: Withdraw ₹500
❌ Without isolation:
- Both read balance as ₹1000
- Final balance becomes wrong
✅ With isolation:
- Transactions are properly ordered or controlled
- Final balance is correct
**Isolation Problems It Prevents**
- **Dirty Reads** – reading uncommitted data
- **Non-repeatable Reads**
- **Phantom Reads**

**Isolation Levels (Brief)**
- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable (strongest)

Higher isolation = more safety, less concurrency

**Real-World Analogy**
Two people editing the same Google Doc:
- Changes are managed so work doesn’t conflict incorrectly
---

### 7.4 Durability
Once Committed, Never Lost
Durability guarantees that once a transaction is committed, its changes will survive system crashes, power failures, or restarts.

**Example:**
- Transaction committed
- Server crashes immediately after
✅ After restart:
- Data is still present
#### How Databases Achieve Durability
- Write-Ahead Logging (WAL)
- Persistent storage (disk, SSD)
- Replication

**Important Point**
Durability applies **only after COMMIT**.
Before commit:
- Data can be lost  
After commit:
- Data must persist

**Real-World Analogy**
Once you receive a payment confirmation, the transaction is recorded permanently—even if the app crashes afterward.

---

## 8. When to Choose a Relational Database?

Pick an RDBMS when:

- Data has **clear relationships**
- You need **ACID transactions**
- Data correctness is critical
- Strong consistency is required
**Common Use Cases**

- Banking systems
- Order management systems
- Inventory systems
- Enterprise applications

---

## 9. Popular Relational Databases

- MySQL
- PostgreSQL
- Oracle
- SQL Server

---

## 10. Summary

Relational Databases:
- Store data in tables
- Maintain relationships
- Enforce constraints and integrity
- Support transactions using ACID

They are best suited for systems where **data correctness, reliability, and consistency** matter more than flexibility.

---
---
End of Notes