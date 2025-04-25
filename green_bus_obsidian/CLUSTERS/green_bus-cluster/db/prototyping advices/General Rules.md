---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices|prototyping advices]]"
generation: 3
---
# General Rules 
- Tables naming
	- Use sneak_case always because databases not case sensitive.
	- Avoid any identifiers such as table names that require identifier quoting like "student name".
	- Stick with plural or singular table names consistently.


- Don't use the type `char(n)` Use `TEXT` every where.
- The `money` data type isn't actually very good for storing monetary values. Numeric, or (rarely) integer may be better.
- [[Don't use serial]]

- Be careful when you want to [[Hard or Soft enforce FK keys|hard enforce FK keys or soft enforce]] them.
- add created_at / modified_at to all tables
-  [[Think hard what kind of index type you need]] (integer guid or order capable guid.)
- Never underestimate the network round trip.
- Document "why" you made an architecture or schema decision.
- Never assume that any data will be bounded unless there is an unambiguous certainty

- UUID7
	- [[Use public keys]] in tables which are will exposed to another API's external users.Use UUID7
	- [[Don't use auto incrementing integer primary keys ]](sequences) that will prevent horizontal scaling.Use UUID7

- Normalization 
	- [[Think About How You're Using the Data]]
	- [[Don't denormalize]] unless you have good test case that demonstrates its value.
# Constraints
there is 2 schools
1. Start strict, Loosening later
2. Start loose, Add constraints for the production deployment later
	- It’s  hard to make table structure changes with indexes on. It’s much easier to have your stuff stood up and exactly how you want it then do indexes for the production deployment

- Be careful to "There are some who will say all [[constraints should be enforced in the application tier]]".
- Use constraints as much as possible by default. Don't force them where not applicable
## reddit

i asked on reddit this question 'When designing databases, what's a piece of hard-earned advice you'd share?

I'm creating PostgreSQL UML diagrams for a side project to improve my database design skills,and I'd like to avoid common pitfalls.

What is your steps to start designing databases?

The project is a medium project.

some one replied with :

'

Never underestimate the network roundtrip and there's more than on type of index.

'
explain what he means by simple example


- list 1
	- list 2
		- list 3
			-  list 4
			- random text
	- random text
	- random text