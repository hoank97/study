# Stored Procedure
## I. What is a Stored Procedure
- A stored procedure is a prepare SQL  code that you can save, so the code can be reused over and over again
- You can store SQL queries as a stored procedure, then call it to execute it
- Stored procedure can also pass the parameters to it

## II. Key characteristics and advantages
 1. **Precompilation**
 - Stored procedures are precompiled and stored in the database. This mean that the DBMS can optimize the execution plan for the stored procedure, leading to faster query execution. Ad-hoc queries, on the other hand, need to be parsed and compiled every time they are executed.
 2. **Reduced Network Traffic**
 - Stored procedures can reduce the amount of data transferred between the application and the database. When using ad-hoc queries, data may need to be sent to the application for processing, while stored procedures can perform more of the data processing within the database itself.
 3. **Security and Access Control**
 - Stored procedures can provide a controlled interface to the database, allowing you to restrict direct access to tables and enforce security policies. This can enhance security and also potentially improve performance by preventing unauthorized or inefficient queries.
 4. **Transaction Management** 
 - Stored procedures can be used to manage transactions effectively, ensuring that a series of SQL operations are executed as a single, atomic transaction. This can help maintain data integrity and performance.
## III. Stored Procedure syntax

```sql
CREATE PROCEDURE procedure_name
AS
sql_statement
GO;
```

## Execute a stored procedure
```sql
EXEC procedure_name;
```

## Stored Procedure with multiple parameters
Example:
- Create
```sql
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @PostalCode nvarchar(10)
AS
SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode
GO;
```
- Execute
```sql
EXEC SelectAllCustomers @City = 'London', @PostalCode = 'WA1 1DP';
```
