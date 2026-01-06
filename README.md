# Záverečný projekt – ELT proces a dátový sklad v Snowflake  
Tento repozitár predstavuje implementáciu ELT procesu v prostredí Snowflake a návrh dátového skladu s dimenzionálnym modelom typu Star Schema. Projekt pracuje s datasetom Opta Data: Football – SAMPLE zo Snowflake Marketplace, ktorý obsahuje detailné eventové dáta zo zápasov anglickej Premier League.
Projekt sa zameriava na analýzu výkonnosti hráčov a tímov na základe metriky Expected Goals (xG), ktorá umožňuje hodnotiť kvalitu streleckých príležitostí nezávisle od výsledného skóre.
# 1. Uvod a popis zdrojových dát 
Cieľom tochto semestrálneho projektu je analyzovať rozsiahle dátové subory zo športovej domény, konkrétne z oblasti futbal, pomocou dát dostupných v Snowflake MarketPlace.
Projekt pracuje s datasetom Opta Data: Football – SAMPLE, ktorý obsahuje detailné informácie o zápasoch anglickej Premier League, hráčoch, tímoch a jednotlivých herných udalostiach.
Zdrojové dáta pochádzajú zo Snowflake Marketplace a sú poskytované v rámci databázy EPL. Dataset obsahuje niekoľko hlavných tabuliek:
- Game
- Team
- Player
- Event
- Event_type
- Event_type_qualifier
- Venue

Účelom ELT procesu bolo tieto dáta extrahovať zo Snowflake Marketplace, transformovať do vhodnej podoby a sprístupniť ich prostredníctvom dimenzionálneho dátoveho skladu so schémou Star Schema, ktorý umožňuje viacdimenyionálnu analytickú analýzu a tvorbu vizualizácií kľučových metrík.
# 1.1 Dátová architektúra
**ERD diagram**
Surové dáta sú usporiadané v relačnom modeli, ktorý je znázornený na entitno-relačnom diagrame (ERD):
<p align="center">
  <img src="https://github.com/Jokovic00/Projekt_databaza/blob/main/Projekt_ERD.png" alt="ERD Schema"></p>

# 2 Dimenzionálny model
V ukážke bola navrhnutá **schéma hviezdy (star schema)** podľa Kimballovej metodológie, ktorá obsahuje 1 tabuľku faktov **`fact_shot`**, ktorá je prepojená s nasledujúcimi 4 dimenziami:
- **`dim_team`**: Obsahuje podrobné informácie o teame (názov, rok, vydavateľ).Typ SCD je 2 (valid_from,valid_to,is_current)
- **`dim_player`**: Obsahuje podrobné informácie o hráčoch.Typ SCD je 2 (valid_from,valid_to,is_current)
- **`dim_date`**: Zahrňuje informácie o dátumoch (deň, mesiac, rok, štvrťrok).
- **`dim_match`**: Obsahuje zápasove údaje (matchday, attendance).
- **`dim_even_type`**: Obsahuje informácie o evente(is_goal_attempt,event_type_name)




<p align="center">
  <img src="https://github.com/Jokovic00/Projekt_databaza/blob/main/Projekt_star.png" alt = "Star Schema"></p> 

## 3. ELT proces v Snowflake
ETL proces zahŕňal tri kľúčové fázy: extrakciu (Extract), transformáciu (Transform) a nahrávanie (Load). V prostredí Snowflake bol tento proces realizovaný s cieľom spracovať zdrojové dáta zo staging vrstvy a pripraviť ich do viacdimenzionálneho dátového modelu vhodného na analytické spracovanie a vizualizáciu.
### 3.1 Extract(Extrahovanie dát)
#### Príklad kódu:
```sql
CREATE OR REPLACE TABLE STG_GAME AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.GAME;

CREATE OR REPLACE TABLE STG_TEAM AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.TEAM;

CREATE OR REPLACE TABLE STG_PLAYER AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.PLAYER;

CREATE OR REPLACE TABLE STG_EVENT AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.EVENT;

CREATE OR REPLACE TABLE STG_EVENT_TYPE AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.EVENT_TYPE;

CREATE OR REPLACE TABLE STG_EVENT_TYPE_QUALIFIER AS
SELECT * 
FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.EVENT_TYPE_QUALIFIER;

CREATE OR REPLACE TABLE STG_VENUE AS
SELECT * FROM OPTA_DATA_FOOTBALL__SAMPLE.EPL.VENUE;
```
### 3.2 Load
#### Príklad kódu:
```sql
INSERT INTO DIM_PLAYER (...)
SELECT ...
FROM STAGING.STG_PLAYER
JOIN DIM_TEAM ...
```
### 3.3 Transfer
#### Príklad kódu
```sql
CASE
  WHEN e.x > 83 AND e.y BETWEEN 21 AND 79 THEN TRUE
  ELSE FALSE
END AS is_inside_box
```
### 4 Visualizácia dát 
Dashboard obsahuje 5 vizualizácií, ktoré poskytujú základný prehlad o potencialnom gole a XG timov a hračov v Premier League. Tieto vizualizácie odpovedajú na dôležité otázky a umožňujú lepšie pochopiť šutovanie na gol hrača a ich preferencie.

