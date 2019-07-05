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

### Question 19

- Les 10 pays ayant le plus haut ratio disponibilité alimentaire/habitant en termes de protéines (en kg) par habitant :

```
SELECT country, year, SUM(protein_supply_quantity_gcapitaday * 365 / 1000 / population * 1000) AS ratio
FROM dispo_alim, population
WHERE dispo_alim.country = population.country AND dispo_alim.year = population.year
GROUP BY population.country
ORDER BY ratio DESC
LIMIT 10;
```

Bermuda
Saint Kitts and Nevis
Dominica
Antigua and Barbuda
Saint Vincent and the Grenadines
Kiribati
Grenada
Saint Lucia
Samoa
Iceland
