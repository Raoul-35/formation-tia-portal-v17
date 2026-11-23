# MODULE 04 — PROGRAMMATION DES AUTOMATES ET LANGAGES IEC 61131-3

# LEÇON 6 — INTRODUCTION À LA PROGRAMMATION EN SCL

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Définir le langage SCL.
- Comprendre son fonctionnement dans TIA Portal V17.
- Identifier sa syntaxe et ses principales instructions.
- Déclarer et utiliser des variables.
- Effectuer des opérations arithmétiques et logiques.
- Utiliser les conditions IF, ELSIF et ELSE.
- Utiliser l'instruction CASE.
- Comprendre les boucles FOR, WHILE et REPEAT.
- Manipuler des tableaux.
- Comparer les langages LAD, FBD et SCL.
- Écrire un programme simple en SCL.

---

## 2. Présentation du langage SCL

### 2.1. Définition

SCL signifie Structured Control Language.

C'est un langage de programmation textuel utilisé dans l'environnement Siemens pour programmer les automates industriels.

Il appartient à la famille des langages structurés et s'appuie sur les principes du langage ST (Structured Text) défini dans la norme IEC 61131-3.

Contrairement au LAD et au FBD, qui utilisent une représentation graphique, le SCL utilise des instructions écrites.

Exemple :

```scl
Moteur := Marche AND Autorisation;
```

Cette instruction signifie que le moteur est commandé lorsque les variables Marche et Autorisation sont toutes les deux vraies.

### 2.2. Comparaison des langages

| Langage | Signification | Représentation |
|---|---|---|
| LAD | Ladder Diagram | Schéma à contacts |
| FBD | Function Block Diagram | Blocs fonctionnels |
| SCL | Structured Control Language | Instructions textuelles |
| ST | Structured Text | Texte structuré normalisé |
| SFC | Sequential Function Chart | Graphes de fonctions séquentielles |
| IL | Instruction List | Liste d'instructions historique |

Remarque : IL est un langage historique retiré des éditions récentes de la norme IEC 61131-3. Il ne faut pas le considérer comme un langage de programmation courant à utiliser dans TIA Portal V17.

### 2.3. Domaines d'utilisation du SCL

Le SCL est particulièrement intéressant pour :

- Les calculs mathématiques.
- Les traitements numériques.
- Les conditions complexes.
- Les boucles.
- Les tableaux.
- Les algorithmes.
- Les traitements de plusieurs capteurs.
- Les calculs de puissance et de température.

Exemple industriel : calculer la température moyenne de plusieurs moteurs à partir d'un tableau de mesures.

---

## 3. Syntaxe fondamentale du SCL

### 3.1. Affectation

En SCL, l'opérateur `:=` permet d'affecter une valeur à une variable.

Exemple :

```scl
Moteur := TRUE;
Temperature := 75.5;
Nombre_Defauts := 2;
```

Interprétation :

- Moteur reçoit la valeur TRUE.
- Temperature reçoit la valeur 75.5.
- Nombre_Defauts reçoit la valeur 2.

Il faut distinguer :

| Symbole | Signification |
|---|---|
| `:=` | Affectation |
| `=` | Comparaison d'égalité |
| `<>` | Différent de |
| `>` | Supérieur à |
| `<` | Inférieur à |
| `>=` | Supérieur ou égal |
| `<=` | Inférieur ou égal |

### 3.2. Fin des instructions

Une instruction SCL se termine généralement par un point-virgule `;`.

Exemple :

```scl
Moteur := TRUE;
Voyant := FALSE;
Temperature := 65.0;
```

L'oubli du point-virgule peut provoquer une erreur de compilation.

### 3.3. Commentaires

Les commentaires permettent d'expliquer le fonctionnement du programme.

Exemple :

```scl
// Activation de la commande moteur
Moteur := TRUE;
```

Le commentaire n'est pas exécuté par l'automate.

---

## 4. Les types de données

Les variables utilisées en SCL doivent posséder un type de données adapté.

| Type | Description | Exemple |
|---|---|---|
| BOOL | Booléen | TRUE ou FALSE |
| INT | Entier signé | -120 |
| DINT | Entier signé long | 150000 |
| UINT | Entier non signé | 250 |
| REAL | Nombre réel | 25.75 |
| LREAL | Nombre réel double précision | 125.456 |
| TIME | Durée | T#5S |
| WORD | Mot de 16 bits | W#16#00FF |
| DWORD | Mot de 32 bits | DW#16#000000FF |

### 4.1. Exemple de déclaration

Dans TIA Portal, les variables d'un bloc sont déclarées dans son interface : Input, Output, InOut, Static ou Temp selon leur fonction.

Exemples de déclarations :

```scl
VAR
    Marche : BOOL;
    Autorisation : BOOL;
    Moteur : BOOL;
    Temperature : REAL;
    Nombre_Defauts : INT;
END_VAR
```

La déclaration ci-dessus illustre les types de variables. Son emplacement exact dépend de la structure du bloc et de l'éditeur utilisé.

### 4.2. Valeurs booléennes

Une variable BOOL peut prendre deux valeurs :

- TRUE : vrai, état logique 1.
- FALSE : faux, état logique 0.

Exemple :

```scl
Marche := TRUE;
Moteur := FALSE;
```

---

## 5. Les opérateurs arithmétiques

Les opérateurs arithmétiques permettent d'effectuer des calculs.

| Opérateur | Fonction | Exemple |
|---|---|---|
| `+` | Addition | A + B |
| `-` | Soustraction | A - B |
| `*` | Multiplication | A * B |
| `/` | Division | A / B |
| `MOD` | Reste de division entière | 10 MOD 3 |

### 5.1. Exemple d'addition

```scl
Somme := Valeur1 + Valeur2;
```

### 5.2. Exemple de puissance électrique

Pour un circuit monophasé :

P = U × I × cos φ

Avec :

- P : puissance active en watts.
- U : tension en volts.
- I : courant en ampères.
- cos φ : facteur de puissance.

En SCL :

```scl
Puissance := Tension * Courant * Cos_Phi;
```

Pour un système triphasé équilibré :

P = √3 × U × I × cos φ

Exemple :

- U = 400 V.
- I = 10 A.
- cos φ = 0,85.

P = 1,732 × 400 × 10 × 0,85

P ≈ 5 889 W.

### 5.3. Calcul d'une moyenne

Pour trois températures :

```scl
Moyenne := (Temp1 + Temp2 + Temp3) / 3.0;
```

Si les températures sont :

- Temp1 = 60 °C.
- Temp2 = 65 °C.
- Temp3 = 75 °C.

Moyenne = (60 + 65 + 75) / 3

Moyenne = 66,67 °C.

Attention : les types des variables doivent être compatibles. Il peut être nécessaire d'utiliser des conversions de types dans les calculs.

---

## 6. Les opérateurs logiques

Les opérateurs logiques permettent de combiner plusieurs conditions.

| Opérateur | Fonction |
|---|---|
| AND | ET logique |
| OR | OU logique |
| NOT | NON logique |
| XOR | OU exclusif |

### 6.1. AND

Toutes les conditions doivent être vraies.

```scl
Moteur := Marche AND Autorisation;
```

Le moteur fonctionne uniquement lorsque Marche et Autorisation sont TRUE.

### 6.2. OR

Au moins une condition doit être vraie.

```scl
Alarme := Defaut_Thermique OR Defaut_Electrique;
```

L'alarme est activée lorsqu'au moins un des deux défauts apparaît.

### 6.3. NOT

L'opérateur NOT inverse l'état logique.

```scl
Voyant_Normal := NOT Defaut;
```

Le voyant normal est activé lorsqu'aucun défaut n'est présent.

### 6.4. XOR

XOR signifie OU exclusif.

Le résultat est vrai lorsque les deux entrées sont différentes.

| A | B | A XOR B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

---

## 7. Les conditions IF, ELSIF et ELSE

Les conditions permettent à l'automate de choisir une action selon une situation.

### 7.1. IF / THEN

Syntaxe :

```scl
IF Condition THEN
    Instruction;
END_IF;
```

Exemple :

```scl
IF Temperature > 80.0 THEN
    Alarme := TRUE;
END_IF;
```

Si la température dépasse 80 °C, l'alarme est activée.

Attention : si la condition devient fausse, cette instruction seule ne remet pas automatiquement Alarme à FALSE.

### 7.2. IF / THEN / ELSE

Syntaxe :

```scl
IF Condition THEN
    Instruction1;
ELSE
    Instruction2;
END_IF;
```

Exemple :

```scl
IF Temperature > 80.0 THEN
    Alarme := TRUE;
ELSE
    Alarme := FALSE;
END_IF;
```

Si la température dépasse 80 °C, l'alarme est activée. Dans le cas contraire, elle est désactivée.

### 7.3. IF / ELSIF / ELSE

Cette structure permet de traiter plusieurs situations.

Exemple de surveillance thermique :

```scl
IF Temperature < 60.0 THEN
    Etat_Thermique := 0;

ELSIF Temperature < 80.0 THEN
    Etat_Thermique := 1;

ELSE
    Etat_Thermique := 2;

END_IF;
```

Interprétation :

| Température | Etat_Thermique | Signification |
|---|---:|---|
| < 60 °C | 0 | Normal |
| 60 à moins de 80 °C | 1 | Avertissement |
| ≥ 80 °C | 2 | Alarme |

Les conditions sont examinées dans l'ordre. Dès qu'une condition est vraie, son traitement est exécuté.

---

## 8. Les opérateurs de comparaison

Ils permettent de comparer des valeurs.

| Opérateur | Signification | Exemple |
|---|---|---|
| `=` | Égal à | Niveau = 50 |
| `<>` | Différent de | Mode <> 0 |
| `>` | Supérieur à | Temperature > 80 |
| `<` | Inférieur à | Niveau < 20 |
| `>=` | Supérieur ou égal | Temperature >= 90 |
| `<=` | Inférieur ou égal | Niveau <= 10 |

Exemple :

```scl
IF Niveau_Pourcent < 20.0 THEN
    Niveau_Bas := TRUE;
ELSE
    Niveau_Bas := FALSE;
END_IF;
```

Cette instruction active Niveau_Bas lorsque le niveau est inférieur à 20 %.

---

## 9. L'instruction CASE

CASE permet de sélectionner un traitement en fonction de la valeur d'une variable.

### 9.1. Syntaxe générale

```scl
CASE Variable OF

    Valeur1:
        Instruction1;

    Valeur2:
        Instruction2;

    ELSE
        Instruction_Defaut;

END_CASE;
```

### 9.2. Exemple : gestion des modes d'une pompe

| Mode_Pompe | Fonctionnement |
|---|---|
| 0 | Arrêt |
| 1 | Manuel |
| 2 | Automatique |

Programme :

```scl
CASE Mode_Pompe OF

    0:
        Pompe := FALSE;

    1:
        Pompe := Commande_Manuelle;

    2:
        Pompe := Commande_Automatique;

    ELSE
        Pompe := FALSE;

END_CASE;
```

Interprétation :

- Mode 0 : pompe arrêtée.
- Mode 1 : commande manuelle.
- Mode 2 : commande automatique.
- Toute autre valeur : pompe arrêtée.

L'utilisation de ELSE permet de prévoir un comportement pour une valeur non prévue.

---

## 10. Les boucles en SCL

Les boucles permettent de répéter des instructions.

Elles sont particulièrement utiles pour traiter plusieurs mesures ou parcourir des tableaux.

### 10.1. Boucle FOR

FOR répète un traitement sur une plage d'indices.

Syntaxe :

```scl
FOR i := 1 TO 5 DO

    Instruction;

END_FOR;
```

Exemple :

```scl
Somme := 0;

FOR i := 1 TO 5 DO
    Somme := Somme + i;
END_FOR;
```

Calcul :

Somme = 1 + 2 + 3 + 4 + 5

Somme = 15.

La variable de boucle i doit être déclarée dans une zone appropriée, par exemple Temp.

### 10.2. Boucle WHILE

WHILE répète les instructions tant que la condition reste vraie.

Syntaxe :

```scl
WHILE Condition DO

    Instruction;

END_WHILE;
```

Exemple :

```scl
i := 1;

WHILE i <= 5 DO

    Somme := Somme + i;
    i := i + 1;

END_WHILE;
```

Attention : si la condition ne devient jamais fausse, la boucle peut ne jamais se terminer et perturber le cycle de l'automate.

### 10.3. Boucle REPEAT

REPEAT exécute les instructions puis vérifie la condition de sortie.

Syntaxe :

```scl
REPEAT

    Instruction;

UNTIL Condition
END_REPEAT;
```

Différence importante :

- WHILE vérifie la condition avant l'exécution.
- REPEAT vérifie la condition après l'exécution.

REPEAT exécute donc au moins une fois les instructions.

### 10.4. Précautions industrielles

Les boucles doivent être utilisées avec prudence dans un automate.

Une boucle trop longue peut augmenter le temps de cycle.

Il faut notamment :

- Éviter les boucles infinies.
- Limiter le nombre d'itérations.
- Vérifier les conditions de sortie.
- Préserver le temps de cycle de l'automate.

---

## 11. Les tableaux en SCL

Un tableau permet de regrouper plusieurs variables de même type sous un seul nom.

Exemple : cinq températures de moteurs.

Déclaration :

```scl
Temperature_Moteurs : ARRAY[1..5] OF REAL;
```

Le tableau contient cinq valeurs réelles.

### 11.1. Affectation des valeurs

```scl
Temperature_Moteurs[1] := 60.0;
Temperature_Moteurs[2] := 65.0;
Temperature_Moteurs[3] := 70.0;
Temperature_Moteurs[4] := 75.0;
Temperature_Moteurs[5] := 80.0;
```

### 11.2. Parcours du tableau avec FOR

```scl
Somme := 0.0;

FOR i := 1 TO 5 DO

    Somme := Somme + Temperature_Moteurs[i];

END_FOR;
```

Résultat :

Somme = 60 + 65 + 70 + 75 + 80

Somme = 350 °C.

### 11.3. Calcul de la moyenne

```scl
Somme := 0.0;

FOR i := 1 TO 5 DO

    Somme := Somme + Temperature_Moteurs[i];

END_FOR;

Moyenne := Somme / 5.0;
```

Résultat :

Moyenne = 350 / 5

Moyenne = 70 °C.

Il faut initialiser Somme avant chaque nouveau calcul.

---

## 12. Organisation des programmes dans TIA Portal

TIA Portal permet d'organiser le programme à l'aide de différents blocs.

| Bloc | Signification | Fonction |
|---|---|---|
| OB | Organization Block | Organisation et exécution du programme |
| FC | Function | Fonction sans mémoire d'instance |
| FB | Function Block | Bloc fonctionnel avec mémoire d'instance |
| DB | Data Block | Stockage des données |

### 12.1. OB1

OB1 est généralement le bloc principal exécuté de manière cyclique.

Il permet d'organiser les appels des autres blocs.

### 12.2. FC

Une FC permet de regrouper un traitement particulier.

Exemple :

`FC_Calcul_Puissance`

Cette fonction peut effectuer un calcul de puissance électrique.

### 12.3. FB

Un FB permet de regrouper un ensemble de traitements avec des données conservées dans un DB d'instance.

Exemple :

`FB_Gestion_Moteur`

### 12.4. DB

Un DB permet de stocker des données utilisées par le programme.

Exemples :

- Consignes.
- États.
- Paramètres.
- Mesures.
- Historique de fonctionnement selon l'architecture retenue.

L'étude approfondie des blocs FC, FB et DB sera réalisée dans le Module 06.

---

## 13. Comparaison LAD, FBD et SCL

| Critère | LAD | FBD | SCL |
|---|---|---|---|
| Représentation | Graphique | Graphique | Textuelle |
| Contacts NO/NF | Oui | Non sous cette forme | Opérateurs logiques |
| Logique combinatoire | Oui | Oui | Oui |
| Calculs complexes | Moins direct | Possible | Très adapté |
| Boucles | Peu direct | Peu direct | Très adapté |
| Tableaux | Possible selon traitement | Possible selon traitement | Très adapté |
| Conditions complexes | Possible | Possible | Très adapté |
| Lisibilité | Électriciens et automaticiens | Automaticiens | Programmeurs et automaticiens |

### 13.1. Quand utiliser LAD ?

LAD est particulièrement adapté :

- Aux commandes à contacts.
- Aux circuits d'auto-maintien.
- Aux interverrouillages.
- Aux logiques de démarrage et d'arrêt.

### 13.2. Quand utiliser FBD ?

FBD est particulièrement adapté :

- Aux blocs fonctionnels.
- Aux combinaisons logiques.
- Aux traitements graphiques.
- À l'organisation de fonctions interconnectées.

### 13.3. Quand utiliser SCL ?

SCL est particulièrement adapté :

- Aux calculs.
- Aux boucles.
- Aux tableaux.
- Aux algorithmes.
- Aux traitements numériques complexes.

Il n'existe pas un langage universellement meilleur. Le choix dépend du problème à résoudre et des habitudes de l'équipe de maintenance et d'automatisme.

---

## 14. Exemple industriel récapitulatif

### 14.1. Cahier des charges

On souhaite commander une pompe industrielle.

Conditions :

- Le bouton Marche doit être activé.
- L'autorisation de fonctionnement doit être présente.
- Aucun défaut thermique ne doit être détecté.

### 14.2. Variables

| Variable | Type | Fonction |
|---|---|---|
| Marche | BOOL | Commande de démarrage |
| Autorisation | BOOL | Autorisation de fonctionnement |
| Defaut_Thermique | BOOL | État du défaut thermique |
| Pompe | BOOL | Commande de la pompe |

### 14.3. Programme SCL

```scl
Pompe := Marche AND Autorisation AND NOT Defaut_Thermique;
```

### 14.4. Table de fonctionnement

| Marche | Autorisation | Défaut thermique | Pompe |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 0 |

Cette logique est un exemple pédagogique. Dans une installation réelle, les fonctions de sécurité et de protection doivent être conçues selon les exigences applicables et ne doivent pas reposer uniquement sur ce programme standard.

---

# 15. QCM DE VALIDATION

## Question 1

Quelle est la principale différence entre SCL et LAD ?

Réponse : B.

Explication : SCL utilise des instructions textuelles tandis que LAD utilise une représentation graphique à contacts.

## Question 2

Que signifie l'instruction suivante ?

```scl
Temperature := 75.5;
```

Réponse : B.

Explication : la variable Temperature reçoit la valeur 75.5.

## Question 3

Quelle est la valeur de Puissance ?

```scl
Puissance := Tension * Courant;
```

Avec Tension = 230 et Courant = 5.

Réponse : B.

Calcul :

P = 230 × 5 = 1 150 W.

## Question 4

Si Temperature = 85 °C, que fait ce programme ?

```scl
IF Temperature > 80.0 THEN
    Alarme := TRUE;
ELSE
    Alarme := FALSE;
END_IF;
```

Réponse : B.

Explication : 85 est supérieur à 80. Alarme reçoit TRUE.

## Question 5

Dans une instruction CASE, si Mode_Pompe = 2 et Commande_Automatique = TRUE, quelle est la valeur de Pompe ?

Réponse : B.

Explication : le traitement associé au mode 2 est exécuté et Pompe reçoit TRUE.

## Question 6

Quelle est la valeur finale de Somme ?

```scl
Somme := 0;

FOR i := 1 TO 4 DO
    Somme := Somme + i;
END_FOR;
```

Réponse : C.

Calcul :

Somme = 1 + 2 + 3 + 4 = 10.

## Question 7

Quel langage est généralement le plus adapté pour parcourir un tableau de 100 températures avec une boucle FOR et calculer leur moyenne ?

Réponse : C — SCL.

Explication : SCL permet d'utiliser directement les boucles, les tableaux et les opérations arithmétiques.

### Résultat obtenu

6 bonnes réponses sur 7.

---

# 16. EXERCICES D'APPLICATION CORRIGÉS

## Exercice 1 — Calcul de puissance triphasée

Données :

- U = 400 V.
- I = 10 A.
- cos φ = 0,85.

Formule :

P = √3 × U × I × cos φ.

Calcul :

P = 1,732 × 400 × 10 × 0,85.

P ≈ 5 889 W.

Réponse : B.

## Exercice 2 — Condition IF/ELSE

Programme :

```scl
IF Temperature > 80.0 THEN
    Alarme := TRUE;
ELSE
    Alarme := FALSE;
END_IF;
```

Donnée : Temperature = 85 °C.

Réponse : B — TRUE.

Explication : 85 > 80.

## Exercice 3 — Conditions multiples

Programme :

```scl
IF Temperature < 60.0 THEN
    Etat_Thermique := 0;
ELSIF Temperature < 80.0 THEN
    Etat_Thermique := 1;
ELSE
    Etat_Thermique := 2;
END_IF;
```

Donnée : Temperature = 75 °C.

Réponse : B — État 1, avertissement.

Explication : la première condition est fausse, la deuxième est vraie.

## Exercice 4 — Sélection CASE

Données :

- Mode_Pompe = 2.
- Commande_Automatique = TRUE.
- Commande_Manuelle = FALSE.

Réponse : B — Pompe = TRUE.

Explication : le mode automatique est sélectionné.

## Exercice 5 — Boucle FOR

Températures :

60, 65, 70, 75 et 80 °C.

Programme :

```scl
Somme := 0.0;

FOR i := 1 TO 5 DO
    Somme := Somme + Temperature_Moteurs[i];
END_FOR;
```

Réponse : C — 350 °C.

Calcul :

60 + 65 + 70 + 75 + 80 = 350.

## Exercice 6 — Traduction logique en SCL

Conditions :

- Marche activé.
- Autorisation présente.
- Aucun défaut thermique.

Réponse : C.

```scl
Moteur := Marche AND Autorisation AND NOT Defaut_Thermique;
```

Explication : les trois conditions nécessaires sont réunies à l'aide des opérateurs AND et NOT.

### Résultat obtenu

6 exercices réussis sur 6.

---

# 17. BILAN DES MANIPULATIONS PRATIQUES

Les manipulations pratiques de création et de compilation d'un bloc SCL ont déjà été réalisées dans le projet Formation_TIA_Portal_V17 au cours du Module 02.

| Manipulation | État |
|---|---|
| Création d'un bloc SCL | Réalisée |
| Saisie d'instructions SCL | Réalisée |
| Compilation du bloc | Réalisée |

Ces manipulations ne sont donc pas reprises inutilement dans cette leçon.

---

# 18. SYNTHÈSE GÉNÉRALE

Le langage SCL est un outil important de programmation des automates Siemens.

Il permet de réaliser des traitements structurés à l'aide :

- Des affectations.
- Des variables et types de données.
- Des opérations arithmétiques.
- Des opérateurs logiques.
- Des comparaisons.
- Des conditions IF, ELSIF et ELSE.
- Des sélections CASE.
- Des boucles FOR, WHILE et REPEAT.
- Des tableaux.

L'association des langages LAD, FBD et SCL permet de développer des programmes industriels adaptés aux différents besoins d'une installation automatisée.

**Fin de la leçon 6 — Introduction à la programmation en SCL.**
