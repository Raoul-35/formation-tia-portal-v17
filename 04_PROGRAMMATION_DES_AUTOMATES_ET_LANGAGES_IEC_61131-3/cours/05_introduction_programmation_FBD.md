# MODULE 04 — PROGRAMMATION DES AUTOMATES ET LANGAGES IEC 61131-3

# LEÇON 5 — INTRODUCTION À LA PROGRAMMATION EN FBD

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Définir le langage FBD.
- Comprendre son principe de fonctionnement.
- Identifier les principaux blocs logiques.
- Comprendre les blocs AND, OR et NOT.
- Lire et interpréter un programme FBD.
- Traduire une expression logique en schéma FBD.
- Concevoir un réseau FBD dans TIA Portal V17.
- Comparer les langages LAD et FBD.

## 2. Définition du langage FBD

FBD signifie Function Block Diagram, que l'on traduit en français par diagramme de blocs fonctionnels.

C'est un langage graphique de programmation des automates industriels défini par la norme IEC 61131-3.

Contrairement au LAD, qui représente les conditions logiques sous forme de contacts et de bobines, le FBD utilise des blocs fonctionnels reliés entre eux par des lignes de connexion.

Chaque bloc réalise une opération déterminée.

Exemples :

- AND : ET logique.
- OR : OU logique.
- NOT : inversion logique.
- XOR : OU exclusif.
- NAND : NON-ET.
- NOR : NON-OU.

Le programme est construit en reliant les entrées, les blocs de traitement et les sorties.

## 3. Principe de fonctionnement du FBD

Un programme FBD peut être organisé autour de trois éléments principaux.

### 3.1. Les entrées

Les entrées représentent les informations reçues par l'automate.

Exemples :

| Adresse | Désignation | Fonction |
|---|---|---|
| I0.0 | BP_Marche | Bouton de marche |
| I0.1 | BP_Arret | Bouton d'arrêt |
| I0.2 | Defaut_Thermique | Défaut thermique |
| I0.3 | Niveau_Bas | Détection du niveau bas |

### 3.2. Les blocs fonctionnels

Les blocs réalisent les opérations logiques ou les traitements nécessaires.

Exemples :

- AND combine plusieurs conditions avec une opération ET.
- OR combine plusieurs conditions avec une opération OU.
- NOT inverse une information logique.

### 3.3. Les sorties

Les sorties représentent les résultats du programme.

Exemples :

| Adresse | Désignation | Fonction |
|---|---|---|
| Q0.0 | KM_Pompe | Commande du contacteur de pompe |
| Q0.1 | Voyant_Marche | Voyant de fonctionnement |
| Q0.2 | Alarme | Signalisation d'alarme |

## 4. Le bloc AND (ET)

### 4.1. Définition

Le bloc AND réalise une opération logique ET.

Sa sortie vaut 1 uniquement lorsque toutes ses entrées valent 1.

### 4.2. Table de vérité

| Entrée A | Entrée B | Sortie |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

### 4.3. Expression logique

Pour deux entrées :

`Q0.0 = I0.0 ET I0.1`

### 4.4. Exemple industriel : commande d'une pompe

Une pompe doit fonctionner lorsque :

- Le niveau bas est détecté.
- L'autorisation de fonctionnement est présente.

Expression :

`Q0.0 = I0.0 ET I0.1`

Représentation conceptuelle :

```text
I0.0 ───────┐
            │
          [AND]──────── Q0.0
            │
I0.1 ───────┘
```

Si les deux conditions sont réunies, la sortie est activée.

### 4.5. AND avec trois entrées

Le bloc AND peut recevoir plusieurs entrées.

Exemple :

`Q0.0 = I0.0 ET I0.1 ET I0.2`

La sortie vaut 1 uniquement si les trois entrées valent 1.

| I0.0 | I0.1 | I0.2 | Q0.0 |
|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |

À retenir : une seule entrée à 0 suffit pour que la sortie AND soit égale à 0.

## 5. Le bloc OR (OU)

### 5.1. Définition

Le bloc OR réalise une opération logique OU.

Sa sortie vaut 1 lorsqu'au moins une entrée vaut 1.

### 5.2. Table de vérité

| Entrée A | Entrée B | Sortie |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

### 5.3. Expression logique

`Q0.0 = I0.0 OU I0.1`

### 5.4. Exemple industriel : alarme

Une alarme doit s'activer lorsqu'au moins un des deux capteurs détecte une anomalie.

- I0.0 : défaut moteur.
- I0.1 : défaut température.

Expression :

`Q0.0 = I0.0 OU I0.1`

Représentation conceptuelle :

```text
I0.0 ───────┐
            │
           [OR]──────── Q0.0
            │
I0.1 ───────┘
```

Si l'un des deux défauts apparaît, l'alarme s'active.

À retenir : avec OR, une seule entrée à 1 suffit pour obtenir une sortie à 1.

## 6. Le bloc NOT (NON)

### 6.1. Définition

Le bloc NOT réalise une inversion logique.

Il possède une entrée et une sortie.

Lorsque l'entrée vaut 0, la sortie vaut 1.

Lorsque l'entrée vaut 1, la sortie vaut 0.

### 6.2. Table de vérité

| Entrée | Sortie |
|---:|---:|
| 0 | 1 |
| 1 | 0 |

### 6.3. Expression logique

`Q0.0 = NON I0.0`

### 6.4. Exemple industriel : absence de défaut

Un voyant doit s'allumer lorsque le capteur de défaut ne détecte aucun défaut.

- I0.0 : défaut thermique.
- Q0.0 : voyant d'absence de défaut.

Expression :

`Q0.0 = NON I0.0`

Représentation conceptuelle :

```text
I0.0 ───────[NOT]──────── Q0.0
```

Si le défaut est absent, I0.0 vaut 0 et le voyant s'allume.

Attention : dans une installation réelle, la signification électrique d'un signal dépend du câblage et de la logique choisie. Il faut vérifier si le capteur fournit un signal actif à 1 ou actif à 0.

## 7. Combinaison de plusieurs blocs logiques

Un programme FBD peut associer plusieurs blocs pour réaliser une commande plus complexe.

### 7.1. Exemple : commande d'une pompe avec défaut thermique

Conditions :

- I0.0 : niveau bas détecté.
- I0.1 : autorisation présente.
- I0.2 : défaut thermique.

La pompe doit fonctionner lorsque le niveau bas est détecté, que l'autorisation est présente et qu'aucun défaut thermique n'est détecté.

Expression :

`Q0.0 = I0.0 ET I0.1 ET NON I0.2`

Représentation conceptuelle :

```text
I0.2 ───────[NOT]──────┐
                       │
I0.0 ──────────────────┤
                       ├──[AND]──── Q0.0
I0.1 ──────────────────┘
```

Calcul avec :

- I0.0 = 1
- I0.1 = 1
- I0.2 = 0

Étape 1 :

`NON I0.2 = NON 0 = 1`

Étape 2 :

`Q0.0 = 1 ET 1 ET 1`

Résultat :

`Q0.0 = 1`

La pompe peut démarrer.

### 7.2. Exemple : commande avec deux capteurs et une autorisation

La pompe doit démarrer si au moins l'un des deux capteurs est activé et si l'autorisation générale est présente.

Expression :

`Q0.0 = (I0.0 OU I0.1) ET I0.2`

Représentation conceptuelle :

```text
I0.0 ───────┐
            │
           [OR]────────┐
            │          │
I0.1 ───────┘          │
                       [AND]──── Q0.0
I0.2 ──────────────────┘
```

Calcul :

- I0.0 = 0
- I0.1 = 1
- I0.2 = 1

Étape 1 :

`0 OU 1 = 1`

Étape 2 :

`1 ET 1 = 1`

Résultat :

`Q0.0 = 1`

## 8. Autres blocs logiques

Après les blocs AND, OR et NOT, il est utile de connaître d'autres opérateurs logiques.

### 8.1. XOR (OU exclusif)

La sortie vaut 1 lorsque les deux entrées sont différentes.

| A | B | A XOR B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Exemple : signalisation lorsque deux états sont différents.

### 8.2. NAND (NON-ET)

Le bloc NAND est l'inverse du bloc AND.

Sa sortie vaut 0 uniquement lorsque toutes les entrées valent 1.

| A | B | A NAND B |
|---:|---:|---:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### 8.3. NOR (NON-OU)

Le bloc NOR est l'inverse du bloc OR.

Sa sortie vaut 1 uniquement lorsque toutes les entrées valent 0.

| A | B | A NOR B |
|---:|---:|---:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

Ces blocs complètent les possibilités de programmation logique en FBD.

## 9. Comparaison entre LAD et FBD

Les langages LAD et FBD sont deux langages graphiques utilisés pour programmer les automates industriels.

| Critère | LAD | FBD |
|---|---|---|
| Signification | Ladder Diagram | Function Block Diagram |
| Représentation | Contacts et bobines | Blocs fonctionnels |
| Aspect | Schéma électrique à contacts | Schéma fonctionnel |
| Opérations logiques | Contacts en série et en parallèle | Blocs AND, OR, NOT |
| Connexions | Rails et branches | Lignes reliant les blocs |
| Lecture | Proche des schémas électriques | Proche des schémas fonctionnels |
| Utilisation | Commandes à contacts, interverrouillages | Traitements logiques et fonctionnels |

### Exemple comparatif

Expression :

`Q0.0 = I0.0 ET I0.1`

En LAD :

Deux contacts NO sont placés en série et commandent une bobine Q0.0.

En FBD :

Deux entrées alimentent un bloc AND dont la sortie commande Q0.0.

Les deux langages réalisent la même fonction logique.

## 10. Principes de conception d'un programme FBD

Pour construire un programme FBD dans TIA Portal V17, il faut suivre une démarche organisée.

1. Identifier les entrées nécessaires.
2. Identifier les sorties à commander.
3. Définir les conditions de fonctionnement.
4. Écrire l'expression logique.
5. Sélectionner les blocs fonctionnels nécessaires.
6. Relier les entrées aux blocs.
7. Relier les sorties des blocs aux variables de destination.
8. Vérifier les connexions et les types de données.
9. Compiler le programme.
10. Tester le comportement en simulation.

### Erreurs courantes

- Oublier une condition logique.
- Utiliser OR à la place de AND.
- Oublier un bloc NOT.
- Inverser les entrées.
- Mal connecter une sortie.
- Confondre l'adresse d'une entrée et celle d'une sortie.
- Ne pas vérifier le résultat logique avant la compilation.

## 11. Sécurité et limites de la programmation standard

La programmation logique standard ne suffit pas toujours à garantir la sécurité des personnes et des équipements.

Par exemple, un arrêt d'urgence réel ne doit pas reposer uniquement sur une simple condition AND ou OR dans un programme standard.

Il faut une architecture de sécurité adaptée, avec les composants et automates de sécurité appropriés selon l'analyse des risques.

Les exemples de cette leçon servent à apprendre la logique de programmation.

## 12. QCM de validation

### Question 1

Expression :

`Q0.0 = I0.0 ET I0.1`

Données : I0.0 = 1 et I0.1 = 0.

Réponse : B. Q0.0 = 0.

Explication : AND exige que toutes les entrées soient à 1.

### Question 2

Expression :

`Q0.0 = I0.0 OU I0.1`

Données : I0.0 = 0 et I0.1 = 1.

Réponse : B. Q0.0 = 1.

Explication : OR renvoie 1 dès qu'au moins une entrée vaut 1.

### Question 3

Expression :

`Q0.0 = NON I0.0`

Donnée : I0.0 = 1.

Réponse : A. Q0.0 = 0.

Explication : NOT inverse l'état logique.

### Question 4

Un bloc AND possède trois entrées : 1, 1 et 0.

Réponse : A. 0.

Explication : une seule entrée à 0 suffit pour obtenir une sortie à 0.

### Question 5

Commande d'une pompe :

`Q0.0 = I0.0 ET I0.1 ET NON I0.2`

Réponse : B.

Explication : les conditions nécessaires sont le niveau bas, l'autorisation et l'absence de défaut thermique.

### Question 6

Quelle est la différence entre LAD et FBD ?

Réponse : B.

Explication : LAD représente des contacts et des bobines, tandis que FBD utilise des blocs fonctionnels reliés entre eux.

### Question 7

Expression :

`Q0.0 = (I0.0 OU I0.1) ET NON I0.2`

Données : I0.0 = 0, I0.1 = 1 et I0.2 = 1.

Réponse : A. 0.

Explication :

`0 OU 1 = 1`

`NON 1 = 0`

`1 ET 0 = 0`

Résultat : Q0.0 = 0.

### Résultat du QCM

Score obtenu : 6/7.

## 13. Exercices d'application corrigés

### Exercice 1 — Commande d'un ventilateur

Expression :

`Q0.0 = I0.0 ET I0.1`

Données :

- I0.0 = 1
- I0.1 = 0

Résultat :

`Q0.0 = 1 ET 0 = 0`

Conclusion : le ventilateur reste arrêté.

### Exercice 2 — Alarme de sécurité

Expression :

`Q0.1 = I0.2 OU I0.3`

Données :

- I0.2 = 0
- I0.3 = 1

Résultat :

`Q0.1 = 0 OU 1 = 1`

Conclusion : l'alarme s'active.

### Exercice 3 — Inversion logique

Expression :

`Q0.2 = NON I0.4`

Donnée :

- I0.4 = 0

Résultat :

`Q0.2 = NON 0 = 1`

Conclusion : le voyant s'allume.

### Exercice 4 — Commande d'une pompe avec défaut thermique

Expression :

`Q0.0 = I0.0 ET I0.1 ET NON I0.2`

Données :

- I0.0 = 1
- I0.1 = 1
- I0.2 = 0

Résultat :

`NON I0.2 = 1`

`Q0.0 = 1 ET 1 ET 1 = 1`

Conclusion : la pompe peut démarrer.

### Exercice 5 — Combinaison OR et AND

Expression :

`Q0.0 = (I0.0 OU I0.1) ET I0.2`

Données :

- I0.0 = 0
- I0.1 = 1
- I0.2 = 1

Résultat :

`0 OU 1 = 1`

`1 ET 1 = 1`

Conclusion : la pompe peut démarrer.

### Bilan des exercices

Cinq exercices ont été réalisés.

Les exercices ont permis de travailler :

- Les opérations AND.
- Les opérations OR.
- Les opérations NOT.
- Les combinaisons de blocs.
- La traduction d'une situation industrielle en expression logique.

## 14. Compte rendu des manipulations pratiques

Les manipulations suivantes ont déjà été réalisées dans TIA Portal V17.

| N° | Manipulation | État |
|---:|---|---|
| 1 | Création d'un réseau FBD | Réalisée |
| 2 | Insertion du bloc AND | Réalisée |
| 3 | Insertion du bloc OR | Réalisée |
| 4 | Insertion du bloc NOT | Réalisée |
| 5 | Connexion des entrées et sorties | Réalisée |
| 6 | Compilation du programme FBD | Réalisée |

Les manipulations n'ont pas été répétées afin d'éviter les doublons dans la formation.

## 15. Conclusion générale

Le langage FBD permet de programmer les automates industriels à l'aide de blocs fonctionnels reliés entre eux.

Les blocs AND, OR et NOT constituent les bases de la programmation logique.

La combinaison de ces blocs permet de construire des commandes industrielles simples ou complexes.

La maîtrise du FBD facilite également la compréhension d'autres langages graphiques et prépare à l'étude du langage SCL.

**Leçon 5 terminée.**
