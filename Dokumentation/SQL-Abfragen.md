# SQL-Abfragen

## Fokus
In diesem Dokument werden die Datenbankabfragen vorgestellt, die im Rahmen dieser Arbeit verwendet werden, um die Leistungsfähigkeit von auf NLP basierter KI zu prüfen. Die Abfragen sind so strukturiert, dass sie eine stufenweise Steigerung der Komplexität aufweisen.

Die Komplexität der einzelnen Stufen wird durch die Anzahl der Operationen in jeder Abfrage definiert. Mit jeder weiteren Stufe werden zusätzliche Operationen (wie COUNT, JOIN etc.) eingeführt, wobei die Operationen der vorherigen Stufen weitestgehend beibehalten werden. Die Operationen sind nach der vermuteten Komplexität für den KI-Algorithmus geordnet.

Dieser systematische Ansatz soll Einblicke in die Stärken und Schwächen von KI-Algorithmen ermöglichen. Die Auflistung der Abfragen ist nicht endgültig und kann im Verlauf der Forschung erweitert oder angepasst werden.

## Abfragen
### Stufe 1: (SELECT, FROM, WHERE):
SELECT * FROM Customer WHERE City = "Prague";
* Zeige mir alle Kunden aus Prague.

### Stufe 2: (SELECT, DISTINCT, FROM)
SELECT DISTINCT BillingCountry FROM Invoice;
* Liste mir alle einzigartigen Rechnungsstellungs-Länder auf.

### Stufe 3: (SELECT, FROM, WHERE, AND)
SELECT * FROM Invoice WHERE BillingCountry = "Brazil" AND Total > 6;
* Liste mir alle Rechnungen aus Brasilien mit einem Betrag von mehr als 6 auf.

### Stufe 4: (SELECT, FROM, ODER BY, DESC, LIMIT):
SELECT Name FROM Track ORDER BY Milliseconds DESC LIMIT 5;
* Gib mir die Namen der 5 Tracks mit der längsten Dauer.

### Stufe 5: (SELECT, SUM, AS, FROM, GROUP BY, ORDER BY, DESC)
SELECT BillingCountry, SUM(Total) AS "TotalSales" FROM invoice GROUP BY BillingCountry ORDER BY TotalSales DESC;
* Zeige mir den Gesamtumsatz pro Land in absteigender Reihenfolge.
 
### Stufe 6: (SELECT, COUNT, AS, FROM, GROUP BY, ODER BY, DESC):
SELECT ArtistId, COUNT(*) AS numberOfAlbums FROM Album GROUP BY ArtistId ORDER BY numberOfAlbums DESC;
* Zeige mir die ID aller Künstler und die Anzahl ihrer Alben in absteigender Reihenfolge.

### Stufe 7: (SELECT, AS, FROM, JOIN, ON, ORDER BY)
SELECT InvoiceLine.*, Track.Name AS "Track" FROM InvoiceLine JOIN Track ON InvoiceLine.TrackId = Track.TrackId ORDER BY Track.Name;
* Liste mir alle Rechnungsposten auf, einschliesslich der zugehörigen Namen der Tracks sortiert nach Track-Namen.

### Stufe 8: (SELECT, COUNT, AS, FROM, JOIN, ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title, COUNT(*) AS numberOfTracks FROM Track JOIN Album ON Track.AlbumId = Album.AlbumId GROUP BY Title ORDER BY numberOfTracks DESC LIMIT 10;
* Zeige mir die Titel und Anzahl Tracks der 10 Alben mit den meisten Tracks.

### Stufe 9: (SELECT, COUNT, AS, FROM, JOIN (multiple), ON, WHERE, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Album.Title, COUNT(*) AS numberOfTracks FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId WHERE PlaylistTrack.PlaylistId = 10 GROUP BY Album.Title ORDER BY numberOfTracks DESC LIMIT 1;
* Zeig mir den Titel und die Anzahl Tracks des Albums, das am meisten Tracks in der Playlist 10 hat.

### Stufe 10: (SELECT, COUNT, AVG, AS, FROM, JOIN, ON, GROUP BY):
SELECT AVG(numberOfTracks) AS averageNumberOfTracks FROM (SELECT Album.AlbumId, COUNT(*) AS numberOfTracks FROM Track JOIN Album ON Track.AlbumId = Album.AlbumId GROUP BY Album.AlbumId) AS numberOfTracks;
* Gib mir die durchschnittliche Anzahl Tracks in einem Album.

 
### Stufe 11: (SELECT, FROM, JOIN (multiple), ON, WHERE, EXCEPT):
SELECT Artist.Name FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId JOIN Artist ON Album.ArtistId = Artist.ArtistId WHERE PlaylistTrack.PlaylistId = 3 EXCEPT SELECT Artist.Name FROM PlaylistTrack JOIN Track ON PlaylistTrack.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId JOIN Artist ON Album.ArtistId = Artist.ArtistId WHERE PlaylistTrack.PlaylistId = 9;
* Gib mir alle Namen der Künstler die einen Track in Playlist 3 haben, aber nicht in Playlist 9.

### Stufe 12: (SELECT, FROM, AVG, SUM, AS, JOIN (multiple), ON, GROUP BY, ORDER BY, DESC, LIMIT):
SELECT Title, trackSales AS moneyEarned, trackSales / (SELECT AVG(InvoiceLine.UnitPrice) FROM InvoiceLine JOIN Track ON InvoiceLine.TrackId = Track.TrackId WHERE Track.AlbumId = maxSales.AlbumId) AS unitsSold FROM (SELECT Album.AlbumId, Album.Title, SUM(InvoiceLine.UnitPrice) AS trackSales FROM InvoiceLine JOIN Track ON InvoiceLine.TrackId = Track.TrackId JOIN Album ON Track.AlbumId = Album.AlbumId GROUP BY Album.AlbumId ORDER BY trackSales DESC LIMIT 1) AS maxSales;
* Gib mir den Namen des Albums, mit den höchsten Gesamteinnahmen aus Track-Verkäufen, sowie die summierten Einnahmen dieser Tracks und die Anzahl verkaufter Tracks des Albums, basierend auf dem durchschnittlichen Einzelpreis der Tracks.

### Stufe 13: (SELECT, FROM, SUM, AS, JOIN (multiple), ON, GROUP BY, CASE, WHEN, THEN, ELSE, END):
SELECT Invoice.BillingCountry, SUM(CASE WHEN Genre.Name = "Rock" THEN 1 ELSE 0 END) AS Rock, SUM(CASE WHEN Genre.Name = "Jazz" THEN 1 ELSE 0 END) AS Jazz, SUM(CASE WHEN Genre.Name = "Metal" THEN 1 ELSE 0 END) AS Metal, SUM(CASE WHEN Genre.Name = "Alternative & Punk" THEN 1 ELSE 0 END) AS Alternative_Punk, SUM(CASE WHEN Genre.Name = "Classical" THEN 1 ELSE 0 END) AS Classical, SUM(CASE WHEN Genre.Name = "Pop" THEN 1 ELSE 0 END) AS Pop, SUM(CASE WHEN Genre.Name = "Latin" THEN 1 ELSE 0 END) AS Latin FROM Invoice JOIN InvoiceLine ON Invoice.InvoiceId = InvoiceLine.InvoiceId JOIN Track ON InvoiceLine.TrackId = Track.TrackId JOIN Genre ON Track.GenreId = Genre.GenreId GROUP BY Invoice.BillingCountry;
* Erstelle eine Tabelle, die die Anzahl der Verkauften Tracks nach Genre in jedem Land aufzeigt.
