---
tags:
  - green_bus-cluster
parent: "[[db|db]]"
generation: 2
---
# General Rules
[[General Rules]]

# Making database workflow steps (Postgres + ORM)
1. Write down **all** the information about the system in your head(كب كل شي براسك)
	- Define users:
		- What user information is needed?
		- what users can do?
	- Scenes: Scenarios describing user interactions with the system, based on the defined users and their capabilities.
	- List all entities that will emerge when considering what users can do and how they interact with the system.
2. Define Database Schema : 
	- Define all tables and their columns.
	- Define their data types.
3. Establish Relationships :
	- Define relationships between entities (one-to-one, one-to-many, many-to-many).
	- Define constraints :primary keys..
4. Normalize Data : Apply normalization techniques to optimize structure and eliminate redundancy.
5. Check [Don't Do This](https://wiki.postgresql.org/wiki/Don't_Do_This)
6. Create ORM Models : 
	- Implement object-relational mapping (ORM) models to map database tables to application entities.
	- useful to test database queries against business requirements 
7. Seed the Database : 
	- Populate the database with initial test data (seeding) for development and testing purposes.
8. Query Validation (Test Queries) : 
	- Verify expected results : Test database queries against business requirements and verify that queries retrieve the desired data.
	- Performance :  Verify that the required queries can be executed efficiently.
9. Repeat (1 -> 6) if there is an issues :
	- Revisit and refine the schema, relationships, or queries.
10. implement schema migrations to track changes.
11. Add new features :
	- Explore new features as needed or when business requirements evolve.
12. Repeat.
# prisma
- use indexes in prisma for speed
- use cascade for deleting data with prisma
- fix ambiguous relations annotate the relation fields with the `@relation` attribute and provide the `name` argument


## need formating

https://gemini.google.com/app/6725931b89860dba?hl=en