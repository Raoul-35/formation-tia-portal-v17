# Leçon 8 — Adressage des entrées et sorties du S7-1200

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le principe de l'adressage d'un automate.
- Identifier les zones mémoire I, Q, M et DB.
- Lire et interpréter une adresse absolue.
- Comprendre les notions de bit, octet, mot et double mot.
- Distinguer une adresse absolue d'un nom symbolique.
- Comprendre l'intérêt des noms symboliques dans un programme industriel.

## 2. Principe de l'adressage

Un automate doit identifier individuellement les informations qu'il reçoit et les commandes qu'il transmet.

Chaque entrée et sortie possède une adresse permettant au programme de communiquer avec elle.

Exemple d'une station de pompage :

- Un bouton Marche est raccordé à une entrée.
- Un capteur de niveau est raccordé à une autre entrée.
- Un contacteur est commandé par une sortie.
- Un voyant est commandé par une autre sortie.

L'adressage permet d'identifier précisément chaque emplacement.

## 3. Les principales zones d'adressage

### 3.1. Les entrées : %I

La zone I (Input) représente les entrées de l'automate.

Exemples :
- Bouton poussoir.
- Détecteur de proximité.
- Pressostat.
- Contact auxiliaire.
- Capteur de niveau TOR.

Exemple : %I0.0 représente le bit 0 de l'octet 0 de la zone des entrées.

### 3.2. Les sorties : %Q

La zone Q (Output) représente les sorties de l'automate.

Exemples :
- Commande d'un relais.
- Voyant lumineux.
- Électrovanne.
- Commande d'un contacteur.

Exemple : %Q0.0 représente le bit 0 de l'octet 0 de la zone des sorties.

### 3.3. Les mémoires internes : %M

La zone M (Merker) permet de stocker des informations internes utilisées par le programme.

Exemples :
- Autorisation de fonctionnement.
- Mémoire de marche.
- Défaut général.
- Mode automatique actif.

Exemple : %M0.0 représente le bit 0 de l'octet 0 de la zone M.

Certaines données peuvent être rémanentes selon la configuration de la CPU et les paramètres de mémoire.

### 3.4. Les blocs de données : DB

Les Data Blocks permettent d'organiser et de stocker des données utilisées par le programme.

Exemples :
- Seuil de niveau haut.
- Seuil de niveau bas.
- Consigne de pression.
- Temps de fonctionnement d'une pompe.

Les DB peuvent contenir des variables de différents types et permettre une organisation structurée des informations.

## 4. Exemple d'adressage d'une station de pompage

| Équipement | Type | Adresse indicative |
|---|---|---|
| Bouton Marche | Entrée | %I0.0 |
| Bouton Arrêt | Entrée | %I0.1 |
| Capteur niveau haut | Entrée | %I0.2 |
| Contacteur pompe | Sortie | %Q0.0 |
| Voyant marche | Sortie | %Q0.1 |
| Mémoire autorisation | Mémoire interne | %M0.0 |
| Paramètres de pompage | Bloc de données | DB1 |

Les adresses physiques réelles dépendent de la référence de la CPU et de la configuration matérielle.

## 5. Organisation des données : bit, octet, mot et double mot

### 5.1. Le bit

Le bit est la plus petite unité d'information.

Il peut prendre deux valeurs :
- 0 : état logique faux.
- 1 : état logique vrai.

Exemple : %M0.0.

### 5.2. L'octet (BYTE)

Un octet contient 8 bits.

Exemple : %MB0 représente l'octet 0 de la zone mémoire M.

Il regroupe les bits :
- %M0.0
- %M0.1
- %M0.2
- %M0.3
- %M0.4
- %M0.5
- %M0.6
- %M0.7

### 5.3. Le mot (WORD)

Un mot contient 16 bits, soit 2 octets.

Exemple : %MW0 occupe les octets MB0 et MB1.

### 5.4. Le double mot (DWORD)

Un double mot contient 32 bits, soit 4 octets.

Exemple : %MD0 occupe les octets MB0, MB1, MB2 et MB3.

### 5.5. Tableau récapitulatif

| Unité | Taille | Exemple |
|---|---|---|
| Bit | 1 bit | %M0.0 |
| Byte | 8 bits | %MB0 |
| Word | 16 bits | %MW0 |
| Double Word | 32 bits | %MD0 |

Attention : ces adresses peuvent se chevaucher. Il faut éviter d'utiliser simultanément les mêmes octets pour des données indépendantes.

## 6. Adresse absolue et nom symbolique

### 6.1. Adresse absolue

Une adresse absolue désigne directement un emplacement mémoire.

Exemples :
- %I0.0
- %I0.1
- %Q0.0
- %M0.0

### 6.2. Nom symbolique

Un nom symbolique est une désignation explicite attribuée à une variable.

Exemples :

| Adresse absolue | Nom symbolique | Fonction |
|---|---|---|
| %I0.0 | BP_Marche_P1 | Bouton Marche |
| %I0.1 | BP_Arret_P1 | Bouton Arrêt |
| %I0.2 | Capteur_Niveau_Haut | Détection niveau haut |
| %Q0.0 | CMD_P1 | Commande pompe |
| %Q0.1 | Voyant_P1 | Voyant de fonctionnement |
| %M0.0 | Memoire_Autorisation_P1 | Autorisation de marche |

Le nom symbolique ne crée pas une nouvelle entrée ou sortie physique. Il permet simplement d'identifier plus facilement une variable existante.

### 6.3. Intérêt des noms symboliques

Les noms symboliques permettent :
- Une meilleure lisibilité du programme.
- Une maintenance facilitée.
- Un dépannage plus rapide.
- Une meilleure compréhension du fonctionnement de l'installation.
- Une réduction des erreurs de programmation.

## 7. Introduction au type BOOL

BOOL est un type de données logique qui peut prendre deux états :

- TRUE : vrai (1).
- FALSE : faux (0).

Exemples :
- Bouton appuyé ou relâché.
- Pompe autorisée ou interdite.
- Défaut présent ou absent.
- Niveau haut détecté ou non détecté.

Les variables associées aux signaux numériques TOR sont généralement de type BOOL.

## 8. Exemple de logique LAD

Considérons :

- BP_Marche_P1 : bouton Marche.
- CMD_P1 : commande de la pompe.

Une logique élémentaire peut être représentée ainsi :

    |----[ BP_Marche_P1 ]----------------( CMD_P1 )----|

Cet exemple illustre l'utilisation des noms symboliques dans un programme LAD. Une commande industrielle réelle doit intégrer les conditions d'arrêt, les défauts et les sécurités nécessaires.

## 9. Synthèse

| Notion | Signification |
|---|---|
| %I | Entrées |
| %Q | Sorties |
| %M | Mémoires internes |
| %DB | Blocs de données |
| Bit | 1 bit |
| Byte | 8 bits |
| Word | 16 bits |
| Double Word | 32 bits |
| Adresse absolue | Emplacement mémoire |
| Nom symbolique | Désignation explicite |
| BOOL | Variable logique TRUE/FALSE |

## 10. Questions de révision

1. Que représente l'adresse %I0.0 ?
2. Que représente l'adresse %Q0.0 ?
3. À quoi servent les mémoires internes %M ?
4. Quel est le rôle des blocs de données DB ?
5. Combien de bits contient un octet ?
6. Combien de bits contient un mot ?
7. Quels bits composent l'octet %MB2 ?
8. Quels octets sont utilisés par %MW0 ?
9. Quelle est la différence entre une adresse absolue et un nom symbolique ?
10. Pourquoi utiliser des noms symboliques dans un programme industriel ?
