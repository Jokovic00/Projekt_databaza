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
<p> align="center">
  <img src= alt="ERD Schema"></p>
