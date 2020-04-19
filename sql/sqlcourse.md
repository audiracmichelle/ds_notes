# sqlcourse.md

- [x] [SQL Course](https://urldefense.proofpoint.com/v2/url?u=http-3A__www.sqlcourse2.com_&d=DwMFaQ&c=5VD0RTtNlTh3ycd41b3MUw&r=lNYEIr-WW4ORZ7IEQXITqQ&m=t74O0k3s3GKq1vDTIy9G6Jjbkbz-j4LQBWgP0utUtMQ&s=ZKX8QV_a9-eBkm7vtgEyB1TyX6Cq36QCITfnRid6_G0&e=)


**typical statement**

```
SELECT <DISTINCT> column1 <column2 | *> <AS alias> <LIMIT limit>
FROM table1 <table2> AS <alias>
WHERE "conditions"
GROUP BY "column_list"
HAVING "conditions"
ORDER BY "column_list" <ASC | DESC>
```

**conditions**

+ **=**, **>**, **<>** or **!=**
+ string comparison `LIKE`, `%` is a wild card
+ `AND`, `OR`
+ `IN`, `NOT IN` it is used to test whether or not a value belongs to a list. List elements are written between parenthesis ('a', 'b', 'c')
+ `BETWEEN` number AND number
+ `IS NULL`, `IS NOT NULL`

**aggregate functions**
+ MIN, MAX, SUM, AVG
+ COUNT	returns the total number of values in a given column
+ COUNT(*)	returns the number of rows in a table

**HAVING** this function works like WHERE but applies to GROUP BY

**math operators**
+ +, -, *, /, %
+ ABS, SIGN, MOD, FLOOR, CEIL, POWER, ROUND(column, d), SQRT

Aliases give a table or a column in a table a temporary name

## Exercises

**items_ordered**

customerid	order_date	item	quantity	price
10330	30-Jun-1999	Pogo stick	1	28.00
10101	30-Jun-1999	Raft	1	58.00
10298	01-Jul-1999	Skateboard	1	33.00
10101	01-Jul-1999	Life Vest	4	125.00
...

**customers**

customerid	firstname	lastname	city	state	 
10101	John	Gray	Lynden	Washington
10298	Leroy	Brown	Pinetop	Arizona
10299	Elroy	Keller	Snoqualmie	Washington
10315	Lisa	Jones	Oshkosh	Wisconsin
...

**OJO!** What are the total number of rows in the items_ordered table?

```
SELECT COUNT(*)
FROM items_ordered;
```


**Exercise 1** From the items_ordered table, select a list of all items purchased for customerid 10449. Display the customerid, item, and price for this customer.

```
SELECT customerid, item, price
FROM items_ordered
WHERE customerid = 10449;
```

**Exercise 2** Select all columns from the items_ordered table for whoever purchased a Tent.

SELECT * FROM items_ordered
WHERE item = 'Tent'; 
```

**Exercise 3** Select the customerid, order_date, and item values from the items_ordered table for any items in the item column that start with the letter "S".

```
SELECT customerid, order_date, item 
FROM items_ordered
WHERE item LIKE 'S%';
```

**Exercise 4** Select the distinct items in the items_ordered table. In other words, display a listing of each of the unique items from the items_ordered table.

```
SELECT DISTINCT *
FROM items_ordered;
```

**Exercise 5** Select the maximum price of any item ordered in the items_ordered table. Hint: Select the maximum price only.

```
SELECT MAX(price) 
FROM items_ordered;
```

**Exercise 6** Select the average price of all of the items ordered that were purchased in the month of Dec.

```
SELECT AVG(price)
FROM items_ordered
WHERE order_date LIKE '%Dec%';
```

**Exercise 7** For all of the tents that were ordered in the items_ordered table, what is the price of the lowest tent? Hint: Your query should return the price only.

```
SELECT MIN(price) FROM items_ordered WHERE item = 'Tent';
```

**Exercise 8** How many people are in each unique state in the customers table? Select the state and display the number of people in each. Hint: count is used to count rows in a column, sum works on numeric data only.

```
SELECT state, count(state)
FROM customers
GROUP BY state;
```

**Exercise 9** From the items_ordered table, select the item, maximum price, and minimum price for each specific item in the table. Hint: The items will need to be broken up into separate groups.

```
SELECT item, max(price), min(price)
FROM items_ordered
GROUP BY item;
```

**Exercise 10** How many orders did each customer make? Use the items_ordered table. Select the customerid, number of orders they made, and the sum of their orders. Click the Group By answers link below if you have any problems.

```
SELECT customerid, count(customerid), sum(price)
FROM items_ordered
GROUP BY customerid;
```

**Exercise 11** How many people are in each unique state in the customers table that have more than one person in the state? Select the state and display the number of how many people are in each if it's greater than 1.

```
SELECT state, count(state) as n
FROM customers
GROUP BY state
HAVING n > 1 ;
```

**Exercise 12** From the items_ordered table, select the item, maximum price, and minimum price for each specific item in the table. Only display the results if the maximum price for one of the items is greater than 190.00.

```
SELECT item, max(price) as m, min(price)
FROM items_ordered
GROUP BY item
HAVING m > 190;
```

**Exercise 13** How many orders did each customer make? Use the items_ordered table. Select the customerid, number of orders they made, and the sum of their orders if they purchased more than 1 item.

```
SELECT customerid, count(customerid) as n, sum(price)
FROM items_ordered
GROUP BY customerid
HAVING n > 1;
```

**Exercise 14** Select the customerid, order_date, and item from the items_ordered table for all items unless they are 'Snow Shoes' or if they are 'Ear Muffs'. Display the rows as long as they are not either of these two items.

```
SELECT customerid, order_date, item
FROM items_ordered
WHERE (item <> 'Snow shoes') AND (item <> 'Ear muffs');
```

**Exercise 15** Select the item and price of all items that start with the letters 'S', 'P', or 'F'.

```
SELECT item, price
FROM items_ordered
WHERE (item LIKE 'S%') OR (item LIKE 'P%');
```

**Exercise 16** Select the date, item, and price from the items_ordered table for all of the rows that have a price value ranging from 10.00 to 80.00.

```
SELECT order_date, item, price
FROM items_ordered
WHERE price BETWEEN 10.00 AND 80.00;
```

**Exercise 17** Select the firstname, city, and state from the customers table for all of the rows where the state value is either: Arizona, Washington, Oklahoma, Colorado, or Hawaii.

```
SELECT firstname, city, state
FROM customers
WHERE state IN ('Arizona', 'Washington', 'Oklahoma', 'Colorado', 'Hawaii');
```
**Exercise 18** Select the item and per unit price for each item in the items_ordered table. Hint: Divide the price by the quantity.

```
SELECT item, price/quantity
FROM items_ordered;
```

**Exercise 19** Write a query using a join to determine which items were ordered by each of the customers in the customers table. Select the customerid, firstname, lastname, order_date, item, and price for everything each customer purchased in the items_ordered table.

```
SELECT c.customerid, c.firstname, c.lastname, i.order_Date, i.item, i.price 
FROM items_ordered as i JOIN customers as c;
```

**Exercise 20** Repeat exercise #1, however display the results sorted by state in descending order.

```
SELECT c.customerid, c.firstname, c.lastname, c.state, i.order_Date, i.item, i.price 
FROM items_ordered as i JOIN customers as c
ORDER BY c.state DESC;
```
