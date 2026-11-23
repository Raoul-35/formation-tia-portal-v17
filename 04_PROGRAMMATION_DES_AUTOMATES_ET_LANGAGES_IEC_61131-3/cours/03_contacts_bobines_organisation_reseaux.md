# MODULE 04 — PROGRAMMATION DES AUTOMATES ET LANGAGES IEC 61131-3

# LEÇON 3 — PROGRAMMATION EN LAD : CONTACTS, BOBINES ET ORGANISATION DES RÉSEAUX

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Identifier et utiliser les différents types de contacts LAD.
- Distinguer les bobines classiques, SET et RESET.
- Comprendre le fonctionnement des réseaux LAD.
- Comprendre l'influence de l'ordre d'exécution des réseaux.
- Organiser un programme LAD de manière structurée.
- Traduire une situation industrielle en logique LAD.
- Identifier les erreurs courantes de programmation.

## 2. Rappel sur la structure d'un réseau LAD

Le LAD (Ladder Diagram) est un langage graphique de programmation des automates industriels.

Un réseau LAD représente une opération logique à partir de contacts, de connexions et de bobines.

Les principaux éléments sont :

| Élément | Symbole | Fonction |
|---|---|---|
| Rail gauche | `│` | Point de départ du circuit logique |
| Contact NO | `—| |—` | Vérifie si une variable vaut 1 |
| Contact NF | `—|/|—` | Vérifie si une variable vaut 0 |
| Bobine classique | `—( )—` | Affecte le résultat logique à une variable |
| Bobine SET | `—(S)—` | Met une variable à 1 et mémorise cet état |
| Bobine RESET | `—(R)—` | Remet une variable à 0 |

### 2.1. Exemple simple

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

Interprétation :

- `I0.0` représente une entrée automate.
- `Q0.0` représente une sortie automate.
- Lorsque `I0.0 = 1`, `Q0.0` devient égal à 1.
- Lorsque `I0.0 = 0`, `Q0.0` devient égal à 0.

Expression logique :

`Q0.0 = I0.0`

## 3. Les différents types de contacts

### 3.1. Contact normalement ouvert (NO)

Symbole :

```text
────| |────
```

Le contact NO est passant lorsque la variable associée vaut 1.

| Variable | État logique du contact | Résultat |
|---:|---|---:|
| 0 | Non passant | 0 |
| 1 | Passant | 1 |

Exemple :

Un bouton Marche est raccordé à l'entrée `I0.0`.

Lorsque le bouton est actionné et que l'entrée vaut 1, le contact LAD NO devient passant.

### 3.2. Contact normalement fermé (NF)

Symbole :

```text
────|/|────
```

Le contact NF est passant lorsque la variable associée vaut 0.

| Variable | État logique du contact | Résultat |
|---:|---|---:|
| 0 | Passant | 1 |
| 1 | Non passant | 0 |

Il réalise une négation logique.

Expression :

`NON I0.1`

Exemple :

Une variable `Defaut_Thermique` vaut 0 en absence de défaut et 1 en présence de défaut.

Un contact NF associé à cette variable laisse passer la logique uniquement lorsque le défaut est absent.

### 3.3. Différence entre contact électrique et contact LAD

Il ne faut pas confondre :

- Le contact physique d'un bouton ou d'un relais.
- Le contact logique utilisé dans le programme LAD.

Un contact NF dans le programme LAD réalise une négation de la variable associée. Il n'indique pas nécessairement que le dispositif physique possède un contact normalement fermé.

Cette distinction est fondamentale pour éviter les erreurs d'interprétation.

## 4. Les différents types de bobines

### 4.1. Bobine classique

Symbole :

```text
────( )────
```

La bobine classique affecte à une variable le résultat logique du réseau.

Exemple :

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

Table de fonctionnement :

| I0.0 | Q0.0 |
|---:|---:|
| 0 | 0 |
| 1 | 1 |

Lorsque la condition logique devient fausse, la variable commandée par cette bobine classique repasse à 0.

La bobine classique ne mémorise donc pas un ordre de fonctionnement.

### 4.2. Bobine SET

Symbole :

```text
────(S)────
```

La bobine SET permet de mettre une variable à 1 et de conserver cet état après la disparition de la condition de commande.

Exemple :

```text
       I0.0          M0.0
  ─────| |───────────(S)────
```

Fonctionnement :

1. Lorsque `I0.0 = 1`, `M0.0` passe à 1.
2. Lorsque `I0.0` repasse à 0, `M0.0` reste à 1.
3. La variable reste mémorisée jusqu'à une commande de réinitialisation ou une autre modification appropriée.

### 4.3. Bobine RESET

Symbole :

```text
────(R)────
```

La bobine RESET permet de remettre une variable à 0.

Exemple :

```text
       I0.1          M0.0
  ─────| |───────────(R)────
```

Lorsque `I0.1 = 1`, la variable `M0.0` est remise à 0.

### 4.4. Comparaison des bobines

| Type de bobine | Fonction | Conservation de l'état |
|---|---|---|
| Classique `( )` | Affectation directe | Non |
| SET `(S)` | Mise à 1 | Oui |
| RESET `(R)` | Mise à 0 | Oui |

### 4.5. Exemple de mémorisation d'une commande

Considérons :

- `I0.0` : bouton Marche.
- `I0.1` : bouton Arrêt.
- `M0.0` : mémoire de marche.

Réseau de mémorisation :

```text
       I0.0          M0.0
  ─────| |───────────(S)────
```

Réseau de réinitialisation :

```text
       I0.1          M0.0
  ─────| |───────────(R)────
```

Le bouton Marche active la mémoire. Le bouton Arrêt la remet à zéro.

**Précaution :** si SET et RESET sont activés simultanément, le résultat dépend de l'organisation et de l'ordre d'exécution des instructions. Il faut donc définir explicitement la priorité souhaitée.

Dans une installation réelle, la commande doit aussi intégrer les conditions d'autorisation, les défauts et les dispositifs de sécurité appropriés.

## 5. Organisation d'un programme en réseaux LAD

Un programme LAD peut être composé de plusieurs réseaux.

Chaque réseau représente une fonction logique ou une partie clairement identifiable du fonctionnement de l'installation.

### Exemple : installation avec trois fonctions

Nous disposons de :

| Adresse | Nom | Fonction |
|---|---|---|
| I0.0 | BP_Voyant | Commande du voyant |
| I0.1 | BP_Ventilateur | Commande du ventilateur |
| I0.2 | Defaut_Thermique | Détection du défaut |
| Q0.0 | Voyant_Vert | Signalisation de marche |
| Q0.1 | Ventilateur | Commande du ventilateur |
| Q0.2 | Voyant_Rouge | Signalisation du défaut |

**Réseau 1 : commande du voyant vert**

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

**Réseau 2 : commande du ventilateur**

```text
       I0.1          Q0.1
  ─────| |───────────( )────
```

**Réseau 3 : signalisation du défaut**

```text
       I0.2          Q0.2
  ─────| |───────────( )────
```

Cette organisation permet de distinguer les fonctions de l'installation.

### 5.1. Bonnes pratiques

- Donner un titre explicite à chaque réseau.
- Utiliser des noms de variables compréhensibles.
- Séparer les fonctions logiques différentes.
- Éviter les affectations multiples inutiles sur une même sortie.
- Vérifier les conditions de fonctionnement.
- Compiler régulièrement le programme.

## 6. Exécution des réseaux LAD

Dans un programme cyclique du S7-1200, l'OB1 est généralement utilisé pour exécuter les instructions principales de l'automate.

Les réseaux d'un bloc sont traités dans leur ordre de programmation.

Exemple :

- Réseau 1.
- Réseau 2.
- Réseau 3.
- Réseau 4.

L'ordre d'exécution est important lorsqu'une même variable est modifiée plusieurs fois.

### Exemple de conflit d'affectation

Supposons que deux réseaux écrivent sur `Q0.0`.

Réseau 1 :

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

Réseau 2 :

```text
       I0.1          Q0.0
  ─────| |───────────(R)────
```

Si plusieurs instructions modifient la même variable pendant un cycle, le résultat final peut dépendre de l'ordre d'exécution.

Il faut donc organiser les affectations pour éviter les conflits.

**Bonne pratique :** privilégier une affectation principale clairement identifiable pour chaque sortie physique.

## 7. Applications industrielles

### 7.1. Commande d'un voyant

Objectif : allumer un voyant lorsque le bouton Marche est actionné.

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

Expression :

`Q0.0 = I0.0`

### 7.2. Signalisation d'un défaut thermique

Objectif : allumer un voyant rouge lorsque le défaut est détecté.

```text
       I0.2          Q0.1
  ─────| |───────────( )────
```

Expression :

`Q0.1 = I0.2`

### 7.3. Autorisation d'une commande moteur

Conditions :

- Autorisation présente.
- Aucun défaut thermique.

```text
 I0.0       I0.1          Q0.0
──| |────────|/|──────────( )────
```

Expression :

`Q0.0 = I0.0 ET NON I0.1`

### 7.4. Commande d'une pompe

Conditions :

- Bouton Marche actionné.
- Niveau bas détecté.
- Aucun défaut thermique.

```text
 I0.0       I0.1       I0.2          Q0.0
──| |────────| |────────|/|──────────( )──
Marche     Niveau bas  Défaut       Pompe
```

Expression :

`Q0.0 = I0.0 ET I0.1 ET NON I0.2`

Cette expression représente une commande combinatoire directe. Elle ne mémorise pas le démarrage de la pompe.

## 8. Erreurs courantes de programmation

| Erreur | Conséquence | Bonne pratique |
|---|---|---|
| Confondre contact physique NF et contact LAD NF | Mauvaise interprétation logique | Distinguer câblage physique et logique programme |
| Utiliser SET sans prévoir RESET | Variable qui reste activée | Prévoir une condition de réinitialisation |
| Affecter plusieurs fois une sortie | Résultat difficile à maîtriser | Organiser les affectations |
| Mélanger plusieurs fonctions dans un réseau | Programme difficile à lire | Séparer les fonctions |
| Oublier la compilation | Erreurs non détectées | Compiler régulièrement |
| Ne pas commenter les réseaux | Maintenance plus difficile | Ajouter des titres explicites |

## 9. QCM avec corrections

### Question 1

Dans un programme LAD, que fait une bobine classique `( )` lorsque le résultat logique du réseau passe de 1 à 0 ?

**Réponse : B — Elle remet la variable associée à 0.**

Explication : la bobine classique suit directement le résultat logique du réseau.

### Question 2

Quelle est la fonction principale d'une bobine SET `(S)` ?

**Réponse : C — Mettre la variable à 1 et conserver cet état.**

Explication : SET mémorise l'état logique à 1 jusqu'à une réinitialisation ou une autre modification appropriée.

### Question 3

Dans un programme LAD, que fait une bobine RESET `(R)` ?

**Réponse : B — Elle remet la variable à 0.**

Explication : RESET réinitialise la variable associée.

### Question 4

Pourquoi est-il déconseillé d'affecter une même sortie physique dans plusieurs réseaux LAD sans raison précise ?

**Réponse : C — Parce que les différentes affectations peuvent se contredire et que l'ordre d'exécution influence le résultat.**

Explication : une affectation exécutée plus tard peut modifier le résultat produit précédemment.

### Question 5

Un contact NF associé à `M0.0` est passant lorsque :

**Réponse : B — M0.0 = 0.**

Explication : le contact NF LAD réalise la négation logique de la variable.

### Question 6

Quel bloc est généralement utilisé pour organiser le programme cyclique principal du S7-1200 ?

**Réponse : C — OB1.**

Explication : l'OB1 est le bloc d'organisation principal utilisé pour le programme cyclique.

### Question 7

Quelle est la meilleure pratique pour organiser un programme LAD industriel ?

**Réponse : B — Créer des réseaux distincts et clairement identifiés pour chaque fonction logique.**

Explication : cette organisation améliore la lisibilité, le diagnostic et la maintenance.

**Résultat du QCM : 7/7.**

## 10. Exercices d'application corrigés

### Exercice 1 — Commande d'un voyant

Données :

- `I0.0` : bouton Marche.
- `Q0.0` : voyant vert.

Le voyant doit s'allumer lorsque le bouton est actionné et s'éteindre lorsqu'il est relâché.

**Réponse : A — Contact NO suivi d'une bobine classique.**

Schéma :

```text
       I0.0          Q0.0
  ─────| |───────────( )────
```

Expression :

`Q0.0 = I0.0`

### Exercice 2 — Signalisation d'un défaut

Données :

- `I0.2` : défaut thermique.
- `Q0.1` : voyant rouge.

Le voyant doit s'allumer lorsque le défaut est présent.

**Réponse : B — Contact NO suivi d'une bobine classique.**

Schéma :

```text
       I0.2          Q0.1
  ─────| |───────────( )────
```

Expression :

`Q0.1 = I0.2`

### Exercice 3 — Mémorisation d'une commande

Données :

- `I0.0` : bouton Marche.
- `M0.0` : mémoire de marche.

La commande doit rester mémorisée après le relâchement du bouton.

**Réponse : C — Bobine SET `(S)`.**

Schéma :

```text
       I0.0          M0.0
  ─────| |───────────(S)────
```

### Exercice 4 — Réinitialisation d'une commande

Données :

- `I0.1` : bouton Arrêt.
- `M0.0` : mémoire de marche.

Le bouton Arrêt doit remettre la mémoire à 0.

**Réponse : A — Contact NO suivi d'une bobine RESET `(R)`.**

Schéma :

```text
       I0.1          M0.0
  ─────| |───────────(R)────
```

### Exercice 5 — Analyse d'un programme LAD

Réseau :

```text
       I0.0       I0.1          Q0.0
  ─────| |────────|/|───────────( )────
```

Données :

- `I0.0` : Autorisation.
- `I0.1` : Défaut.
- `Q0.0` : Commande moteur.

**Réponse : B — L'autorisation vaut 1 et le défaut vaut 0.**

Expression :

`Q0.0 = I0.0 ET NON I0.1`

Table de vérité :

| I0.0 | I0.1 | Q0.0 |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Exercice 6 — Combinaison de conditions

Une pompe doit fonctionner lorsque :

- Le bouton Marche `I0.0` est actionné.
- Le niveau bas `I0.1` est détecté.
- Aucun défaut thermique `I0.2` n'est présent.

**Réponse : C**

`Q0.0 = I0.0 ET I0.1 ET NON I0.2`

Schéma :

```text
 I0.0       I0.1       I0.2          Q0.0
──| |────────| |────────|/|──────────( )──
```

Les trois conditions doivent être réunies.

### Exercice 7 — Organisation des réseaux

Une installation comprend :

- Un moteur commandé par `Q0.0`.
- Un voyant de marche commandé par `Q0.1`.
- Un voyant de défaut commandé par `Q0.2`.

**Réponse : B — Utiliser un réseau pour chaque fonction logique avec des commentaires explicites.**

Explication : cette organisation facilite la compréhension et la maintenance du programme.

**Résultat des exercices : 6/7.**

La difficulté rencontrée à l'exercice 5 concernait l'interprétation d'un contact NF associé à une variable de défaut.

## 11. Compte rendu des manipulations pratiques

L'utilisateur a confirmé que toutes les manipulations suivantes avaient déjà été réalisées dans TIA Portal V17.

| N° | Manipulation | État |
|---|---|---|
| 1 | Utilisation des contacts NO | Réalisée |
| 2 | Utilisation des contacts NF | Réalisée |
| 3 | Insertion de bobines classiques | Réalisée |
| 4 | Utilisation des bobines SET | Réalisée |
| 5 | Utilisation des bobines RESET | Réalisée |
| 6 | Organisation d'un programme en plusieurs réseaux LAD | Réalisée |

Aucune nouvelle manipulation n'est nécessaire pour cette leçon.

Les travaux pratiques précédemment effectués sont considérés comme validés et ne doivent pas être recommencés.

## 12. Synthèse générale

Les éléments essentiels de cette leçon sont :

1. Un contact NO est passant lorsque la variable associée vaut 1.
2. Un contact NF est passant lorsque la variable associée vaut 0.
3. Une bobine classique suit directement le résultat logique du réseau.
4. Une bobine SET permet de mémoriser un état à 1.
5. Une bobine RESET permet de remettre une variable à 0.
6. Un programme LAD est constitué de réseaux exécutés dans leur ordre de programmation.
7. Une bonne organisation des réseaux facilite le diagnostic et la maintenance.
8. Les affectations multiples d'une même variable doivent être maîtrisées.

### Progression du Module 04

| Leçon | Intitulé | État |
|---|---|---|
| 01 | Introduction à la programmation LAD | Terminée et archivée |
| 02 | Opérations logiques et tables de vérité | Terminée et archivée |
| 03 | Contacts, bobines et organisation des réseaux | Terminée, à archiver |
| 04 | Programmation en FBD | À venir |
| 05 | Programmation en SCL | À venir |
| 06 | Comparaison LAD, FBD et SCL | À venir |
| 07 | Traduction d'un automatisme dans différents langages | À venir |
| 08 | Choix du langage selon l'application industrielle | À venir |

**Fin de la leçon 3.**
