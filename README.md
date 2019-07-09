# Réalisez une étude de santé publique
Projet 3 du parcours de formation Openclassrooms Data Analyst

## Notes & Questions

- Influence de NaN et Inf dans les opérations ?

## Base de données

### Création

```
sqlite> .open P3.db
sqlite> .mode csv
sqlite> .import population_export.csv population
sqlite> .import dispo_alim_export.csv dispo_alim
sqlite> .import equilibre_prod_export.csv equilibre_prod
sqlite> .import sous_nutrition_export.csv sous_nutrition
```

### Question 15
```
PRAGMA foreign_keys = 0;

CREATE TABLE sqlitestudio_temp_table AS SELECT *
                                          FROM population;

DROP TABLE population;

CREATE TABLE population (
    country_code INT  NOT NULL,
    country      TEXT,
    year         INT  NOT NULL,
    population   INT
);

INSERT INTO population (
                           country_code,
                           country,
                           year,
                           population
                       )
                       SELECT country_code,
                              country,
                              year,
                              population
                         FROM sqlitestudio_temp_table;

DROP TABLE sqlitestudio_temp_table;

CREATE UNIQUE INDEX idx ON population (
    country_code,
    year
);

PRAGMA foreign_keys = 1;

```

### Question 19

- Les 10 pays ayant le plus haut ratio disponibilité alimentaire/habitant en termes de protéines (en kg) par habitant :

```
SELECT country, year, SUM((protein_supply_quantity_gcapitaday / 1000) * 365) AS ratio
FROM dispo_alim
GROUP BY country
ORDER BY ratio DESC
LIMIT 10
```
Iceland

China, Hong Kong SAR

Israel

Lithuania

Maldives

Finland

Luxembourg

Netherlands

Albania

France

- Les 10 pays ayant le plus haut ratio disponibilité alimentaire/habitant en termes de Calories (kcal) par habitant :

```
SELECT country, year, SUM(food_supply_kcalcapitaday * 365) AS ratio
FROM dispo_alim
GROUP BY country
ORDER BY ratio DESC
LIMIT 10;
```

Austria

Belgium

Turkey

United States of America

Israel

Ireland

Italy

Luxembourg

Egypt

Germany

- Les 10 pays ayant le plus bas ratio disponibilité alimentaire/habitant en termes de Calories (kcal) par habitant :
```
SELECT country, year, SUM(food_supply_kcalcapitaday * 365) AS ratio
FROM dispo_alim
GROUP BY country
ORDER BY ratio ASC
LIMIT 10;
```

Zambia

Central African Republic

Madagascar

Haiti

Afghanistan

Democratic People's Republic of Korea

Chad

Ethiopia

Timor-Leste

Uganda

- Les 10 pays ayant le plus bas ratio disponibilité alimentaire/habitant en termes de protéines (en kg) par habitant :
```
SELECT country, year, SUM(protein_supply_quantity_gcapitaday * 365) AS ratio
FROM dispo_alim
GROUP BY country
ORDER BY ratio ASC
LIMIT 10;
```

Liberia

Guinea-Bissau

Mozambique

Madagascar

Haiti

Central African Republic

Zimbabwe

Congo

Sao Tome and Principe

Uganda
