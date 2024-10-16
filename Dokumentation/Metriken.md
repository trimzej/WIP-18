# Metriken

## Fokus
Im Folgenden werden einige Metriken definiert, die im Rahmen dieser Forschung verwendet werden, um sowohl die Genauigkeit als auch die Effizienz von KI-basierten Datenbankabfragen zu bewerten.

Diese Auflistung ist nicht endgültig und kann im Verlauf der Forschung erweitert oder angepasst werden.


## Genauigkeit:

### Accuracy:
Die Accuracy (Genauigkeit) misst den Prozentsatz der korrekt generierten Abfragen. Eine Abfrage gilt dabei als entweder vollständig richtig oder falsch.

### Precision:
Die Precision (Präzision) misst den Anteil korrekter SQL-Elemente innerhalb einer Abfrage (z.B. JOIN, WHERE, etc.). Die Precision berechnet sich wie folgt:

(Anzahl korrekter Elemente in der generierter Abfrage) / (Gesamtzahl der Elemente in der generierten Abfrage)

### Recall:
Der Recall (Trefferquote) misst den Anteil der durch die KI korrekt identifizierten Elemente einer Abfrage. Der Recall berechnet sich wie folgt:

(Anzahl korrekter identifizierter Elemente) / (Gesamtzahl der relevanten Elemente in der Referenzabfrage)

### F1-Score:
Der F1-Score ist das harmonische Mittel von Precision und Recall und wird verwendet, um das Gleichgewicht zwischen Precision und Recall zu bewerten. Der F1-Score wird wie folgt berechnet:

2* (Precision * Recall) / (Precision + Recall)


## Effizienz

* Durchschnittliche Antwortzeit: Zeit von Eingabe der Frage in natürlicher Sprache bis zur Rückgabe der SQL-Abfrage. 

* Ressourcennutzung: CPU- und Speichernutzung.

* Durchsatz: Anzahl der Abfragen pro Zeiteinheit.