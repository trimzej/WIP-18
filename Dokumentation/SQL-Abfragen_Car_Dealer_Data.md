# SQL-Abfragen - Car_Dealer_Data-Datenbank

## Fokus
Da die Chinook-Datenbank eine etablierte Datenbank ist und das LLM möglicherweise Kenntniss der Datenbank hat, wurde die Car_Dealer_Data-Datenbank herangezogen, um die Bias zu überprüfen.

Im folgenden werden die Datenbankabfragen vorgestellt, welche für die Überprüfung der Bias verwendet wurden. Hierbei wurden einige der Abfragen, welche bereits für die Chinook-Datenbank verwendet wurden genommen und an die Daten dieser, dem LLM weniger bekannten, Datenbank angepasst. Somit soll sichergestellt werden, dass kein abweichender Schwierigkeitsgrad entsteht und somit die Ergebnisse verzerrt.


## Abfragen
### Stufe 1: (SELECT, FROM, WHERE):
SELECT *
FROM customers
WHERE firstname = 'Ashley';
* Zeige mir die Daten aller Kunden mit dem Vornamen 'Ashley'

---

### Stufe 2: (SELECT, DISTINCT, FROM)
SELECT DISTINCT model
FROM cars;
* Liste mir alle einzigartigen Automodelle auf.
---

### Stufe 3: (SELECT, FROM, WHERE, AND)
SELECT *
FROM servicerecords
WHERE mechanicid = 'MTA6ED59YC'
  AND cost > 1500;
* Liste mir alle Servicerecords vom Mechaniker mit ID 'MTA6ED59YC' und kosten von mehr als 1500 auf.

---

### Stufe 4: (SELECT, FROM, ORDER BY, DESC, LIMIT):
SELECT carid
FROM cars
ORDER BY price DESC
LIMIT 5;
* Gib mir die ID der 5 Autos mit dem höchsten Preis.

---

### Stufe 5: (SELECT, SUM, AS, FROM, GROUP BY, ORDER BY, DESC)
SELECT salespersonid, SUM(saleprice) AS TotalSales
FROM sales
GROUP BY salespersonid
ORDER BY TotalSales DESC;
* Zeige mir die kumulierten Verkaufspreise pro Verkäufer in absteigender Reihenfolge.

---

### Stufe 6: (SELECT, COUNT, AVG, AS, FROM, GROUP BY):
SELECT AVG(NumberOfSales) AS AverageNumberOfSales
FROM (
  SELECT COUNT(*) AS NumberOfSales
  FROM sales
  GROUP BY salespersonid
);
* Gib mir die durchschnittliche Anzahl Verkäufe von Verkäufern.

--- 

### Stufe 7: (SELECT, COUNT, AS, FROM, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT customerid, COUNT(*) AS NumberOfTransactions
FROM sales
GROUP BY customerid
ORDER BY NumberOfTransactions DESC
LIMIT 10;
* Zeige mir die ID und die Anzahl Käufe der 10 Kunden mit den meisten Käufen, in absteigender Reihenfolge.

---

### Stufe 9: (SELECT, AS, FROM, JOIN, ON, ORDER BY, WHERE)
SELECT sales.*, customers.firstname AS Name
FROM sales
JOIN customers ON sales.customerid = customers.customerid
WHERE saleprice < 15100
ORDER BY Name;
* Liste mir die Verkäufe mit einem Preis kleiner als 15100, einschliesslich der zugehörigen Vornamen der Kunden und sortiere sie in alphabetischer Reihenfolge basierend auf dem Kunden Name.

---

### Stufe 10: (SELECT, COUNT, AS, FROM, JOIN, ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT mechanics.firstname, COUNT(*) AS NumberOfRepairs
FROM servicerecords
JOIN mechanics ON servicerecords.mechanicid = mechanics.mechanicid
GROUP BY mechanics.firstname
ORDER BY NumberOfRepairs DESC
LIMIT 3;
* Zeige mir die Vornamen und die Anzahl Reparaturen der 3 Mechaniker mit den meisten Reparaturen.

---

### Stufe 13: (SELECT, COUNT, AS, FROM, JOIN (multiple), ON, WHERE, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT salespersons.firstname, COUNT(*) AS NumberOfSales
FROM sales
JOIN salespersons ON sales.salespersonid = salespersons.salespersonid
JOIN cars ON sales.carid = cars.carid
WHERE cars.make = 'Toyota'
GROUP BY salespersons.firstname
ORDER BY NumberOfSales DESC
LIMIT 1;
* Zeig mir den Vornamen und die Anzahl Verkauften Autos der Marke 'Toyota' des Verkäufers, der am meisten Autos der Marke 'Toyota' verkauft hat.

---

### Stufe 14: (SELECT, FROM, AVG, SUM, AS, JOIN (multiple), ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT firstname, SUM(saleprice) AS MoneyEarned, COUNT(*) AS UnitsSold
FROM salespersons
JOIN sales ON salespersons.salespersonid = sales.salespersonid
JOIN cars ON sales.carid = cars.carid
GROUP BY salespersons.salespersonid
ORDER BY MoneyEarned DESC
LIMIT 1;
* Gib mir den Vornamen des Verkäufers, mit den höchsten Gesamteinnahmen aus Autoverkäufen, sowie die summierten Einnahmen dieser Verkäufe, basierend auf den durchschnittlichen Einzelpreisen der Autos und die Anzahl verkaufter Autos dieses Verkäufers.

---

### Stufe 15: (SELECT, FROM, JOIN (multiple), ON, WHERE, EXCEPT):
SELECT DISTINCT customers.customerid, customers.firstname
FROM customers
JOIN sales ON customers.customerid = sales.customerid
JOIN salespersons ON sales.salespersonid = salespersons.salespersonid
WHERE salespersons.firstname = 'Robert'
EXCEPT
SELECT DISTINCT customers.customerid, customers.firstname
FROM customers
JOIN sales ON customers.customerid = sales.customerid
JOIN salespersons ON sales.salespersonid = salespersons.salespersonid
WHERE salespersons.firstname = 'Michael';
* Gib mir den Vornamen und die ID aller Kunden die ein Auto beim Verkäufer Robert gekauft haben und keines beim Verkäufer Michael.

---

### Stufe 18: (SELECT, FROM, SUM, AS, JOIN (multiple), ON, GROUP BY, CASE, WHEN, THEN, ELSE, END):
SELECT salespersons.firstname, 
       SUM(CASE WHEN cars.make = 'Toyota' THEN 1 ELSE 0 END) AS Toyota, 
       SUM(CASE WHEN cars.make = 'Ford' THEN 1 ELSE 0 END) AS Ford, 
       SUM(CASE WHEN cars.make = 'Honda' THEN 1 ELSE 0 END) AS Honda, 
       SUM(CASE WHEN cars.make = 'BMW' THEN 1 ELSE 0 END) AS BMW, 
       SUM(CASE WHEN cars.make = 'Chevrolet' THEN 1 ELSE 0 END) AS Chevrolet, 
       SUM(CASE WHEN cars.make = 'Mercedes' THEN 1 ELSE 0 END) AS Mercedes
FROM salespersons
JOIN sales ON salespersons.salespersonid = sales.salespersonid
JOIN cars ON sales.carid = cars.carid
GROUP BY salespersons.salespersonid;
* Erstelle eine Tabelle, die die Anzahl der Verkauften Autos nach Marke von jedem Verkäufer mit seinem Vornamen aufzeigt, mit Marken: Toyota, Ford, Honda, BMW, Chevrolet, Mercedes.