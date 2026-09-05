# FIFA World Cup Intelligence: Pressure Dynamics, Squad Demographics & Historical Match Analytics (1930–2026)
#systemyaokothznation

An end-to-end Business Intelligence project built with **Microsoft Power BI**, **Power Query**, and **DAX**. This project analyzes historical World Cup matches (1,058 fixtures), squad demographic profiles across the expanded 48-nation 2026 format (1,248 players), and high-stakes penalty shootouts (352 kicks).

---

## 📌 Project Overview
1. **Psychology Under Pressure**: Quantifies the drop in penalty conversion rates when shooters face elimination (`Is_Must_Score = "Yes"`) versus standard or match-winning kicks.
2. **2026 Tournament Composition**: Analyzes squad age profiles, tactical positional distributions, and club representation across all 48 qualified countries.
3. **Scoring Velocity Evolution**: Tracks how goal frequency and knockout resolution mechanisms evolved from 1930 to 2026.

---

## 🏛 Data Architecture & Star Schema
The data model uses **`DimCountry`** as a central geopolitical dimension hub, connecting normalized fact tables:
* **`FactMatches`**: 1,058 historical matches (1930–2026).
* **`FactShootouts`**: 352 penalty kicks under pressure.
* **`FactSquads2026`**: 1,248 rostered players.
* **`FactTeamSummary`**: 48 national team summaries.

---

## 🧹 Power Query & Data Cleaning Highlights
* **Deduplication**: Pruned 16 verbatim duplicate records from the 2014 knockout stages while preserving legitimate historical replays (1934, 1938, 1954).
* **HTML Artifact Cleanup**: Removed web-scraping artifacts (`rn">`) from country and team names.
* **Type Conversion**: Resolved decimal text strings (`"1930.0"`, `"4.0"`) into whole numbers to prevent query errors.

---

## 📊 Dashboard View Architecture
* **Page 1: Penalty Pressure Lab**: Investigates the penalty "choke effect," showing conversion rates drop from a **69.0%** baseline down to **17.6%** on elimination kicks.
* **Page 2: 2026 Squad Demographics**: Analyzes team maturity (tournament mean age of **27.4 years**) and top club talent suppliers (Manchester City, Bayern Munich, PSG).
* **Page 3: Historical Match Dynamics**: Tracks scoring velocity peaks (5.38 goals/match in 1954) down to modern tactical stabilization (~2.6 goals/match).

---

## 🚀 Repository Contents
* `docs/DATA_DICTIONARY.md`: Full schema and column definitions.
* `powerbi/`: Report files and data models.
