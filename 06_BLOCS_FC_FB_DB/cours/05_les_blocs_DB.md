# MODULE 06 — BLOCS FC, FB ET DB
## Leçon 5 : Les blocs de données (DB)

### 1. Définition

Un DB (Data Block) est un bloc destiné au stockage des données utilisées par un programme automate.

Il permet de séparer les données de la logique de programmation et de faciliter leur exploitation.

Les données d'un DB peuvent être utilisées par différents blocs du programme.

### 2. Organisation des données

Un DB peut contenir différents types de données :

| Type | Exemple |
|---|---|
| BOOL | Alarme_Pression |
| INT | Nombre_Pompes_Actives |
| REAL | Pression_Actuelle |
| TIME | Temporisation_Generale |
| D'autres structures | Tableaux et structures de données |

### 3. Application : DB_PARAMETRES

Ce DB global contient les paramètres de fonctionnement de l'installation :

| Variable | Type | Valeur initiale |
|---|---|---:|
| Pression_Min | REAL | 2.5 |
| Pression_Max | REAL | 8.0 |
| Temporisation_Generale | TIME | T#5S |
| Nb_Max_Demarrages | INT | 10 |

Il centralise les paramètres qui peuvent être utilisés par plusieurs blocs.

### 4. Application : DB_POMPAGE

Un deuxième DB global a été créé pour centraliser les informations de la station de pompage.

| Variable | Type | Valeur initiale |
|---|---|---|
| Pression_Actuelle | REAL | 0.0 |
| Pompe_1_Active | BOOL | FALSE |
| Pompe_2_Active | BOOL | FALSE |
| Nombre_Pompes_Actives | INT | 0 |
| Alarme_Pression | BOOL | FALSE |

### 5. Exploitation d'un DB depuis un FC

Une instruction SCL permet de comparer la pression actuelle à la pression maximale :

```scl
"DB_POMPAGE".Alarme_Pression :=
    "DB_POMPAGE".Pression_Actuelle >
    "DB_PARAMETRES".Pression_Max;
```

Résultat :
- Pression supérieure à 8 bar : alarme TRUE.
- Pression inférieure ou égale à 8 bar : alarme FALSE.

Le FC peut ainsi lire des données dans un DB et écrire un résultat dans un autre DB.

### 6. Exploitation d'un DB depuis un FB

Dans `FB_GESTION_POMPE`, l'état de la pompe est publié dans le DB global :

```scl
"DB_POMPAGE".Pompe_1_Active := Pompe_Marche;
```

Cette affectation permet à d'autres blocs de connaître l'état de la pompe sans accéder directement à la mémoire interne du FB.

Attention : si plusieurs instances du même FB écrivent dans une variable globale identique, elles peuvent se remplacer mutuellement. Chaque équipement doit disposer de sa propre variable d'état.

### 7. Intérêt des DB dans une installation industrielle

Les DB permettent notamment :
- De centraliser les paramètres.
- De partager des informations entre plusieurs blocs.
- De séparer les données de la logique.
- De faciliter la supervision avec WinCC.
- De rendre le programme plus organisé et évolutif.

### 8. Travaux pratiques réalisés

- Création du DB global `DB_POMPAGE`.
- Déclaration de cinq variables.
- Exploitation de `DB_PARAMETRES` depuis un FC.
- Comparaison de la pression actuelle à la pression maximale.
- Écriture d'une alarme dans `DB_POMPAGE`.
- Publication de l'état de la pompe depuis `FB_GESTION_POMPE`.
- Compilation des blocs.

### 9. Validation

| Compétence | État |
|---|---|
| Création d'un DB global | Réalisée |
| Déclaration des variables | Réalisée |
| Lecture des données depuis un FC | Réalisée |
| Écriture dans un DB depuis un FC | Réalisée |
| Écriture dans un DB depuis un FB | Réalisée |
| Compilation | Réussie |
| Test avec PLCSIM | À réaliser |

### Conclusion

Les blocs DB constituent la mémoire organisée du programme automate. Ils permettent de centraliser les paramètres et les états de fonctionnement afin de faciliter les échanges entre les blocs FC, FB et les systèmes de supervision.
