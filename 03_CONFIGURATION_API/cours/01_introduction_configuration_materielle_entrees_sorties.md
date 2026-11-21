# MODULE 03 — CONFIGURATION DE L'API S7-1200

# LEÇON 1 — INTRODUCTION À LA CONFIGURATION MATÉRIELLE ET À LA STRUCTURE DES ENTRÉES/SORTIES

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Expliquer le principe de la configuration matérielle d'un automate.
- Identifier les principaux composants matériels d'un S7-1200.
- Distinguer les entrées numériques et analogiques.
- Distinguer les sorties numériques et analogiques.
- Comprendre le rôle des modules d'extension.
- Associer les équipements physiques aux types d'entrées et de sorties.
- Comprendre la relation entre les équipements physiques et les adresses utilisées dans le programme API.

---

## 2. Introduction à la configuration matérielle

La configuration matérielle consiste à représenter dans TIA Portal les équipements physiques qui constituent un système d'automatisation.

Elle permet de définir le matériel utilisé par le programme utilisateur.

Elle comprend notamment :

- La CPU.
- Les modules d'entrées numériques.
- Les modules de sorties numériques.
- Les modules d'entrées analogiques.
- Les modules de sorties analogiques.
- Les modules de communication.
- Les périphériques industriels compatibles.

### Exemple industriel : station de pompage

Imaginons une station de pompage comprenant :

- Deux pompes électriques.
- Deux contacteurs.
- Deux boutons de marche.
- Deux boutons d'arrêt.
- Deux capteurs de niveau.
- Un transmetteur de pression 4–20 mA.

L'automate doit recevoir les informations des capteurs et des boutons, les traiter, puis commander les contacteurs des pompes.

Pour cela, nous devons déterminer les entrées et sorties nécessaires et configurer le matériel correspondant dans TIA Portal.

---

## 3. Architecture matérielle d'un S7-1200

Un automate S7-1200 peut être constitué de plusieurs éléments.

### 3.1. La CPU

La CPU (Central Processing Unit) est l'unité centrale de traitement de l'automate.

Elle assure notamment :

- L'exécution du programme utilisateur.
- Le traitement des informations provenant des entrées.
- La commande des sorties.
- La gestion des communications.
- Le diagnostic de fonctionnement.

Dans notre projet de formation, nous utilisons :

| Caractéristique | Valeur |
|---|---|
| Famille | Siemens S7-1200 |
| Modèle | CPU 1214C DC/DC/DC |
| Firmware | V4.5 |
| Projet | Formation_TIA_Portal_V17 |

### 3.2. Les entrées et sorties intégrées

Certaines CPU possèdent directement des entrées et sorties intégrées.

Cela signifie qu'il n'est pas nécessaire d'ajouter un module externe pour utiliser ces E/S.

Les caractéristiques dépendent de la référence exacte de la CPU.

### 3.3. Les modules d'extension

Lorsque les entrées et sorties intégrées ne suffisent plus, nous pouvons ajouter des modules d'extension compatibles.

Exemples :

| Module | Fonction |
|---|---|
| SM 1221 | Entrées numériques |
| SM 1222 | Sorties numériques |
| SM 1231 | Entrées analogiques |
| SM 1232 | Sorties analogiques |
| SM 1234 | Entrées et sorties analogiques |

Il faut toujours vérifier la référence complète, les caractéristiques électriques et la compatibilité avec la CPU.

---

## 4. Les quatre grandes catégories d'entrées et sorties

### 4.1. DI — Digital Input (Entrée numérique)

Une entrée numérique reçoit une information logique.

Elle possède généralement deux états :

- 0 : état logique bas.
- 1 : état logique haut.

Exemples :

- Bouton-poussoir Marche.
- Bouton-poussoir Arrêt.
- Fin de course.
- Détecteur de proximité.
- Contact auxiliaire d'un contacteur.

Exemple :

Un bouton Marche est raccordé à une entrée `%I0.0`.

Lorsque le bouton est actionné, l'entrée peut prendre la valeur TRUE, selon le câblage et la logique électrique.

Attention : l'état électrique d'un contact et l'état logique interprété par le programme dépendent du câblage et de la polarité retenue.

### 4.2. DQ — Digital Output (Sortie numérique)

Une sortie numérique commande un équipement selon un état logique.

Elle possède généralement deux états :

- FALSE (0) : sortie désactivée.
- TRUE (1) : sortie activée.

Exemples :

- Relais.
- Contacteur, avec interface de commande adaptée.
- Voyant lumineux.
- Électrovanne tout ou rien.

Exemple :

La sortie `%Q0.0` commande un relais intermédiaire destiné à piloter le contacteur d'une pompe.

Lorsque `%Q0.0 = TRUE`, la sortie est activée, sous réserve des conditions électriques et de fonctionnement.

Attention : il faut vérifier la tension, le courant admissible et le type de sortie de l'automate avant de raccorder une charge.

### 4.3. AI — Analog Input (Entrée analogique)

Une entrée analogique reçoit un signal électrique variable représentant une grandeur physique.

Exemples :

- Température.
- Pression.
- Niveau.
- Débit.
- Vitesse.

Signaux industriels courants :

| Signal | Signification |
|---|---|
| 0–10 V | Tension variable |
| 4–20 mA | Courant variable |
| 0–20 mA | Courant variable |

#### Exemple industriel : mesure de pression

Un transmetteur mesure une pression comprise entre 0 et 10 bar.

Il délivre un signal de 4–20 mA à une entrée analogique de l'automate.

Correspondance :

| Courant | Pression |
|---|---:|
| 4 mA | 0 bar |
| 8 mA | 2,5 bar |
| 12 mA | 5 bar |
| 16 mA | 7,5 bar |
| 20 mA | 10 bar |

L'automate reçoit une valeur numérique issue de la conversion analogique-numérique.

Il faut ensuite convertir cette valeur en une grandeur physique exploitable.

### 4.4. AQ — Analog Output (Sortie analogique)

Une sortie analogique produit un signal électrique variable destiné à commander un équipement.

Exemples :

- Variateur de fréquence.
- Positionneur de vanne.
- Régulateur de vitesse.
- Actionneur proportionnel.

#### Exemple industriel : commande d'un variateur

L'automate délivre une consigne analogique de 0–10 V au variateur.

| Tension | Consigne illustrative |
|---|---:|
| 0 V | 0 % |
| 2,5 V | 25 % |
| 5 V | 50 % |
| 7,5 V | 75 % |
| 10 V | 100 % |

Cette correspondance suppose une configuration linéaire du variateur.

---

## 5. Relation entre les équipements physiques et les E/S

Dans une installation industrielle, les équipements physiques sont raccordés aux bornes de l'automate.

Chaque information doit être associée à une entrée ou une sortie adaptée.

Exemple :

| Équipement physique | Type d'E/S | Adresse illustrative |
|---|---|---|
| Bouton Marche P1 | DI | `%I0.0` |
| Bouton Arrêt P1 | DI | `%I0.1` |
| Défaut thermique P1 | DI | `%I0.2` |
| Commande contacteur P1 | DQ | `%Q0.0` |
| Transmetteur de pression | AI | `%IW64` |
| Consigne analogique variateur | AQ | `%QW64` |

Les adresses sont illustratives. Elles doivent être vérifiées dans la configuration matérielle réelle.

### Différence entre capteur, transmetteur et entrée analogique

Il faut distinguer trois éléments :

1. Le capteur mesure une grandeur physique.
2. Le transmetteur conditionne et transmet un signal exploitable.
3. L'entrée analogique de l'automate reçoit le signal électrique.

Exemple :

Pression dans une canalisation → élément sensible du transmetteur → signal 4–20 mA → entrée analogique de l'automate.

---

## 6. Principe général de fonctionnement

Le fonctionnement d'un système automatisé peut être résumé en quatre étapes :

1. Acquisition des informations.
2. Traitement des informations par la CPU.
3. Transmission des commandes par les sorties.
4. Action des équipements.

Exemple :

Un capteur détecte un niveau bas dans une cuve.

L'information arrive à une entrée de l'automate.

La CPU vérifie les conditions de fonctionnement et active la sortie qui commande le contacteur de la pompe, si toutes les conditions d'autorisation sont satisfaites.

---

## 7. Dimensionnement des entrées et sorties

Avant de choisir les modules d'extension, il faut établir un inventaire des équipements à raccorder.

Exemple d'une station de pompage :

| Équipement | Quantité | Type d'E/S |
|---|---:|---|
| Boutons-poussoirs | 4 | DI |
| Détecteurs de proximité | 6 | DI |
| Transmetteurs de pression 4–20 mA | 2 | AI |
| Contacteurs de pompes | 3 | DQ |
| Variateur commandé en 0–10 V | 1 | AQ |

Bilan :

| Type d'E/S | Quantité |
|---|---:|
| Entrées numériques (DI) | 10 |
| Sorties numériques (DQ) | 3 |
| Entrées analogiques (AI) | 2 |
| Sorties analogiques (AQ) | 1 |
| Total | 16 |

Le dimensionnement doit également tenir compte :

- Des réserves pour les extensions futures.
- Des caractéristiques électriques.
- Du type de signal.
- Des limites de la CPU.
- De la compatibilité des modules.
- Des conditions d'installation.

---

# 8. ÉVALUATION THÉORIQUE — QCM CORRIGÉ

### Question 1
Quel est le rôle principal de la configuration matérielle ?

Réponse B : Décrire dans le logiciel les équipements matériels constituant le système d'automatisation.

### Question 2
Quelle est la différence entre DI et AI ?

Réponse B : Une DI reçoit généralement une information à deux états logiques, tandis qu'une AI reçoit un signal électrique variable représentant une grandeur physique.

### Question 3
Quel est le rôle d'une sortie numérique ?

Réponse C : Commander un équipement selon un état logique.

### Question 4
Un transmetteur de pression mesure de 0 à 10 bar avec un signal 4–20 mA. À quelle pression correspond 12 mA ?

Réponse B : 5 bar.

Calcul :

P = ((12 - 4) / (20 - 4)) × 10

P = 5 bar.

### Question 5
Quel est le rôle d'une sortie analogique ?

Réponse C : Produire un signal électrique variable pour commander un équipement proportionnellement.

### Question 6
Pourquoi utilise-t-on des modules d'extension ?

Réponse B : Pour augmenter les capacités matérielles de l'automate, notamment le nombre ou les types d'entrées et sorties disponibles.

### Question 7
Quelle est la succession logique des opérations ?

Réponse C : Les capteurs transmettent les informations aux entrées, la CPU traite ces informations, puis les sorties commandent les équipements.

**Résultat : 7/7 — 100 %.**

---

# 9. EXERCICES D'APPLICATION CORRIGÉS

### Exercice 1 — Identification des E/S

Équipements :

1. Bouton-poussoir Marche.
2. Bouton-poussoir Arrêt.
3. Transmetteur de pression 4–20 mA.
4. Contacteur de pompe commandé par l'automate.
5. Transmetteur de niveau 0–10 V.

Question : quels équipements doivent être raccordés à des entrées analogiques ?

Réponse B : équipements 3 et 5.

Explication :

- Boutons : DI.
- Transmetteurs : AI.
- Contacteur commandé : DQ.

### Exercice 2 — Calcul d'une mesure analogique

Données :

- Étendue de mesure : 0 à 5 m.
- Signal : 4–20 mA.
- Courant mesuré : 16 mA.

Formule :

N = ((I - 4) / (20 - 4)) × 5

Application :

N = ((16 - 4) / 16) × 5

N = 12 / 16 × 5

N = 3,75 m.

**Résultat exact : 3,75 m.**

Remarque : le QCM initial proposait 4 m comme réponse la plus proche, mais aucune proposition ne donnait la valeur exacte.

### Exercice 3 — Choix d'un module

Question : quel module choisir pour ajouter des entrées numériques ?

Réponse A : SM 1221.

### Exercice 4 — Décompte des entrées numériques

Équipements :

- 4 boutons-poussoirs.
- 6 détecteurs de proximité.
- 2 transmetteurs de pression.
- 3 contacteurs.
- 1 variateur commandé en 0–10 V.

Calcul :

DI = 4 + 6 = 10.

**Résultat : 10 entrées numériques.**

Les transmetteurs sont des AI, les contacteurs commandés sont associés à des DQ et la consigne 0–10 V du variateur correspond à une AQ.

### Exercice 5 — Décompte des sorties numériques

Trois contacteurs possèdent chacun une commande indépendante.

Calcul :

DQ = 3.

**Résultat : 3 sorties numériques.**

---

# 10. COMPTE RENDU DE LA PRATIQUE

## Environnement utilisé

- Logiciel : TIA Portal V17.
- Projet : Formation_TIA_Portal_V17.
- Automate : Siemens S7-1200 CPU 1214C DC/DC/DC.
- Firmware : V4.5.

## Manipulations réalisées

Les manipulations pratiques ont déjà été réalisées dans TIA Portal.

Elles ont notamment porté sur :

- La consultation des entrées et sorties intégrées de la CPU.
- L'exploration de la configuration matérielle.
- La consultation du catalogue matériel.
- L'identification des possibilités d'ajout de modules d'extension.
- La découverte des différentes catégories de modules.

Ces manipulations ne sont pas répétées dans cette leçon.

## Conclusion pratique

Cette manipulation a permis de mieux comprendre l'organisation matérielle d'un automate S7-1200 et la distinction entre les différentes catégories d'entrées et de sorties.

Elle constitue une base nécessaire pour la configuration détaillée des E/S et leur utilisation dans le programme utilisateur.

---

# 11. CONCLUSION GÉNÉRALE

La configuration matérielle constitue une étape fondamentale dans la réalisation d'un système automatisé.

Elle permet de représenter les équipements physiques, de déterminer les besoins en entrées et sorties et de sélectionner les modules adaptés.

La maîtrise des DI, DQ, AI et AQ est indispensable avant d'aborder l'affectation des adresses, la configuration détaillée des modules et la programmation des automatismes industriels.
