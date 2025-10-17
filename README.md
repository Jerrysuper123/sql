# sql

# PL SQL package
A **PL/SQL package** in Oracle is like a **container** that groups related **procedures, functions, variables, and cursors** together — so they can be managed and used as a single unit.

Think of it as a **toolbox** that keeps all related tools (code objects) in one place.

---

### 🧱 Structure of a PL/SQL Package

A package has **two parts**:

1. **Package Specification** (the *interface* — what’s visible)

   * Declares what can be used outside the package: procedures, functions, variables, constants, etc.
   * Similar to a header file or API definition.

2. **Package Body** (the *implementation*)

   * Contains the actual code for the procedures and functions declared in the spec.

---

### 💡 Example

```sql
-- 1️⃣ Package specification
-- create or replace the package with the name "employee_pkg"
-- this is like an interface in Java

CREATE OR REPLACE PACKAGE employee_pkg AS
  PROCEDURE add_employee(p_name VARCHAR2, p_salary NUMBER);
  FUNCTION get_total_employees RETURN NUMBER;
END employee_pkg;
/

-- 2️⃣ Package body
CREATE OR REPLACE PACKAGE BODY employee_pkg AS
  v_count NUMBER := 0;

  PROCEDURE add_employee(p_name VARCHAR2, p_salary NUMBER) IS
  BEGIN
    INSERT INTO employees (name, salary) VALUES (p_name, p_salary);
    v_count := v_count + 1;
  END;

  FUNCTION get_total_employees RETURN NUMBER IS
  BEGIN
    RETURN v_count;
  END;
END employee_pkg;
/
```

Then you can call it like this:

```sql
EXEC employee_pkg.add_employee('John', 5000);
SELECT employee_pkg.get_total_employees FROM dual;
```

---

### 🧠 Benefits of Using Packages

* **Encapsulation:** Hides internal logic; only exposes what’s needed.
* **Organization:** Groups related procedures and functions together.
* **Performance:** Loaded into memory once, so repeated calls are faster.
* **Maintainability:** Easier to update or fix one part without breaking others.

---

# what is / in sql
Excellent question 👏

The **`/` (forward slash)** in PL/SQL scripts is a **special SQL*Plus (or SQL Developer) command** — **not part of PL/SQL itself**.

---

### 💡 In simple terms:

`/` tells Oracle **“execute the last PL/SQL block you just finished writing.”**

---

### 🧱 Example

```sql
CREATE OR REPLACE PACKAGE employee_pkg AS
  PROCEDURE add_employee(p_name VARCHAR2, p_salary NUMBER);
  FUNCTION get_total_employees RETURN NUMBER;
END employee_pkg;
/
```

Here’s what happens:

1. The semicolon `;` ends the **last statement** in the PL/SQL block.
2. The `/` tells the SQL tool to **send the whole package definition (from `CREATE` to `END`) to the database** for compilation.

Without the `/`, the tool might think you’re still typing and not execute the code.

---

### ⚙️ Where It’s Needed

You’ll typically use `/` after:

* `CREATE OR REPLACE PACKAGE ... END;`
* `CREATE OR REPLACE PACKAGE BODY ... END;`
* `CREATE OR REPLACE PROCEDURE ... END;`
* `BEGIN ... END;` anonymous blocks

---

### 🔍 Why It Exists

When you run PL/SQL code in tools like:

* **SQL*Plus**
* **SQL Developer**
* **SQLcl**

…these tools read input **line by line**, not as one big block.
The `/` is a signal that says:

> “Okay, I’m done typing the block — now compile or run it.”

---

### 🧩 Example contrast:

Without `/`:

```sql
CREATE OR REPLACE PROCEDURE test_proc IS
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello');
END;
-- nothing happens yet
```

With `/`:

```sql
CREATE OR REPLACE PROCEDURE test_proc IS
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello');
END;
/
-- ✅ Procedure is now compiled
```

---

✅ **Summary**

| Symbol | Purpose                                               |
| ------ | ----------------------------------------------------- |
| `;`    | Ends a statement inside the block                     |
| `/`    | Executes or compiles the *whole* block you just wrote |

---

# begin and end
Excellent — this is one of the most fundamental parts of PL/SQL 👏

Let’s go step by step.

---

### 🧱 In simple terms:

`BEGIN` and `END` mark the **start and end of a PL/SQL block** —
they tell Oracle **“this is the code to run.”**

Think of it like:

```text
BEGIN   --> start of the action
  (your code)
END;    --> end of the action
```

---

### 💡 Every PL/SQL block has this structure:

```sql
DECLARE
  -- (optional) variable declarations
BEGIN
  -- executable statements (code that runs)
EXCEPTION
  -- (optional) error handling
END;
/
```

---

### 🔹 Example 1 — Anonymous block

```sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello World!');
END;
/
```

**What happens:**

* Oracle runs everything between `BEGIN` and `END;`
* It prints “Hello World!” to the output console.

---

### 🔹 Example 2 — With DECLARE

```sql
DECLARE
  v_name VARCHAR2(50) := 'Jerry';
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello, ' || v_name);
END;
/
```

Here:

1. `DECLARE` defines a variable `v_name`.
2. `BEGIN ... END;` runs the logic.
3. It prints `Hello, Jerry`.

---

### 🔹 Example 3 — Inside a procedure or function

```sql
CREATE OR REPLACE PROCEDURE greet_user(p_name VARCHAR2) AS
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello, ' || p_name);
END;
/
```

* The `BEGIN ... END;` block defines what the procedure **does** when called.
* You can call it with:

  ```sql
  EXEC greet_user('John');
  ```

---

### 🧠 Think of it like:

| Keyword     | Meaning                                   |
| ----------- | ----------------------------------------- |
| **DECLARE** | (Optional) Define your variables          |
| **BEGIN**   | Start running your code                   |
| **END;**    | Stop the block of code                    |
| **/**       | Execute it (in SQL*Plus or SQL Developer) |

---

### ✅ In short:

* `BEGIN` = start of executable code
* `END;` = end of that code block
* Together, they define a **PL/SQL block**, the smallest runnable unit of PL/SQL.

---


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



