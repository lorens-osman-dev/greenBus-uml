---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
# Use UUID7
UUID7 is a newer type of UUID (Universally Unique Identifier) that is time-ordered while still being globally unique. Let me explain what it is and its advantages and disadvantages in PostgreSQL.

## UUID7 in PostgreSQL
UUID7 is a time-ordered UUID format that combines a timestamp with random data. While PostgreSQL doesn't have a native UUID7 type, you can implement it with extensions like `pg_uuidv7` or custom functions.

## Pros of UUID7

1. **Time-sortable**: The first portion contains a timestamp, making it naturally sortable by creation time
2. **Improved database performance**: Better indexing and locality of reference compared to random UUIDs
3. **Global uniqueness**: Like other UUIDs, they're globally unique across systems
4. **No coordination needed**: Distributed systems can generate IDs without central coordination
5. **Reduced index fragmentation**: Creates less B-tree fragmentation than random UUIDs

## Cons of UUID7

1. **Size**: 16 bytes (128 bits) versus 4-8 bytes for serial/bigserial
2. **Extension required**: Not built into PostgreSQL natively
3. **Less established**: Newer than UUID v4, so less widely adopted
4. **Potential timestamp privacy concerns**: Embeds creation time

## Simple Example

```sql
-- Install the extension (if available)
CREATE EXTENSION IF NOT EXISTS pg_uuidv7;

-- Create a table using UUID7
CREATE TABLE products (
    id uuid DEFAULT uuid_generate_v7() PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Insert some data
INSERT INTO products (name, price) VALUES 
    ('Laptop', 999.99),
    ('Smartphone', 699.99);

-- Query showing time-ordered results
SELECT id, name, created_at FROM products ORDER BY id;
```

The results would show entries sorted chronologically because UUID7 embeds the timestamp in the most significant bits.
