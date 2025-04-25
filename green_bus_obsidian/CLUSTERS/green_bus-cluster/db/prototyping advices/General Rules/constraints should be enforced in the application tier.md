---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
There are some who will say all constraints should be enforced in the application tier, so to continue with this example, they'd be against adding the phone number check constraint. I simply don't believe them based on my experience. What tends to happen is the one single application being the source of truth becomes two or three or more. Maybe the one application is demoted to the legacy application when a new one is created. Often the two are run concurrently until the legacy can be phased out. But oops, the new application doesn't have the same format validation. Also, again in my experience, there is always a way to insert data outside of the application, such as submitting a SQL script to the DBA to run, because maybe the application doesn't have an admin UI to handle a certain kind of data change. So there's always a way to bypass application-tier constraints.