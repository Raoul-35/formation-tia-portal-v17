# MODULE 04 — PROGRAMMATION DES AUTOMATES ET LANGAGES IEC 61131-3

# LEÇON 7 — DÉCOUVERTE DES AUTRES LANGAGES IEC 61131-3 : ST, SFC ET IL

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Définir les langages ST, SFC et IL.
- Comprendre leurs principes de fonctionnement.
- Identifier les domaines d'utilisation de chaque langage.
- Comprendre la relation entre ST et SCL.
- Comprendre le fonctionnement d'une programmation séquentielle.
- Identifier le rôle de GRAPH dans TIA Portal.
- Comparer les différents langages IEC 61131-3.
- Choisir un langage adapté à une application industrielle.

## 2. Présentation de la norme IEC 61131-3

La norme IEC 61131-3 définit les langages de programmation utilisés pour les automates programmables industriels.

Les principaux langages étudiés dans notre formation sont :

| Langage | Désignation | Type |
|---|---|---|
| LD / LAD | Ladder Diagram | Graphique |
| FBD | Function Block Diagram | Graphique |
| ST | Structured Text | Textuel |
| SFC | Sequential Function Chart | Graphique séquentiel |
| IL | Instruction List | Textuel historique |

**Remarque importante :** IL est un langage historique retiré des éditions récentes de la norme. Il reste intéressant à connaître pour comprendre et maintenir d'anciennes installations.

Dans l'environnement Siemens, nous utilisons notamment SCL, un langage textuel fondé sur les principes du Structured Text.

## 3. Le langage ST — Structured Text

### 3.1. Définition

Structured Text (ST) est un langage de programmation textuel normalisé par IEC 61131-3.

Il permet d'écrire des instructions sous forme de texte, contrairement à LAD et FBD qui utilisent principalement des représentations graphiques.

Il est particulièrement adapté :

- Aux calculs mathématiques.
- Aux traitements numériques.
- Aux conditions complexes.
- Aux boucles.
- Aux tableaux.
- Aux algorithmes.

### 3.2. Exemple : surveillance de température

Objectif : déclencher une alarme lorsque la température dépasse 80 °C.

```text
IF Temperature > 80.0 THEN
    Alarme := TRUE;
ELSE
    Alarme := FALSE;
END_IF;
```

Explication :

- IF : commence le test d'une condition.
- THEN : introduit les instructions à exécuter si la condition est vraie.
- ELSE : introduit les instructions alternatives.
- END_IF : termine la structure conditionnelle.
- `:=` : opérateur d'affectation.

### 3.3. Les structures conditionnelles

**Structure IF :**

```text
IF Condition THEN
    Instruction;
END_IF;
```

**Structure IF / ELSE :**

```text
IF Condition THEN
    Instruction_1;
ELSE
    Instruction_2;
END_IF;
```

**Structure IF / ELSIF / ELSE :**

```text
IF Temperature > 80.0 THEN
    Niveau_Alarme := 2;
ELSIF Temperature > 60.0 THEN
    Niveau_Alarme := 1;
ELSE
    Niveau_Alarme := 0;
END_IF;
```

Cette structure permet d'examiner plusieurs conditions successivement.

### 3.4. ST et SCL : quelle différence ?

ST est un langage normalisé par IEC 61131-3.

SCL est l'implémentation Siemens fondée sur les principes du Structured Text.

| ST | SCL |
|---|---|
| Langage normalisé IEC | Langage Siemens |
| Définit des principes et constructions normalisés | Adaptation à l'environnement Siemens |
| Utilisé sur différentes plateformes | Utilisé notamment dans TIA Portal |
| Syntaxe dépendant des spécifications de la norme | Certaines particularités dépendent de Siemens |

Les deux langages sont très proches dans leurs principes.

## 4. Le langage SFC — Sequential Function Chart

### 4.1. Définition

SFC est un langage graphique permettant de programmer des automatismes séquentiels.

Un automatisme séquentiel est un système qui réalise plusieurs opérations dans un ordre déterminé.

Exemples industriels :

- Remplissage automatique d'une cuve.
- Station de pompage.
- Ligne de conditionnement.
- Cycle de lavage industriel.
- Système de transfert de produits.

### 4.2. Les trois éléments fondamentaux

Un programme SFC repose principalement sur trois éléments.

**A. Les étapes (Steps)**

Une étape représente une phase du processus.

Exemples :

- Attente.
- Ouverture d'une vanne.
- Démarrage d'une pompe.
- Fonctionnement normal.
- Arrêt.

Une étape initiale indique le point de départ de la séquence.

**B. Les transitions**

Une transition est une condition logique permettant de passer d'une étape à une autre.

Exemple :

`Pression_Atteinte = TRUE`

Lorsque cette condition devient vraie, la transition correspondante autorise le passage à l'étape suivante.

**C. Les actions**

Les actions représentent les opérations exécutées pendant une étape.

Exemples :

- Activer une électrovanne.
- Commander une pompe.
- Allumer un voyant.
- Autoriser une opération.

### 4.3. Exemple industriel : station de pompage

Nous considérons une installation composée :

- D'une pompe.
- D'une vanne motorisée.
- D'un capteur de pression.
- D'une commande de demande de pompage.

Séquence :

```text
          ┌─────────────────────┐
          │ E0 : ATTENTE        │
          │ Pompe = ARRÊT       │
          └──────────┬──────────┘
                     │
                     │ Demande_Pompage
                     ▼
          ┌─────────────────────┐
          │ E1 : OUVERTURE      │
          │ Vanne = OUVERTE     │
          └──────────┬──────────┘
                     │
                     │ Vanne_Ouverte
                     ▼
          ┌─────────────────────┐
          │ E2 : POMPAGE        │
          │ Pompe = MARCHE      │
          └──────────┬──────────┘
                     │
                     │ Pression_Atteinte
                     ▼
          ┌─────────────────────┐
          │ E3 : ARRÊT          │
          │ Pompe = ARRÊT       │
          │ Vanne = FERMÉE      │
          └──────────┬──────────┘
                     │
                     │ Fin_Operation
                     ▼
                    E0
```

### 4.4. Fonctionnement de la séquence

| Étape | Action | Condition de passage |
|---|---|---|
| E0 | Attente d'une demande | Demande_Pompage |
| E1 | Ouverture de la vanne | Vanne_Ouverte |
| E2 | Démarrage et fonctionnement de la pompe | Pression_Atteinte |
| E3 | Arrêt de la pompe et fermeture de la vanne | Fin_Operation |

La séquence suit un ordre précis.

Une transition vraie autorise le passage vers l'étape suivante définie dans le graphe.

### 4.5. Les branches parallèles

Certaines installations doivent effectuer plusieurs opérations simultanément.

Exemple :

Lorsqu'une machine démarre, elle peut devoir :

- Activer un ventilateur.
- Alimenter un convoyeur.
- Activer un système de lubrification.

Une représentation séquentielle peut comporter des branches parallèles afin de gérer plusieurs activités simultanées.

### 4.6. SFC et GRAPH dans TIA Portal

GRAPH est un outil Siemens destiné à la programmation séquentielle.

Il permet notamment de travailler avec :

- Des étapes.
- Des transitions.
- Des actions.
- Des séquences.
- Des branches alternatives ou parallèles.

GRAPH est apparenté aux principes des graphes de séquence, mais il ne faut pas assimiler automatiquement l'outil Siemens GRAPH à l'ensemble du langage normalisé SFC.

## 5. Le langage IL — Instruction List

### 5.1. Définition

Instruction List (IL) est un ancien langage textuel de programmation des automates.

Il utilise des instructions courtes pour effectuer des opérations logiques et arithmétiques.

Il est aujourd'hui principalement étudié pour comprendre les anciens programmes industriels.

### 5.2. Exemple de programme IL

```text
LD Marche
AND Autorisation
ST Moteur
```

Explication :

| Instruction | Signification |
|---|---|
| LD | Charge une variable |
| AND | Effectue un ET logique |
| OR | Effectue un OU logique |
| NOT | Effectue une négation |
| ST | Affecte le résultat à une variable |
| ADD | Addition |
| SUB | Soustraction |
| MUL | Multiplication |
| DIV | Division |

### 5.3. Analyse de l'exemple

Nous avons :

- Marche = TRUE.
- Autorisation = FALSE.

Le programme réalise :

`TRUE AND FALSE = FALSE`

L'instruction ST affecte ensuite ce résultat à Moteur.

Résultat :

`Moteur = FALSE`

Le moteur ne reçoit donc pas de commande de fonctionnement.

### 5.4. Situation actuelle du langage IL

IL est un langage historique retiré des éditions récentes de la norme IEC 61131-3.

Il est important de savoir le reconnaître lorsqu'on intervient sur d'anciens automates.

Il ne faut pas le considérer comme un langage actuel à programmer dans TIA Portal V17.

## 6. Comparaison des langages IEC 61131-3

| Critère | LAD | FBD | ST / SCL | SFC / GRAPH | IL |
|---|---|---|---|---|---|
| Représentation | Graphique | Graphique | Textuelle | Séquentielle graphique | Textuelle |
| Logique à contacts | Très adapté | Possible | Possible | Non destiné à cela directement | Possible |
| Calculs complexes | Peu pratique | Possible | Très adapté | Peu adapté | Limité |
| Séquences | Possible | Possible | Possible | Très adapté | Possible |
| Lisibilité | Bonne pour les électriciens | Bonne pour les blocs | Bonne pour les algorithmes | Très bonne pour les séquences | Plus difficile |
| Situation actuelle | Utilisé | Utilisé | Utilisé | Utilisé selon plateforme | Historique |

## 7. Choisir le langage selon l'application industrielle

Le choix du langage dépend du problème à résoudre.

| Application | Langage conseillé |
|---|---|
| Commande Marche/Arrêt d'un moteur | LAD |
| Association de conditions logiques | LAD ou FBD |
| Calcul de puissance électrique | SCL/ST |
| Calcul de consommation énergétique | SCL/ST |
| Traitement de tableaux de mesures | SCL/ST |
| Cycle automatique de remplissage | GRAPH/SFC |
| Séquence de conditionnement | GRAPH/SFC |
| Lecture d'un ancien programme | IL |

Un automaticien peut utiliser plusieurs langages dans un même projet.

Il est donc important de maîtriser leurs principes et leurs domaines d'utilisation.

## 8. Synthèse générale du Module 04 à ce stade

Nous avons étudié les principaux langages de programmation des automates.

| Leçon | Sujet |
|---|---|
| 1 | Introduction à la programmation en LAD |
| 2 | Opérations logiques et tables de vérité |
| 3 | Contacts, bobines et organisation des réseaux |
| 4 | Auto-maintien et mémorisation des commandes |
| 5 | Introduction à la programmation en FBD |
| 6 | Introduction à la programmation en SCL |
| 7 | Découverte des autres langages IEC 61131-3 : ST, SFC et IL |

## 9. QCM corrigé

**Question 1 :** Quel langage est adapté à une séquence graphique avec étapes et transitions ?

Réponse : SFC.

Explication : SFC permet de représenter les étapes d'un processus et les conditions de passage entre ces étapes.

**Question 2 :** Quelle est la relation entre ST et SCL ?

Réponse : SCL est le langage Siemens fondé sur les principes de ST.

**Question 3 :** Que représente une étape SFC ?

Réponse : Une phase ou un état du processus.

**Question 4 :** Quelle est la situation du langage IL ?

Réponse : C'est un langage historique retiré des éditions récentes de la norme.

**Question 5 :** Quel outil Siemens est destiné à la programmation séquentielle ?

Réponse : GRAPH.

**Question 6 :** Quelle structure permet d'examiner plusieurs conditions en ST ?

Réponse : IF / THEN / ELSIF / ELSE.

**Question 7 :** Que représente une transition SFC ?

Réponse : Une condition permettant de passer d'une étape à une autre.

## 10. Exercices d'application corrigés

### Exercice 1 : Choix des langages

Associer les besoins industriels aux langages adaptés.

| Opération | Langage |
|---|---|
| Commande moteur avec deux conditions | LAD |
| Calcul de puissance électrique | SCL/ST |
| Séquence de pompage | GRAPH/SFC |

### Exercice 2 : Interprétation d'une séquence

Données :

- E0 : Attente.
- E1 : Démarrage de la pompe.
- E2 : Fonctionnement normal.
- T1 : Pression atteinte.

Question : que se passe-t-il lorsque T1 devient vraie pendant E1 ?

Réponse : le système passe à E2.

Explication : la transition vraie autorise le passage vers l'étape suivante.

### Exercice 3 : Analyse d'un programme ST

```text
IF Temperature > 80.0 THEN
    Alarme := TRUE;
ELSE
    Alarme := FALSE;
END_IF;
```

Donnée : Temperature = 85 °C.

Réponse : Alarme = TRUE.

Explication : 85 est supérieur à 80.

### Exercice 4 : Analyse d'un programme IL

```text
LD Marche
AND Autorisation
ST Moteur
```

Données :

- Marche = TRUE.
- Autorisation = FALSE.

Réponse : Moteur = FALSE.

Explication : le résultat de l'opération ET est faux.

### Exercice 5 : Choix du langage en maintenance

| Besoin | Langage |
|---|---|
| Commandes Marche/Arrêt | LAD |
| Calculs électriques | SCL/ST |
| Séquence automatique de remplissage | GRAPH/SFC |
| Maintenance d'un ancien programme | IL |

## 11. Compte rendu des manipulations pratiques

Les manipulations pratiques suivantes ont déjà été réalisées dans TIA Portal V17 :

- Découverte de l'environnement GRAPH.
- Création d'une séquence.
- Création des étapes.
- Configuration des transitions.
- Association des actions aux étapes.
- Vérification et compilation.

Aucune répétition des manipulations n'est nécessaire pour cette leçon.

## 12. Conclusion

Cette leçon nous a permis d'élargir notre connaissance des langages de programmation des automates.

Nous savons désormais que :

- LAD est particulièrement adapté aux commandes logiques à contacts.
- FBD permet de programmer à partir de blocs fonctionnels.
- ST et SCL sont adaptés aux calculs et aux traitements complexes.
- SFC permet de représenter les séquences industrielles.
- GRAPH constitue un outil Siemens de programmation séquentielle.
- IL est un langage historique utile pour comprendre les anciennes installations.

La maîtrise de ces différents langages nous permettra de choisir une méthode de programmation adaptée aux exigences de chaque application industrielle.
