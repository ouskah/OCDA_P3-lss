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
CREATE TABLE population (
    country_code INT  NOT NULL,
    country      TEXT,
    year         INT  NOT NULL,
    population   INT,
    PRIMARY KEY (
        country_code,
        year
    )
);
```
### Question 16

```
CREATE TABLE dispo_alim (
    country                            TEXT,
    country_code                       INT     NOT NULL,
    year                               INT     NOT NULL,
    item                               TEXT,
    item_code                          INT     NOT NULL,
    origin                             TEXT,
    food_supply_quantity_kgcapitayr    DECIMAL,
    food_supply_kcalcapitaday          DECIMAL,
    protein_supply_quantity_gcapitaday DECIMAL,
    fat_supply_quantity_gcapitaday     DECIMAL,
    PRIMARY KEY (
        country_code,
        year,
        item_code
    )
);
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
