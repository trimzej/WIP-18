# SQL-Abfragen

## Fokus
In diesem Dokument werden die Datenbankabfragen vorgestellt, die im Rahmen dieser Arbeit verwendet werden, um die Leistungsfähigkeit von auf NLP basierter KI zu prüfen. Die Abfragen sind so strukturiert, dass sie eine stufenweise Steigerung der Komplexität aufweisen.

Die Komplexität der einzelnen Stufen wird durch die Anzahl der Operationen in jeder Abfrage definiert. Mit jeder weiteren Stufe werden zusätzliche Operationen (wie COUNT, JOIN etc.) eingeführt, wobei die Operationen der vorherigen Stufen weitestgehend beibehalten werden. Die Operationen sind nach der vermuteten Komplexität für den KI-Algorithmus geordnet.

Dieser systematische Ansatz soll Einblicke in die Stärken und Schwächen von KI-Algorithmen ermöglichen. Die Auflistung der Abfragen ist nicht endgültig und kann im Verlauf der Forschung erweitert oder angepasst werden.

## Abfragen
### Stufe 1: (SELECT, FROM, WHERE):
SELECT * FROM Customer WHERE City = 'Prague';
* Zeige mir die Daten aller Kunden aus Prague.

---

### Stufe 2: (SELECT, DISTINCT, FROM)
SELECT DISTINCT BillingCountry FROM Invoice;
* Liste mir alle einzigartigen Rechnungsstellungs-Länder auf.

---

### Stufe 3: (SELECT, FROM, WHERE, AND)
SELECT * FROM Invoice WHERE BillingCountry = 'Brazil' AND Total > 6;
* Liste mir alle Rechnungen aus Brasilien mit einem Betrag von mehr als 6 auf.

---

### Stufe 4: (SELECT, FROM, ODER BY, DESC, LIMIT):
SELECT Name FROM Track ORDER BY Milliseconds DESC LIMIT 5;
* Gib mir die Namen der 5 Tracks mit der längsten Dauer.

---

### Stufe 5: (SELECT, SUM, AS, FROM, GROUP BY, ORDER BY, DESC)
SELECT BillingCountry, SUM(Total) AS TotalSales FROM Invoice GROUP BY BillingCountry ORDER BY TotalSales DESC;
* Zeige mir den Gesamtumsatz pro Land in absteigender Reihenfolge.

---

### Stufe 6: (SELECT, COUNT, AVG, AS, FROM, GROUP BY):
SELECT AVG(numberOfTracks) AS averageNumberOfTracks FROM (SELECT COUNT(*) AS numberOfTracks FROM Track GROUP BY AlbumId) AS numberOfTracks;
* Gib mir die durchschnittliche Anzahl Tracks in einem Album.

--- 

### Stufe 7: (SELECT, COUNT, AS, FROM, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT ArtistId, COUNT(*) AS numberOfAlbums FROM Album GROUP BY ArtistId ORDER BY numberOfAlbums DESC LIMIT 10;
* Zeige mir die ID und die Anzahl Alben der 10 Künstler mit den meisten Alben, in absteigender Reihenfolge.

---

### Stufe 8: (SELECT, AS, FROM, JOIN, ON, ORDER BY, WHERE)
SELECT InvoiceLine.*, Track.Name AS Track FROM InvoiceLine JOIN Track ON InvoiceLine.TrackId = Track.TrackId WHERE InvoiceLineId < 21 ORDER BY Track;
* Liste mir die Rechnungsposten mit ID 1-20 auf, einschliesslich der zugehörigen Namen der Tracks und sortiere sie in alphabetischer Reihenfolge basierend auf dem Track Namen.

---

### Stufe 9: (SELECT, COUNT, AS, FROM, JOIN, ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title, COUNT(*) AS numberOfTracks FROM Track JOIN Album ON Track.AlbumId = Album.AlbumId GROUP BY Title ORDER BY numberOfTracks DESC LIMIT 10;
* Zeige mir die Titel und Anzahl Tracks der 10 Alben mit den meisten Tracks.

---

### Stufe 10: (SELECT, COUNT, AS, FROM, JOIN (multiple), ON, WHERE, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title, COUNT(*) AS numberOfTracks FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId WHERE PlaylistTrack.PlaylistId = 10 GROUP BY Title ORDER BY numberOfTracks DESC LIMIT 1;
* Zeig mir den Titel und die Anzahl Tracks des Albums, das am meisten Tracks in der Playlist mit der ID 10 hat.

---

### Stufe 11: (SELECT, FROM, AVG, SUM, AS, JOIN (multiple), ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Title, SUM(InvoiceLine.UnitPrice) AS moneyEarned, COUNT(*) AS unitsSold FROM Album JOIN Track ON Album.AlbumId = Track.AlbumId JOIN InvoiceLine ON Track.TrackId = InvoiceLine.TrackId GROUP BY Album.AlbumId ORDER BY moneyEarned DESC LIMIT 1;
* Gib mir den Namen des Albums, mit den höchsten Gesamteinnahmen aus Track-Verkäufen, sowie die summierten Einnahmen dieser Tracks, basierend auf den durchschnittlichen Einzelpreisen der Tracks und die Anzahl verkaufter Tracks des Albums.

---

### Stufe 12: (SELECT, FROM, JOIN (multiple), ON, WHERE, EXCEPT):
SELECT Artist.Name FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId JOIN Artist ON Album.ArtistId = Artist.ArtistId WHERE PlaylistTrack.PlaylistId = 3 EXCEPT SELECT Artist.Name FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId JOIN Artist ON Album.ArtistId = Artist.ArtistId WHERE PlaylistTrack.PlaylistId = 9;
* Gib mir alle Namen der Künstler, die einen Track in der Playlist mit der ID 3 haben, aber nicht in der Playlist mit der ID 9.

---

### Stufe 13: (SELECT, FROM, SUM, AS, JOIN (multiple), ON, GROUP BY, CASE, WHEN, THEN, ELSE, END):
SELECT Invoice.BillingCountry, SUM(CASE WHEN Genre.Name = 'Rock' THEN 1 ELSE 0 END) AS Rock, SUM(CASE WHEN Genre.Name = 'Jazz' THEN 1 ELSE 0 END) AS Jazz, SUM(CASE WHEN Genre.Name = 'Metal' THEN 1 ELSE 0 END) AS Metal, SUM(CASE WHEN Genre.Name = 'Alternative & Punk' THEN 1 ELSE 0 END) AS Alternative_Punk, SUM(CASE WHEN Genre.Name = 'Classical' THEN 1 ELSE 0 END) AS Classical, SUM(CASE WHEN Genre.Name = 'Pop' THEN 1 ELSE 0 END) AS Pop, SUM(CASE WHEN Genre.Name = 'Latin' THEN 1 ELSE 0 END) AS Latin FROM Invoice JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId JOIN Track ON InvoiceLine.TrackId = Track.TrackId JOIN Genre ON Track.GenreId = Genre.GenreId GROUP BY Invoice.BillingCountry;
* Erstelle eine Tabelle, die die Anzahl der Verkauften Tracks nach Genre in jedem Land aufzeigt, mit den Genres: Rock, Jazz, Metal, Alternative & Punk, Classical, Pop und Latin.

---

### Stufe 14: (WITH, CASE, JOIN (multiple), GROUP BY, COUNT, RANK, PARTITION BY, ORDER BY (multiple), WHERE)
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

### Stufe 15: (SELECT, FROM, WHERE, EXISTS, AND)
SELECT FirstName, LastName
FROM Customer c
WHERE EXISTS (
    SELECT 1 
    FROM Invoice i 
    WHERE i.CustomerId = c.CustomerId AND i.Total > 20
) AND Country = 'USA';
* Zeige mir die Vornamen und Nachnamen der Kunden aus den USA, die mindestens eine Rechnung mit einem Betrag von über 20 haben.

---

### Stufe 16: (SELECT, FROM, JOIN, GROUP BY, HAVING, COUNT, ORDER BY, DESC)
SELECT g.Name AS Genre, COUNT(t.TrackId) AS TrackCount
FROM Track t
JOIN Genre g ON t.GenreId = g.GenreId
GROUP BY g.Name
HAVING COUNT(t.TrackId) > 50
ORDER BY TrackCount DESC;
* Liste mir alle Genres mit mehr als 50 Tracks, zusammen mit der Anzahl der Tracks, in absteigender Reihenfolge.

---

### Stufe 17: (SELECT, SUM, AS, JOIN, GROUP BY, HAVING, ORDER BY, DESC, COUNT, ROUND)
SELECT a.Name AS Artist, ROUND(SUM(il.UnitPrice * il.Quantity), 2) AS TotalSales
FROM InvoiceLine il
JOIN Track t ON il.TrackId = t.TrackId
JOIN Album al ON t.AlbumId = al.AlbumId
JOIN Artist a ON al.ArtistId = a.ArtistId
GROUP BY a.Name
HAVING COUNT(il.InvoiceLineId) > 100
ORDER BY TotalSales DESC;
* Zeige mir die Künstler und deren Gesamtumsatz, aber nur für Künstler mit mehr als 100 verkauften Tracks, in absteigender Reihenfolge.

---

### Stufe 18: (SELECT, FROM, JOIN (multiple), GROUP BY, COUNT, MAX, CASE, WHERE, ORDER BY)
SELECT c.CustomerId, c.FirstName, c.LastName,
       CASE 
           WHEN MAX(i.Total) > 50 THEN 'High Spender'
           WHEN MAX(i.Total) BETWEEN 20 AND 50 THEN 'Medium Spender'
           ELSE 'Low Spender'
       END AS SpendingCategory
FROM Customer c
JOIN Invoice i ON c.CustomerId = i.CustomerId
GROUP BY c.CustomerId, c.FirstName, c.LastName
ORDER BY SpendingCategory DESC;
* Kategorisiere jeden Kunden als 'High Spender' (>50), 'Medium Spender (20-50)' oder 'Low Spender' (<20) basierend auf dem höchsten Rechnungsbetrag, und gib alle Kunden mit den entsprechenden kategorien aus.

---

### Stufe 19: (WITH, SELECT, JOIN (multiple), ROW_NUMBER, PARTITION BY, ORDER BY, SUM, AVG)
WITH CustomerSpending AS (
    SELECT 
        c.CustomerId,
        c.FirstName,
        c.LastName,
        SUM(i.Total) AS TotalSpent,
        AVG(i.Total) AS AvgSpent,
        ROW_NUMBER() OVER (PARTITION BY c.Country ORDER BY SUM(i.Total) DESC) AS SpendingRank
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    GROUP BY c.CustomerId, c.FirstName, c.LastName, c.Country
)
SELECT CustomerId, FirstName, LastName, TotalSpent, AvgSpent
FROM CustomerSpending
WHERE SpendingRank = 1
ORDER BY TotalSpent DESC;
* Zeige mir für jedes Land den Kunden mit den höchsten Gesamtausgaben und zeige seine Gesamtausgaben und durchschnittlichen Ausgaben an.

---

### Stufe 20: (WITH, JOIN (multiple), CASE, COUNT, SUM, RANK, PARTITION BY, WHERE, ORDER BY)
WITH GenrePreferences AS (
    SELECT 
        c.CustomerId,
        g.Name AS Genre,
        COUNT(il.InvoiceLineId) AS GenreCount,
        SUM(il.UnitPrice * il.Quantity) AS TotalSpent,
        RANK() OVER (PARTITION BY c.CustomerId ORDER BY COUNT(il.InvoiceLineId) DESC) AS GenreRank
    FROM Customer c
    JOIN Invoice i ON c.CustomerId = i.CustomerId
    JOIN InvoiceLine il ON i.InvoiceId = il.InvoiceId
    JOIN Track t ON il.TrackId = t.TrackId
    JOIN Genre g ON t.GenreId = g.GenreId
    GROUP BY c.CustomerId, g.Name
)
SELECT CustomerId, Genre, GenreCount, TotalSpent
FROM GenrePreferences
WHERE GenreRank = 1
ORDER BY TotalSpent DESC;
* Zeige für jeden Kunden das Genre, das er am häufigsten gekauft hat, zusammen mit der Anzahl und dem Gesamtumsatz dieses Genres, und sortiere das Ergebnis nach dem Gesamtumsatz in absteigender Reihenfolge.