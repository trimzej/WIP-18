# SQL-Abfragen - Chinook-Datenbank

## Fokus
Im Folgenden werden die Datenbankabfragen vorgestellt, die im Rahmen dieser Studie verwendet werden, um die Leistungsfähigkeit von KI bei der Generierung von SQL-Abfragen zu untersuchen. Die Abfragen sind so strukturiert, dass sie eine stufenweise Steigerung der Komplexität aufweisen.

Die Komplexität der einzelnen Stufen wird durch die Anzahl der Operationen in jeder Abfrage definiert. Mit jeder weiteren Stufe werden zusätzliche Operationen (wie COUNT, JOIN etc.) eingeführt, wobei die Operationen der vorherigen Stufen weitestgehend beibehalten werden. Die Operationen sind nach der vermuteten Komplexität für den KI-Algorithmus geordnet.

Dieser systematische Ansatz soll Einblicke in die Stärken und Schwächen von KI-Algorithmen ermöglichen. Die Auflistung der Abfragen ist nicht endgültig und kann im Verlauf der Forschung erweitert oder angepasst werden.

## Abfragen
### Stufe 1: (SELECT, FROM, WHERE):
SELECT *
FROM Customer
WHERE City = 'Prague';
* Zeige mir die Daten aller Kunden aus Prague.

---

### Stufe 2: (SELECT, DISTINCT, FROM)
SELECT DISTINCT BillingCountry
FROM Invoice;
* Liste mir alle einzigartigen Rechnungsstellungs-Länder auf.

---

### Stufe 3: (SELECT, FROM, WHERE, AND)
SELECT *
FROM Invoice
WHERE BillingCountry = 'Brazil'
  AND Total > 6;
* Liste mir alle Rechnungen aus Brasilien mit einem Betrag von mehr als 6 auf.

---

### Stufe 4: (SELECT, FROM, ORDER BY, DESC, LIMIT):
SELECT Name
FROM Track
ORDER BY Milliseconds DESC
LIMIT 5;
* Gib mir die Namen der 5 Tracks mit der längsten Dauer.

---

### Stufe 5: (SELECT, SUM, AS, FROM, GROUP BY, ORDER BY, DESC)
SELECT BillingCountry,
       SUM(Total) AS TotalSales
FROM Invoice
GROUP BY BillingCountry
ORDER BY TotalSales DESC;
* Zeige mir den Gesamtumsatz pro Land in absteigender Reihenfolge.

---

### Stufe 6: (SELECT, COUNT, AVG, AS, FROM, GROUP BY):
SELECT AVG(NumberOfTracks) AS AverageNumberOfTracks
FROM (
    SELECT COUNT(*) AS NumberOfTracks
    FROM Track
    GROUP BY AlbumId
);
* Gib mir die durchschnittliche Anzahl Tracks in einem Album.

--- 

### Stufe 7: (SELECT, COUNT, AS, FROM, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT ArtistId,
       COUNT(*) AS NumberOfAlbums
FROM Album
GROUP BY ArtistId
ORDER BY NumberOfAlbums DESC
LIMIT 10;
* Zeige mir die ID und die Anzahl Alben der 10 Künstler mit den meisten Alben, in absteigender Reihenfolge.

---

### Stufe 8: (SELECT, DISTINCT, FROM, WHERE, AND, JOIN, ON)
SELECT DISTINCT Customer.FirstName,
                Customer.LastName
FROM Customer
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
WHERE Customer.Country = 'USA'
  AND Invoice.Total > 20;
* Zeige mir die Vornamen und Nachnamen der Kunden aus den USA, die mindestens eine Rechnung mit einem Betrag von über 20 haben.

---

### Stufe 9: (SELECT, AS, FROM, JOIN, ON, ORDER BY, WHERE)
SELECT InvoiceLine.*,
       Track.Name AS Track
FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
WHERE InvoiceLineId < 21
ORDER BY Track;
* Liste mir die Rechnungsposten mit ID 1-20 auf, einschliesslich der zugehörigen Namen der Tracks und sortiere sie in alphabetischer Reihenfolge basierend auf dem Track Namen.

---

### Stufe 10: (SELECT, COUNT, AS, FROM, JOIN, ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title,
       COUNT(*) AS NumberOfTracks
FROM Album
JOIN Track ON Album.AlbumId = Track.AlbumId
GROUP BY Album.AlbumId
ORDER BY NumberOfTracks DESC
LIMIT 10;
* Zeige mir die Titel und Anzahl Tracks der 10 Alben mit den meisten Tracks.

---

### Stufe 11: (SELECT, AS, FROM, JOIN, ON, GROUP BY, HAVING, COUNT, ORDER BY, DESC)
SELECT Genre.Name AS Genre,
       COUNT(Track.TrackId) AS TrackCount
FROM Track
JOIN Genre ON Track.GenreId = Genre.GenreId
GROUP BY Genre.Name
HAVING COUNT(Track.TrackId) > 50
ORDER BY TrackCount DESC;
* Liste mir alle Genres mit mehr als 50 Tracks, zusammen mit der Anzahl der Tracks, in absteigender Reihenfolge.

---

### Stufe 12: (SELECT, SUM, AS, FROM, JOIN (multiple), GROUP BY, HAVING, ORDER BY, DESC, COUNT)
SELECT Artist.Name AS Artist,
       SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS TotalSales
FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
GROUP BY Artist
HAVING COUNT(InvoiceLine.InvoiceLineId) > 100
ORDER BY TotalSales DESC;
* Zeige mir die Künstler und deren Gesamtumsatz in absteigender Reihenfolge nach dem Gesamtumsatz, aber nur für Künstler mit mehr als 100 verkauften Tracks.

---

### Stufe 13: (SELECT, COUNT, AS, FROM, JOIN (multiple), ON, WHERE, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title,
       COUNT(*) AS NumberOfTracks
FROM PlaylistTrack
JOIN Track ON PlaylistTrack.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
WHERE PlaylistTrack.PlaylistId = 10
GROUP BY Album.AlbumId
ORDER BY NumberOfTracks DESC
LIMIT 1;
* Zeig mir den Titel des Albums, das am meisten Tracks in der Playlist mit der ID 10 hat, sowie die Anzahl Tracks, das dieses Album in der Playlilst mit der ID 10 hat.

---

### Stufe 14: (SELECT, FROM, AVG, SUM, AS, JOIN (multiple), ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Title,
       SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS MoneyEarned,
       COUNT(InvoiceLine.Quantity) AS UnitsSold
FROM Album
JOIN Track ON Album.AlbumId = Track.AlbumId
JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId
GROUP BY Album.AlbumId
ORDER BY MoneyEarned DESC
LIMIT 1;
* Gib mir den Namen des Albums, mit den höchsten Gesamteinnahmen aus Track-Verkäufen, sowie die summierten Einnahmen dieser Tracks, basierend auf den durchschnittlichen Einzelpreisen der Tracks und die Anzahl verkaufter Tracks des Albums.

---

### Stufe 15: (SELECT, FROM, JOIN (multiple), ON, WHERE, EXCEPT):
SELECT Artist.Name
FROM PlaylistTrack
JOIN Track ON PlaylistTrack.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
WHERE PlaylistTrack.PlaylistId = 3
EXCEPT
SELECT Artist.Name
FROM PlaylistTrack
JOIN Track ON PlaylistTrack.TrackId = Track.TrackId
JOIN Album ON Track.AlbumId = Album.AlbumId
JOIN Artist ON Album.ArtistId = Artist.ArtistId
WHERE PlaylistTrack.PlaylistId = 9;
* Gib mir alle Namen der Künstler, die einen Track in der Playlist mit der ID 3 haben, aber nicht in der Playlist mit der ID 9.

---

### Stufe 16: (SELECT, FROM, JOIN, ON, GROUP BY, ORDER BY, MAX, CASE, WHEN, THEN, ELSE, END, AS, BETWEEN, AND, WHERE, DESC)
SELECT Customer.CustomerId,
       Customer.FirstName,
       Customer.LastName,
       CASE
           WHEN MAX(Invoice.Total) > 50 THEN 'High Spender'
           WHEN MAX(Invoice.Total) BETWEEN 20 AND 50 THEN 'Medium Spender'
           ELSE 'Low Spender'
       END AS SpendingCategory
FROM Customer
JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
GROUP BY Customer.CustomerId
ORDER BY SpendingCategory DESC;
* Kategorisiere jeden Kunden als "High Spender", "Medium Spender" oder "Low Spender" basierend auf dem höchsten Rechnungsbetrag, und sortiere die Kunden nach diesen Kategorien: (High Spender > 50, Medium Spender 20-50, Low Spender < 20).

---

### Stufe 17: (WITH, FROM, WHERE, AS, SELECT, JOIN, ON, ROW_NUMBER, OVER, PARTITION BY, GROUP BY, ORDER BY (multiple), SUM, AVG, DESC)
WITH CustomerSpending AS (
    SELECT Customer.Country,
           Customer.FirstName,
           Customer.LastName,
           SUM(Invoice.Total) AS TotalSpent,
           AVG(Invoice.Total) AS AvgSpent,
           ROW_NUMBER() OVER (PARTITION BY Customer.Country ORDER BY SUM(Invoice.Total) DESC) AS SpendingRank
    FROM Customer
    JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
    GROUP BY Customer.CustomerId
)
SELECT Country,
       FirstName,
       LastName,
       TotalSpent,
       AvgSpent
FROM CustomerSpending
WHERE SpendingRank = 1
ORDER BY TotalSpent DESC;
* Zeige mir für jedes Land den Kunden (Vorname, Nachname) mit den höchsten Gesamtausgaben und zeige seine Gesamtausgaben und durchschnittlichen Ausgaben an.

---

### Stufe 18: (SELECT, FROM, SUM, AS, JOIN (multiple), ON, GROUP BY, CASE, WHEN, THEN, ELSE, END):
SELECT Invoice.BillingCountry,
       SUM(CASE WHEN Genre.Name = 'Rock' THEN 1 ELSE 0 END) AS Rock,
       SUM(CASE WHEN Genre.Name = 'Jazz' THEN 1 ELSE 0 END) AS Jazz,
       SUM(CASE WHEN Genre.Name = 'Metal' THEN 1 ELSE 0 END) AS Metal,
       SUM(CASE WHEN Genre.Name = 'Alternative & Punk' THEN 1 ELSE 0 END) AS Alternative_Punk,
       SUM(CASE WHEN Genre.Name = 'Classical' THEN 1 ELSE 0 END) AS Classical,
       SUM(CASE WHEN Genre.Name = 'Pop' THEN 1 ELSE 0 END) AS Pop,
       SUM(CASE WHEN Genre.Name = 'Latin' THEN 1 ELSE 0 END) AS Latin
FROM Invoice
JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
JOIN Track ON InvoiceLine.TrackId = Track.TrackId
JOIN Genre ON Track.GenreId = Genre.GenreId
GROUP BY Invoice.BillingCountry;
* Erstelle eine Tabelle, die die Anzahl der verkauften Tracks nach Genre in jedem Land aufzeigt, mit den Genres: Rock, Jazz, Metal, Alternative & Punk, Classical, Pop und Latin.

---

### Stufe 19: (WITH, AS, SELECT, COUNT(multiple), SUM, RANK, OVER, PARTITION BY, ORDER BY (multiple), DESC (multiple), FROM, JOIN (multiple), ON, GROUP BY, WHERE)
WITH GenrePreferences AS (
    SELECT Customer.CustomerId,
           Genre.Name AS Genre,
           COUNT(InvoiceLine.InvoiceLineId) AS GenreCount,
           SUM(InvoiceLine.UnitPrice * InvoiceLine.Quantity) AS TotalSpent,
           RANK() OVER (PARTITION BY Customer.CustomerId ORDER BY COUNT(InvoiceLine.InvoiceLineId) DESC) AS GenreRank
    FROM Customer
    JOIN Invoice ON Customer.CustomerId = Invoice.CustomerId
    JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId
    JOIN Track ON InvoiceLine.TrackId = Track.TrackId
    JOIN Genre ON Track.GenreId = Genre.GenreId
    GROUP BY Customer.CustomerId, Genre.Name
)
SELECT CustomerId,
       Genre,
       GenreCount,
       TotalSpent
FROM GenrePreferences
WHERE GenreRank = 1
ORDER BY TotalSpent DESC;
* Zeige für jeden Kunden das Genre, das er am häufigsten gekauft hat, zusammen mit der Anzahl und dem Gesamtumsatz dieses Genres, und sortiere das Ergebnis nach dem Gesamtumsatz in absteigender Reihenfolge.

---

IDK????

### Stufe 20: (WITH, CASE, JOIN (multiple), GROUP BY, COUNT, RANK, PARTITION BY, ORDER BY (multiple), WHERE)
WITH SeasonalPurchases AS (
    SELECT 
        c.CustomerId,
        g.Name AS Genre,
        CASE 
            WHEN MONTH(i.InvoiceDate) IN (3, 4, 5) THEN 'Spring'
            WHEN MONTH(i.InvoiceDate) IN (6, 7, 8) THEN 'Summer'
            WHEN MONTH(i.InvoiceDate) IN (9, 10, 11) THEN 'Fall'
            ELSE 'Winter'
        END AS Season,
        COUNT(il.TrackId) AS GenreCount
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY c.CustomerId, g.Name, 
             CASE 
                 WHEN MONTH(i.InvoiceDate) IN (3, 4, 5) THEN 'Spring'
                 WHEN MONTH(i.InvoiceDate) IN (6, 7, 8) THEN 'Summer'
                 WHEN MONTH(i.InvoiceDate) IN (9, 10, 11) THEN 'Fall'
                 ELSE 'Winter'
             END
),
RankedGenres AS (
    SELECT 
        CustomerId,
        Season,
        Genre,
        GenreCount,
        RANK() OVER (PARTITION BY CustomerId, Season ORDER BY GenreCount DESC) AS GenreRank
    FROM SeasonalPurchases
)
SELECT 
    CustomerId,
    Season,
    Genre,
    GenreCount
FROM RankedGenres
WHERE GenreRank = 1
ORDER BY CustomerId, Season;
* Zeige mir für jeden Kunden und jede Jahreszeit das Genre, das er am häufigsten gekauft hat.

---
