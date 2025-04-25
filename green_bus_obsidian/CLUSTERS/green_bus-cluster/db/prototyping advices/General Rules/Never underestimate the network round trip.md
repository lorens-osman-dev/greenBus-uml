---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
## Never Underestimate the Network Roundtrip

- **What It Means:**
    
    - When your application interacts with a database, it's often over a network. This means data has to travel from your application server to the database server and back.
    - Each "roundtrip" (the journey there and back) takes time, even on fast networks.
    - Too many roundtrips can significantly slow down your application.
- **Simple Example:**
    
    - Imagine you need to retrieve a user's profile and their last 10 orders.
    - **Bad Approach (Multiple Roundtrips):**
        - Your application first queries the `users` table to get the user's profile.
        - Then, it makes a separate query to the `orders` table for each of the user's last 10 orders.
        - This results in 11 network roundtrips!
    - **Good Approach (Fewer Roundtrips):**
        - Write a single, more complex query that retrieves both the user's profile and their last 10 orders in one go.
        - This reduces the number of network roundtrips to just one.
        - This can be done using joins, or subqueries.
    - **Stored Procedures/Functions:**
        - If the database supports it, like PostgreSQL does, move logic into stored procedures or functions. This allows the database to do much of the work, and return only the final result to the application. This drastically reduces network traffic.
- **Key Considerations:**
    
    - **Latency:** Network latency (the delay in data transmission) is a significant factor.
    - **Query Efficiency:** Write efficient queries that retrieve only the necessary data.
    - **Batching:** When possible, batch multiple operations into a single request.
    - **Connection Pooling:** Maintain a pool of database connections to avoid the overhead of establishing new connections for each request.

