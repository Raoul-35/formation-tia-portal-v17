# Module 02 — Découverte de TIA Portal V17

# Leçon 2 — Organisation générale d'un projet TIA Portal

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre la notion de projet TIA Portal.
- Identifier les principales parties d'un projet.
- Comprendre le rôle de Devices & Networks.
- Comprendre le rôle de PLC tags.
- Identifier les blocs de programme.
- Comprendre l'organisation de la partie HMI.
- Identifier les outils de communication et de diagnostic.

## 2. Qu'est-ce qu'un projet TIA Portal ?

Un projet TIA Portal est un ensemble organisé de configurations, de programmes et de données permettant de développer un système d'automatisation industrielle.

Il peut contenir :

- Un ou plusieurs automates.
- Des interfaces homme-machine.
- Des variateurs de vitesse.
- Des réseaux de communication.
- Des programmes utilisateurs.
- Des tables de variables.
- Des paramètres matériels.
- Des configurations d'alarmes.

Exemple : une station de pompage équipée de quatre pompes peut être représentée dans un projet TIA Portal comprenant un automate S7-1200, une IHM, les variables et les programmes nécessaires à son fonctionnement.

## 3. Les principales parties d'un projet

| Élément | Rôle |
|---|---|
| Devices & Networks | Configuration des appareils et des réseaux |
| PLC | Configuration et programmation de l'automate |
| Program blocks | Organisation du programme utilisateur |
| PLC tags | Déclaration et organisation des variables API |
| HMI | Configuration de l'interface opérateur |
| Connections | Configuration des communications |
| Online & diagnostics | Surveillance et diagnostic des équipements |

## 4. Devices & Networks : appareils et réseaux

Cette partie permet de représenter et de configurer les équipements qui constituent le système automatisé.

Exemples :

- Automate S7-1200.
- IHM Siemens.
- Variateur de vitesse.
- Réseau Ethernet/PROFINET.

Elle permet notamment de définir les caractéristiques matérielles des appareils et leurs connexions réseau.

Exemple : ajouter un automate S7-1200 et une IHM, puis configurer leur connexion Ethernet/PROFINET.

## 5. PLC : automate programmable

PLC signifie Programmable Logic Controller.

Cette partie concerne la configuration et la programmation de l'automate.

Elle regroupe notamment :

- La configuration matérielle.
- Les paramètres de communication.
- Les variables API.
- Les blocs de programme.

### 5.1 Program blocks

Les blocs de programme permettent d'organiser la logique de commande.

| Bloc | Signification | Fonction |
|---|---|---|
| OB | Organization Block | Organisation de l'exécution |
| FC | Function | Fonction sans mémoire d'instance |
| FB | Function Block | Bloc fonctionnel avec mémoire d'instance |
| DB | Data Block | Stockage de données |

Exemples :

- OB1 : programme cyclique principal.
- FC1 : fonction de calcul.
- FB1 : gestion d'une pompe.
- DB1 : stockage des données d'un bloc ou d'une application.

Ces blocs seront étudiés en détail dans le module 06.

## 6. PLC tags : variables de l'automate

Les PLC tags permettent de déclarer et d'organiser les variables utilisées dans le programme.

Exemple :

| Nom symbolique | Adresse | Type |
|---|---|---|
| BP_Marche_P1 | %I0.0 | BOOL |
| BP_Arret_P1 | %I0.1 | BOOL |
| Moteur_P1 | %Q0.0 | BOOL |
| Temperature_P1 | %MD100 | REAL |
| Nombre_Demarrages | %MW110 | UINT |

Attention : ces adresses sont illustratives. Elles doivent être vérifiées par rapport à la configuration matérielle et aux variables réellement utilisées.

Une variable possède généralement :

- Un nom.
- Un type de données.
- Une adresse lorsqu'elle est adressée directement.
- Un commentaire facultatif.

Les noms symboliques facilitent la lecture et la maintenance du programme.

## 7. HMI : interface homme-machine

HMI signifie Human Machine Interface.

Cette partie permet de configurer les écrans destinés à l'opérateur.

Exemple pour une station de pompage :

- Affichage des quatre pompes.
- État de fonctionnement de chaque pompe.
- Boutons Marche et Arrêt.
- Affichage de la pression.
- Affichage du niveau de la cuve.
- Alarmes de fonctionnement.

Les variables API peuvent être associées aux objets graphiques de l'IHM pour permettre les échanges d'informations.

## 8. Connections : communications

Cette partie concerne la configuration des connexions nécessaires aux échanges entre les équipements.

Exemples :

- Automate vers IHM.
- Automate vers autres équipements compatibles.
- Échanges via Ethernet/PROFINET.

Il faut distinguer la connexion physique ou réseau de la configuration logique des échanges.

## 9. Online & diagnostics : surveillance et diagnostic

Cette partie permet notamment de :

- Vérifier l'état de fonctionnement de l'automate.
- Consulter les informations de diagnostic.
- Surveiller les variables.
- Identifier des erreurs de configuration ou de communication.

Elle est particulièrement utile lors de la mise en service et de la maintenance.

## 10. Mise en situation industrielle

Nous travaillons sur une station de pompage comprenant un automate S7-1200 et une IHM.

| Opération | Partie TIA Portal |
|---|---|
| Déclarer Temperature_Moteur | PLC tags |
| Créer un écran de supervision | HMI |
| Ajouter un automate S7-1200 | Devices & Networks |
| Créer un FB de gestion d'une pompe | Program blocks |
| Vérifier si l'automate est en RUN | Online & diagnostics |

## 11. Synthèse

Les principales parties étudiées sont :

1. Devices & Networks : équipements et réseaux.
2. PLC tags : variables de l'automate.
3. Program blocks : blocs de programmation.
4. HMI : interfaces opérateur.
5. Connections : communications.
6. Online & diagnostics : surveillance et diagnostic.

## 12. Évaluation des connaissances

Résultat obtenu : 6/6.

Les notions évaluées concernent l'organisation générale d'un projet TIA Portal et le rôle de ses principales parties.

## 13. Conclusion

La compréhension de l'organisation d'un projet TIA Portal est indispensable avant de commencer la programmation.

Elle permet de retrouver facilement les équipements, les variables, les blocs de programme, les interfaces et les outils de diagnostic.

Dans la prochaine leçon, nous découvrirons concrètement la vue du portail et la vue du projet dans TIA Portal V17.
