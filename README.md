# java-sql

A student that completes this project shows that they can:
* Query data from a single table
* Query data from multiple tables
* Create a new datadaase using PostgreSQL

# Introduction

Working with SQL

# Instructions

Surf to [SQL Try Editor at W3Schools.com](https://www.w3schools.com/Sql/tryit.asp?filename=trysql_select_top)  

### **Clicking the `Restore Database` button in the page will repopulate the database with the original data and discard all changes you have made**.

Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below. You will be submitting that through the regular fork, change, pull process.


### find all customers that live in London. Returns 6 records.
SELECT * FROM customers WHERE city='London';

### find all customers with postal code 1010. Returns 3 customers.
SELECT * FROM customers WHERE postalcode='1010';

### find the phone number for the supplier with the id 11. Should be (010) 9984510.
SELECT Phone FROM Suppliers WHERE SupplierID=11;

### list orders descending by the order date. The order with date 1997-02-12 should be at the top.
SELECT * FROM Orders ORDER BY OrderDate DESC;

### find all suppliers who have names longer than 20 characters. You can use `length(SupplierName)` to get the length of the name. Returns 11 records.
SELECT * FROM Suppliers WHERE LEN(SupplierName) > 20;

### find all customers that include the word "market" in the name. Should return 4 records.
SELECT * FROM customers WHERE CustomerName LIKE '%market%';

> Don't forget the wildcard '%' symbols at the beginning and end of your substring to denote it can appear anywhere in the string in question

### add a customer record for _"The Shire"_, the contact name is _"Bilbo Baggins"_ the address is _"1 Hobbit-Hole"_ in _"Bag End"_, postal code _"111"_ and the country is _"Middle Earth"_.
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ("The Shire", "Bilbo Baggins", "1 Hobbit-Hole in Bag End", "The Shire", "111", "Middle Earth");

### update _Bilbo Baggins_ record so that the postal code changes to _"11122"_.
UPDATE Customers
SET PostalCode = '11122'
WHERE ContactName = "Bilbo Baggins";

### list orders grouped by customer showing the number of orders per customer. _Rattlesnake Canyon Grocery_ should have 7 orders.
SELECT CustomerName, customers.CustomerID, COUNT(*) AS number_of_orders
FROM  customers, orders
WHERE customers.CustomerID = orders.CustomerID
GROUP BY CustomerName


> There is more information about the COUNT clause on [W3 Schools](https://www.w3schools.com/sql/sql_count_avg_sum.asp)

### list customers names and the number of orders per customer. Sort the list by number of orders in descending order. _Ernst Handel_ should be at the top with 10 orders followed by _QUICK-Stop_, _Rattlesnake Canyon Grocery_ and _Wartian Herkku_ with 7 orders each.
SELECT CustomerName, customers.CustomerID, COUNT(*) AS number_of_orders
FROM  customers, orders
WHERE customers.CustomerID = orders.CustomerID
GROUP BY CustomerName
ORDER BY number_of_orders DESC;

### list orders grouped by customer's city showing number of orders per city. Returns 58 Records with _Aachen_ showing 2 orders and _Albuquerque_ showing 7 orders.
SELECT City, customers.CustomerID, COUNT(*) AS number_of_orders
FROM  customers, orders
WHERE customers.CustomerID = orders.CustomerID
GROUP BY City
ORDER BY number_of_orders DESC;


## Data Normalization

Note: This step does not use PostgreSQL!

Take the following data and normalize it into a 3NF database.

| Person Name | PersonID | Fenced Yard | City Dweller |
|-------------|----------|-------------|--------------|
| Jane        | 1        | No          | Yes          |
| Bob         | 2        | No          | No           |
| Sam         | 3        | Yes         | No           |


| PetID | PersonID| PetName   | PetType   |
|-------|---------|-----------|-----------|
| 1     | 1       | Ellie     | Dog       |
| 2     | 1       | Tiger     | Cat       |
| 3     | 1       | Toby      | Turtle    |
| 4     | 2       | Joe       | Horse     |
| 5     | 3       | Ginger    | Dog       |
| 6     | 3       | MKitty    | Cat       |
| 7     | 3       | Bubble    | Fish      |

---
## Stretch Goals

// *DONE* delete all customers that have no orders. Should delete 17 (or 18 if you haven't deleted the record added) records.
DELETE FROM Customers 
WHERE CustomerID NOT IN 
       (SELECT CustomerID 
FROM Orders)

> In the WHERE clause, you can provide another list with an IN keyword this list can be the result of another SELECT query. Write a query to return a list of CustomerIDs that meet the criteria above. Pass that to the IN keyword of the WHERE clause as the list of IDs to be deleted
 
// *DONE* > Use a LEFT JOIN to join the Orders table onto the Customers table and check for a NULL value in the OrderID column 

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = null
ORDER BY Customers.CustomerName;

// *DONE* Create Database and Table

### Keep track of the code you write and paste at the end of this document

- use pgAdmin to create a database, naming it `budget`.
- add an `accounts` table with the following _schema_:

  - `id`, numeric value with no decimal places that should autoincrement.
  - `name`, string, add whatever is necessary to make searching by name faster.
  - `budget` numeric value.

- constraints
  - the `id` should be the primary key for the table.
  - account `name` should be unique.
  - account `budget` is required.
----------------------------CODE------------------------------------
CREATE DATABASE budget; 
CREATE TABLE acounts (
    id LONG PRIMARY KEY,
    name VARCHAR(255) UNIQUE,
    budget INT NOT NULL
); 
