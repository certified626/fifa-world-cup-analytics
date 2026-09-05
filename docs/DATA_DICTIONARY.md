# 📖 Data Dictionary — FIFA World Cup Analytics

This document details the schema architecture, column definitions, data types, and primary/foreign keys across the 5 tables in the data model.

---

## 1. `DimCountry` (Central Dimension Hub)
* **Grain:** One record per qualified national football association.
* **Row Count:** 48 rows

| Column Name | Data Type | Key Type | Nullable | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Country` | Text | Primary Key (`PK`) | No | Full national country name | `Brazil` |
| `ISO_3_Code` | Text | Alternate Key | No | Standard Alpha-3 geopolitical code used for GIS map visual rendering | `BRA` |

---

## 2. `FactMatches` (Match History Fact)
* **Grain:** One record per tournament fixture (1930–2026).
* **Row Count:** 1,058 rows (after deduplication)

| Column Name | Data Type | Key Type | Nullable | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Year` | Whole Number | Dimension FK | No | Four-digit tournament year | `2014` |
| `Datetime` | Text | Attribute | Yes | Date and kickoff time (null for 2026 simulated fixtures) | `28 Jun 2014 - 13:00` |
| `Stage` | Text | Attribute | No | Tournament phase | `Round of 16` |
| `Stadium` | Text | Attribute | No | Host venue stadium name | `Estadio do Maracana` |
| `City` | Text | Attribute | No | Host metropolitan city | `Rio De Janeiro` |
| `Home Team Name` | Text | Attribute | No | Designated home squad (cleaned of artifacts) | `Brazil` |
| `Home Team Goals`| Whole Number | Measure | No | Goals scored by home squad | `1` |
| `Away Team Goals`| Whole Number | Measure | No | Goals scored by away squad | `1` |
| `Away Team Name` | Text | Attribute | No | Designated away squad (cleaned of artifacts) | `Chile` |
| `Win conditions` | Text | Attribute | Yes | Overtime or penalty shootout details | `Brazil win on penalties (3 - 2)` |
| `DecidedBy` | Text | Attribute | No | Resolution mode: `Regular Time`, `Extra Time`, `Shootout` | `Shootout` |
| `TotalGoals` | Whole Number | Measure | No | Calculated: `[Home Team Goals] + [Away Team Goals]` | `2` |
| `GoalDifference`| Whole Number | Measure | No | Absolute score gap: `\|[Home Team Goals] - [Away Team Goals]\|` | `0` |
| `MatchOutcome` | Text | Attribute | No | Regulation result: `Home Win`, `Away Win`, `Draw` | `Draw` |

---

## 3. `FactShootouts` (Penalty Kick Fact)
* **Grain:** One record per penalty kick taken in a World Cup shootout (1982–2026).
* **Row Count:** 352 rows

| Column Name | Data Type | Key Type | Nullable | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Tournament_Year`| Whole Number | Dimension FK | No | Tournament edition year | `1982` |
| `Match_Stage` | Text | Attribute | No | Stage where the shootout occurred | `Semi-finals` |
| `Team_A` | Text | Attribute | No | First-shooting nation | `West Germany` |
| `Team_B` | Text | Attribute | No | Second-shooting nation | `France` |
| `Shooter_Team` | Text | Foreign Key (`FK`) | No | Kicker's national squad | `France` |
| `Shooter_Name` | Text | Attribute | No | Full name of penalty taker | `Alain Giresse` |
| `Goalkeeper_Name`| Text | Attribute | No | Full name of opposing goalkeeper | `Harald Schumacher` |
| `Kick_Number` | Whole Number | Attribute | No | Sequence of kick in shootout (1 to 12) | `1` |
| `Team_Kick_Number`| Whole Number| Attribute | No | Sequence of kick for that team (1 to 6) | `1` |
| `Score_Before_Kick`| Text | Attribute | No | Shootout tally prior to attempt | `0-0` |
| `Is_Must_Score` | Text (`Yes`/`No`)| Attribute | No | Sudden-death elimination flag | `No` |
| `Is_Winning_Kick`| Text (`Yes`/`No`)| Attribute | No | Championship / match-clinching kick flag | `No` |
| `Shoot_Outcome` | Text | Attribute | No | Cleaned outcome: `Goal`, `Saved`, `Missed` | `Goal` |
| `IsGoal` | Whole Number | Indicator (0/1) | No | Binary flag: `1` if Goal, `0` otherwise | `1` |

---

## 4. `FactSquads2026` (Roster Fact)
* **Grain:** One record per registered player for the 2026 tournament.
* **Row Count:** 1,248 rows (48 squads $\times$ 26 players)

| Column Name | Data Type | Key Type | Nullable | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `name` | Text | Attribute | No | Player full name | `Matěj Kovář` |
| `age` | Whole Number | Measure | No | Player age at tournament start | `26` |
| `country` | Text | Foreign Key (`FK`) | No | Represented nation (links to `DimCountry[Country]`) | `Czechia` |
| `position` | Text | Attribute | No | Granular pitch role (`GK`, `CB`, `CM`, `ST`, etc.) | `GK` |
| `club` | Text | Attribute | No | Employer club team | `PSV Eindhoven` |
| `jersey_number` | Whole Number | Attribute | No | Squad kit number | `1` |
| `Role` | Text | Attribute | No | Macro tactical role: `Goalkeeper`, `Defender`, `Midfielder`, `Forward` | `Goalkeeper` |

---

## 5. `FactTeamSummary` (Team Summary Fact)
* **Grain:** One record per qualified national team.
* **Row Count:** 48 rows

| Column Name | Data Type | Key Type | Nullable | Description | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `Country` | Text | Foreign Key (`FK`) | No | Country name (links to `DimCountry[Country]`) | `Brazil` |
| `2026_Average_Age`| Decimal Number | Measure | No | Mean player age across the squad | `28.8` |
| `2026_Unique_Clubs_Represented` | Whole Number | Measure | No | Count of distinct clubs across squad members | `20` |
| `Historical_Matches_Played` | Whole Number | Measure | No | All-time World Cup fixtures played (1930–2026) | `123` |
| `Historical_Goals_Scored` | Whole Number | Measure | No | All-time tournament goals scored (1930–2026) | `251` |
