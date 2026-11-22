# MODULE 04 – PROGRAMMATION LAD

# LEÇON 1 – INTRODUCTION À LA PROGRAMMATION EN LANGAGE LAD (LADDER)

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

1. Expliquer le rôle d'un langage de programmation automate.
2. Définir le langage LAD et son principe de fonctionnement.
3. Comprendre la représentation graphique des contacts et des bobines.
4. Différencier les contacts normalement ouverts et normalement fermés.
5. Comprendre les associations logiques ET et OU.
6. Lire et interpréter un réseau LAD simple.
7. Identifier les applications industrielles du langage LAD.
8. Traduire une situation industrielle en une expression logique et en un réseau LAD.

---

## 2. Qu'est-ce que la programmation d'un automate ?

Un automate programmable industriel (API) reçoit des informations provenant de son environnement, les traite selon un programme et commande des équipements.

Prenons l'exemple d'une pompe industrielle.

L'installation possède :

- Un bouton Marche.
- Un bouton Arrêt.
- Un contacteur commandant le moteur.
- Un automate S7-1200.

Lorsque l'opérateur appuie sur Marche, l'automate doit commander le contacteur pour démarrer la pompe, sous réserve que les conditions nécessaires soient satisfaites.

La programmation permet de décrire à l'automate les opérations qu'il doit effectuer.

**Définition :**

La programmation d'un automate consiste à écrire une suite d'instructions logiques et numériques permettant de traiter les informations reçues et de commander les sorties.

### 2.1 Principe général

Une installation automatisée peut être représentée par la chaîne suivante :

```text
CAPTEURS ET COMMANDES
        |
        v
ENTRÉES DE L'AUTOMATE
        |
        v
TRAITEMENT DU PROGRAMME
        |
        v
SORTIES DE L'AUTOMATE
        |
        v
ACTIONNEURS
```

Exemple :

- Un capteur détecte un niveau suffisant.
- L'information arrive sur une entrée de l'automate.
- Le programme vérifie les conditions de fonctionnement.
- L'automate active une sortie.
- La sortie commande le contacteur de la pompe.

---

## 3. Les principaux langages de programmation API

La norme IEC 61131-3 définit plusieurs langages utilisés dans l'automatisation industrielle.

| Langage | Nom complet | Représentation |
|---|---|---|
| LAD | Ladder Diagram | Schéma à contacts |
| FBD | Function Block Diagram | Blocs fonctionnels |
| ST | Structured Text | Texte structuré |
| SFC | Sequential Function Chart | Diagramme fonctionnel séquentiel |
| IL | Instruction List | Liste d'instructions historique |

Remarque : IL est un langage historique, retiré de la norme IEC 61131-3 depuis son édition 3.

### 3.1 Le langage LAD

LAD signifie Ladder Diagram.

Il est généralement traduit par langage à contacts ou langage en échelle.

Il permet de représenter graphiquement les conditions logiques à l'aide de contacts, de bobines et de blocs fonctionnels.

LAD est particulièrement répandu dans l'industrie parce que sa représentation ressemble aux schémas électriques de commande traditionnels.

### 3.2 Comparaison avec un circuit électrique

Dans un circuit électrique traditionnel :

- Un interrupteur représente une condition de commande.
- Une lampe représente un récepteur.
- Les conducteurs assurent la liaison électrique.

Dans un programme LAD :

- Les contacts représentent des conditions logiques.
- Les bobines représentent des commandes ou des affectations de variables.
- Les lignes graphiques représentent les relations logiques entre les éléments.

Attention : le chemin logique représenté dans un réseau LAD ne correspond pas nécessairement à une circulation réelle de courant électrique. Il s'agit d'une représentation logique exécutée par l'automate.

---

## 4. Organisation graphique d'un réseau LAD

Un réseau LAD est généralement représenté entre deux rails verticaux.

```text
       Rail gauche                 Rail droit
           |                           |
           |----[ Contact ]----( Bobine )----|
           |                           |
```

Les éléments fondamentaux sont :

- Les contacts.
- Les bobines.
- Les branches parallèles.
- Les blocs fonctionnels.

Les conditions logiques sont évaluées pour déterminer le résultat du réseau.

---

## 5. Les contacts LAD

Il existe deux représentations fondamentales des contacts.

### 5.1 Contact normalement ouvert (NO)

Symbole :

```text
----| |----
```

Un contact NO transmet la condition logique lorsque la variable associée vaut 1.

Exemple :

```text
Variable : BP_Marche
Adresse  : %I0.0
Type     : Bool
```

Table de fonctionnement :

| BP_Marche | État logique du contact NO |
|---:|---|
| 0 | Faux |
| 1 | Vrai |

Explication :

- Si BP_Marche vaut 0, le contact n'est pas passant logiquement.
- Si BP_Marche vaut 1, le contact est passant logiquement.

### 5.2 Contact normalement fermé (NF)

Symbole :

```text
----|/|----
```

Un contact NF transmet la condition logique lorsque la variable associée vaut 0.

Exemple :

```text
Variable : Defaut_Thermique
Adresse  : %I0.2
Type     : Bool
```

Table de fonctionnement :

| Defaut_Thermique | État logique du contact NF |
|---:|---|
| 0 | Vrai |
| 1 | Faux |

Explication :

- Si Defaut_Thermique vaut 0, le contact est passant logiquement.
- Si Defaut_Thermique vaut 1, le contact est bloqué logiquement.

### 5.3 Différence entre contact physique et contact LAD

Il faut distinguer deux notions :

**Le contact physique :**

C'est un contact électrique réel situé dans un bouton, un relais, un contacteur ou un capteur.

**Le contact LAD :**

C'est une instruction logique qui teste l'état d'une variable dans le programme de l'automate.

Un contact NF dans le programme LAD ne signifie donc pas nécessairement que le bouton physique est un bouton à contact NF.

Le comportement physique dépend du câblage et de l'état électrique transmis à l'entrée de l'automate.

Cette distinction est particulièrement importante pour les boutons d'arrêt, les relais thermiques et les chaînes de sécurité.

---

## 6. Les bobines LAD

La bobine classique est représentée ainsi :

```text
----( )----
```

Elle permet d'affecter le résultat logique du réseau à une variable booléenne.

Exemple :

```text
Variable : Voyant_Vert
Adresse  : %Q0.0
Type     : Bool
```

Table de fonctionnement :

| Résultat logique du réseau | Voyant_Vert |
|---:|---:|
| 0 | 0 |
| 1 | 1 |

Si les conditions logiques du réseau sont vraies, la bobine active la variable de sortie.

Dans une installation réelle, cette sortie peut commander un relais, un contacteur ou un voyant selon le câblage.

Attention : dans un programme, il faut éviter les affectations multiples non maîtrisées d'une même variable de sortie. Plusieurs bobines classiques associées à la même variable peuvent provoquer des comportements inattendus selon l'ordre d'exécution.

---

## 7. Première logique LAD : commande simple

Imaginons une installation dans laquelle un bouton Marche commande directement un voyant.

```text
       BP_Marche                 Voyant_Vert
          |                           |
          |----| |----------------( )--|
```

Équation logique :

Voyant_Vert = BP_Marche

Table de vérité :

| BP_Marche | Voyant_Vert |
|---:|---:|
| 0 | 0 |
| 1 | 1 |

Interprétation :

- Lorsque BP_Marche vaut 0, le voyant reste éteint.
- Lorsque BP_Marche vaut 1, le voyant s'allume.

Ce réseau représente une commande directe, sans fonction d'auto-maintien.

---

## 8. Association de contacts : logique ET

Lorsque plusieurs contacts sont placés en série, nous obtenons une condition logique ET.

Exemple industriel :

Une pompe ne peut démarrer que si :

- L'opérateur appuie sur Marche.
- Le niveau du réservoir est suffisant.

Représentation :

```text
 BP_Marche       Niveau_OK       Pompe
     |               |              |
     |----| |--------| |---------( )-|
```

Équation logique :

Pompe = BP_Marche ET Niveau_OK

Table de vérité :

| BP_Marche | Niveau_OK | Pompe |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Interprétation :

La pompe ne fonctionne que lorsque les deux conditions sont vraies.

**À retenir : contacts en série = logique ET.**

---

## 9. Association de contacts : logique OU

Lorsque plusieurs contacts sont placés en parallèle, nous obtenons une condition logique OU.

Exemple industriel :

Un ventilateur peut être activé par :

- Un ordre automatique.
- Un ordre manuel.

Représentation :

```text
             Ordre_Auto
           +----| |----+
           |           |
-----------+           +--------( Ventilateur )
           |           |
           +----| |----+
             Ordre_Manuel
```

Équation logique :

Ventilateur = Ordre_Auto OU Ordre_Manuel

Table de vérité :

| Ordre_Auto | Ordre_Manuel | Ventilateur |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Interprétation :

Une seule condition vraie suffit pour activer le ventilateur.

**À retenir : contacts en parallèle = logique OU.**

---

## 10. Application industrielle : commande d'une pompe

Considérons une pompe dont le fonctionnement est autorisé lorsque :

- Le bouton Marche est activé.
- Le niveau du réservoir est suffisant.
- Le défaut thermique est absent.

Variables :

| Variable | Adresse | Type |
|---|---|---|
| BP_Marche | %I0.0 | Bool |
| Niveau_OK | %I0.1 | Bool |
| Defaut_Thermique | %I0.2 | Bool |
| Pompe | %Q0.0 | Bool |

Réseau LAD :

```text
 BP_Marche    Niveau_OK    Defaut_Thermique
     |            |              |
     |----| |-----| |------------|/|----( Pompe )
```

Équation logique :

Pompe = BP_Marche ET Niveau_OK ET NON Defaut_Thermique

La pompe est autorisée uniquement si les trois conditions sont satisfaites.

Cette représentation est une commande combinatoire directe. Elle ne réalise pas encore un maintien de la commande après relâchement du bouton Marche.

Nous étudierons ultérieurement l'auto-maintien avec les contacts et les bobines appropriés.

### 10.1 Précision sur la sécurité industrielle

La représentation logique d'un arrêt d'urgence ou d'une protection thermique dans un réseau LAD standard ne constitue pas, à elle seule, une architecture de sécurité certifiée.

Les fonctions de sécurité doivent être conçues avec des équipements et des architectures adaptés : relais de sécurité, automate de sécurité, circuits redondants et dispositifs appropriés selon l'analyse de risques.

---

# 11. QCM – Vérification des connaissances

## Question 1

Que signifie l'abréviation LAD dans TIA Portal ?

A. Logic Automation Device  
B. Ladder Diagram  
C. Logical Analog Diagram  
D. Linear Automation Design  

**Bonne réponse : B. Ladder Diagram**

Explication :

LAD signifie Ladder Diagram, traduit par langage à contacts ou langage en échelle.

Il permet de représenter graphiquement les conditions logiques d'un automate à l'aide de contacts, de bobines et de blocs fonctionnels.

## Question 2

Dans un programme LAD, quel est le rôle d'un contact normalement ouvert (NO) ?

A. Il transmet toujours la condition logique.  
B. Il transmet la condition logique lorsque la variable associée vaut 1.  
C. Il transmet la condition logique lorsque la variable associée vaut 0.  
D. Il commande directement une sortie analogique.

**Bonne réponse : B.**

Explication :

Un contact NO teste l'état de la variable associée. Il devient passant logiquement lorsque cette variable vaut 1.

## Question 3

Dans un programme LAD, que signifie un contact normalement fermé (NF) ?

A. Il transmet la condition logique lorsque la variable associée vaut 1.  
B. Il transmet toujours la condition logique.  
C. Il transmet la condition logique lorsque la variable associée vaut 0.  
D. Il désactive définitivement une sortie.

**Bonne réponse : C.**

Explication :

Un contact NF réalise une négation logique de la variable associée. Il est passant lorsque cette variable vaut 0.

## Question 4

Dans un réseau LAD, deux contacts placés en série représentent quelle opération logique ?

A. OU  
B. NON  
C. ET  
D. XOR

**Bonne réponse : C. ET**

Explication :

Les contacts en série imposent que toutes les conditions soient vraies pour que le résultat logique soit vrai.

## Question 5

Dans un réseau LAD, deux contacts placés en parallèle représentent quelle opération logique ?

A. ET  
B. OU  
C. NON  
D. XOR

**Bonne réponse : B. OU**

Explication :

Les contacts en parallèle permettent d'obtenir un résultat vrai dès qu'au moins une branche est vraie.

## Question 6

Quel est le rôle d'une bobine classique dans un programme LAD ?

A. Lire une entrée analogique.  
B. Affecter le résultat logique du réseau à une variable booléenne.  
C. Mesurer la température d'un moteur.  
D. Convertir une valeur entière en réel.

**Bonne réponse : B.**

Explication :

La bobine classique affecte le résultat logique du réseau à la variable booléenne associée.

## Question 7

Une pompe doit fonctionner uniquement si les trois conditions suivantes sont satisfaites :

- Bouton Marche activé.
- Niveau suffisant.
- Aucun défaut thermique.

Quelle équation logique correspond à cette commande ?

A. Pompe = BP_Marche OU Niveau_OK OU Defaut_Thermique

B. Pompe = BP_Marche ET Niveau_OK ET NON Defaut_Thermique

C. Pompe = BP_Marche ET Niveau_OK OU Defaut_Thermique

D. Pompe = NON BP_Marche ET Niveau_OK ET Defaut_Thermique

**Bonne réponse : B.**

Explication :

La demande de démarrage doit être présente, le niveau doit être suffisant et le défaut thermique doit être absent.

Équation :

Pompe = BP_Marche ET Niveau_OK ET NON Defaut_Thermique

### Résultat du QCM

Score obtenu : 7/7.

Toutes les questions ont reçu une réponse correcte.

---

# 12. Exercices d'application corrigés

## Exercice 1 – Commande d'un voyant

Un voyant vert doit s'allumer uniquement lorsque deux conditions sont satisfaites :

- BP_Marche = 1
- Autorisation = 1

Variables :

| Variable | Adresse |
|---|---|
| BP_Marche | %I0.0 |
| Autorisation | %I0.1 |
| Voyant_Vert | %Q0.0 |

Question :

Quelle association logique faut-il utiliser ?

A. Deux contacts en parallèle (OU).  
B. Deux contacts en série (ET).  
C. Un contact NF en série avec un contact NO.  
D. Une bobine en parallèle avec les deux entrées.

**Bonne réponse : B.**

Correction :

Les deux conditions doivent être vraies simultanément.

Réseau LAD :

```text
 BP_Marche    Autorisation      Voyant_Vert
    |              |                 |
    |----| |-------| |-------------( )-|
```

Équation :

Voyant_Vert = BP_Marche ET Autorisation

Table de vérité :

| BP_Marche | Autorisation | Voyant_Vert |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

## Exercice 2 – Commande d'un ventilateur

Dans une installation industrielle, un ventilateur peut être démarré par deux commandes indépendantes :

- Marche_Auto = 1 : démarrage automatique.
- Marche_Manuel = 1 : démarrage manuel.

Le ventilateur doit fonctionner si au moins une des deux commandes est activée.

Question :

Quelle association logique faut-il utiliser ?

A. Deux contacts en série (ET).  
B. Deux contacts en parallèle (OU).  
C. Deux contacts NF en série.  
D. Une bobine en série avec une autre bobine.

**Bonne réponse : B.**

Correction :

Une seule commande active suffit. Les deux contacts doivent donc être placés en parallèle.

Réseau LAD :

```text
             Marche_Auto
           +----| |----+
           |           |
-----------+           +--------( Ventilateur )
           |           |
           +----| |----+
            Marche_Manuel
```

Équation :

Ventilateur = Marche_Auto OU Marche_Manuel

Table de vérité :

| Marche_Auto | Marche_Manuel | Ventilateur |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

## Exercice 3 – Détection d'un défaut thermique

Un moteur industriel possède un relais thermique.

La variable Defaut_Thermique prend les valeurs suivantes :

- 0 : aucun défaut.
- 1 : défaut thermique détecté.

Un voyant vert Voyant_Marche_Autorisee doit être allumé uniquement lorsqu'aucun défaut thermique n'est présent.

Question :

Quel contact LAD faut-il utiliser ?

A. Un contact NO.  
B. Un contact NF.  
C. Une bobine classique.  
D. Deux contacts NO en parallèle.

**Bonne réponse : B.**

Correction :

La variable vaut 0 lorsque le défaut est absent.

Le contact NF est passant lorsque la variable associée vaut 0.

Réseau LAD :

```text
 Defaut_Thermique       Voyant_Marche_Autorisee
        |                         |
        |---------|/|------------( )---|
```

Équation :

Voyant_Marche_Autorisee = NON Defaut_Thermique

Table de vérité :

| Defaut_Thermique | Contact NF | Voyant |
|---:|---:|---:|
| 0 | Passant | 1 |
| 1 | Bloqué | 0 |

## Exercice 4 – Autorisation de démarrage d'une pompe

Une pompe doit fonctionner lorsque :

- Le bouton Marche est activé.
- Le niveau du réservoir est suffisant.
- Aucun défaut thermique n'est présent.

Question :

Quelle représentation LAD correspond à cette logique ?

**Proposition A :**

```text
 BP_Marche
     |
     +----| |----------------------+
     |                             |
 Niveau_OK                          +----( Pompe )
     |                             |
     +----| |----------------------+
```

**Proposition B :**

```text
 BP_Marche    Niveau_OK    Defaut_Thermique
     |            |              |
     |----| |-----| |------------|/|----( Pompe )
```

**Proposition C :**

```text
 BP_Marche    Niveau_OK    Defaut_Thermique
     |            |              |
     |----|/|-----|/|------------| |----( Pompe )
```

**Bonne réponse : B.**

Correction :

- BP_Marche utilise un contact NO.
- Niveau_OK utilise un contact NO.
- Defaut_Thermique utilise un contact NF.

Équation :

Pompe = BP_Marche ET Niveau_OK ET NON Defaut_Thermique

Les trois conditions doivent être réunies.

## Exercice 5 – Analyse d'une table de vérité

Considérons le réseau LAD suivant :

```text
 BP1          BP2
  |            |
  |----| |-----| |----------------( Moteur )
```

Les deux contacts sont normalement ouverts et placés en série.

Question :

Si BP1 = 1 et BP2 = 0, quel sera l'état de la sortie Moteur ?

A. Moteur = 1  
B. Moteur = 0  
C. Moteur = 1 uniquement pendant une seconde  
D. Impossible à déterminer

**Bonne réponse : B.**

Correction :

Les deux contacts sont placés en série et réalisent une opération ET.

- BP1 = 1 : premier contact passant.
- BP2 = 0 : deuxième contact bloqué.

Le deuxième contact bloque le chemin logique.

Équation :

Moteur = BP1 ET BP2

Table de vérité :

| BP1 | BP2 | Moteur |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

## Exercice 6 – Alarme sonore

Une alarme sonore doit s'activer dans deux situations :

- Une température excessive est détectée (Temperature_Haute = 1).
- Une pression excessive est détectée (Pression_Haute = 1).

L'alarme doit fonctionner dès qu'au moins une de ces situations apparaît.

Question :

Quelle équation logique convient ?

A. Alarme = Temperature_Haute ET Pression_Haute

B. Alarme = Temperature_Haute OU Pression_Haute

C. Alarme = NON Temperature_Haute ET NON Pression_Haute

D. Alarme = Temperature_Haute ET NON Pression_Haute

**Bonne réponse : B.**

Correction :

Une seule condition vraie suffit à déclencher l'alarme.

Réseau LAD :

```text
             Temperature_Haute
           +-------| |-------+
           |                 |
-----------+                 +--------( Alarme )
           |                 |
           +-------| |-------+
              Pression_Haute
```

Table de vérité :

| Temperature_Haute | Pression_Haute | Alarme |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

## Exercice 7 – Synthèse

Un moteur doit être autorisé à fonctionner lorsque :

- La commande automatique est active OU la commande manuelle est active.
- Le relais thermique ne signale aucun défaut.
- L'arrêt d'urgence n'est pas activé.

Question :

Quelle équation logique correspond à cette situation ?

A. Moteur = (Auto ET Manuel) OU Thermique ET Arrêt_Urgence

B. Moteur = (Auto OU Manuel) ET NON Thermique ET NON Arrêt_Urgence

C. Moteur = Auto OU Manuel OU Thermique OU Arrêt_Urgence

D. Moteur = (Auto ET Manuel) ET Thermique ET Arrêt_Urgence

**Bonne réponse : B.**

Correction :

Le moteur doit recevoir une demande automatique ou manuelle.

Il faut également que le défaut thermique soit absent et que l'arrêt d'urgence ne soit pas activé.

Équation :

Moteur = (Auto OU Manuel) ET NON Thermique ET NON Arrêt_Urgence

Représentation LAD :

```text
          Auto
        +--| |--+
        |       |
--------+       +----|/|----|/|----( Moteur )
        |       |  Thermique  Arrêt_Urgence
        +--| |--+
          Manuel
```

Précision de sécurité :

Cette représentation illustre une logique fonctionnelle. Un arrêt d'urgence réel doit être traité par une architecture de sécurité appropriée et ne doit pas dépendre uniquement d'un automate standard.

### Résultat des exercices

Score obtenu : 6/7.

La seule erreur concernait l'exercice 3 : le choix du contact permettant de représenter l'absence d'un défaut thermique.

La notion à consolider est la suivante :

- NO : condition vraie lorsque la variable vaut 1.
- NF : condition vraie lorsque la variable vaut 0.

---

# 13. Compte rendu des manipulations pratiques

L'utilisateur a confirmé avoir déjà réalisé les manipulations suivantes dans TIA Portal V17.

| Manipulation | État |
|---|---|
| Création d'un bloc de programme en LAD | Déjà réalisée |
| Insertion d'un contact NO | Déjà réalisée |
| Insertion d'un contact NF | Déjà réalisée |
| Insertion d'une bobine classique | Déjà réalisée |
| Association de contacts en série | Déjà réalisée |
| Association de contacts en parallèle | Déjà réalisée |
| Compilation d'un programme LAD | Déjà réalisée |

Aucune répétition des manipulations n'est nécessaire pour cette leçon.

### Bilan pédagogique

| Évaluation | Résultat |
|---|---:|
| QCM | 7/7 |
| Exercices | 6/7 |
| Pratique | Déjà réalisée |

### Compétences acquises

À l'issue de cette leçon, les compétences suivantes ont été travaillées :

- Reconnaître les principaux éléments d'un réseau LAD.
- Interpréter les contacts NO et NF.
- Comprendre le rôle d'une bobine classique.
- Traduire une logique ET en contacts série.
- Traduire une logique OU en contacts parallèles.
- Construire une expression logique combinant ET, OU et NON.
- Lire une commande industrielle simple.

---

# 14. Préparation de la leçon suivante

La prochaine leçon sera consacrée à :

**Leçon 2 – Les opérations logiques et les tables de vérité en programmation LAD.**

Nous approfondirons :

- Les opérateurs ET, OU et NON.
- Les expressions logiques complexes.
- Les priorités entre opérations.
- Les tables de vérité à plusieurs variables.
- La simplification des expressions logiques.
- La traduction d'expressions booléennes en réseaux LAD.
- Les applications industrielles.

Fin de la leçon 1.
