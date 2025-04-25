---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
This PostgreSQL recommendation about avoiding SERIAL and using identity columns instead relates to addressing several underlying issues with the SERIAL data type.
When PostgreSQL documentation says "Don't use SERIAL," they're referring to legacy behavior that can cause problems:

How SERIAL works behind the scenes: When you create a SERIAL column, PostgreSQL automatically:

Creates a sequence object (separate from your table)
Sets the column default to pull from this sequence
This creates implicit dependencies that aren't always obvious


## Problems with SERIAL:

- Permission issues: Users need separate permissions on the sequence object
- Schema migration challenges: Moving/copying tables with SERIAL requires manual sequence handling
- Dependency tracking complexity: The sequence exists as a separate object that must be managed
- Dump/restore complications: The separate sequence objects require special handling


## The better alternative - Identity columns:

Introduced in PostgreSQL 10
Use GENERATED { ALWAYS | BY DEFAULT } AS IDENTITY
The sequence mechanism is managed internally as part of the table
Permissions, dependencies, and schema operations work more intuitively


Example of the recommended approach:
```sql
-- Don't use this:
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username TEXT
);

-- Use this instead:
CREATE TABLE users (
  id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  username TEXT
);
```


The identity column approach follows more modern SQL standards, provides better semantics for managing auto-incrementing values, and eliminates several edge case problems that can occur with the traditional SERIAL type.