# Module 02 — Découverte de TIA Portal V17

# Leçon 1 — Présentation générale de TIA Portal V17

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Définir TIA Portal.
- Comprendre son rôle dans l'automatisation industrielle.
- Identifier ses principaux composants.
- Distinguer STEP 7, WinCC Advanced, S7-PLCSIM et WinCC Runtime Advanced.
- Comprendre les grandes étapes d'un projet d'automatisation.

## 2. Définition de TIA Portal

TIA Portal signifie Totally Integrated Automation Portal.

En français : portail d'automatisation totalement intégrée.

C'est un environnement d'ingénierie développé par Siemens. Il permet de concevoir, programmer, configurer, superviser et diagnostiquer des systèmes d'automatisation industrielle.

Il centralise plusieurs outils dans un environnement de travail commun.

### Exemple industriel

Une station de pompage peut comporter :

- Un automate programmable S7-1200.
- Une interface opérateur tactile (IHM).
- Des moteurs électriques.
- Des capteurs de niveau et de pression.
- Des variateurs de vitesse.

TIA Portal permet de travailler sur plusieurs de ces équipements dans un même environnement d'ingénierie.

## 3. Les principaux composants

| Composant | Fonction principale |
|---|---|
| STEP 7 | Configuration et programmation des automates Siemens |
| WinCC | Création et configuration des interfaces homme-machine |
| S7-PLCSIM | Simulation du fonctionnement d'un automate |
| Startdrive | Configuration et mise en service de variateurs Siemens |
| Safety | Configuration des fonctions de sécurité des équipements compatibles |

Tous ces composants ne sont pas nécessairement installés dans chaque édition de TIA Portal.

## 4. Les outils utilisés dans notre formation

Notre environnement de travail comprend :

### 4.1 STEP 7 V17

Il permet de :

- Ajouter et configurer un automate S7-1200.
- Déclarer les variables API.
- Programmer en LAD, FBD ou SCL.
- Compiler le programme.
- Effectuer des tests et diagnostics.

### 4.2 WinCC Advanced V17

Il permet de :

- Créer les écrans IHM.
- Ajouter des boutons et des voyants.
- Afficher les mesures industrielles.
- Configurer les alarmes.
- Concevoir les interfaces de supervision.

### 4.3 S7-PLCSIM

Il permet de simuler le fonctionnement d'un automate Siemens sans disposer d'un automate physique.

Il permet notamment de tester les entrées, les sorties et la logique du programme.

La simulation ne remplace cependant pas tous les essais sur une installation réelle.

### 4.4 WinCC Runtime Advanced

Il permet d'exécuter l'interface graphique conçue avec WinCC Advanced.

Il sert notamment à tester les boutons, les voyants, les affichages et les alarmes.

## 5. Complémentarité des composants

Prenons l'exemple d'une station de pompage.

L'opérateur appuie sur le bouton Marche de l'IHM.

1. WinCC transmet la commande à l'automate.
2. STEP 7 contient le programme qui traite cette commande.
3. L'automate vérifie les conditions programmées.
4. Si les conditions sont satisfaites, la sortie correspondante est activée.
5. Le moteur démarre.
6. L'état de fonctionnement peut être affiché sur l'IHM.

Dans notre environnement de simulation :

- STEP 7 sert à programmer.
- S7-PLCSIM simule l'automate.
- WinCC Advanced sert à concevoir l'IHM.
- WinCC Runtime Advanced exécute l'IHM.

## 6. Les grandes étapes d'un projet TIA Portal

| N° | Étape | Travail réalisé |
|---|---|---|
| 1 | Création du projet | Définition du projet |
| 2 | Configuration API | Ajout et configuration de l'automate |
| 3 | Configuration E/S | Organisation des entrées et sorties |
| 4 | Déclaration des variables | Création des variables API |
| 5 | Programmation | Développement du programme automate |
| 6 | Configuration IHM | Création des écrans opérateur |
| 7 | Communication | Établissement des échanges API-IHM |
| 8 | Compilation | Vérification du projet |
| 9 | Simulation | Tests du programme et de l'IHM |
| 10 | Mise en service | Déploiement sur l'installation réelle |

## 7. Tableau récapitulatif

| Logiciel | Fonction |
|---|---|
| STEP 7 | Programmer l'automate |
| WinCC Advanced | Concevoir l'interface IHM |
| S7-PLCSIM | Simuler l'automate |
| WinCC Runtime Advanced | Exécuter l'interface IHM |

## 8. Évaluation des connaissances

Résultat obtenu : 6/6.

Les notions évaluées sont :

- Signification de TIA Portal.
- Rôle de STEP 7.
- Rôle de WinCC Advanced.
- Rôle de S7-PLCSIM.
- Rôle de WinCC Runtime Advanced.
- Complémentarité des différents composants.

## 9. Conclusion

TIA Portal est un environnement central dans l'ingénierie des systèmes d'automatisation Siemens.

La maîtrise de ses différents composants constitue une base indispensable pour configurer, programmer, simuler et superviser une installation industrielle.

Dans les prochaines leçons, nous découvrirons progressivement son interface et son organisation interne avant de réaliser nos premiers projets pratiques.
