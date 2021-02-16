  
# **MYSQL Cheat Sheet**  

The following is a cheat sheet of basic syntax used to set up, and manipulate databases using Mysql.  
This cheat sheet assumes that you already have mysql installed and initialized on your VM.  

First, log into mysql from terminal, or the SSH connection of your choice.  

	mysql> mysql -u root -p  

Enter your mysql root password when prompted.  


## **Databases  

To create a new database, use the CREATE command.  

	mysql> CREATE DATABASE _database_name_here_;  

To work with any of the databases on your server, enter the USE command.  

	mysql> USE _database_name_here_;  

To show databases.  

	mysql> SHOW databases;  

To delkete a database.  

	mysql> DROP -database_name_;  


## **Users**  

Here I will show you how to create new users, show list of currently added users, and grant user privileges.  

**Create user**  

Use the following code to create a new user and password.  

	mysql> CREATE USER '_user_name_here_'@'localhost' IDENTIFIED BY '_your_password_here_';  

Show users.  

	mysql> SELECT * FROM mysql.user;  

**User Privileges**  

In most cases the user privileges are granted based on a particular database. It is common practice for each unique mysql database on a server to have it's own unique user with full privileges. To grant a user full control over a specific database, ure the following syntax.  

	mysql> GRANT ALL PRIVILEGES ON _database_name_here_.* TO '_user_name_here_'@'localhost' IDENTIFIED BY '_your_password_here_';  

After setting new user privileges you need to flush privileges to update.  

	mysql> FLUSH PRIVILEGES;  

The GRANT command allows you to set or restrict a wide range of privileges to users on the database. For a full description of the GRANT commands capabilities, please refer to the official Mysql documentation.  

Show granted permissions to current mysql user.  

	mysql> SHOW GRANTS;  

Show granted permissions for a specific user.  

	mysql> SHOW GRANTS FOR '_user_name_here_'@'localhost';  

To remove privileges for a user on Mysql, simply replace the GRANT command with REVOKE.  

	mysql> REVOKE ALL PRIVELEGES ON _database_name_here_.* FROM '_user_name_here_'@'localhost';  


## **Tables**  

Create tables within your datatbase to store and access information. Use the following syntax to create a new table, and designate the tables columns, and the data types to be stored in them.  

	mysql> CREATE TABLE _table_name_here_ (  
	_column1_name_ _data_type,  
	_column2_name _data_type  
	);  

To show all tables in a database.  

	mysql> SHOW tables;  

To delete a table.  

	mysql> DROP _table_name_;  


## **Adding and Viewing Information**  

To insert information (row) into a table.  

	mysql> INSERT INTO _table_name_    
	(_column1_name_, _column2_name_, ...)  
	VALUES  
	(_column1_value_, _column2_value_, ...);  

To insert multiple rows, simply add more bracketed groups seperated by commas.  

	mysql> INSERT INTO _table_name_  
	(_column1_name_, _column2_name_, ...),
	(_column1_name_, _column2_name_, ...),
	(_column1_name_, _column2_name_, ...)  
	VALUES  
	(_column1_value_, _column2_value_, ...),  
	(_column1_value_, _column2_value_, ...),  
	(_column1_value_, _column2_value_, ...);  

To information in an existing row use UPDATE.  

	mysql> UPDATE _table_name_ SET _column_name_ = _new_value_ WHERE _row_id_ = _row_id_;  

To delete an existing row.  

	mysql> DELETE FROM _table_name_ WHERE _condition_;  

To view all of the information in a table use the * symbol.

	mysql> SELECT * FROM _table_name_;  

To view specific information in a table use the WHERE clause.  

	mysql> SELECT * FROM _table_name_ WHERE _information_condition_;  

To view multiple conditional values in the WHERE clause use IN.  

	mysql> SELECT * FROM _table_name_ WHERE _column_name_ IN (_value1_, _value2_, ...);  

To view all information whithin a certain range on a table simply add a BETWEEN condition to the WHERE clause.  

	mysql> SELECT * FROM _table_name_ WHERE _condition_ BETWEEN _range_start_ AND _range_end_;  

To view information omitting rows with duplicate values use DISTINCT.  

	mysql> SELECT DISTINCT _column_name_ FROM _table_name_;  

To view information in a certain order use ORDER BY and specify ascending or descending order.  

	mysql> SELECT * FROM _table_name_ ORDER BY _column_name_ [ASC | DESC];  

To order information by one column while avoiding duplicate values from another column use DISTINCT with ORDER BY. 

	mysql> SELECT DISTINCT _distinct_column_, _order_by_column_ ORDER BY _order_by_column_;  
	
To add a column to an existing table use ALTER TABLE to ADD the column, data type, and any constraint if desired.  

	mysql> ALTER TABLE _table_name_  
	ADD _column_name_ _data_type_ _column_constraint_;  

To modify an existing column (change data type).  

	mysql> ALTER TABLE _table_name_  
	MODIFY _column_name_ _new_data_type_;  

To concatinate columns together use the CONCAT() method.  

	mysql> SELECT _column1_name_, _column2_name_, ...) AS _display_group_name_ FROM _table_name_;  

To view information containing certain values use the LIKE constraint with the WHERE clause.  

	mysql> SELECT * FROM _table_name_ WHERE _column_name_ LIKE _pattern_or_value_;  

Some examples of LIKE operators are as follows:  

WHERE CustomerName LIKE 'a%' 	Finds any values that start with "a"  
WHERE CustomerName LIKE '%a' 	Finds any values that end with "a"  
WHERE CustomerName LIKE '%or%' 	Finds any values that have "or" in any position  
WHERE CustomerName LIKE '_r%' 	Finds any values that have "r" in the second position  
WHERE CustomerName LIKE 'a_%' 	Finds any values that start with "a" and are at least 2 characters in length  
WHERE CustomerName LIKE 'a__%' 	Finds any values that start with "a" and are at least 3 characters in length  
WHERE ContactName LIKE 'a%o' 	Finds any values that start with "a" and ends with "o"  

Indexes are best added when you CREATE your table, but can also be added to existing tables as well. For demonstration purposes we'll use the UNIQUE index. The UNIQUE index prevents duplicate value entries within a column.   

To add an index.  

	mysql> ALTER TABLE _table_name_ ADD UNIQE _index_name_ (_column_list_);  

To remove an index.  

	mysql> ALTER TABLE _table_name_ DROP UNIQUE (_column_list);  


## **Joining Tables**  

To access information from multiple tables we use JOIN. Each table will be created with a Primary Key(PK), this PK will be included as a Foreign Key(FK) in any other tables needing to access information from it.  

Create tables with FK and PK.  

	mysql> CREATE TABLE Products (  
	productID INT NOT NULL AUTO_INCREMENT,  
	product_name VARCHAR(100) NOT NULL,  
	product_quantity INT NOT NULL,
	PRIMARY KEY (productID)  
	);  
	
	mysql> CREATE TABLE Sales (  
	sale_number INT NOT NULL AUTO_INCREMENT,  
	units_sold INT NOT NULL,  
	unit_price FLOAT NOT NULL,  
	productID INT NOT NULL,  
	date_of_sale DATE NOT NULL,  
	PRIMARY KEY (sale_number),
	FOREIGN KEY (productID)  
	);  

Assuming tables hold information, view Sales showing units sold, unit price, date of sale and product name.  

	mysql> SELECT Sales.units_sold, Sales.unit_price, Sales.date_of_sale, Products.product_name  
	FROM Sales  
	INNER JOIN  
	Products  
	ON  
	Sales.productID = Products.productID;  

To join more than two tables simply add another JOIN statement after the first.  

	mysql> SELECT Sales.units_sold, Sales.unit_price, Sales.date_of_sale, Products.product_name  
	FROM Sales  
	INNER JOIN  
	Products  
	ON  
	Sales.productID = Products.productID  
	INNER JOIN  
	_table_name_  
	ON  
	Sales.FK = _table_name_.PK;  











	


