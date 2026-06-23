# relation_test_code_smell_ML

Studying relationship between test smell and code smell for applying machine learning.

## Pipeline actuel

Le dépôt contient des scripts pour :

1. cloner les projets open source dans `dataset/projects` ;
2. extraire des métriques CK dans `dataset/metrics` ;
3. détecter des code smells avec PMD dans `dataset/code_smells` ;
4. détecter des test smells avec TestSmellDetector dans `dataset/test_smells` ;
5. construire le dataset final appariant chaque classe de production avec sa classe de test associée.

## Construire le dataset final apparié

Le script `dataset_transformer/build_paired_dataset.py` transforme les CSV existants en fichiers finaux où chaque ligne met côte à côte :

- le projet ;
- la classe de production (`class_file`, `class_name`, `class_key`) ;
- la classe/fichier de test associé (`test_file`) ;
- les compteurs de code smells PMD ;
- les indicateurs de test smells ;
- deux colonnes binaires (`has_code_smell`, `has_test_smell`) utiles pour chercher des corrélations.

Commande recommandée depuis la racine du dépôt :

```bash
python dataset_transformer/build_paired_dataset.py \
  --code-smells-dir dataset/code_smells \
  --test-smells-dir dataset/test_smells \
  --output-dir dataset/final
```

Le script ne scanne pas `dataset/projects` : il lit uniquement les CSV déjà générés dans `dataset/code_smells` et `dataset/test_smells`. Les fichiers produits sont :

- `dataset/final/<projet>.csv` pour chaque projet ;
- `dataset/final/all_projects.csv` pour un dataset consolidé.

Si nécessaire, on peut limiter l'exécution à certains projets :

```bash
python dataset_transformer/build_paired_dataset.py --projects spring-boot quarkus
```

## Projets open source supplémentaires

Une liste de 10 dépôts Java populaires à ajouter au dataset est disponible dans [`docs/open_source_projects_to_add.md`](docs/open_source_projects_to_add.md).
