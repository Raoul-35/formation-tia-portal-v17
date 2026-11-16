# Leçon 5 – Découverte des éditeurs de programmation de TIA Portal V17

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- identifier les principaux langages de programmation de TIA Portal ;
- comprendre les principes du LAD, du FBD et du SCL ;
- distinguer une variable d'une instruction ;
- comprendre la structure conditionnelle IF ;
- utiliser les opérateurs d'affectation et de comparaison ;
- reconnaître les principales instructions SCL.

## 2. Définition d'un éditeur de programmation

Un éditeur de programmation est un environnement permettant d'écrire, de représenter et de modifier les instructions exécutées par l'automate.

Dans TIA Portal V17, plusieurs langages sont disponibles pour programmer les automates Siemens S7-1200.

Les trois principaux langages étudiés sont :

- LAD : Ladder Diagram ;
- FBD : Function Block Diagram ;
- SCL : Structured Control Language.

## 3. Le langage LAD

### 3.1. Définition

LAD signifie Ladder Diagram.

Il est également appelé langage à contacts.

Sa représentation graphique ressemble aux schémas électriques traditionnels utilisés dans les installations industrielles.

### 3.2. Principaux éléments

Un réseau LAD comporte notamment :

- des contacts ;
- des bobines ;
- des branches ;
- des instructions et blocs fonctionnels.

### 3.3. Exemple industriel

Commande simple d'un moteur :

```text
       BP_Marche       Moteur
|------| |------------- ( )------|
