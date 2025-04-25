---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
Don't denormalize or otherwise prematurely hack together optimizations, without _at minimum_ constructing a test case, with realistic data size (and other data properties such as cardinalities) to back it up. Even then, make sure you're actually solving a real problem. If realistic queries on realistic data sets will demonstrably take 20 ms longer without the denormalization (or whatever) in place, ask yourself if that's even a problem. If the straightforward schema design causes a web page to load in 1020 ms instead of 1000 ms, it's unlikely to be a problem. On the other hand, if an API the database is serving needs to respond to requests in 10 ms or less, then sure, an extra 20 ms is a problem. But even then, there may be another solution to the problem. In many cases folks fear a vaguely defined "slowness" or "I want to avoid an extra join." Make sure the problem is well defined, at the very least.

Kind of tying into the above: don't make wacky optimization attempts that actually make performance worse. I once (torturously) experienced a TEXT column containing a comma-delimited list of integer IDs referencing another table that should've been a many-to-many junction table. The original designer perhaps didn't think we'd ever need to join, but lo and behold by the time I was summoned to fix a query, I found that the query parsed the delimited ID string at query time on the fly, and used those parsed-out IDs to join to the parent table. (It was a scheduled query that took 4 hours to run when it should've taken seconds.) Additionally, not all of the IDs were a valid reference to the parent table because it couldn't have a foreign key. (I know some folks prefer forgoing FKs, and that's fine, but the reason for forgoing them shouldn't be that integer values are encoded in a string.) On top of that, it didn't even have _type_ validation, so some rows contained alpha characters, which obviously didn't match any of the integer IDs in the parent table!

>[!important]
>You can justify it with a good test case that demonstrates its value

