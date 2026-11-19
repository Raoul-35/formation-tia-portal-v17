# Leçon 8 — Configuration des appareils et gestion du catalogue matériel

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre la configuration des appareils dans TIA Portal.
- Identifier les références matérielles Siemens.
- Comprendre la notion de MLFB.
- Utiliser le catalogue matériel.
- Comprendre la différence entre TIA Portal et le firmware.
- Ajouter, supprimer et remplacer un équipement.
- Vérifier la compatibilité des équipements.

## 2. Configuration des appareils

La configuration des appareils consiste à sélectionner et organiser les équipements matériels qui constituent un système d'automatisation.

Cette opération s'effectue principalement dans la vue des appareils (Device view).

On peut y représenter :

- Une CPU.
- Des modules d'entrées numériques.
- Des modules de sorties numériques.
- Des modules analogiques.
- Des modules de communication.
- Des équipements compatibles.

Exemple : une station de pompage peut comporter une CPU S7-1200, un module d'entrées analogiques, un module de sorties numériques et différents équipements de terrain.

La configuration doit correspondre aux équipements réellement prévus dans l'installation.

## 3. Référence matérielle Siemens : MLFB

Chaque équipement Siemens possède une référence permettant de l'identifier précisément.

Cette référence est appelée MLFB (Machine-Readable Product Designation).

Exemple :

`6ES7 214-1AG40-0XB0`

La référence complète permet de distinguer les différentes variantes d'un équipement.

Deux CPU appartenant à la même famille peuvent présenter des différences concernant :

- Le nombre d'entrées et de sorties.
- Le type d'alimentation.
- Le type de sorties.
- Les capacités de communication.
- Les fonctionnalités disponibles.
- La compatibilité avec les modules.

Le nom commercial indique généralement la famille et le modèle, tandis que la référence complète identifie la variante précise du produit.

**Règle professionnelle :** vérifier la référence complète avant de sélectionner un équipement.

## 4. Le catalogue matériel (Hardware catalog)

Le Hardware catalog est la bibliothèque des équipements disponibles dans TIA Portal.

Il permet de rechercher et de sélectionner les équipements nécessaires au projet.

On peut notamment y retrouver :

| Famille | Équipements |
|---|---|
| SIMATIC S7-1200 | CPU et modules d'extension |
| SIMATIC S7-1500 | CPU et modules associés |
| SIMATIC ET 200 | Systèmes d'E/S distribuées |
| HMI | Pupitres opérateur |
| Drives | Variateurs et équipements d'entraînement |

Le catalogue est organisé en catégories et sous-catégories.

Exemple de recherche :

`Hardware catalog → Controllers → SIMATIC S7-1200`

L'organisation exacte des catégories peut varier selon les composants installés et la version de TIA Portal.

## 5. La version de firmware

Le firmware est le logiciel interne qui permet à un équipement matériel de fonctionner.

Il ne faut pas le confondre avec le logiciel TIA Portal.

| Élément | Fonction |
|---|---|
| TIA Portal V17 | Logiciel d'ingénierie installé sur l'ordinateur |
| Firmware V4.5 | Logiciel interne de la CPU |
| Référence matérielle | Identification précise de l'équipement |

### 5.1 Importance du firmware

Certaines fonctionnalités et compatibilités dépendent de la version du firmware.

Une CPU disposant d'un firmware récent peut proposer des fonctionnalités qui ne sont pas disponibles sur une version plus ancienne.

Il faut donc vérifier la cohérence entre :

- La CPU physique.
- La référence matérielle.
- La version du firmware.
- La version de TIA Portal utilisée.

### 5.2 Environnement de notre formation

- Logiciel : TIA Portal V17.
- Automate : S7-1200.
- CPU : 1214C DC/DC/DC.
- Firmware : V4.5.

## 6. Les opérations sur les appareils

### 6.1 Ajouter un équipement

On peut ajouter un équipement en le sélectionnant dans le catalogue matériel et en l'intégrant à la configuration.

Exemple : ajouter un module d'entrées analogiques SM 1231 à une CPU S7-1200.

### 6.2 Supprimer un équipement

On peut retirer un module qui n'est plus nécessaire dans l'architecture du projet.

Avant la suppression, il faut vérifier si des variables, des adresses ou des programmes dépendent de cet équipement.

### 6.3 Remplacer un équipement

On peut remplacer un équipement par une autre référence compatible.

Exemple : remplacer une CPU par une autre variante.

Ce remplacement peut entraîner des modifications :

- Des adresses d'E/S.
- Des caractéristiques électriques.
- Des capacités de communication.
- Des fonctionnalités disponibles.
- De la compatibilité du programme.

Après le remplacement, il faut contrôler la configuration et le programme.

### 6.4 Modifier les propriétés d'un équipement

Chaque appareil possède des propriétés configurables.

Selon le matériel, on peut notamment retrouver :

- Les paramètres généraux.
- Les paramètres de communication.
- Les adresses d'E/S.
- Les paramètres des modules.
- Les caractéristiques électriques configurables.

## 7. Importance de la compatibilité matérielle

La compatibilité matérielle doit être vérifiée avant d'intégrer un équipement dans un projet.

Il faut notamment vérifier :

- La référence exacte.
- La compatibilité avec la CPU.
- La version du firmware.
- Les caractéristiques électriques.
- Les types de signaux acceptés.
- Le nombre de voies disponibles.
- Les limites d'extension du système.

Exemple : deux CPU 1214C peuvent avoir des caractéristiques différentes selon leur variante.

Une CPU possédant des sorties à relais n'est pas équivalente à une CPU possédant des sorties à transistor.

Une mauvaise sélection peut provoquer :

- Une configuration incohérente.
- Des erreurs de compilation ou de chargement.
- Des différences de comportement.
- Des risques lors du raccordement électrique.

**Règle professionnelle :** toujours identifier le matériel réel à partir de sa référence complète et de sa documentation technique.

## 8. Exemple d'application industrielle

Une station de pompage possède :

| Équipement | Quantité |
|---|---:|
| CPU S7-1200 CPU 1214C DC/DC/DC | 1 |
| Module SM 1231 | 1 |
| Module SM 1222 | 1 |
| Transmetteurs de pression 4–20 mA | 3 |
| Boutons-poussoirs | 5 |
| Contacteurs de pompes | 3 |

### Rôle des modules

**SM 1231 :**

Module d'entrées analogiques permettant de recevoir les signaux des transmetteurs de pression, sous réserve que la référence exacte accepte le signal 4–20 mA et possède suffisamment de voies.

**SM 1222 :**

Module de sorties numériques permettant d'ajouter des sorties pour commander des équipements.

Les sorties peuvent notamment commander les bobines des contacteurs, en respectant les caractéristiques électriques et les protections nécessaires.

## 9. Évaluation des connaissances

### 9.1 Résultat du QCM

| Question | Réponse correcte | Résultat |
|---|---|---|
| 1. Rôle du Hardware catalog | B | Correct |
| 2. Signification de MLFB | C | Correct |
| 3. Différence TIA Portal et firmware | B | Correct |
| 4. Remplacement d'une CPU | C | Correct |
| 5. Suppression d'un module | B | Correct |
| 6. Importance du firmware | B | Correct |

**Résultat : 6/6.**

### 9.2 Résultat de l'exercice d'application

| Question | Réponse correcte | Résultat |
|---|---|---|
| 1. Rôle du SM 1231 | B | Correct |
| 2. Rôle du SM 1222 | B | Correct |
| 3. Vérification avant ajout d'un module | B | Correct |
| 4. Remplacement d'une CPU | B | Correct |
| 5. Cohérence matériel et configuration | B | Correct |

**Résultat : 5/5.**

## 10. Compte rendu des manipulations pratiques

Les manipulations pratiques relatives à la configuration des appareils et à la découverte du catalogue matériel ont déjà été réalisées dans TIA Portal V17.

Environnement utilisé :

- Logiciel : TIA Portal V17.
- Projet : Formation_TIA_Portal_V17.
- Automate : S7-1200 CPU 1214C DC/DC/DC.
- Firmware : V4.5.

Conformément à la progression pédagogique, les manipulations ne sont pas répétées dans cette séance.

## 11. Synthèse

La configuration des appareils permet de construire l'architecture matérielle d'un projet d'automatisation.

Le catalogue matériel facilite la sélection des équipements Siemens.

La référence MLFB permet d'identifier précisément un produit, tandis que le firmware indique la version du logiciel interne de l'équipement.

Toute modification de la configuration doit être suivie d'une vérification de la compatibilité matérielle, des adresses et du programme utilisateur.
