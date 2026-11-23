# MODULE 04 – PROGRAMMATION DES AUTOMATES
# LEÇON 2 – OPÉRATIONS LOGIQUES ET TABLES DE VÉRITÉ

## 1. Objectifs pédagogiques

À la fin de cette leçon, l'apprenant doit être capable de :

- Comprendre les opérations logiques fondamentales.
- Expliquer les opérations ET, OU et NON.
- Construire et interpréter les tables de vérité.
- Traduire une expression booléenne en réseau LAD.
- Combiner plusieurs opérations logiques.
- Respecter les priorités des opérations logiques.
- Analyser le comportement logique d'un automatisme industriel.

## 2. Introduction aux opérations logiques

Un automate programmable industriel (API) reçoit des informations provenant de capteurs, de boutons-poussoirs, de détecteurs et d'autres équipements.

À partir de ces informations, il exécute un programme pour commander des actionneurs tels que :

- Les moteurs électriques.
- Les pompes.
- Les ventilateurs.
- Les électrovannes.
- Les voyants.
- Les systèmes d'alarme.

Pour prendre une décision, l'automate doit analyser les différentes conditions qui lui sont communiquées.

Les opérations logiques permettent de combiner ces conditions afin de déterminer si une commande doit être activée ou non.

### 2.1. Les états logiques

En automatisme numérique, une variable booléenne possède deux états :

| Valeur | Signification |
|---|---|
| 0 | Faux, inactif ou condition non satisfaite |
| 1 | Vrai, actif ou condition satisfaite |

Exemples :

| Variable | Valeur 0 | Valeur 1 |
|---|---|---|
| BP_Marche | Bouton non activé | Bouton activé |
| Niveau_OK | Niveau insuffisant | Niveau suffisant |
| Defaut_Thermique | Aucun défaut | Défaut présent |
| Moteur | Moteur arrêté | Moteur commandé |

Attention : la valeur logique d'une variable dépend de sa signification dans le programme. Une entrée physique peut notamment être câblée de telle sorte que son état électrique ne corresponde pas directement à la signification fonctionnelle souhaitée.

## 3. L'opération logique ET (AND)

### 3.1. Définition

L'opération ET exige que toutes les conditions soient vraies simultanément.

Si une seule condition est fausse, le résultat est faux.

Symbole logique : ∧

Expression générale :

`S = A ET B`

### 3.2. Exemple industriel

Une pompe doit démarrer uniquement lorsque :

- Le bouton Marche est activé.
- Le niveau du réservoir est suffisant.

Expression :

`Pompe = Marche ET Niveau_OK`

### 3.3. Table de vérité

| A | B | A ET B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Conclusion : l'opération ET donne 1 uniquement lorsque toutes ses entrées valent 1.

### 3.4. Représentation en LAD

En LAD, l'opération ET est représentée par des contacts placés en série.

```text
      Marche       Niveau_OK
--------| |-----------| |------------( Pompe )
```

Le résultat logique du réseau est vrai uniquement si les deux contacts laissent passer la continuité logique.

### 3.5. Autres applications industrielles

Une autorisation de démarrage peut dépendre de plusieurs conditions :

`Moteur = BP_Marche ET Autorisation ET Pression_OK`

Le moteur ne peut être commandé que si les trois conditions sont satisfaites.

## 4. L'opération logique OU (OR)

### 4.1. Définition

L'opération OU signifie qu'au moins une condition doit être vraie.

Si une seule entrée vaut 1, le résultat vaut 1.

Symbole logique : ∨

Expression générale :

`S = A OU B`

### 4.2. Exemple industriel

Un ventilateur doit fonctionner lorsque :

- La température est élevée.
- OU l'opérateur active la commande manuelle.

Expression :

`Ventilateur = Temperature_Haute OU Marche_Manuelle`

### 4.3. Table de vérité

| A | B | A OU B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Conclusion : l'opération OU donne 1 lorsqu'au moins une entrée vaut 1.

### 4.4. Représentation en LAD

En LAD, l'opération OU est représentée par des branches parallèles.

```text
       Temperature_Haute
--------| |----------------
        |                 |
        | Marche_Manuelle |
        +----| |----------+----( Ventilateur )
```

Le ventilateur peut être commandé par l'une ou l'autre des deux conditions.

### 4.5. Autres applications industrielles

Une alarme peut être déclenchée si :

- La pression est trop élevée.
- OU la température est trop élevée.

Expression :

`Alarme = Pression_Haute OU Temperature_Haute`

## 5. L'opération logique NON (NOT)

### 5.1. Définition

L'opération NON inverse l'état logique d'une variable.

- Si l'entrée vaut 0, le résultat vaut 1.
- Si l'entrée vaut 1, le résultat vaut 0.

Symbole logique : ¬

Expression :

`S = NON A`

### 5.2. Table de vérité

| A | NON A |
|---|---|
| 0 | 1 |
| 1 | 0 |

### 5.3. Exemple industriel

Une pompe doit fonctionner uniquement lorsqu'aucun défaut thermique n'est présent.

Variable :

`Defaut_Thermique`

Expression :

`Autorisation = NON Defaut_Thermique`

| Defaut_Thermique | Autorisation |
|---|---|
| 0 | 1 |
| 1 | 0 |

### 5.4. Représentation en LAD

La négation est représentée par un contact NF LAD.

```text
      Defaut_Thermique
----------|/|----------------( Autorisation )
```

Lorsque la variable `Defaut_Thermique` vaut 0, le contact NF LAD est passant.

Lorsqu'elle vaut 1, le contact NF LAD est non passant.

### 5.5. Attention à la distinction entre contact physique et contact LAD

Un contact NF dans un programme LAD ne signifie pas nécessairement qu'un contact physique NF est installé sur le terrain.

Le contact LAD représente une condition logique :

- Contact NO LAD : vrai lorsque la variable vaut 1.
- Contact NF LAD : vrai lorsque la variable vaut 0.

Il faut donc distinguer le câblage physique et le traitement logique effectué par le programme.

## 6. Combinaison des opérations logiques

Dans les applications industrielles, plusieurs opérations logiques sont généralement combinées.

### 6.1. Exemple : commande d'une pompe

Une pompe doit fonctionner lorsque :

- Le mode automatique OU le mode manuel est actif.
- Le niveau est suffisant.
- Aucun défaut thermique n'est présent.

Expression :

`Pompe = (Mode_Auto OU Mode_Manuel) ET Niveau_OK ET NON Defaut_Thermique`

### 6.2. Analyse des conditions

| Condition | État exigé |
|---|---|
| Mode_Auto OU Mode_Manuel | Au moins un mode actif |
| Niveau_OK | 1 |
| Defaut_Thermique | 0 |

### 6.3. Représentation LAD

```text
       Mode_Auto
--------| |---------
        |           |
        | Mode_Manuel
        +----| |----+
                    |
                    | Niveau_OK
                    +----| |------+
                    |             |
                    | Defaut      |
                    | Thermique   |
                    +----|/|------+
                                  |
                                  +----( Pompe )
```

### 6.4. Analyse avec des valeurs

| Mode_Auto | Mode_Manuel | Niveau_OK | Defaut_Thermique | Pompe |
|---|---|---|---|---|
| 0 | 0 | 1 | 0 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 0 |

La pompe fonctionne uniquement si toutes les conditions générales sont satisfaites.

## 7. Priorité des opérations logiques

Lorsque plusieurs opérations sont présentes dans une expression, leur ordre d'évaluation doit être respecté.

Ordre habituel :

1. Parenthèses.
2. NON.
3. ET.
4. OU.

### 7.1. Exemple 1

Expression :

`Moteur = A OU (B ET C)`

Données :

- A = 0
- B = 1
- C = 1

Étape 1 : calcul de B ET C.

`1 ET 1 = 1`

Étape 2 : calcul de A OU le résultat.

`0 OU 1 = 1`

Résultat :

`Moteur = 1`

### 7.2. Exemple 2

Expression :

`Pompe = (A OU B) ET C`

Données :

- A = 1
- B = 0
- C = 0

Étape 1 :

`1 OU 0 = 1`

Étape 2 :

`1 ET 0 = 0`

Résultat :

`Pompe = 0`

### 7.3. Importance des parenthèses

Les expressions suivantes ne sont pas équivalentes :

`A OU (B ET C)`

`(A OU B) ET C`

Les parenthèses changent l'ordre des opérations et peuvent donc modifier le comportement d'une commande industrielle.

## 8. Traduction des expressions booléennes en LAD

La programmation LAD permet de représenter graphiquement les opérations logiques.

Correspondances fondamentales :

| Opération logique | Représentation LAD |
|---|---|
| ET | Contacts en série |
| OU | Contacts en parallèle |
| NON | Contact NF |
| Résultat logique | Bobine |

### 8.1. Exemple : (A ET B) OU C

Expression :

`Moteur = (A ET B) OU C`

Représentation :

```text
       A          B
-------| |--------| |---------
       |                     |
       |          C          |
       +---------| |---------+----( Moteur )
```

Interprétation :

- Première branche : A ET B.
- Deuxième branche : C.
- Les deux branches sont en parallèle : A ET B OU C.

### 8.2. Exemple : (A OU B) ET NON C

Expression :

`Moteur = (A OU B) ET NON C`

Représentation :

```text
       A
-------| |---------
       |           |
       | B         |
       +---| |-----+
                   |
                   | C
                   +---|/|--------( Moteur )
```

Le moteur peut fonctionner si A ou B est actif, à condition que C soit égal à 0.

## 9. Notions complémentaires : autres opérations logiques

En plus des trois opérations fondamentales, il existe d'autres opérations logiques.

### 9.1. OU exclusif (XOR)

Le XOR signifie OU exclusif.

Le résultat vaut 1 lorsque les deux entrées sont différentes.

| A | B | A XOR B |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Exemple : détecter une situation où une seule de deux commandes est active.

### 9.2. NON-ET (NAND)

Le NAND est l'inverse de l'opération ET.

`S = NON (A ET B)`

| A | B | NAND |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### 9.3. NON-OU (NOR)

Le NOR est l'inverse de l'opération OU.

`S = NON (A OU B)`

| A | B | NOR |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

Ces opérations pourront être représentées à l'aide de combinaisons de contacts et de négations dans les réseaux LAD.

## 10. Application industrielle : commande d'une station de pompage

### 10.1. Cahier des charges simplifié

Une pompe de remplissage doit fonctionner lorsque :

- Le niveau est bas.
- La pompe est disponible.
- Aucun défaut thermique n'est présent.
- L'arrêt manuel n'est pas activé.

### 10.2. Variables utilisées

| Variable | Type | Signification |
|---|---|---|
| Niveau_Bas | BOOL | Niveau bas détecté |
| Pompe_Disponible | BOOL | Pompe disponible |
| Defaut_Thermique | BOOL | Défaut thermique présent |
| Arret_Manuel | BOOL | Arrêt manuel activé |
| Pompe | BOOL | Commande de la pompe |

### 10.3. Expression booléenne

`Pompe = Niveau_Bas ET Pompe_Disponible ET NON Defaut_Thermique ET NON Arret_Manuel`

### 10.4. Réseau LAD

```text
 Niveau_Bas  Pompe_Disponible  Defaut_Thermique  Arret_Manuel
----| |----------| |---------------|/|---------------|/|----( Pompe )
```

### 10.5. Analyse

| Niveau_Bas | Disponible | Défaut thermique | Arrêt manuel | Pompe |
|---|---|---|---|---|
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 | 0 |
| 1 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 |

Cette logique représente une commande fonctionnelle simplifiée. Dans une installation réelle, les fonctions de sécurité doivent être conçues et validées selon les exigences de sécurité applicables. Un programme LAD standard ne remplace pas une architecture de sécurité appropriée.

## 11. QCM de vérification des connaissances

### Question 1

Une pompe doit démarrer uniquement lorsque le bouton Marche ET le capteur Niveau_OK sont actifs.

Quelle opération faut-il utiliser ?

A. OU  
B. ET  
C. NON  
D. XOR

**Réponse : B. ET.**

Explication : toutes les conditions doivent être vraies simultanément.

### Question 2

Un ventilateur doit démarrer si la température est élevée OU si l'opérateur active la commande manuelle.

Quelle opération faut-il utiliser ?

A. ET  
B. NON  
C. OU  
D. XOR

**Réponse : C. OU.**

Explication : une seule des deux conditions suffit.

### Question 3

Une autorisation doit être active uniquement lorsque `Defaut_Thermique = 0`.

Quelle expression est correcte ?

A. `Autorisation = Defaut_Thermique`  
B. `Autorisation = NON Defaut_Thermique`  
C. `Autorisation = Defaut_Thermique ET 1`  
D. `Autorisation = Defaut_Thermique OU 1`

**Réponse : B.**

Explication : la négation inverse l'état logique de la variable.

### Question 4

Expression :

`Moteur = A OU (B ET C)`

Avec A = 0, B = 1 et C = 1, quelle est la valeur de Moteur ?

A. 0  
B. 1  
C. 2  
D. Impossible à déterminer

**Réponse : B. 1.**

Explication :

- B ET C = 1.
- A OU 1 = 1.

### Question 5

Expression :

`Pompe = (A OU B) ET C`

Avec A = 1, B = 0 et C = 0, quelle est la valeur de Pompe ?

A. 0  
B. 1  
C. 2  
D. Impossible à déterminer

**Réponse : A. 0.**

Explication :

- A OU B = 1.
- 1 ET C = 0.

### Question 6

En LAD, quelle disposition permet de réaliser une opération ET ?

A. Contacts en parallèle.  
B. Contacts en série.  
C. Contacts sans liaison.  
D. Une bobine suivie d'un contact.

**Réponse : B. Contacts en série.**

Explication : tous les contacts doivent être passants pour que le résultat soit vrai.

### Question 7

Une pompe doit fonctionner si le mode automatique OU manuel est actif, si le niveau est suffisant et si aucun défaut thermique n'est présent.

Quelle expression est correcte ?

A. `Pompe = (Auto ET Manuel) OU Niveau_OK OU Defaut_Thermique`

B. `Pompe = (Auto OU Manuel) ET Niveau_OK ET NON Defaut_Thermique`

C. `Pompe = Auto OU (Manuel ET Niveau_OK ET Defaut_Thermique)`

D. `Pompe = NON Auto ET NON Manuel ET Niveau_OK`

**Réponse : B.**

Explication : au moins un mode doit être actif, le niveau doit être suffisant et le défaut thermique doit être absent.

### Résultat du QCM

Score obtenu : 7/7.

Toutes les questions ont été correctement traitées.

## 12. Exercices d'application corrigés

### Exercice 1 – Commande d'un moteur

Un moteur doit démarrer lorsque :

- BP_Marche = 1.
- Autorisation = 1.
- Defaut = 0.

Expression correcte :

`Moteur = BP_Marche ET Autorisation ET NON Defaut`

Réponse retenue : B.

Correction : les deux autorisations doivent être actives et le défaut doit être absent.

Résultat : correct.

### Exercice 2 – Ventilation industrielle

Un ventilateur doit fonctionner si la température est élevée OU si la commande manuelle est activée, mais il doit rester arrêté lorsqu'un défaut électrique est présent.

Expression correcte :

`Ventilateur = (Temperature_Haute OU Marche_Manuelle) ET NON Defaut_Electrique`

Réponse retenue : A.

Correction : la commande automatique ou manuelle est autorisée uniquement en l'absence de défaut électrique.

Résultat : correct.

### Exercice 3 – Station de pompage

Une pompe doit fonctionner lorsque :

- Le niveau est bas.
- La pompe est disponible.
- Aucun défaut thermique n'est présent.
- L'arrêt manuel n'est pas activé.

Expression correcte :

`Pompe = Niveau_Bas ET Pompe_Disponible ET NON Defaut_Thermique ET NON Arret_Manuel`

Réponse retenue : B.

Correction : les quatre conditions doivent être réunies.

Résultat : correct.

### Exercice 4 – Analyse d'une expression

Expression :

`Moteur = (A OU B) ET NON C`

Données :

- A = 0
- B = 1
- C = 1

Calcul :

1. A OU B = 0 OU 1 = 1.
2. NON C = NON 1 = 0.
3. Moteur = 1 ET 0 = 0.

Réponse : A.

Résultat : correct.

### Exercice 5 – Application combinée

Expression :

`Pompe = (Mode_Auto OU Mode_Manuel) ET Pression_OK ET Niveau_OK ET NON Defaut_Electrique`

Données :

| Variable | Valeur |
|---|---|
| Mode_Auto | 0 |
| Mode_Manuel | 1 |
| Pression_OK | 1 |
| Niveau_OK | 1 |
| Defaut_Electrique | 0 |

Calcul :

1. Mode_Auto OU Mode_Manuel = 0 OU 1 = 1.
2. Pression_OK = 1.
3. Niveau_OK = 1.
4. NON Defaut_Electrique = NON 0 = 1.
5. Résultat final = 1.

Réponse : B.

Résultat : correct.

### Exercice 6 – Traduction en LAD

Expression :

`Moteur = (A ET B) OU C`

La représentation correcte comporte :

- Une première branche avec A et B en série.
- Une deuxième branche avec C.
- Les deux branches en parallèle.

Réponse : A.

Résultat : correct.

### Exercice 7 – Analyse d'un automatisme industriel

Expression :

`Pompe = (Auto OU Manuel) ET Temperature_Haute ET NON Defaut_Moteur`

Données :

| Variable | Valeur |
|---|---|
| Auto | 1 |
| Manuel | 0 |
| Temperature_Haute | 1 |
| Defaut_Moteur | 0 |

Calcul :

1. Auto OU Manuel = 1 OU 0 = 1.
2. Temperature_Haute = 1.
3. NON Defaut_Moteur = NON 0 = 1.
4. Pompe = 1 ET 1 ET 1 = 1.

Réponse : B.

Résultat : correct.

### Résultat global des exercices

Score obtenu : 7/7.

Les sept exercices ont été correctement résolus.

## 13. Compte rendu des manipulations pratiques

L'apprenant a confirmé avoir déjà réalisé les manipulations suivantes dans TIA Portal V17 :

| Manipulation | État |
|---|---|
| Construction d'un réseau avec plusieurs contacts en série | Réalisée |
| Construction d'un réseau avec plusieurs contacts en parallèle | Réalisée |
| Combinaison de branches parallèles et de contacts en série | Réalisée |
| Utilisation de contacts NF pour représenter une négation logique | Réalisée |
| Compilation du programme | Réalisée |
| Vérification des réseaux LAD | Réalisée |

Aucune répétition des manipulations n'est nécessaire à ce stade.

## 14. Synthèse générale

Les trois opérations logiques fondamentales étudiées sont :

| Opération | Signification | Représentation LAD |
|---|---|---|
| ET (AND) | Toutes les conditions doivent être vraies | Contacts en série |
| OU (OR) | Au moins une condition doit être vraie | Contacts en parallèle |
| NON (NOT) | Inversion de l'état logique | Contact NF |

Les autres notions importantes sont :

- Une variable booléenne possède deux états : 0 et 1.
- Les tables de vérité permettent de prévoir le résultat d'une opération logique.
- Les parenthèses permettent de contrôler l'ordre des opérations.
- Plusieurs opérations peuvent être combinées dans une même expression.
- Les réseaux LAD traduisent graphiquement les expressions booléennes.

### Résultats de l'évaluation

| Évaluation | Score |
|---|---|
| QCM | 7/7 |
| Exercices | 7/7 |
| Pratique TIA Portal | Déjà réalisée et confirmée |

## 15. Conclusion et transition

Cette leçon a permis d'étudier les opérations logiques fondamentales utilisées dans la programmation des automates industriels.

La maîtrise de ces opérations est indispensable pour construire des programmes LAD fiables et comprendre le fonctionnement des automatismes industriels.

La prochaine leçon portera sur :

**LEÇON 3 – Programmation en LAD : contacts, bobines et organisation des réseaux.**

Nous approfondirons notamment les types de contacts et de bobines, les affectations, les réseaux de programmation et les règles de construction d'un programme LAD structuré.
