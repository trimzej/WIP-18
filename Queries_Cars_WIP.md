- **Stufe 1**: Zeige mir alle Kunden mit dem Vornamen 'Ashley'  
**SQL Statement**  
SELECT * FROM customers WHERE firstname = 'Ashley';

---

- **Stufe 2**: Liste mir alle einzigartigen Automodelle auf.
SELECT DISTINCT model FROM cars;


- Stufe 3: Liste mir alle Servicerecords vom Mechaniker mit ID 'MTA6ED59YC' und kosten von mehr als 1500 auf.
SELECT * FROM servicerecords WHERE mechanicid = 'MTA6ED59YC' AND cost > 1500;

---

- Stufe 4: Gib mir die ID der 5 Autos mit dem höchsten Preis.
SELECT carid FROM cars ORDER BY price DESC LIMIT 5;

---

- Stufe 5: Zeige mir die kumulierten Verkaufspreise pro Verkäufer in absteigender Reihenfolge.
SELECT salespersonid, SUM(saleprice) AS TotalSales FROM sales GROUP BY salespersonid ORDER BY TotalSales DESC;

---

- Stufe 6: Gib mir die durchschnittliche Anzahl Verkäufe von Verkäufern.
SELECT AVG(numberOfSales) AS averageNumberOfSales FROM (SELECT COUNT(*) AS numberOfSales FROM sales GROUP BY salespersonid) AS numberOfSales;

---

- Stufe 7: Zeige mir die ID und die Anzahl Käufe der 10 Kunden mit den meisten Käufen, in absteigender Reihenfolge.
SELECT customerid, COUNT(*) AS numberOfTransactions FROM sales GROUP BY customerid ORDER BY numberOfTransactions DESC LIMIT 10;

---

- Stufe 8: Liste mir die Verkäufe mit einem Preis kleiner als 15100, einschliesslich der zugehörigen Vornamen der Kunden und sortiere sie in alphabetischer Reihenfolge basierend auf dem Kunden Name.
SELECT sales.*, customers.firstname AS Name FROM sales JOIN customers ON sales.customerid = customers.customerid WHERE saleprice < 15100 ORDER BY Name;

---

- Stufe 9: Zeige mir die Vornamen und die Anzahl Reparaturen der 3 Mechaniker mit den meisten Reparaturen.
SELECT mechanics.firstname, COUNT(*) AS numberOfRepairs FROM servicerecords JOIN mechanics ON servicerecords.mechanicid = mechanics.mechanicid GROUP BY firstname ORDER BY numberOfRepairs DESC LIMIT 3;

---

- Stufe 10: Zeig mir den Vornamen und die Anzahl Verkäufe (Angebote) des Verkäufers, der am meisten Autos der Marke 'Toyota' verkaufte.
SELECT salespersons.firstname, COUNT(*) AS numberOfSales FROM sales JOIN salespersons ON sales.salespersonid = salespersons.salespersonid JOIN cars ON sales.carid = cars.carid WHERE cars.make = 'Toyota' GROUP BY firstname ORDER BY numberOfSales DESC LIMIT 1;

---

- Stufe 11: Gib mir den Vornamen des Verkäufers, mit den höchsten Gesamteinnahmen aus Autoverkäufen, sowie die summierten Einnahmen dieser Verkäufe und die Anzahl verkaufter Autos dieses Verkäufers.
SELECT firstname, SUM(saleprice) AS moneyEarned, COUNT(*) AS unitsSold FROM salespersons JOIN sales ON salespersons.salespersonid = sales.salespersonid JOIN cars ON sales.carid = cars.carid GROUP BY salespersons.salespersonid ORDER BY moneyEarned DESC LIMIT 1;

---

- Stufe 12: Gib mir die Namen aller Kunden die ein Auto beim Verkäufer Robert gekauft haben und keines beim Verkäufer Michael.
>HAT EIN JOIN WENIGER...
- SELECT customers.firstname FROM customers JOIN sales ON customers.customerid = sales.customerid JOIN salespersons ON sales.salespersonid = salespersons.salespersonid WHERE salespersons.firstname = 'Robert' EXCEPT SELECT customers.firstname FROM customers JOIN sales ON customers.customerid = sales.customerid JOIN salespersons ON sales.salespersonid = salespersons.salespersonid WHERE salespersons.firstname = 'Michael';

---

- Stufe 13: Erstelle eine Tabelle, die die Anzahl der Verkauften Autos nach Marke von jedem Verkäufer mit seinem Vornamen aufzeigt, mit Marken: Toyota, Ford, Honda, BMW, Chevrolet, Mercedes.
SELECT salespersons.firstname, SUM(CASE WHEN cars.make = 'Toyota' THEN 1 ELSE 0 END) AS Toyota, SUM(CASE WHEN cars.make = 'Ford' THEN 1 ELSE 0 END) AS Ford, SUM(CASE WHEN cars.make = 'Honda' THEN 1 ELSE 0 END) AS Honda, SUM(CASE WHEN cars.make = 'BMW' THEN 1 ELSE 0 END) AS BMW, SUM(CASE WHEN cars.make = 'Chevrolet' THEN 1 ELSE 0 END) AS Chevrolet, SUM(CASE WHEN cars.make = 'Mercedes' THEN 1 ELSE 0 END) AS Mercedes FROM salespersons JOIN sales ON salespersons.salespersonid = sales.salespersonid JOIN cars ON sales.carid = cars.carid GROUP BY salespersons.salespersonid;