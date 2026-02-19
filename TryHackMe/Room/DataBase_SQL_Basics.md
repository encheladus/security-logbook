# Database SQL Basics — TryHackMe

**Date:** 2026-02-17  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduces the fundamentals of databases and SQL.  
The objective was to understand how data is structured in relational databases and how to write basic SQL queries to retrieve information.

---

## 2) Initial hypothesis

Before starting, the goal was to:

- Understand what data is and why it matters  
- Explain what a database is and why it is used  
- Understand what SQL is and what it is used for  
- Identify tables, rows, and columns  
- Write simple SQL queries to retrieve information  

I expected this room to focus on foundational concepts necessary before studying database-related attacks such as SQL Injection.

---

## 3) Tools used

- TryHackMe in-browser SQL environment  

---

## 4) Approach (high level)

The room was approached progressively, starting from conceptual understanding before writing queries.

First, I focused on understanding how databases are structured:

- A database stores structured data in tables.  
- A table is organised into columns (fields) and rows (records).  
- Each column defines a specific type of data (e.g., `username`, `price`).  
- Each row represents a complete record (e.g., one order or one user).  

Then, I moved to SQL basics and learned how to query data using key clauses:

- `SELECT` to specify which data to retrieve  
- `FROM` to define the table  
- `WHERE` to filter results  
- `ORDER BY` to sort results  

I practiced writing simple queries such as:

- Retrieving all columns from a table  
- Selecting specific columns only  
- Filtering rows based on conditions  
- Sorting results in ascending and descending order  
- Combining filtering and sorting in a single query  

The focus was on understanding the logical structure of a query rather than memorizing syntax.

---

## 5) Results / Evidence (sanitized)

By the end of the room, I was able to:

- Retrieve full tables using `SELECT * FROM table;`  
- Extract specific columns to reduce unnecessary data exposure  
- Filter results using conditions (e.g., selecting specific records)  
- Sort results in ascending and descending order  
- Combine multiple clauses in a structured and readable way  

I clearly understood that `SELECT` queries retrieve data without modifying the database state.

---

## 6) Recommended remediation

From a defensive and secure development perspective:

- Use parameterized queries to prevent SQL Injection.  
- Avoid dynamically concatenating user input into SQL queries.  
- Apply the principle of least privilege to database accounts.  
- Restrict unnecessary data exposure by selecting only required columns.  
- Implement proper input validation on all user-supplied data.  

---

## 7) Lessons learned

- A database stores structured data in tables composed of rows and columns.  
- SQL is the language used to communicate with relational databases.  
- `SELECT` retrieves data without altering it.  
- `WHERE` filters records before results are returned.  
- `ORDER BY` sorts the final result set.  
- Query structure and syntax precision are critical; small mistakes break queries.  
- Understanding normal SQL behaviour is mandatory before studying SQL Injection.  
- Databases are a central attack surface in web applications.  

---

## 8) Links / Resources

- Room: TryHackMe – Database  
- SQL documentation: https://www.w3schools.com/sql/  
- Personal notes: Notion (private)  

---