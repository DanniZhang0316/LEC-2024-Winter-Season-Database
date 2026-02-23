# LEC 2024 Winter Season Database System  
League of Legends EMEA Championship (LEC)

## Overview

This project designs and implements a fully normalized relational database for the **LEC 2024 Winter Season**.  

The goal of the system is to centralize structured information about teams, players, matches, match statistics, and tournament formats, enabling both fans and team managers to analyze performance and strategic patterns.

The project covers conceptual modeling, relational schema design, normalization (3NF), and advanced SQL implementation.

>  **Note:** The full analytical report is in **Estonian**.
---

## Problem Statement

Information about the LEC winter season is often fragmented across multiple websites and sources.  

This database provides a structured and centralized system where users can:
- Analyze team performance
- Evaluate player statistics (K/D/A)
- Identify most-used champions and win rates
- Compare players by role
- Model tournament structure rules

---

## System Design

### Conceptual Model (ER Model)

The system includes the following core entities:

- Players  
- Teams  
- Matches  
- Match Details  
- Tournament Format  
- Roles  

Business rules such as:
- Each team has exactly five active players (one per role)
- A match follows a specific format (Bo1, Bo3, Bo5)
- A team plays only one match per day
- Players specialize in one role

were formally defined and enforced in the schema.

---

## Relational Schema & Normalization

The database was transformed into **Third Normal Form (3NF)** by:

- Analyzing functional dependencies  
- Eliminating partial and transitive dependencies  
- Introducing surrogate keys where necessary  
- Enforcing referential integrity  

Tables include:

- `mangijad` (Players)  
- `meeskonnad` (Teams)  
- `matsid` (Matches)  
- `matsiinfo` (Match Details)  
- `formaat` (Tournament Format)  

---

## Implementation (PostgreSQL)

The project includes:

### Constraints
- Primary and foreign keys  
- CHECK constraints (e.g., match format limits, date validation)  
- Unique constraints  
- Cascading updates/deletes  

### Analytical View
- `v_meeskonnavoite`  
  Displays total wins per team during the winter season.

### Stored Functions

- `f_kangelasevoidumaar(player_id)`  
  Returns top 3 most-used champions per player and their win rates.

- `f_top3(role)`  
  Returns top 3 performing players per role (role-specific logic).

- `f_voitepanustajad(team_id)`  
  Identifies which player contributed most to team victories.

### Stored Procedures

- `sp_uus_meeskond_mangija`  
  Adds a new team and player with historical tracking of team changes.

- `sp_mangijavordlus`  
  Compares two players’ contribution scores in the same role.

---

## Business Logic Integration

Tournament structure modeled:

- Stage 1: Round Robin (10 teams)
- Stage 2: Double Elimination Playoffs (Top 8 teams)
- Match formats:
  - Best of 1
  - Best of 3
  - Best of 5

Logical constraints ensure score consistency with match format rules.

---

## Example Use Cases

- Find the most successful team of the season  
- Identify top-performing ADC players  
- Analyze which champions yield the highest win rate  
- Compare two mid-laners' performance contribution  
