# SQL Project
This is a project I did as part of my comprehensive exams in fulfillment of my degree in Computer Information Systems and Business Analystics. Based on the Entity Relationship Diagram (ERD) and the data, sql queries and statements are written using SQLite to create a database in order to extract data to answer some questions.

#### Entity Relationship Diagram (ERD)
![ERD](https://github.com/kdamoah/sql-project/blob/main/images/ERD.JPG "Entity Relationship Diagram")

#### Customer Table
![customer](https://github.com/kdamoah/sql-project/blob/main/images/customer%20table.JPG "Customer Table")

#### Sales Person Table
![sales person](https://github.com/kdamoah/sql-project/blob/main/images/sales%20person%20table.JPG "Sales Person Table")

#### Encounter Table
![encounter](https://github.com/kdamoah/sql-project/blob/main/images/encounter%20table.JPG "encounter")

#### Credit Rating Table
![credit rating](https://github.com/kdamoah/sql-project/blob/main/images/credit%20rating%20table.JPG "credit rating")



## Answered Questions and SQL Queries
**Question 1**: Write statements for creating the four tables & insert data into each table as shown above.

CREATE TABLE "Customer" (
	"CustomerID"	INTEGER,
	"CustomerFirstName"	TEXT,
	"CustomerLastName"	TEXT,
	"CustomerPhone"	TEXT,
	"AnnualIncome"	REAL,
	"CreditID"	INTEGER,
	FOREIGN KEY("CreditID") REFERENCES "CreditRating"("CreditID"),
	PRIMARY KEY("CustomerID")
);

CREATE TABLE "SalesPerson" (
	"SalesID"	INTEGER,
	"SalesFirstName"	TEXT,
	"SalesLastName"	TEXT,
	"SalesHireDate"	TEXT,
	"SalesSalary"	REAL,
	PRIMARY KEY("SalesID")
);

CREATE TABLE "Encounter" (
	"EncID"	TEXT,
	"SalesID"	INTEGER,
	"CustomerID"	INTEGER,
	"EncDate"	TEXT,
	"Purchase"	TEXT,
	FOREIGN KEY("SalesID") REFERENCES "SalesPerson"("SalesID"),
	FOREIGN KEY("CustomerID") REFERENCES "Customer"("CustomerID")
);

CREATE TABLE "CreditRating" (
	"CreditID"	INTEGER,
	"CreditDescription"	TEXT,
	"MinFICO"	INTEGER,
	"MaxFICO"	INTEGER,
	"Comments"	TEXT,
	PRIMARY KEY("CreditID")
);

INSERT INTO Customer
VALUES (1, "Clark", "Adams", 8017686043, 64250.00, 1),
(10, "Hans", "Joiner", 8017638922, 38125.00, 6),
(11, "Carl", "Hughes", 8013733284, 115625.00, 5),
(12, "Simon", "Prescott", 8013745487, 63625.00, 6),
(2, "Pablo", "Martinez", 8013731976, 61875.00, 2),
(3, "Susanna", "Miner", 8017631882, 120250.00, 4),
(4, "Femi", "Silva", 8013731465, 24250.00, 5),
(5, "Lola", "McCloud", 8013746692, 91375.00, 6),
(6, "Maggy", "Redmond", 8017667251, 46375.00, 6),
(7, "Lile", "Kimball", 8017855151, 52250.00, 3),
(8, "Okon", "Okur", 8013561024, 29250.00, 4),
(9, "Eric", "Knudsen", 8017689149, 40875.00, 7);

INSERT INTO SalesPerson
VALUES (1, "Lewis", "Peoples", "1989-02-13", 140000.00),
(2, "Richard", "Martin", "1989-05-02", 82000.00),
(3, "Juan", "Rodriguez", "1989-05-02", 93000.00),
(4, "Rachel", "Scholls", "1996-04-27", 56000.00),
(5, "Jesse", "Lukes", "1996-05-15", 75000.00),
(6, "Maggy", "Adelman", "2001-06-01", 75000.00);

INSERT INTO Encounter
VALUES ("001", 1, 2, "2019-07-01", "Yes"),
("002", 1, 4, "2019-07-16", "Yes"),
("003", 2, 5, "2019-08-01", "Yes"),
("004", 2, 9, "2019-08-12", "Yes"),
("005", 3, 1, "2019-08-13", "No"), 
("006", 3, 12, "2019-08-19", "Yes"),
("007", 3, 11, "2019-09-02", "Yes"),
("008", 4, 10, "2019-09-03", "No"),
("009", 5, 6, "2019-10-06", "Yes"),
("010", 6, 8, "2019-10-18", "Yes"),
("011", 6, 3, "2019-07-02", "Yes"),
("012", 6, 7, "2019-07-02", "Yes");

INSERT INTO CreditRating
VALUES (1, "Extremely Poor", 300, 499, "Cannot extend credit"),
(2, "Very Poor", 500, 580, "Owner approval required to extend credit"),
(3, "Poor", 580, 619, "Credit extended at extremely high interest rates"),
(4, "Fair", 620, 679, "Credit extended at high interest rates"),
(5, "Good", 680, 699, "Credit extended at normal interest rates"),
(6, "Very Good", 700, 850, "Credit extended at low interest rates");

INSERT INTO CreditRating (CreditID, CreditDescription, Comments)
VALUES (7, "Unkown", "Paid cash without looking into financing options");


**Question 2**: Write a query to show a list of customers whose last name begin with the letter “K”. Show the first and last names of these customers. Sort the list of customers in descending order by last name.

SELECT CustomerFirstName, CustomerLastName 
FROM Customer 
WHERE CustomerLastName LIKE 'K%'
ORDER BY CustomerLastName DESC;


**Question 3**: Write a query to generate a list of customers with annual incomes greater than $50,000 that purchased a car. Show the first name, last name, and annual income for each of these customers. (HINT: Purchase will have a value of “Yes”)

SELECT CustomerFirstName, CustomerLastName, AnnualIncome
FROM Customer, Encounter
WHERE Customer.CustomerID = Encounter.CustomerID AND AnnualIncome > 50000 AND Purchase = 'Yes';


**Question 4**: Write a query to find which customers purchased vehicles despite having a “Good” or “Very Good” credit description?  Show the first name, last name, and credit description for these customers.

SELECT CustomerFirstName, CustomerLastName, CreditDescription
FROM Customer, Encounter, CreditRating
WHERE Customer.CustomerID = Encounter.CustomerID AND Customer.CreditID = CreditRating.CreditID AND Purchase = 'Yes' AND CreditDescription IN ('Good', 'Very Good');


**Question 5**: Write a query that list salespeople’s first name, last name, and salary for salespeople who have 2 or more customers.

SELECT SalesPerson.SalesFirstName, SalesPerson.SalesLastName, SalesPerson.SalesSalary
FROM Encounter
INNER JOIN SalesPerson ON Encounter.SalesID = SalesPerson.SalesID
GROUP BY SalesFirstName
HAVING COUNT(SalesFirstName) >= 2;


**Question 6**: Write a query to compute a commission (5% of salary) and show this calculated field as commission for salespeople who sold 3 or more cars. Also display salespeople’s first name and last name. Sort the list in ascending order by salespeople’s last name.

SELECT SalesPerson.SalesFirstName, SalesPerson.SalesLastName, (SalesPerson.SalesSalary * 0.05) AS SalesCommission
FROM Encounter
INNER JOIN SalesPerson ON Encounter.SalesID = SalesPerson.SalesID
GROUP BY SalesFirstName
HAVING Purchase = 'Yes' AND COUNT(Purchase) >= 3
ORDER BY SalesLastName ASC;


**Question 7**: Construct a query to show the salespeople’s first name and the average annual income of their customers as “Average Income” in your result. (HINT: You do not need to include a criterion for Purchase in this query)

SELECT SalesFirstName, AVG(AnnualIncome) AS 'Average Income'
FROM Encounter
INNER JOIN Customer ON Encounter.CustomerID = Customer.CustomerID
INNER JOIN SalesPerson ON Encounter.SalesID = SalesPerson.SalesID
GROUP BY SalesFirstName;
