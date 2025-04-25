---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
**foreign key (FK) enforcement**, which can be done in two ways:

1. **Hard enforcement** (using actual foreign key constraints in the database).
2. **Soft enforcement** (managing relationships at the application level without explicit foreign key constraints).

---

### **Hard Enforcement (Strict Foreign Keys)**

- The database **enforces referential integrity** by ensuring that a foreign key in a table **must** reference a valid primary key in another table.
- If a referenced record is deleted, the database can prevent deletion or cascade changes.

#### **Example (Hard FK Enforcement)**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

- If you try to insert an order with a `user_id` that doesn’t exist, PostgreSQL will reject it.
- If a user is deleted, all their orders will also be deleted (`ON DELETE CASCADE`).

---

### **Soft Enforcement (Application-Controlled FKs)**

- Instead of defining foreign keys in the database, the application logic ensures relationships are valid.
- This approach is more flexible but **risks data inconsistency** if not handled properly.

#### **Example (Soft FK Enforcement)**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL -- No FK constraint
);
```

- The application is responsible for ensuring `user_id` exists in `users`.
- If a user is deleted, the `orders` table will still have orphaned records unless manually cleaned up.

---

### **Pros & Cons**

|Approach|Pros|Cons|
|---|---|---|
|**Hard FK Enforcement**|Ensures data integrity, prevents orphaned records|Less flexibility, can be slower for large-scale deletes|
|**Soft FK Enforcement**|More flexible, allows for partial data loading (e.g., for analytics)|Requires manual validation, risks orphaned records|

---

### **When to Choose What?**

- **Use hard FKs** if you need strict data integrity (e.g., financial transactions).
- **Use soft FKs** if you have large-scale deletions or denormalized data (e.g., analytics, logging).

For most **medium projects**, **hard enforcement** is recommended unless there's a compelling reason to avoid it.