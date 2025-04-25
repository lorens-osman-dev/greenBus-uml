---
tags:
  - green_bus-cluster
parent: "[[CLUSTERS/green_bus-cluster/db/prototyping advices/General Rules|General Rules]]"
generation: 4
---
# Never assume that any data will be bounded
Example :Your assumption that  "Users will never have usernames longer than 20 characters."
## The Concept of `Bounded Data`
When we say data is "bounded," we mean it has a defined, fixed limit or range. In database design, this often refers to:
- **String lengths:** How many characters a text field can hold.
- **Numeric ranges:** The minimum and maximum values a number can have.
- **Cardinality:** The number of related records (e.g., one user can have a maximum of 5 addresses).

**The Pitfall of Assuming Bounds**
The problem arises when we _assume_ a bound without concrete, future-proof evidence. Here's a scenario:

**Example: Designing a "User" Table**
Let's say you're designing a database for a social media platform. You have a `users` table with a `username` field.
- **Your Initial Assumption (Incorrect):** "Users will never have usernames longer than 20 characters. Let's set the `username` field to `VARCHAR(20)`."
- **The Problem:**
    
    - What if, in the future, your platform decides to allow users to use their full names as usernames, and some users have very long names?
    - What if your platform expands to support languages with longer character sets?
    - Now you have to alter your database, which can be difficult on a production database, and you might have to truncate usernames.
- **The Correct Approach (Avoiding Assumptions):**
    - Instead of `VARCHAR(20)`, you could use `VARCHAR(255)` or `TEXT`. `TEXT` has no length restrictions.
    - If you have a very strong reason to limit the length, add a comment in the database schema explaining why this limit is in place.
    - Consider adding a second field for display name.

**Another Example: Phone Numbers**
- **Your Initial Assumption (Incorrect):** "Phone numbers are always 10 digits. Let's use `VARCHAR(10)`."
- **The Problem:**
    - International phone numbers can vary significantly in length.
    - Some countries use extensions or other prefixes.
    - You are limiting your application to one country, and making internationalization much harder.
- **The Correct Approach (Avoiding Assumptions):**
    - Use `VARCHAR(20)` or similar, or have separate fields for country code, area code, and number.
    - Store the phone number with international format.

