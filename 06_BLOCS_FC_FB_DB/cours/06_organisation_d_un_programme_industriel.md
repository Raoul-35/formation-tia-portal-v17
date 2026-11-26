# MODULE 06 — BLOCS FC, FB ET DB
## Leçon 6 : Organisation d'un programme industriel

### 1. Objectifs pédagogiques

À la fin de cette leçon, l'apprenant doit être capable de :
- Organiser les blocs d'un projet TIA Portal.
- Répartir les responsabilités entre OB, FC, FB et DB.
- Structurer un programme industriel modulaire.
- Appliquer une convention de nommage cohérente.
- Faciliter la maintenance et les évolutions du programme.

### 2. Principe de programmation modulaire

La programmation modulaire consiste à diviser un programme industriel en plusieurs blocs indépendants, chacun ayant une responsabilité précise.

Il est préférable d'éviter de concentrer toutes les opérations dans OB1.

Une bonne architecture facilite :
- La maintenance.
- Le diagnostic.
- La réutilisation du code.
- Les modifications futures.
- La compréhension du programme par d'autres techniciens.

### 3. Architecture recommandée

Exemple d'une station de pompage :

```text
PROGRAMME AUTOMATE
       |
       v
      OB1
       |
       +---- FC_ACQUISITION
       |
       +---- FB_GESTION_POMPE
       |          |
       |          +---- DB_POMPE_1
       |          +---- DB_POMPE_2
       |          +---- DB_POMPE_3
       |
       +---- FC_SURVEILLANCE
       |
       +---- FC_GESTION_ALARMES
       |
       +---- DB_PARAMETRES
       |
       +---- DB_POMPAGE
```

### 4. Responsabilité des blocs

| Bloc | Responsabilité |
|---|---|
| OB1 | Organisation de l'exécution cyclique |
| FC_ACQUISITION | Acquisition et traitement des mesures |
| FB_GESTION_POMPE | Gestion du fonctionnement d'une pompe |
| DB_POMPE_1 à 3 | Mémorisation des données propres aux pompes |
| FC_SURVEILLANCE | Surveillance des conditions de fonctionnement |
| FC_GESTION_ALARMES | Traitement des conditions d'alarme |
| DB_PARAMETRES | Stockage des seuils et paramètres |
| DB_POMPAGE | Centralisation des informations de la station |

### 5. Convention de nommage

Une convention de nommage permet d'identifier rapidement les blocs et les variables.

| Préfixe | Signification | Exemple |
|---|---|---|
| OB | Organization Block | OB1 |
| FC | Function | FC_SURVEILLANCE |
| FB | Function Block | FB_GESTION_POMPE |
| DB | Data Block | DB_PARAMETRES |
| E | Entrée | E_Marche |
| S | Sortie | S_Pompe |
| M | Mémoire | M_Autorisation |

Les noms doivent être explicites et cohérents dans tout le projet.

### 6. Réutilisation des blocs FB

Une même logique FB peut être utilisée pour plusieurs équipements.

Exemple :

| Équipement | FB | DB d'instance |
|---|---|---|
| Pompe 1 | FB_GESTION_POMPE | DB_POMPE_1 |
| Pompe 2 | FB_GESTION_POMPE | DB_POMPE_2 |
| Pompe 3 | FB_GESTION_POMPE | DB_POMPE_3 |
| Pompe 4 | FB_GESTION_POMPE | DB_POMPE_4 |

Pour ajouter une quatrième pompe, il faut :
1. Créer un quatrième DB d'instance.
2. Ajouter un nouvel appel du FB dans OB1.
3. Affecter les entrées et sorties correspondant à la pompe 4.

Il n'est pas nécessaire de créer un nouveau FB pour chaque pompe.

### 7. Avantages de la programmation modulaire

| Avantage | Explication |
|---|---|
| Maintenance | Retrouver rapidement le bloc concerné |
| Évolutivité | Ajouter des équipements plus facilement |
| Réutilisation | Employer les mêmes blocs pour plusieurs équipements |
| Diagnostic | Isoler plus facilement les erreurs |
| Lisibilité | Faciliter la compréhension du programme |

### 8. Exercices de validation

**Exercice 1 : affectation des blocs**

1. Conversion d'une valeur analogique : FC.
2. Mémorisation de l'état d'une pompe : FB.
3. Stockage des seuils : DB.
4. Exécution cyclique : OB.

**Exercice 2 : réutilisation**

Pour trois pompes identiques :
- Un seul FB.
- Trois DB d'instance.

**Exercice 3 : organisation**

Les fonctions d'acquisition, de gestion des pompes, de surveillance et d'alarmes sont séparées pour faciliter la maintenance et les évolutions.

**Exercice 4 : ajout d'une pompe**

La quatrième pompe nécessite un quatrième DB d'instance et un nouvel appel du FB existant.

### 9. Validation de la leçon

| Compétence | État |
|---|---|
| Répartition des responsabilités | Acquise |
| Architecture modulaire | Acquise |
| Convention de nommage | Étudiée |
| Réutilisation des FB | Acquise |
| Organisation d'un programme industriel | Acquise |

### Conclusion

Une architecture industrielle bien organisée repose sur une répartition claire des responsabilités entre les blocs OB, FC, FB et DB.

La programmation modulaire facilite la maintenance, le diagnostic et l'évolution des installations automatisées.
