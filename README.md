# sql

# surrogate key is the system generated key, natural key has key like NRIC number, primary key can use either surrogate or natural key

A **surrogate key** is **often used as a primary key**, but they’re not the same thing.

Here’s the difference 👇

### 🔹 Surrogate Key

* A **system-generated** unique identifier for a record (e.g. an auto-increment ID or UUID).
* It has **no business meaning** — it’s just used to uniquely identify a row.
* Example: `customer_id = 101`

### 🔹 Primary Key

* A **constraint** in the table that uniquely identifies each record.
* It can be based on any column(s) — either a **surrogate key** or a **natural key** (something meaningful like `email` or `NRIC`).

### ✅ So in practice:

* The **surrogate key is often chosen as the primary key**, because it’s simple and never changes.
* But a **primary key doesn’t have to be a surrogate key** — it could be a natural key instead.

**Example:**

```sql
CREATE TABLE customers (
  customer_id SERIAL PRIMARY KEY,  -- surrogate key
  email VARCHAR(255) UNIQUE,
  name VARCHAR(100)
);
```

Here, `customer_id` is a surrogate key **and** the primary key.

Would you like me to show a case where a primary key is *not* a surrogate key (i.e. a natural key)?


Great foundational question — let’s break it down clearly:

---

### 📘 **What is a Schema in a Database?**

A **schema** is a **named collection of database objects**, like:

- Tables
- Views
- Indexes
- Functions
- Stored procedures

It's like a **folder or namespace** inside a database that helps **organize** and **manage** related data structures.

---

### 📂 Analogy:
Think of a database as a **filing cabinet**.  
Each **schema** is a **drawer** in that cabinet.  
Inside each drawer (schema), you have **files (tables, views, etc.)**.

---

### 🧱 Structure Example:

In many SQL systems (like PostgreSQL, Oracle, Snowflake, etc.):

```sql
-- Database: company_db
-- Schema: hr
-- Table: employees

//your hr schema can house multiple tables
SELECT * FROM hr.employees;
```

- `company_db` = Database
- `hr` = Schema
- `employees` = Table

---

### 🔐 Why Use Schemas?

- **Organization**: Group related tables/functions logically.
- **Access Control**: You can set permissions at the schema level.
- **Modularity**: Let multiple teams/apps share a DB but stay isolated.
- **Naming Clarity**: Avoid table name clashes like `users` in two apps.

---

### ⚙️ Notes by DB System

| Database      | Schema Support? | Notes |
|---------------|------------------|-------|
| **PostgreSQL** | ✅ Fully supported | Default schema is `public` |
| **MySQL**      | ✅ But called **databases** | One schema per DB |
| **Oracle**     | ✅ Schema = user | Schema = user account |
| **SQL Server** | ✅ Fully supported | Good for access control |
| **Snowflake**  | ✅ Fully supported | `database.schema.table` hierarchy |

---

### ✅ TL;DR

> A **schema** in a database is a **named namespace** for organizing tables, views, and other database objects. It's used to manage structure, access, and clarity inside a larger database.



