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
### Question 17
```
CREATE TABLE equilibre_prod (
    country                  TEXT,
    country_code             INT,
    year                     INT,
    item                     TEXT,
    item_code                INT     NOT NULL,
    origin                   TEXT,
    domestic_supply_quantity DECIMAL,
    feed                     DECIMAL,
    seed                     DECIMAL,
    waste                    DECIMAL,
    processing               DECIMAL,
    food                     DECIMAL,
    other_uses               DECIMAL,
    PRIMARY KEY (
        country_code,
        year,
        item_code
    )
);
```
### Question 18
```
CREATE TABLE sous_nutrition (
    country_code INT,
    country      TEXT,
    year         INT,
    population   INT,
    PRIMARY KEY (
        country_code,
        year
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
LIMIT 10;
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
SELECT country, year, SUM((protein_supply_quantity_gcapitaday /1000) * 365) AS ratio
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

- Les 10 pays pour lesquels la proportion de personnes sous-alimentées est la plus forte :
```
SELECT country, year, ((sous_nutrition.population/population.population) * 1000) AS ratio
FROM sous_nutrition, population
GROUP BY sous_nutrition.country_code
ORDER BY ratio DESC
LIMIT 10
```

India

China

Pakistan

Bangladesh

Ethiopia

Nigeria

Indonesia

United Republic of Tanzania

Uganda

Philippines

- Les 10 produits pour lesquels le ratio Autres utilisations/Disponibilité intérieure est le plus élevé :
```
SELECT item, SUM(other_uses/domestic_supply_quantity) AS ratio
FROM equilibre_prod
GROUP BY item_code
ORDER BY ratio DESC
LIMIT 10;
```

Alcohol, Non-Food

Aquatic Plants

Cassava and products

Coconut Oil

Fats, Animals, Raw

Fish, Body Oil

Oilcrops Oil, Other

Palm Oil

Palmkernel Oil

Rape and Mustard Oil

### Question 20

Les huiles notamment l'huile de palme sont utilisées comme bio carburant. Leur culture est de plus en plus favorisée par les agriculteurs au détriment du blé par exemple. Elle cause des problèmes de déforestation et de conditions de travail.
L'Assemblée nationale française a voté le 19 décembre 2018, contre l'avis du gouvernement, un sous-amendement au projet de la loi de finances 2019 disposant que les produits à base d'huile de palme ne sont pas considérés comme des biocarburants.
