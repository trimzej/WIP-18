# KI-gestützte Text-to-SQL-Abfragen mit LangChain

Dieses Repository enthält die Implementierung und Evaluierung eines KI-gestützten SQL-Agenten, der SQL-Abfragen aus natürlicher Sprache generiert. Der Fokus des Projekts liegt auf der Untersuchung der Leistungsfähigkeit von Large Language Models (LLMs) wie GPT-4 in der Datenbankabfrage. 

## Projektübersicht

Das Ziel des Projekts ist es, die Reife und Effizienz von KI-gestützten Datenbankabfragen zu bewerten und die wichtigsten Einflussfaktoren auf die Leistungsfähigkeit dieser Systeme zu identifizieren. 

### Verwendete Datenbanken:
- **Chinook-Datenbank**: Eine etablierte SQL-Datenbank, die häufig in Tutorials verwendet wird.
- **CarDealer-Datenbank**: Eine herangezogene Datenbank, die zur Analyse der Generalisierungsfähigkeit und möglicher Modell-Biases dient.

### Features:
- **Text-to-SQL-Funktionalität**: Natürliche Spracheingaben werden in SQL-Abfragen umgewandelt.
- **Unterstützung mehrerer Prompting-Techniken**:
  - Zero-Shot
  - Few-Shot
  - Contextual Few-Shot
- **Dynamic Few-Shot Prompts**: Kontextbasierte Auswahl relevanter Abfragebeispiele zur Verbesserung der Genauigkeit.
- **ER-Diagramme**: Visualisierung der Datenbankstrukturen.
- **SQL-Abfragen**: Sammlung getesteter und dokumentierter Abfragen für Chinook und CarDealer.

### Ergebnisse:
- **Chinook-Datenbank**: 84 % korrekte SQL-Abfragen bei der Generierung durch das LLM.
- **CarDealer-Datenbank**: Generalisierungsfähigkeit mit einer Genauigkeit von 77 %.
- Erkenntnisse zur Optimierung der Prompt-Techniken und zur Minimierung von Modell-Biases.

## 🗂 Verzeichnisstruktur

```plaintext
├── Datenbanken
│   ├── CarDealer.db                 # SQLite-Datenbank für CarDealer
│   ├── Chinook.db                   # SQLite-Datenbank für Chinook
│   ├── Chinook_Sqlite.sql           # SQL-Skript der Chinook-Datenbank
│   ├── CarDealer.xlsx               # Ursprungsdaten der CarDealer-Datenbank
│   └── car_dealer_data.db           # Alternativer Datenbankexport
├── Dokumentation
│   ├── ER-Diagram                   # ER-Diagramme der Datenbanken
│   │   ├── Car Dealer ER.jpg
│   │   ├── Chinook ER.png
│   │   └── Chinook ER (erweitert).jpg
│   ├── SQL-Abfragen
│   │   ├── SQL-Abfragen_Car_Dealer_Data.md
│   │   ├── SQL-Abfragen_Chinook.md
├── Dynamic few-shot-prompts
│   ├── few-shot-prompts.json        # Few-Shot-Beispiele
│   ├── template_prefix.txt          # Prompt-Template
├── SQL_Agents.ipynb                 # Notebook zur SQL-Agent-Ausführung
├── .gitignore                       # Ignorierte Dateien für Git
├── README.md                        # Dieses README
```

## ER-Diagramme

Die Struktur der verwendeten Datenbanken wird durch die folgenden Entity-Relationship-Diagramme visualisiert:

- **CarDealer-Datenbank**:
  ![Car Dealer ER](Dokumentation/ER-Diagram/Car%20Dealer%20ER.jpg)

- **Chinook-Datenbank (Standard)**:
  ![Chinook ER](Dokumentation/ER-Diagram/Chinook%20ER.png)

- **Chinook-Datenbank (erweitert)**:
  ![Chinook ER (erweitert)](Dokumentation/ER-Diagram/Chinook%20ER%20(erweitert).jpg)

## 🕇 Autoren

- **Pascal Kern**
- **Leandros Giagiozis**
- **Trim Zejnullahu**

