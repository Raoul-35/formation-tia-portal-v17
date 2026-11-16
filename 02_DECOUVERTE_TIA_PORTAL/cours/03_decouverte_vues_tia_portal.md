# Module 02 — Découverte de TIA Portal V17

# Leçon 3 — Découverte des vues de TIA Portal

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Distinguer la Portal view de la Project view.
- Comprendre l'organisation de la Project view.
- Identifier les principales zones de travail.
- Découvrir la Device view, la Network view et la Topology view.
- Utiliser le Hardware catalog.
- Identifier les modules DI, DQ, AI et AQ.
- Comprendre les propriétés matérielles et réseau d'un automate.

## 2. Les deux environnements principaux

TIA Portal propose deux environnements de travail complémentaires.

### 2.1 Portal view (Vue du portail)

La Portal view est une interface orientée vers les grandes tâches d'ingénierie.

Elle permet notamment de :

- Créer un nouveau projet.
- Ouvrir un projet existant.
- Accéder aux principales tâches d'ingénierie.
- Choisir les opérations à effectuer.

### 2.2 Project view (Vue du projet)

La Project view est l'environnement principal de développement.

Elle permet de travailler directement sur :

- Les automates.
- Les blocs de programme.
- Les variables API.
- Les interfaces HMI.
- Les réseaux de communication.
- Les configurations matérielles.

La programmation et la configuration détaillée du système s'effectuent principalement dans cet environnement.

## 3. Comparaison des deux vues

| Portal view | Project view |
|---|---|
| Vue orientée tâches | Vue orientée développement |
| Création de projets | Modification du contenu du projet |
| Ouverture des projets | Configuration des équipements |
| Accès aux grandes fonctions | Accès détaillé aux éléments |

## 4. Organisation de la Project view

La Project view comporte plusieurs zones importantes.

### 4.1 Project tree (Arborescence du projet)

Située généralement à gauche, elle permet de naviguer dans les différents éléments du projet.

On peut notamment retrouver :

- Devices.
- PLC programming.
- Program blocks.
- PLC tags.
- HMI.
- Device configuration.

### 4.2 Zone de travail

Elle se trouve généralement au centre de l'interface.

Elle permet d'afficher et de modifier les éléments sélectionnés :

- Configuration matérielle.
- Programme LAD.
- Tables de variables.
- Écrans HMI.
- Paramètres réseau.

### 4.3 Inspector window

Cette fenêtre affiche les propriétés et informations relatives à l'élément sélectionné.

Elle permet notamment de consulter ou modifier certains paramètres de configuration.

### 4.4 Task cards

Les Task cards donnent accès à différents outils et fonctions selon le contexte de travail.

### 4.5 Menu et barres d'outils

Ils regroupent les commandes générales du logiciel :

- Sauvegarde.
- Compilation.
- Annulation.
- Rétablissement.
- Affichage.
- Configuration.

## 5. Device configuration

La Device configuration permet de configurer le matériel du système automatisé.

Exemples :

- CPU S7-1200.
- Modules d'entrées numériques.
- Modules de sorties numériques.
- Modules analogiques.
- Modules de communication.

### 5.1 Device view

La Device view permet de visualiser et de configurer un équipement individuellement.

On peut notamment :

- Visualiser la CPU.
- Identifier les modules intégrés.
- Ajouter des modules d'extension compatibles.
- Consulter les propriétés matérielles.
- Configurer certains paramètres des entrées et sorties.

### 5.2 Properties (Propriétés)

La fenêtre Properties affiche les paramètres de l'équipement sélectionné.

Pour une CPU S7-1200, on peut notamment retrouver :

- Informations générales.
- Paramètres de l'interface PROFINET.
- Adresse IP.
- Masque de sous-réseau.
- Paramètres des entrées et sorties.
- Paramètres système.

## 6. Configuration réseau

### 6.1 Adresse IP

Une adresse IP permet d'identifier un équipement sur un réseau IP.

Exemple de configuration :

| Équipement | Adresse IP |
|---|---|
| Automate S7-1200 | 192.168.0.1 |
| IHM | 192.168.0.2 |
| PC | 192.168.0.10 |

Ces valeurs sont illustratives.

Les équipements doivent posséder des adresses compatibles et uniques sur le même réseau.

### 6.2 Masque de sous-réseau

Exemple :

255.255.255.0

Avec cette configuration, les trois premiers octets identifient le réseau dans un réseau IPv4 classique.

L'adresse réseau correspondante est :

192.168.0.0

### 6.3 Passerelle

La passerelle permet à un équipement de communiquer avec d'autres réseaux.

Elle n'est généralement pas indispensable pour les échanges locaux entre équipements appartenant au même sous-réseau.

## 7. Network view (Vue réseau)

La Network view permet de représenter et de configurer les connexions réseau logiques entre les équipements.

Exemples :

- Automate vers IHM.
- Automate vers variateur.
- Communication entre automates.

Elle permet notamment de configurer les réseaux Ethernet/PROFINET.

## 8. Topology view (Vue topologique)

La Topology view permet de représenter et de configurer la topologie physique du réseau pour les équipements compatibles.

Elle permet notamment de visualiser :

- Les ports physiques.
- Les connexions entre ports.
- Les liaisons réseau.
- La disposition physique des équipements compatibles.

## 9. Comparaison des trois vues

| Vue | Fonction principale |
|---|---|
| Device view | Configuration matérielle d'un équipement |
| Network view | Configuration des connexions réseau logiques |
| Topology view | Configuration de la topologie physique du réseau |

## 10. Hardware catalog (Catalogue matériel)

Le Hardware catalog est une bibliothèque contenant les équipements matériels disponibles dans TIA Portal.

Il permet d'ajouter des modules compatibles à la configuration matérielle.

### 10.1 Modules DI

DI signifie Digital Inputs.

Ce sont les entrées numériques.

Exemples :

- Bouton Marche.
- Bouton Arrêt.
- Détecteur de niveau TOR.
- Contact de défaut thermique.

### 10.2 Modules DQ

DQ signifie Digital Outputs.

Ce sont les sorties numériques.

Exemples :

- Commande d'un contacteur.
- Commande d'un relais.
- Voyant lumineux.
- Ordre numérique vers un équipement compatible.

### 10.3 Modules AI

AI signifie Analog Inputs.

Ce sont les entrées analogiques.

Exemples :

- Transmetteur de pression 4–20 mA.
- Transmetteur de niveau 4–20 mA.
- Transmetteur de température 0–10 V.

### 10.4 Modules AQ

AQ signifie Analog Outputs.

Ce sont les sorties analogiques.

Exemples :

- Consigne de vitesse d'un variateur.
- Consigne de position d'un actionneur compatible.
- Commande analogique d'un équipement industriel.

## 11. Comparaison des modules d'entrées et sorties

| Module | Fonction | Exemple |
|---|---|---|
| DI | Entrée numérique | Bouton Marche |
| DQ | Sortie numérique | Commande contacteur |
| AI | Entrée analogique | Mesure de pression |
| AQ | Sortie analogique | Consigne variateur |

## 12. Mise en pratique réalisée

### Projet utilisé

Nom : Formation_TIA_Portal_V17

### Configuration matérielle

| Paramètre | Valeur |
|---|---|
| Famille | SIMATIC S7-1200 |
| CPU | CPU 1214C DC/DC/DC |
| Firmware proposé | V4.5 |
| Adresse IP observée | 192.168.0.1 |
| Masque | 255.255.255.0 |
| Passerelle | Non configurée |

### Manipulations réalisées

1. Lancement de TIA Portal V17.
2. Observation de la Portal view.
3. Création du projet Formation_TIA_Portal_V17.
4. Découverte de la Project view.
5. Exploration du Project tree.
6. Ajout d'une CPU S7-1200.
7. Ouverture de Device configuration.
8. Observation de la Device view.
9. Consultation des propriétés de la CPU.
10. Observation de l'adresse IP et du masque réseau.
11. Découverte de la Network view.
12. Découverte de la Topology view.
13. Exploration du Hardware catalog.
14. Identification des modules DI, DQ, AI et AQ.
15. Sauvegarde du projet.

## 13. Évaluation des connaissances

### Résultats du QCM

6/6 réponses correctes.

### Exercice d'association

5/5 réponses correctes.

### Questions pratiques

- Identification de la Device view : correcte.
- Identification de la Network view : correcte.
- Identification de la Topology view : correcte.
- Distinction entre DI, DQ, AI et AQ : correcte.

## 14. Conclusion

Cette leçon nous a permis de découvrir concrètement l'environnement de travail de TIA Portal V17.

Nous savons maintenant distinguer les principales vues du logiciel et identifier les éléments matériels et réseau d'un projet.

Ces connaissances constituent une base indispensable avant d'aborder la navigation approfondie dans l'arborescence du projet et la programmation de l'automate.
