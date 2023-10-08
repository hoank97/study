# Function in SQL
## 1. What is function in sql
- Function in SQL server are the database objects that contains a set of SQL statements to perform a specific task.
- A function accepts input params, perform actions, and then return the result. Function always return either a single value or a table.
- We can build functions one time and use them in multiple locations based on our needs.
- SQL server does not allow to use functions for inserting, deleting  
## 2. Key characteristics
-  Functions always return a value after the execution of queries.
- Each and every time function are called, they must be compiled again.
- The function can be called using Store Procedure
- A function used only to read data.
- A function can be operated in the SELECT statement
- Functions do not permit transaction management
### 3. Types of functions
- `System functions`: Functions that are defined by the system are known as system functions. In other words, all the built-in functions supported by the server are referred to as System functions
- `User defined functions`: Functions that are created by the user in the system database or a user-defined database are known as user-defined functions. The UDF functions accept parameters, perform actions, and returns the result
### 4. Example
This example will create a function to calculate the net sales based on the quantity, price, and discount value:
```sql
CREATE FUNCTION udfNet_Sales(  
    @quantity INT,  
    @price DEC(10,2),  
    @discount DEC(3,2)  
)  
RETURNS DEC(10,2)  
AS   
BEGIN  
    RETURN @quantity * @price * (1 - @discount);  
END;   
```

execute the function:
```sql
SELECT dbo.udfNet_Sales(25, 500, 0.2) AS net_sales;  
```
result:

![Alt text](https://static.javatpoint.com/sqlserver/images/sql-server-functions3.png)