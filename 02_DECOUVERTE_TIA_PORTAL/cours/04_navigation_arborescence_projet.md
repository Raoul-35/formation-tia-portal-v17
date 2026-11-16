# Leçon 4 – Navigation dans l'arborescence du projet TIA Portal V17

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- comprendre l'organisation de l'arborescence d'un projet TIA Portal ;
- identifier les principaux dossiers et équipements ;
- retrouver les blocs de programmation ;
- accéder aux variables de l'automate ;
- localiser la configuration matérielle ;
- retrouver les équipements et les connexions réseau.

## 2. Définition de l'arborescence du projet

L'arborescence du projet (Project tree) est une zone de TIA Portal qui présente les différents éléments du projet sous forme de dossiers et de sous-dossiers.

Elle facilite l'organisation, la navigation et l'accès aux ressources du projet.

Elle fonctionne de manière similaire à un explorateur de fichiers.

## 3. Organisation générale

Un projet TIA Portal peut contenir plusieurs équipements et ressources.

Exemple simplifié :

```text
Formation_TIA_Portal_V17
|
|-- PLC_1
|   |
|   |-- Device configuration
|   |
|   |-- Program blocks
|   |   |
|   |   |-- Main [OB1]
|   |
|   |-- PLC tags
|   |   |
|   |   |-- Default tag table
|   |
|   |-- PLC data types
|
|-- Devices & Networks
|
|-- HMI_1
