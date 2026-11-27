# PARCOURS PRATIQUE
## CONCEPTION ET PROGRAMMATION MODULAIRE DES INSTALLATIONS INDUSTRIELLES AVEC SIEMENS

### 1. Présentation générale

Ce parcours pratique a pour objectif de développer les compétences nécessaires à la conception, à la programmation, à la simulation et à la supervision des installations industrielles automatisées utilisant les automates Siemens.

La formation s'appuie sur trois projets industriels progressifs :

1. Station industrielle de dosage de liquide.
2. Ligne de convoyage industrielle.
3. Station industrielle de traitement d'eau.

Les trois applications seront développées avec une approche professionnelle de programmation modulaire.

### 2. Environnement logiciel

- TIA Portal V17 : développement de l'application.
- SIMATIC S7-1200 CPU 1214C : automate cible de formation.
- S7-PLCSIM : simulation de l'automate.
- WinCC Runtime Advanced : supervision.
- LAD : programmation graphique.
- FBD : programmation par blocs fonctionnels.
- SCL : programmation structurée.
- Git et GitHub : archivage des projets et supports pédagogiques.

Aucun automate physique n'est requis pour les exercices de simulation.

### 3. Principes de programmation

Les applications respecteront les principes suivants :

- OB : organisation et coordination de l'exécution du programme.
- FB : gestion des équipements et des fonctions nécessitant une mémoire.
- FC : fonctions de traitement et calculs.
- DB : stockage des données et des états.
- DB d'instance : mémorisation des données propres aux FB.
- UDT : structuration et standardisation des données.
- Bibliothèques : réutilisation des blocs développés.

Chaque bloc devra avoir une responsabilité clairement définie.

Les réseaux LAD et le code SCL devront être commentés.

L'OB1 sera organisé comme un programme principal de coordination et ne devra pas contenir inutilement toute la logique du procédé.

### 4. Projet 01 – Station industrielle de dosage de liquide

Objectif : concevoir une installation de préparation et de dosage automatique de plusieurs liquides.

Fonctions principales :
- Gestion des cuves ;
- Gestion des pompes ;
- Commande des vannes ;
- Dosage des liquides ;
- Gestion des recettes ;
- Agitation ;
- Vidange ;
- Gestion des défauts ;
- Supervision.

Compétences principales :
- Création des FB, FC et DB ;
- Utilisation des UDT ;
- Programmation séquentielle ;
- Gestion des équipements répétitifs ;
- Gestion des modes de fonctionnement ;
- Supervision WinCC.

### 5. Projet 02 – Ligne de convoyage industrielle

Objectif : concevoir une ligne automatisée de transport, d'identification et de tri des produits.

Fonctions principales :
- Gestion des convoyeurs ;
- Gestion des moteurs ;
- Détection des produits ;
- Identification ;
- Orientation ;
- Gestion des vérins ;
- Comptage de production ;
- Gestion des défauts ;
- Supervision.

Compétences principales :
- Réutilisation des FB ;
- Gestion de plusieurs équipements identiques ;
- Coordination de plusieurs postes ;
- Gestion des séquences ;
- Gestion des priorités ;
- Suivi des produits ;
- Diagnostic des défauts.

### 6. Projet 03 – Station industrielle de traitement d'eau

Objectif : concevoir une installation automatisée de réception, filtration, transfert et stockage de l'eau.

Fonctions principales :
- Gestion des cuves ;
- Gestion des pompes ;
- Gestion des vannes ;
- Surveillance des niveaux ;
- Surveillance des pressions ;
- Mesure des débits ;
- Filtration ;
- Régulation ;
- Alternance des pompes ;
- Gestion des alarmes ;
- Maintenance.

Compétences principales :
- Gestion d'un procédé industriel complet ;
- Traitement des mesures analogiques ;
- Mise à l'échelle ;
- Régulation PID ;
- Gestion des équipements ;
- Gestion des alarmes ;
- Supervision et tendances.

### 7. Méthodologie commune

Chaque projet suivra les étapes suivantes :

1. Étude du procédé.
2. Analyse fonctionnelle.
3. Rédaction du cahier des charges.
4. Élaboration du schéma fonctionnel.
5. Élaboration du Grafcet.
6. Définition des entrées et sorties.
7. Conception de l'architecture logicielle.
8. Création des UDT et DB.
9. Développement des FB et FC.
10. Organisation de l'OB1.
11. Simulation sous PLCSIM.
12. Développement de la supervision WinCC.
13. Tests et validation.
14. Documentation et archivage Git.

### 8. Méthode pédagogique

La progression sera réalisée étape par étape.

Chaque leçon comprendra :
- Des objectifs pédagogiques ;
- Une explication théorique ;
- Une application pratique ;
- Des instructions précises dans TIA Portal ;
- Des commentaires sur les réseaux et les blocs ;
- Des tests de fonctionnement ;
- Une validation avant la suite.

Les erreurs de programmation seront analysées et corrigées avant de poursuivre.

Aucune modification importante de l'architecture ne sera introduite sans justification.

### 9. Simulation et validation

Les programmes seront exécutés dans S7-PLCSIM.

WinCC Runtime Advanced assurera la visualisation et l'interaction avec l'application.

Les tests porteront notamment sur :
- Le fonctionnement normal ;
- Les modes automatique et manuel ;
- Les défauts des équipements ;
- Les arrêts normaux ;
- Les arrêts d'urgence simulés ;
- Les interverrouillages ;
- Les séquences ;
- Les alarmes ;
- Les reprises après arrêt.

Les essais en simulation ne remplacent pas les validations de sécurité et les essais physiques sur une installation réelle.

### 10. Livrables attendus

Pour chaque projet :

- Projet TIA Portal ;
- Programme automate complet ;
- Architecture des blocs ;
- Grafcet ;
- Documentation fonctionnelle ;
- Documentation technique ;
- Supervision WinCC ;
- Rapport de tests ;
- Support pédagogique Markdown ;
- Archivage Git et GitHub.

### 11. Objectif final

À la fin de ce parcours, l'apprenant devra être capable de concevoir, programmer, simuler, superviser, documenter et maintenir une application industrielle complète avec les automates Siemens S7-1200 et S7-1500.
