# MODULE 05 — TEMPORISATEURS ET COMPTEURS

# LEÇON 2 : INTRODUCTION AUX COMPTEURS INDUSTRIELS

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le rôle des compteurs industriels.
- Différencier CTU, CTD et CTUD.
- Identifier les entrées et sorties de chaque compteur.
- Comprendre le fonctionnement des fronts montants.
- Utiliser les fonctions Reset et Load.
- Configurer et tester les compteurs dans TIA Portal V17.
- Exploiter les compteurs dans des applications industrielles.

## 2. Généralités sur les compteurs industriels

Un compteur industriel est une fonction permettant de compter des événements détectés par un automate programmable industriel (API).

Contrairement à un temporisateur qui mesure une durée, un compteur enregistre le nombre d'événements.

### Exemple industriel

Sur un convoyeur, un capteur détecte chaque bouteille qui passe.

Si 150 bouteilles passent devant le capteur, le compteur enregistre 150 événements.

Applications :
- Comptage de bouteilles.
- Comptage de pièces fabriquées.
- Gestion des stocks.
- Comptage des cycles d'une machine.
- Comptage des produits distribués.

## 3. Notion de front montant

Les compteurs industriels utilisent généralement le front montant du signal pour détecter un événement.

Un front montant correspond au passage du signal de FALSE à TRUE.

| État précédent | Nouvel état | Détection |
|---|---|---|
| FALSE | FALSE | Aucun événement |
| FALSE | TRUE | Front montant détecté |
| TRUE | TRUE | Aucun nouvel événement |
| TRUE | FALSE | Front descendant |

**Point essentiel :** un signal maintenu à TRUE pendant cinq secondes ne produit pas cinq incrémentations. Il produit une seule incrémentation.

Pour compter un nouvel événement, le signal doit revenir à FALSE avant de repasser à TRUE.

## 4. Le compteur CTU — Count Up

### 4.1. Définition

CTU signifie Count Up.

Il permet d'effectuer un comptage croissant.

Chaque front montant appliqué à son entrée CU incrémente la valeur actuelle du compteur.

### 4.2. Paramètres

| Paramètre | Type | Fonction |
|---|---|---|
| CU | BOOL | Entrée de comptage croissant |
| R | BOOL | Remise à zéro |
| PV | INT | Valeur prédéfinie |
| Q | BOOL | Sortie de seuil atteint |
| CV | INT | Valeur actuelle du compteur |

### 4.3. Fonctionnement

Supposons :

- PV = 100
- CV initial = 0

Chaque bouteille détectée augmente CV de 1.

| Nombre de bouteilles | CV | Q |
|---:|---:|---|
| 0 | 0 | FALSE |
| 25 | 25 | FALSE |
| 50 | 50 | FALSE |
| 75 | 75 | FALSE |
| 99 | 99 | FALSE |
| 100 | 100 | TRUE |

Lorsque CV atteint ou dépasse PV, Q devient TRUE.

### 4.4. Remise à zéro

Lorsque l'entrée R est activée :

- CV revient à 0.
- Q repasse à FALSE si le seuil n'est plus atteint.

### 4.5. Application industrielle

Un convoyeur doit compter 200 bouteilles.

Lorsque le compteur atteint 200, l'automate peut déclencher une sortie pour arrêter le convoyeur ou signaler la fin du lot.

## 5. Le compteur CTD — Count Down

### 5.1. Définition

CTD signifie Count Down.

Il réalise un comptage décroissant.

Chaque front montant appliqué à l'entrée CD diminue la valeur actuelle du compteur.

### 5.2. Paramètres

| Paramètre | Type | Fonction |
|---|---|---|
| CD | BOOL | Entrée de comptage décroissant |
| LD | BOOL | Chargement de la valeur prédéfinie |
| PV | INT | Valeur prédéfinie |
| Q | BOOL | Sortie de fin de comptage |
| CV | INT | Valeur actuelle du compteur |

### 5.3. Fonctionnement

Supposons :

- PV = 80
- LD activée au démarrage.

Le compteur charge 80 dans CV.

Chaque pièce prélevée diminue CV de 1.

| Pièces prélevées | CV | Q |
|---:|---:|---|
| 0 | 80 | FALSE |
| 10 | 70 | FALSE |
| 20 | 60 | FALSE |
| 40 | 40 | FALSE |
| 60 | 20 | FALSE |
| 80 | 0 | TRUE |

Lorsque CV atteint ou passe sous zéro, Q devient TRUE.

### 5.4. Application industrielle

Un magasin possède un stock initial de 80 pièces.

Chaque prélèvement est enregistré par un capteur.

Le compteur diminue progressivement jusqu'à zéro.

Lorsque le stock est épuisé, l'automate peut déclencher une alarme de rupture de stock.

## 6. Le compteur CTUD — Count Up/Down

### 6.1. Définition

CTUD signifie Count Up/Down.

Il permet d'effectuer simultanément des comptages croissants et décroissants.

Il est particulièrement utile lorsqu'un système doit enregistrer des entrées et des sorties.

### 6.2. Paramètres

| Paramètre | Type | Fonction |
|---|---|---|
| CU | BOOL | Comptage croissant |
| CD | BOOL | Comptage décroissant |
| R | BOOL | Remise à zéro |
| LD | BOOL | Chargement de PV |
| PV | INT | Valeur prédéfinie |
| QU | BOOL | Indication de seuil supérieur atteint |
| QD | BOOL | Indication de seuil inférieur atteint |
| CV | INT | Valeur actuelle du compteur |

### 6.3. Fonctionnement

Supposons :

- CV initial = 50.
- 18 pièces réceptionnées.
- 12 pièces distribuées.

Calcul :

CV = 50 + 18 - 12

CV = 56

Le stock final est donc de 56 pièces.

### 6.4. Fonctionnement des sorties

- QU devient TRUE lorsque CV >= PV.
- QD devient TRUE lorsque CV <= 0.

Exemple :

PV = 100 et CV = 56.

Résultat :

- QU = FALSE.
- QD = FALSE.

### 6.5. Application industrielle

Dans un magasin automatisé :

- Un premier capteur détecte les pièces entrantes.
- Un deuxième capteur détecte les pièces sortantes.
- Le compteur CTUD calcule le stock disponible.

Cette solution permet de suivre les mouvements de stock en temps réel.

## 7. Comparaison des compteurs

| Critère | CTU | CTD | CTUD |
|---|---|---|---|
| Fonction | Comptage croissant | Comptage décroissant | Comptage dans les deux sens |
| Entrée de comptage | CU | CD | CU et CD |
| Remise à zéro | R | Selon la structure du bloc | R |
| Chargement PV | Non | LD | LD |
| Valeur actuelle | CV | CV | CV |
| Sortie de seuil | Q | Q | QU et QD |
| Application | Production | Gestion de stock consommé | Gestion des entrées et sorties |

## 8. Manipulation pratique dans TIA Portal V17

La manipulation pratique des trois compteurs a été réalisée dans TIA Portal V17.

### Compétences pratiques validées

- Insertion du compteur CTU.
- Configuration de ses entrées et sorties.
- Définition de PV.
- Observation de CV.
- Test de la remise à zéro.
- Insertion du compteur CTD.
- Test de l'entrée LD.
- Observation du comptage décroissant.
- Insertion du compteur CTUD.
- Test des entrées CU et CD.
- Observation des sorties QU et QD.

Les trois types de compteurs ont été configurés et testés.

## 9. Évaluation des connaissances

### QCM corrigés

| N° | Question | Bonne réponse |
|---:|---|---|
| 1 | Compter les bouteilles sur un convoyeur | CTU |
| 2 | Décompter 30 pièces distribuées | CTD |
| 3 | Suivre les entrées et sorties d'un stock | CTUD |
| 4 | Un signal CU reste TRUE pendant 5 secondes | Une incrémentation |
| 5 | Activation de R sur un CTU | CV = 0 |
| 6 | Activation de LD sur un CTD avec PV = 40 | CV = 40 |
| 7 | CTUD : un front sur CU puis un front sur CD | CV revient à sa valeur initiale |

### Exercice 1 — Comptage croissant

Un convoyeur transporte des bouteilles.

Données :
- PV = 200.
- CV initial = 0.
- Nombre de bouteilles détectées = 135.

Résultats :

CV = 135.

Q = FALSE.

Bouteilles restantes = 200 - 135 = 65.

### Exercice 2 — Comptage décroissant

Un magasin possède un stock initial de 80 pièces.

Données :
- PV = 80.
- LD activée au démarrage.
- 32 pièces prélevées.

Résultats :

CV = 80 - 32 = 48.

Q = FALSE.

Stock restant à épuisement = 0.

### Exercice 3 — Comptage croissant et décroissant

Un magasin possède initialement 50 pièces.

Pendant la journée :
- 18 pièces réceptionnées.
- 12 pièces distribuées.
- PV = 100.

Résultats :

CV = 50 + 18 - 12 = 56.

Stock final = 56 pièces.

QU = FALSE car 56 < 100.

QD = FALSE car 56 > 0.

## 10. Synthèse de la leçon

Les compteurs constituent des fonctions essentielles dans les systèmes automatisés.

Les trois compteurs étudiés ont des rôles complémentaires :

- CTU : compter les événements dans le sens croissant.
- CTD : compter les événements dans le sens décroissant.
- CTUD : gérer les événements dans les deux sens.

La compréhension des fronts montants, de Reset, de Load et des sorties de seuil est indispensable pour leur utilisation correcte.

## 11. Conclusion

Cette leçon nous a permis d'étudier les principaux compteurs industriels disponibles dans les automates programmables Siemens.

Les connaissances acquises peuvent être exploitées dans les applications de production, de convoyage, de gestion des stocks et de contrôle des cycles industriels.

La partie théorique, les évaluations et la manipulation pratique des compteurs CTU, CTD et CTUD sont terminées.
