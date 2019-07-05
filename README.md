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
```
