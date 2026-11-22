# LEÇON 4 — CONFIGURATION DES ENTRÉES ET SORTIES ANALOGIQUES (AI/AQ)

**Module :** 03 — Configuration de l'API S7-1200  
**Environnement :** TIA Portal V17  
**Automate étudié :** Siemens S7-1200 CPU 1214C DC/DC/DC  
**Projet pratique :** Formation_TIA_Portal_V17

---

# 1. OBJECTIFS PÉDAGOGIQUES

À la fin de cette leçon, nous devons être capables de :

- Expliquer le rôle des entrées analogiques AI et des sorties analogiques AQ.
- Distinguer les signaux numériques des signaux analogiques.
- Identifier les principaux standards de signaux industriels : 0–10 V, 0–20 mA et 4–20 mA.
- Expliquer le principe du zéro vivant.
- Comprendre le raccordement fonctionnel d'un transmetteur analogique.
- Expliquer la conversion d'une grandeur physique en signal électrique.
- Comprendre le rôle du convertisseur analogique-numérique (CAN).
- Interpréter une valeur brute provenant d'une entrée analogique.
- Effectuer une mise à l'échelle avec les fonctions NORM_X et SCALE_X.
- Comprendre la commande d'un variateur ou d'une vanne proportionnelle par une sortie analogique.
- Identifier les modules analogiques disponibles pour le S7-1200.
- Distinguer résolution et précision.
- Diagnostiquer les erreurs courantes liées aux mesures analogiques.

# 2. INTRODUCTION AUX SIGNAUX ANALOGIQUES

## 2.1. Qu'est-ce qu'un signal analogique ?

Un signal analogique est un signal dont la valeur peut évoluer progressivement dans une plage définie.

Contrairement à un signal numérique qui possède généralement deux états logiques (0 ou 1), un signal analogique peut prendre une multitude de valeurs intermédiaires.

Prenons l'exemple d'une cuve industrielle équipée d'un capteur de niveau.

Le niveau d'eau peut être :

- 0 m ;
- 0,5 m ;
- 1,25 m ;
- 2,40 m ;
- 3,75 m ;
- 5 m.

Le niveau ne passe pas simplement de 0 à 1. Il évolue progressivement.

Le système d'automatisation doit donc pouvoir recevoir une information représentant cette évolution.

C'est le rôle d'une entrée analogique AI.

## 2.2. Différence entre une entrée numérique DI et une entrée analogique AI

| Critère | Entrée numérique DI | Entrée analogique AI |
|---|---|---|
| Nature du signal | Tout ou rien | Variable |
| États ou valeurs | 0 ou 1 | Plusieurs valeurs |
| Exemple | Bouton-poussoir | Capteur de pression |
| Information reçue | Présence ou absence | Valeur mesurée |
| Application | Détection d'une fin de course | Mesure d'un niveau |

**Analogie :**

Une entrée numérique fonctionne comme un interrupteur : il est ouvert ou fermé.

Une entrée analogique fonctionne davantage comme un potentiomètre : sa position peut varier progressivement.

## 2.3. Exemples de grandeurs physiques mesurées

| Grandeur physique | Instrument industriel | Application |
|---|---|---|
| Pression | Transmetteur de pression | Surveillance d'un réseau d'air comprimé |
| Température | Sonde avec transmetteur | Contrôle d'une chaudière |
| Niveau | Transmetteur de niveau | Gestion d'une cuve |
| Débit | Débitmètre avec sortie analogique | Contrôle d'une conduite |
| Position | Capteur de position | Positionnement d'un actionneur |

# 3. LES PRINCIPAUX STANDARDS DE SIGNAUX ANALOGIQUES

Les instruments industriels convertissent généralement les grandeurs physiques en signaux électriques normalisés.

Les trois standards étudiés sont :

- 0–10 V ;
- 0–20 mA ;
- 4–20 mA.

## 3.1. Le signal 0–10 V

Dans ce standard, la tension varie entre 0 et 10 volts.

Exemple : un transmetteur de pression mesure une pression comprise entre 0 et 10 bar.

On établit la correspondance suivante :

| Tension | Pression |
|---:|---:|
| 0 V | 0 bar |
| 2,5 V | 2,5 bar |
| 5 V | 5 bar |
| 7,5 V | 7,5 bar |
| 10 V | 10 bar |

Si l'automate reçoit 5 V, la pression correspondante est de 5 bar.

**Avantages :**

- Signal simple à comprendre.
- Facile à exploiter dans une installation adaptée.
- Couramment utilisé pour certaines commandes analogiques, notamment les consignes de variateurs.

**Limites :**

La tension peut être influencée par les chutes de tension et les perturbations électromagnétiques. Ces problèmes deviennent particulièrement importants lorsque les câbles sont longs ou mal installés.

## 3.2. Le signal 0–20 mA

Dans ce standard, le courant varie entre 0 et 20 mA.

Exemple : un transmetteur de température mesure une température comprise entre 0 et 100 °C.

| Courant | Température |
|---:|---:|
| 0 mA | 0 °C |
| 5 mA | 25 °C |
| 10 mA | 50 °C |
| 15 mA | 75 °C |
| 20 mA | 100 °C |

Le courant représente directement la valeur mesurée selon une relation définie par le transmetteur.

Le signal en courant présente un avantage important : il est généralement moins sensible aux effets des chutes de tension dans les câbles que le signal en tension.

## 3.3. Le signal 4–20 mA

Le standard 4–20 mA est particulièrement répandu dans l'instrumentation industrielle.

Il fonctionne avec :

- 4 mA correspondant à la valeur minimale ;
- 20 mA correspondant à la valeur maximale.

Exemple : un transmetteur de pression configuré pour mesurer de 0 à 10 bar.

| Courant | Pression |
|---:|---:|
| 4 mA | 0 bar |
| 8 mA | 2,5 bar |
| 12 mA | 5 bar |
| 16 mA | 7,5 bar |
| 20 mA | 10 bar |

### Pourquoi commencer à 4 mA au lieu de 0 mA ?

C'est le principe du **zéro vivant**.

Dans un système 4–20 mA, 4 mA représentent la valeur minimale normale de la mesure.

Par conséquent, un courant proche de 0 mA peut signaler une anomalie :

- Rupture du câble.
- Perte d'alimentation du transmetteur.
- Défaut de raccordement.
- Défaillance de l'instrument.

Le zéro vivant permet donc de distinguer la valeur minimale normale d'une absence de signal.

Attention : les seuils exacts de diagnostic dépendent du transmetteur, du module analogique et de leur configuration. Certains instruments utilisent des courants hors plage pour signaler différents défauts.

### Exemple industriel

Un transmetteur mesure la pression d'une conduite de 0 à 10 bar.

En fonctionnement normal, même lorsque la pression est nulle, le transmetteur fournit 4 mA.

Si l'automate détecte un courant anormalement proche de 0 mA, il peut signaler une rupture de boucle ou une perte d'alimentation, selon les caractéristiques du système.

## 3.4. Comparaison des trois standards

| Critère | 0–10 V | 0–20 mA | 4–20 mA |
|---|---|---|---|
| Grandeur électrique | Tension | Courant | Courant |
| Valeur minimale | 0 V | 0 mA | 4 mA |
| Valeur maximale | 10 V | 20 mA | 20 mA |
| Zéro vivant | Non | Non | Oui |
| Diagnostic d'une rupture | Plus difficile | Plus difficile | Généralement facilité |
| Sensibilité aux perturbations | Plus importante selon installation | Généralement faible | Généralement faible |
| Utilisation | Mesure et commande | Mesure | Mesure industrielle courante |

**Point important :** une entrée analogique doit être configurée pour le type de signal utilisé. Il ne faut pas raccorder directement un transmetteur en courant à une entrée configurée uniquement pour mesurer une tension sans dispositif d'adaptation approprié.

# 4. RACCORDEMENT FONCTIONNEL D'UN TRANSMETTEUR ANALOGIQUE

## 4.1. Différence entre capteur et transmetteur

Le capteur détecte une grandeur physique.

Le transmetteur conditionne cette information et la convertit en un signal électrique exploitable par l'automate.

Exemple :

- Une sonde mesure la température.
- Un transmetteur associé convertit cette mesure en un signal 4–20 mA.
- L'entrée analogique de l'automate reçoit ce signal.

## 4.2. Principe d'une boucle 4–20 mA à deux fils

Une boucle classique peut comprendre :

1. Une alimentation 24 V DC.
2. Un transmetteur deux fils.
3. Une entrée analogique configurée pour le courant.
4. Les conducteurs assurant la circulation du courant.

Schéma fonctionnel simplifié :

```text
       ALIMENTATION 24 V DC
              |
              |
       TRANSMETTEUR
          4–20 mA
              |
              |
       ENTREE ANALOGIQUE AI
              |
              |
        RETOUR DE BOUCLE
```

Ce schéma représente le principe de circulation du courant, et non un plan de raccordement borne par borne.

Le raccordement exact dépend notamment :

- Du type de transmetteur : actif ou passif.
- Du type d'entrée analogique.
- De la présence ou non d'une alimentation de boucle.
- Des bornes et caractéristiques du module utilisé.
- Des recommandations du constructeur.

Il faut donc toujours consulter les schémas de raccordement du transmetteur et du module Siemens concernés.

# 5. CONVERSION D'UN SIGNAL ANALOGIQUE EN GRANDEUR PHYSIQUE

L'automate reçoit une information électrique. Cependant, l'opérateur doit généralement connaître la grandeur physique correspondante.

Par exemple, l'automate reçoit 12 mA, mais l'écran de supervision doit afficher 5 bar.

Pour effectuer cette conversion, nous utilisons une relation mathématique.

## 5.1. Formule générale de mise à l'échelle

La formule générale est :

**X = Xmin + ((S - Smin) / (Smax - Smin)) × (Xmax - Xmin)**

Avec :

- X : grandeur physique recherchée.
- Xmin : valeur physique minimale.
- Xmax : valeur physique maximale.
- S : signal électrique mesuré.
- Smin : signal électrique minimal.
- Smax : signal électrique maximal.

## 5.2. Exemple 1 : mesure de pression avec un transmetteur 4–20 mA

Données :

- Étendue de mesure : 0 à 10 bar.
- Signal : 4 à 20 mA.
- Courant mesuré : 12 mA.

**Étape 1 : identifier les valeurs**

Xmin = 0 bar

Xmax = 10 bar

Smin = 4 mA

Smax = 20 mA

S = 12 mA

**Étape 2 : appliquer la formule**

P = 0 + ((12 - 4) / (20 - 4)) × (10 - 0)

**Étape 3 : calculer**

P = (8 / 16) × 10

P = 0,5 × 10

**Résultat : P = 5 bar**

Le courant de 12 mA correspond donc à une pression de 5 bar.

## 5.3. Exemple 2 : mesure de niveau

Données :

- Niveau : 0 à 5 m.
- Signal : 4 à 20 mA.
- Courant mesuré : 16 mA.

N = 0 + ((16 - 4) / (20 - 4)) × 5

N = (12 / 16) × 5

N = 0,75 × 5

**Résultat : N = 3,75 m**

Le niveau mesuré est de 3,75 m.

## 5.4. Formule simplifiée pour un transmetteur 4–20 mA

Lorsque le signal est compris entre 4 et 20 mA :

**X = Xmin + ((I - 4) / 16) × (Xmax - Xmin)**

Cette formule permet de convertir directement le courant mesuré en grandeur physique.

## 5.5. Formule simplifiée pour un transmetteur 0–10 V

Lorsque le signal est compris entre 0 et 10 V :

**X = Xmin + (U / 10) × (Xmax - Xmin)**

Exemple : transmetteur de pression 0–10 bar, tension mesurée 7,5 V.

P = 0 + (7,5 / 10) × 10

P = 0,75 × 10

**Résultat : P = 7,5 bar**

# 6. CONVERTISSEUR ANALOGIQUE-NUMÉRIQUE (CAN) ET VALEUR BRUTE

## 6.1. Pourquoi faut-il un convertisseur ?

Un automate programmable industriel fonctionne avec des informations numériques.

Or, un transmetteur analogique fournit une tension ou un courant variable.

L'automate doit donc convertir cette information électrique en une valeur numérique.

Cette conversion est réalisée par un **convertisseur analogique-numérique (CAN)**.

Le CAN transforme le signal analogique en une représentation numérique exploitable par le programme automate.

## 6.2. Chaîne de traitement d'une mesure analogique

```text
GRANDEUR PHYSIQUE
       |
       v
CAPTEUR / TRANSMETTEUR
       |
       v
SIGNAL ELECTRIQUE
(0–10 V ou 4–20 mA)
       |
       v
ENTREE ANALOGIQUE AI
       |
       v
CONVERTISSEUR CAN
       |
       v
VALEUR NUMERIQUE BRUTE
       |
       v
MISE A L'ECHELLE
       |
       v
GRANDEUR PHYSIQUE
       |
       v
AFFICHAGE WINCC
```

Chaque étape a une fonction précise :

- Le capteur mesure la grandeur physique.
- Le transmetteur convertit cette mesure en signal électrique.
- L'entrée AI reçoit le signal.
- Le CAN convertit le signal en donnée numérique.
- Le programme automate transforme la valeur brute en unité physique.
- WinCC affiche la valeur à l'opérateur.

## 6.3. Comprendre la valeur brute

La valeur brute est la représentation numérique produite par le traitement de l'entrée analogique.

Pour comprendre le principe, prenons un exemple pédagogique :

Une entrée analogique fournit une valeur comprise entre 0 et 27648 pour une plage de mesure configurée.

Supposons que cette plage représente une pression de 0 à 10 bar.

| Valeur brute illustrative | Pression |
|---:|---:|
| 0 | 0 bar |
| 6912 | 2,5 bar |
| 13824 | 5 bar |
| 20736 | 7,5 bar |
| 27648 | 10 bar |

Si l'automate reçoit 13824 :

Normalisation = 13824 / 27648

Normalisation = 0,5

La valeur représente 50 % de l'étendue de mesure.

Pression = 0,5 × 10

**Résultat : P = 5 bar**

**Attention :** la plage 0–27648 utilisée ici est un exemple pédagogique. Les valeurs brutes réelles dépendent de la référence du module, de la CPU, du type de signal et de la configuration retenue. Il faut consulter la documentation technique Siemens du matériel concerné.

# 7. MISE À L'ÉCHELLE AVEC NORM_X ET SCALE_X

Dans TIA Portal, deux fonctions permettent de réaliser une mise à l'échelle en deux étapes :

- NORM_X ;
- SCALE_X.

## 7.1. Fonction NORM_X

La fonction NORM_X transforme une valeur comprise dans une plage donnée en une valeur normalisée comprise entre 0.0 et 1.0.

Exemple :

- Valeur brute minimale : 0.
- Valeur brute maximale : 27648.
- Valeur brute reçue : 13824.

La normalisation est :

N = (13824 - 0) / (27648 - 0)

N = 13824 / 27648

**Résultat : N = 0,5**

La valeur normalisée est donc 0,5, soit 50 %.

NORM_X ne donne pas encore la pression en bar. Elle exprime uniquement la position relative de la mesure dans sa plage.

## 7.2. Fonction SCALE_X

SCALE_X transforme une valeur normalisée en grandeur physique.

Supposons :

- Valeur normalisée : 0,5.
- Pression minimale : 0 bar.
- Pression maximale : 10 bar.

P = 0 + 0,5 × (10 - 0)

**Résultat : P = 5 bar**

SCALE_X permet donc de convertir la valeur normalisée en unité physique.

## 7.3. Utilisation successive de NORM_X et SCALE_X

La chaîne de traitement est :

```text
VALEUR BRUTE
     |
     v
  NORM_X
     |
     v
VALEUR NORMALISEE
(0.0 à 1.0)
     |
     v
  SCALE_X
     |
     v
GRANDEUR PHYSIQUE
(exemple : pression en bar)
```

### Exemple complet

Données :

- Valeur brute : 20736.
- Plage brute : 0 à 27648.
- Pression : 0 à 12 bar.

**Étape 1 : normalisation**

N = 20736 / 27648

N = 0,75

**Étape 2 : mise à l'échelle**

P = 0 + 0,75 × (12 - 0)

**Résultat : P = 9 bar**

La pression correspondante est de 9 bar.

**Remarque TIA Portal :** il faut respecter les types de données, les paramètres d'entrée et les plages de fonctionnement des instructions. La valeur brute et les valeurs de sortie doivent être converties ou déclarées avec les types adaptés. Il faut également vérifier les limites et le traitement des valeurs hors plage.

# 8. LES SORTIES ANALOGIQUES AQ

## 8.1. Définition

Une sortie analogique AQ produit un signal électrique variable destiné à commander un équipement externe.

Contrairement à une entrée analogique AI qui reçoit une information, une sortie analogique AQ fournit une consigne.

Exemples :

- Commande de vitesse d'un variateur.
- Commande d'ouverture d'une vanne proportionnelle.
- Positionnement d'un actionneur.
- Réglage d'une consigne de débit.

## 8.2. Commande d'un variateur par un signal 0–10 V

Considérons un variateur configuré pour recevoir une consigne analogique 0–10 V correspondant à une fréquence de 0 à 50 Hz.

| Tension de commande AQ | Fréquence demandée |
|---:|---:|
| 0 V | 0 Hz |
| 2,5 V | 12,5 Hz |
| 5 V | 25 Hz |
| 7,5 V | 37,5 Hz |
| 10 V | 50 Hz |

Si l'automate fournit 5 V, le variateur reçoit une consigne correspondant à 25 Hz.

**Attention :** la sortie analogique ne fournit pas directement la puissance nécessaire au moteur. Elle transmet une consigne au variateur. C'est le variateur qui assure la commande de puissance du moteur.

Le variateur doit être configuré pour utiliser son entrée analogique comme source de consigne.

## 8.3. Commande d'une vanne proportionnelle en 4–20 mA

Une vanne équipée d'un positionneur peut recevoir un signal 4–20 mA.

Supposons que :

- 4 mA correspondent à une ouverture de 0 % ;
- 20 mA correspondent à une ouverture de 100 %.

| Courant AQ | Ouverture demandée |
|---:|---:|
| 4 mA | 0 % |
| 8 mA | 25 % |
| 12 mA | 50 % |
| 16 mA | 75 % |
| 20 mA | 100 % |

Le positionneur interprète le signal et commande la position de la vanne selon ses caractéristiques.

## 8.4. Différence entre AI et AQ

| Critère | AI | AQ |
|---|---|---|
| Signification | Analog Input | Analog Output |
| Fonction | Recevoir une mesure | Produire une consigne |
| Sens de circulation | Instrument vers automate | Automate vers équipement |
| Exemple | Mesure de pression | Consigne de vitesse |
| Équipement associé | Transmetteur | Variateur ou positionneur |

**À retenir :**

- AI = l'automate reçoit une information analogique.
- AQ = l'automate envoie une consigne analogique.

# 9. MODULES ANALOGIQUES DU S7-1200

La gamme S7-1200 propose différents modules permettant d'étendre les capacités analogiques de l'automate.

| Module | Fonction |
|---|---|
| SM 1231 AI | Entrées analogiques |
| SM 1232 AQ | Sorties analogiques |
| SM 1234 AI/AQ | Entrées et sorties analogiques |

Les caractéristiques exactes dépendent de la référence matérielle.

Avant de choisir un module, il faut vérifier :

- Le nombre d'entrées ou de sorties.
- Les types de signaux acceptés.
- Les plages de tension.
- Les plages de courant.
- La résolution.
- La précision.
- Les possibilités de diagnostic.
- La compatibilité avec la CPU utilisée.

La CPU S7-1200 1214C DC/DC/DC étudiée dans notre projet possède des entrées analogiques intégrées. Il faut toutefois consulter la fiche technique exacte de la référence pour connaître leurs caractéristiques.

Il ne faut pas supposer que toutes les CPU S7-1200 possèdent exactement les mêmes capacités analogiques.

# 10. RÉSOLUTION ET PRÉCISION

Ces deux notions sont importantes pour comprendre les performances d'une chaîne de mesure.

## 10.1. Résolution

La résolution correspond à la finesse de représentation numérique du signal.

Elle dépend notamment du nombre de bits du convertisseur.

Exemple :

- 12 bits : 2¹² = 4096 niveaux possibles.
- 16 bits : 2¹⁶ = 65536 niveaux possibles.

Une résolution plus élevée permet de représenter davantage de niveaux numériques dans une plage donnée.

## 10.2. Précision

La précision représente la proximité entre la valeur mesurée et la valeur réelle.

Exemple :

Un instrument mesure une pression réelle de 5 bar.

- Mesure A : 5,01 bar.
- Mesure B : 5,40 bar.

La mesure A est plus proche de la valeur réelle.

Un convertisseur ayant une résolution élevée ne garantit donc pas automatiquement une excellente précision.

La précision dépend aussi :

- Du transmetteur.
- Du module analogique.
- De l'installation.
- Des perturbations électromagnétiques.
- De l'étalonnage.
- Des conditions de fonctionnement.

# 11. DIAGNOSTIC DES DÉFAUTS ANALOGIQUES

Une mesure analogique incorrecte peut avoir plusieurs origines.

## 11.1. Principales causes

| Défaut | Conséquence possible |
|---|---|
| Rupture de câble | Absence ou anomalie du signal |
| Mauvais raccordement | Mesure incorrecte |
| Mauvaise polarité | Fonctionnement incorrect |
| Perturbations électromagnétiques | Mesure instable |
| Mauvaise configuration AI | Signal mal interprété |
| Transmetteur défectueux | Valeur erronée |
| Mise à l'échelle incorrecte | Affichage physique incorrect |
| Mauvais type de signal | Valeur incohérente |

## 11.2. Méthode de diagnostic

Lorsqu'une mesure affichée dans WinCC est incorrecte, il ne faut pas remplacer immédiatement le transmetteur.

Il faut procéder méthodiquement :

1. Vérifier l'état du transmetteur.
2. Contrôler son alimentation.
3. Vérifier le câblage et la polarité.
4. Mesurer le courant ou la tension avec un instrument adapté.
5. Vérifier la configuration de l'entrée analogique.
6. Lire la valeur brute dans l'automate.
7. Contrôler la normalisation NORM_X.
8. Contrôler la mise à l'échelle SCALE_X.
9. Vérifier la variable utilisée dans WinCC.
10. Comparer la valeur affichée à une mesure de référence.

### Exemple de diagnostic

Un transmetteur de pression mesure de 0 à 10 bar avec un signal 4–20 mA.

Le courant mesuré est de 12 mA.

La pression réelle correspondante est :

P = ((12 - 4) / (20 - 4)) × 10

P = (8 / 16) × 10

P = 0,5 × 10

**Résultat : P = 5 bar**

Cependant, WinCC affiche 10 bar.

La première hypothèse est une erreur dans la chaîne de traitement ou de mise à l'échelle.

Il faut vérifier :

- Le courant réellement reçu.
- La valeur brute.
- La configuration du canal.
- Les paramètres NORM_X.
- Les paramètres SCALE_X.
- La variable affichée dans WinCC.

Le transmetteur n'est pas nécessairement défectueux.

## 11.3. Bonnes pratiques d'installation

- Respecter les recommandations de raccordement du constructeur.
- Utiliser des câbles adaptés aux signaux analogiques.
- Respecter les règles de blindage et de mise à la terre.
- Séparer autant que possible les câbles de mesure des câbles de puissance.
- Vérifier la polarité.
- Configurer correctement le type de signal.
- Contrôler régulièrement la chaîne de mesure.

# 12. SYNTHÈSE GÉNÉRALE

| Notion | Fonction essentielle |
|---|---|
| AI | Reçoit un signal analogique |
| AQ | Produit un signal analogique |
| 0–10 V | Signal analogique en tension |
| 0–20 mA | Signal analogique en courant |
| 4–20 mA | Signal en courant avec zéro vivant |
| CAN | Convertit le signal analogique en valeur numérique |
| Valeur brute | Représentation numérique de la mesure |
| NORM_X | Normalise une valeur dans une plage 0.0–1.0 |
| SCALE_X | Convertit une valeur normalisée en grandeur physique |
| Résolution | Nombre de niveaux numériques disponibles |
| Précision | Proximité entre mesure et valeur réelle |

# 13. QCM DE VALIDATION AVEC CORRECTIONS

## Question 1 — Fonction d'une entrée analogique

Quel est le rôle principal d'une entrée analogique AI ?

A. Commander directement un moteur triphasé.  
B. Recevoir une information électrique variable provenant d'un instrument.  
C. Alimenter un transmetteur en puissance.  
D. Produire un signal numérique tout ou rien.

**Réponse correcte : B.**

**Explication :**

Une entrée analogique reçoit un signal variable provenant d'un instrument de mesure. Ce signal représente une grandeur physique telle que la pression, la température, le niveau ou le débit.

Elle ne commande pas directement la puissance d'un moteur.

## Question 2 — Intérêt du 4–20 mA

Pourquoi le standard 4–20 mA est-il particulièrement intéressant dans l'industrie ?

A. Il ne nécessite jamais d'alimentation.  
B. Il transforme automatiquement le signal en température.  
C. Son zéro vivant facilite la détection de certaines anomalies de boucle.  
D. Il fonctionne uniquement avec des signaux numériques.

**Réponse correcte : C.**

**Explication :**

Dans une boucle 4–20 mA, 4 mA représentent la valeur minimale normale.

Un courant proche de 0 mA peut donc révéler une rupture de câble, une perte d'alimentation ou un défaut de boucle, selon les caractéristiques du matériel.

## Question 3 — Calcul d'un niveau

Un transmetteur mesure un niveau de 0 à 5 m avec un signal 4–20 mA.

Le courant mesuré est de 12 mA.

Quelle est la valeur du niveau ?

A. 2,5 m.  
B. 3,75 m.  
C. 5 m.  
D. 1,25 m.

**Réponse correcte : A.**

**Explication :**

N = (12 - 4) / (20 - 4)

N = 8 / 16

N = 0,5

Niveau = 0 + 0,5 × 5

**Résultat : Niveau = 2,5 m**

Le courant de 12 mA représente 50 % de l'étendue de mesure.

## Question 4 — Fonction NORM_X

Quelle est la fonction principale de NORM_X ?

A. Convertir directement une pression en tension.  
B. Normaliser une valeur dans une plage comprise entre 0.0 et 1.0.  
C. Commander un moteur par une sortie numérique.  
D. Convertir une valeur analogique en fréquence.

**Réponse correcte : B.**

**Explication :**

NORM_X transforme une valeur comprise dans une plage d'entrée définie en une valeur normalisée comprise entre 0.0 et 1.0.

Elle prépare la donnée pour une mise à l'échelle ultérieure.

## Question 5 — Fonction d'une sortie analogique

Quel est le rôle principal d'une sortie analogique AQ ?

A. Mesurer la température d'un moteur.  
B. Lire l'état d'un bouton-poussoir.  
C. Produire un signal électrique variable pour transmettre une consigne à un équipement.  
D. Détecter une rupture de câble sur une entrée numérique.

**Réponse correcte : C.**

**Explication :**

Une sortie analogique produit une tension ou un courant variable. Elle peut transmettre une consigne à un variateur, un positionneur ou un autre équipement compatible.

## Question 6 — Convertisseur analogique-numérique

Quel est le rôle du CAN ?

A. Convertir une valeur numérique en courant de sortie.  
B. Convertir un signal analogique en représentation numérique.  
C. Alimenter les capteurs en 24 V.  
D. Commander directement les contacteurs.

**Réponse correcte : B.**

**Explication :**

Le CAN transforme le signal électrique analogique reçu par le module en une donnée numérique exploitable par l'automate.

## Question 7 — Fonction SCALE_X

Quelle est la fonction de SCALE_X ?

A. Détecter un court-circuit sur une sortie numérique.  
B. Convertir une température en courant 4–20 mA.  
C. Convertir une valeur normalisée en grandeur physique dans une plage définie.  
D. Configurer automatiquement un transmetteur.

**Réponse correcte : C.**

**Explication :**

SCALE_X prend une valeur normalisée et la transforme en valeur physique dans la plage définie.

Exemple : 0,5 normalisé devient 5 bar pour une plage de 0 à 10 bar.

# 14. EXERCICES D'APPLICATION AVEC CORRECTIONS DÉTAILLÉES

## Exercice 1 — Mesure de pression

Un transmetteur mesure une pression comprise entre 0 et 16 bar avec un signal 4–20 mA.

Le courant mesuré est de 10 mA.

Quelle est la pression correspondante ?

A. 4 bar.  
B. 6 bar.  
C. 8 bar.  
D. 10 bar.

**Réponse correcte : B — 6 bar.**

### Correction détaillée

Données :

- Pmin = 0 bar.
- Pmax = 16 bar.
- Imin = 4 mA.
- Imax = 20 mA.
- I = 10 mA.

Formule :

P = Pmin + ((I - Imin) / (Imax - Imin)) × (Pmax - Pmin)

Application :

P = 0 + ((10 - 4) / (20 - 4)) × (16 - 0)

P = (6 / 16) × 16

P = 0,375 × 16

**Résultat : P = 6 bar**

## Exercice 2 — Mesure de température

Un transmetteur de température fonctionne avec un signal 0–10 V.

Son étendue de mesure est de 0 à 200 °C.

La tension mesurée est de 7,5 V.

Quelle est la température ?

A. 75 °C.  
B. 100 °C.  
C. 150 °C.  
D. 175 °C.

**Réponse correcte : C — 150 °C.**

### Correction détaillée

Données :

- Tmin = 0 °C.
- Tmax = 200 °C.
- Umin = 0 V.
- Umax = 10 V.
- U = 7,5 V.

Formule :

T = Tmin + ((U - Umin) / (Umax - Umin)) × (Tmax - Tmin)

Application :

T = 0 + ((7,5 - 0) / (10 - 0)) × (200 - 0)

T = (7,5 / 10) × 200

T = 0,75 × 200

**Résultat : T = 150 °C**

## Exercice 3 — Mise à l'échelle d'une valeur brute

Une entrée analogique fournit une valeur brute de 20736.

La plage brute utilisée dans cet exercice est de 0 à 27648.

La pression à mesurer est comprise entre 0 et 12 bar.

Quelle est la pression correspondante ?

A. 6 bar.  
B. 9 bar.  
C. 10 bar.  
D. 12 bar.

**Réponse correcte : B — 9 bar.**

### Correction détaillée

**Étape 1 : normalisation**

N = (20736 - 0) / (27648 - 0)

N = 20736 / 27648

N = 0,75

**Étape 2 : mise à l'échelle**

P = 0 + 0,75 × (12 - 0)

**Résultat : P = 9 bar**

La pression représente 75 % de l'étendue de mesure.

## Exercice 4 — Commande d'un variateur

Un automate doit envoyer une consigne de vitesse à un variateur.

Le variateur est configuré pour recevoir un signal 0–10 V correspondant à une fréquence de 0 à 50 Hz.

Quelle sortie de l'automate doit être utilisée pour produire cette consigne ?

A. DI.  
B. DQ.  
C. AI.  
D. AQ.

**Réponse correcte : D — AQ.**

### Correction détaillée

Le variateur doit recevoir une consigne analogique variable.

Une entrée AI reçoit un signal provenant d'un instrument.

Une sortie AQ produit un signal analogique vers un équipement externe.

La sortie adaptée est donc AQ.

La sortie analogique transmet la consigne au variateur. Le variateur commande ensuite le moteur.

## Exercice 5 — Diagnostic d'une mesure incorrecte

Un transmetteur de pression mesure de 0 à 10 bar avec un signal 4–20 mA.

Le courant mesuré est de 12 mA.

WinCC affiche pourtant 10 bar.

Quelle est l'hypothèse à vérifier en priorité ?

A. Le moteur est nécessairement défectueux.  
B. La mise à l'échelle ou la chaîne de traitement analogique est probablement incorrecte.  
C. Le bouton d'arrêt d'urgence est défectueux.  
D. Le variateur doit être remplacé.

**Réponse correcte : B.**

### Correction détaillée

Le courant de 12 mA correspond à :

P = ((12 - 4) / (20 - 4)) × 10

P = (8 / 16) × 10

P = 0,5 × 10

**Résultat : P = 5 bar**

WinCC devrait donc afficher 5 bar, si le transmetteur et toute la chaîne de mesure sont correctement configurés.

Puisque l'écran affiche 10 bar, il faut vérifier :

1. La valeur réelle du courant.
2. Le raccordement du transmetteur.
3. La configuration de l'entrée AI.
4. La valeur brute lue par l'automate.
5. La normalisation NORM_X.
6. La mise à l'échelle SCALE_X.
7. La variable associée à l'affichage WinCC.

Il ne faut pas remplacer le transmetteur sans avoir identifié l'origine de l'erreur.

# 15. COMPTE RENDU DE LA MANIPULATION PRATIQUE TIA PORTAL V17

## 15.1. Environnement de travail

La manipulation a été réalisée dans le projet :

**Formation_TIA_Portal_V17**

Matériel configuré :

- Automate Siemens S7-1200.
- CPU 1214C DC/DC/DC.
- Firmware V4.5.
- Environnement TIA Portal V17.

## 15.2. Opérations déjà réalisées

Les opérations suivantes ont été effectuées et ne doivent pas être répétées :

1. Consultation des entrées analogiques intégrées à la CPU 1214C DC/DC/DC.
2. Exploration de la configuration matérielle de la CPU.
3. Consultation du catalogue matériel TIA Portal.
4. Recherche du module SM 1231 AI.
5. Recherche du module SM 1232 AQ.
6. Observation des possibilités de configuration des entrées et sorties analogiques.

## 15.3. Résultat de la pratique

Cette manipulation a permis de mieux comprendre :

- La présence d'entrées analogiques intégrées sur la CPU étudiée.
- La possibilité d'étendre les capacités analogiques par des modules supplémentaires.
- La distinction entre modules d'entrées analogiques et modules de sorties analogiques.
- L'importance de la référence matérielle dans le choix d'un module.
- Le rôle de la configuration matérielle dans l'exploitation des signaux analogiques.

**État de la pratique :** consultation et exploration déjà réalisées. Aucune nouvelle manipulation ne doit être imposée pour répéter ces opérations.

# 16. CONCLUSION DE LA LEÇON

Les entrées et sorties analogiques constituent une partie essentielle des systèmes automatisés industriels.

Les entrées AI permettent à l'automate de recevoir des informations provenant des instruments de mesure.

Les sorties AQ permettent à l'automate de transmettre des consignes variables aux équipements compatibles.

La compréhension des signaux 0–10 V, 0–20 mA et 4–20 mA est indispensable pour exploiter correctement les instruments industriels.

La conversion du signal électrique en grandeur physique nécessite une mise à l'échelle adaptée. Les fonctions NORM_X et SCALE_X facilitent cette opération dans TIA Portal.

Enfin, le diagnostic d'une mesure analogique doit prendre en compte toute la chaîne : transmetteur, câblage, entrée analogique, conversion numérique, mise à l'échelle et affichage WinCC.

**Fin de la leçon 4 — Configuration des entrées et sorties analogiques AI/AQ.**
