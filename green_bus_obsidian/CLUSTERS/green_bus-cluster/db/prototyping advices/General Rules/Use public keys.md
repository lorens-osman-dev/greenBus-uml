---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
# Use public keys
Why public keys crucial for external users.

**The Problem: Exposing Your True Primary Keys**
Imagine you're building an e-commerce platform. You have a `products` table:

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(255),
    price DECIMAL(10, 2),
    -- ... other product details
);
```

- `product_id` is your primary key. It's a unique, auto-incrementing integer (using `SERIAL` in PostgreSQL). This is perfect for internal database management.

Now, let's say you have external partners who want to integrate with your platform. They need to refer to your products in their own systems.
- **The Bad Idea (Exposing `product_id`):** You give your partners the `product_id` values. They store these IDs in their systems to link to your products.
- **Why This Is a Trap:**
    - **Business Meaning:** Your partners might start to associate business meaning with the `product_id` values. For example, they might assume that product IDs in the 1000s are "electronics," or they might hardcode these IDs into their reports.
    - **Rigidity:** If you ever need to change your `product_id` scheme (e.g., to switch to UUIDs, or to re-seed the IDs after a data migration), you'll break all your partners' integrations.
    - **Security:** Exposing sequential integer IDs can sometimes reveal information about the size of your database.

## The Solution: Public Keys (or Alternate Keys)
Creating a separate "public key" for external users. Here's how you might do it:

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    public_product_code VARCHAR(50) UNIQUE NOT NULL,
    product_name VARCHAR(255),
    price DECIMAL(10, 2),
    -- ... other product details
);
```

- **`public_product_code`:** This is your public key. It's a string (e.g., a short, human-readable code or a UUID) that you expose to external users.
    - `UNIQUE NOT NULL` constraints ensure that each public code is unique and never null.

**How It Works**
1. **Internal Use:** Your application and database use `product_id` for all internal operations.
2. **External Use:** You give your partners the `public_product_code`. They use this code to refer to your products in their systems.
3. **Flexibility:** If you need to change your `product_id` scheme, you can do so without affecting your partners. They only rely on the `public_product_code`, which you can keep stable.
4. **Data Migration:** If you need to migrate data, you can keep the public key the same, and remap the internal primary keys.
5. **Business Changes:** If the business rules change, and the public key needs to be changed, you can communicate this change to your partners, and have a transition period.

**Benefits**
- **Decoupling:** You decouple your internal database structure from external integrations.
- **Flexibility:** You gain the freedom to change your internal primary keys without breaking external systems.
- **Stability:** You provide a stable identifier for external users.
- **Reduced Risk:** You reduce the risk of external users building dependencies on your internal primary keys.

### Example of UUID as Public Key:

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    public_product_uuid UUID UNIQUE NOT NULL DEFAULT gen_random_uuid(),
    product_name VARCHAR(255),
    price DECIMAL(10, 2),
    -- ... other product details
);
```

- `gen_random_uuid()` is a PostgreSQL function to generate UUIDs. UUIDs are universally unique identifiers, virtually guaranteeing no collisions.